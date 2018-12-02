title: STM32 定时器实现红外遥控数据接收
date: 2017/8/9 12:35:23
tags:
- STM32
- 红外线
categories:
- STM32
thumbnail: http://blog.cyang.top/20170808231029615?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


![这里写图片描述](http://blog.cyang.top/20170808231029615?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

# 一、原理

## 1、红外发射协议
- 红外通信的协议有很多种。这个实验使用的是NEC协议。这个协议采用PWM的方法进行调制，利用脉冲宽度来表示 0 和 1 。

- NEC 遥控指令的数据格式为：同步码头、地址码、地址反码、控制码、控制反码。同步码由一个 9ms 的低电平和一个 4.5ms 的高电平组成，地址码、地址反码、控制码、控制反码均是 8 位数据格式。按照低位在前，高位在后的顺序发送。采用反码是为了增加传输的可靠性。因此，每帧的数据为 32 位，包括地址码，地址反码，控制码，控制反码。反码可用于解码时进行校验比对。

- NEC码的位定义：一个脉冲对应 560us 的连续载波，一个逻辑 1 传输需要 2.25ms（560us 脉冲+1680us 低电平），一个逻辑 0 的传输需要 1.125ms（560us 脉冲+560us 低电平）。而遥控接收头在收到脉冲的时候为低电平，在没有脉冲的时候为高电平，这样，**在接收头端收到的信号为：逻辑 1 应该是 560us 低+1680us 高，逻辑 0 应该是 560us 低+560us 高。**

   ​

- 红外数据的波形如下图：包括一个同步头和 32 帧数据。
   ![这里写图片描述](http://blog.cyang.top/20170808230617825?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 下图可看出，同步头为 9ms 低电平加上 4.5ms 高电平，控制码为 8 个 0，控制反码为 8 个 1。
    ![这里写图片描述](http://blog.cyang.top/20170808231029615?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


## 2、定时器计数
- 定时器就是按照一个特定的频率对计数值进行加一或减一操作，当数值溢出时则产生一个标志或中断。这里是用定时器计数产生一个周期性的中断。

## 3、实现方法
- 利用定时器记录两个下降沿之间的时间，通过该时间判断是否是同步头信息、数据 1 或者数据 0。当检测到同步头，开始记录 32 个数据的时间值。
  ![这里写图片描述](http://blog.cyang.top/20170808225212279?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


# 二、实现

## 1、配置 GPIO 口下降沿触发中断

- 示例代码中使用 PA7 管脚，配置为上拉输入模式。

- 选择下降沿触发，是因为红外接收管默认情况下保持高电平，接收到数据时从高电平转变为低电平。

- 中断源选择为 `EXTI_Line7` ，在库函数中对该中断源定义的服务函数为 `EXTI9_5_IRQHandler()`，也就是说外部中断 5 到 9 是 共用一个中断服务函数的。

- 配置代码如下:

  ```c
  void IR_Pin_init()
  {
      GPIO_InitTypeDef GPIO_InitStructure;
      EXTI_InitTypeDef EXTI_InitStructure;
      NVIC_InitTypeDef NVIC_InitStructure;
      RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA|RCC_APB2Periph_AFIO,ENABLE);

      GPIO_InitStructure.GPIO_Pin=GPIO_Pin_7;
      GPIO_InitStructure.GPIO_Mode=GPIO_Mode_IPU;
      GPIO_Init(GPIOA,&GPIO_InitStructure);

      EXTI_ClearITPendingBit(EXTI_Line7);
      GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource7); 
      EXTI_InitStructure.EXTI_Line=EXTI_Line7;
      EXTI_InitStructure.EXTI_Mode=EXTI_Mode_Interrupt;
      EXTI_InitStructure.EXTI_Trigger=EXTI_Trigger_Falling;
      EXTI_InitStructure.EXTI_LineCmd=ENABLE;
      EXTI_Init(&EXTI_InitStructure); 

      NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1); 
      NVIC_InitStructure.NVIC_IRQChannel = EXTI9_5_IRQn; 
      NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0; 
      NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1; 	 
      NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;  
      NVIC_Init(&NVIC_InitStructure);
  }
  ```


## 2、配置定时器计数值

- 定时器使用的是 TIM2 通用定时器，模式为向上计数。在该模式中，计数器从 0 计数到自动加载值 (TIMx_ARR计数器的内容) ，然后重新从 0 开始计数并且产生一个计数器溢出事件。 

- 示例函数接收两个参数，分别为预分频器的值和自动加载值。通过调整这两个参数，可以灵活地改变定时器的计数周期。例如在 TIM2 的默认时钟源 PCLK1 为96MHz时，使用语句 `Tim2_UPCount_Init(SystemCoreClock/1000000-1,100-1);  //0.1ms` 进行初始化，可以每 0.1ms 产生一次中断。

- 示例代码如下：

  ```c
  void Tim2_UPCount_Init(u16 Prescaler,u16 Period)
  {
      TIM_TimeBaseInitTypeDef TIM_StructInit;
      NVIC_InitTypeDef NVIC_StructInit;
  	
      RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);
  		
      TIM_StructInit.TIM_Period=Period;
      TIM_StructInit.TIM_Prescaler=Prescaler;
      TIM_StructInit.TIM_ClockDivision=TIM_CKD_DIV1;
      TIM_StructInit.TIM_CounterMode=TIM_CounterMode_Up;
      TIM_StructInit.TIM_RepetitionCounter=0;
      TIM_TimeBaseInit(TIM2, &TIM_StructInit);
  	
      TIM_ITConfig(TIM2, TIM_IT_Update, ENABLE);
      TIM_Cmd(TIM2, ENABLE);	
      TIM_ClearFlag(TIM2, TIM_FLAG_Update);
  	
      NVIC_StructInit.NVIC_IRQChannel=TIM2_IRQn;
      NVIC_StructInit.NVIC_IRQChannelCmd=ENABLE;
      NVIC_StructInit.NVIC_IRQChannelPreemptionPriority=0;
      NVIC_StructInit.NVIC_IRQChannelSubPriority=1;
      NVIC_Init(&NVIC_StructInit);
  }
  ```



## 3、定时器中断函数统计时间

- 如上所说，定时器每 0.1ms 计数完成，产生中断，在中断函数中对标志位 `ucTim2Flag` 加 1，意味着时间过去了 0.1ms。

- 时间标志位原型为 `uint16_t ucTim2Flag;` 。

- 示例代码如下：

  ```c
  void TIM2_IRQHandler(void)
  {
      TIM_ClearITPendingBit(TIM2, TIM_IT_Update);
      ucTim2Flag++;
  }
  ```



## 4、GPIO 中断函数中接收 32 位数据

- 在下降沿触发的 IO 口中断函数中，需要实现统计两个下降沿之间的时间，并将其记录在数组中。

- 下降沿第一次触发时，清除当前定时器中的计数值，以便统计时间。之后每一次下降沿触发就记录下当前计数值，然后再对其清零。如果该时间在同步头的时间区间内，对索引进行清零，表示重新开始接收数据。

- 完整接收同步头和 32 个数据之后，表示接收完成。

- 示例代码如下：

  ```c
  uint8_t irdata[33];   //用于记录两个下降沿之间的时间
  bool receiveComplete; //接收完成标志位
  uint8_t idx;          //用于索引接收到的数值
  bool startflag;       //表示开始接收

  void EXTI9_5_IRQHandler(void)
  {
      uint16_t ir_time;
      if(startflag)
      {
          ir_time = ucTim2Flag;	
          if(ucTim2Flag < 150 && ucTim2Flag >= 50 ) // 接收到同步头
          {
              idx=0;  // 数组下标清零
          }
  		
          irdata[idx] = ucTim2Flag;  // 获取计数时间
          ucTim2Flag = 0;            // 清零计数时间，以便下次统计
          idx++;                     // 接收到一个数据，索引加1
  		
          if(idx==33)       // 如果接收到33个数据，包括32位数和以一个同步头
          {
              idx=0;
              ucTim2Flag = 0;
              receiveComplete = TRUE;
          }
      }
      else   // 下降沿第一次触发
      {
          idx = 0;
          ucTim2Flag = 0;
          startflag = TRUE;
      }
  	
      EXTI_ClearITPendingBit(EXTI_Line7);  // 清除中断标志
  }
  ```

  ​

## 5、判断控制码值

- 由于中断函数中接收并记录下的数据是两个下降沿之间的时间，并不是红外所发送的数据。因此需要根据红外协议，对 32 个时间进行判断，从而获得红外真正发送的数据。

- 下面这个函数需要在红外完整接收数据后执行，可通过判断接收完成标志位 `receiveComplete`  来实现。

- 示例代码如下：

  ```c
  uint8_t Ir_Server()
  {
      uint8_t i,j,idx=1; //idx 从1 开始表示对同步头的时间不处理
      uint8_t temp;
      for(i=0; i<4; i++)
      {
          for(j=0; j<8; j++)
          {
              if(irdata[idx] >=8 && irdata[idx] < 15)   //表示 0
              {
                  temp = 0;
              }
              else if(irdata[idx] >=18 && irdata[idx]<25) //表示 1
              {
                  temp = 1;
              }
              remote_code[i] <<= 1;
              remote_code[i] |= temp;
              idx++;
          }
      }
  	
      return remote_code[2];  // 该数组中记录的是控制码，每个按键不一样
      //for(idx=0; idx<4; idx++)
      //{
      //	printf("remote_code[%d] = %#x\n",idx,remote_code[idx]);
      //}
  }
  ```

  ​

## 6、主函数

- 在 main 函数中，对 IO 口和 定时器进行初始化。

- 主循环中，通过判断接收完成标志位，对接收完成的按键控制码进行打印。

- 示例代码如下：

  ```c
  void main()
  {
      ...
  	
      IR_Pin_init();
      Tim2_UPCount_Init(SystemCoreClock/1000000-1,100-1);
   	
      while(1)
      {
          if(repeatEnable)
          {
              repeatEnable = FALSE;
              Ir_Server();
              printf("key_code = %#x\n",remote_code[2]);
          }
      }
    
  }
  ```

  ​

# 三、演示

如下图为串口打印出接收的红外按键值信息：
![这里写图片描述](http://blog.cyang.top/20170808225232539?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

说明1：这只是实现红外接收的其中一种方法，网上还有一种比较常见的方法是利用下降沿触发，在中断中进行延迟，判断高电平持续时间以此来判断信号类别。个人感觉这不是一种很好的方法，因为在中断中进行延时会导致主函数得不到及时的处理。

说明2：在调试时，不要在中断处理中加入过多无关语句，例如打印语句，这会导致结果出错。

> 本文档基于 STM32 F1 系列 MCU，固件库版本 3.5。其他 MCU 及固件库仅需要对库函数略作修改。

> [参考链接：STM32 红外线实验](http://cyang.tech/2016/07/19/STM32%20%E7%BA%A2%E5%A4%96%E7%BA%BF%E5%AE%9E%E9%AA%8C/)

> [更多关于 STM32 的文章](http://cyang.tech/tags/STM32/)

