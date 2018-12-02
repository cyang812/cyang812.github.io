title: STM32 定时器输入捕获实现红外遥控数据接收
date: 2017/9/11 12:35:23
tags:
- STM32
- 红外线
- 定时器
- 输入捕获
categories:
- STM32
thumbnail: http://blog.cyang.top/IR_Tim_Capture.png
---


![这里写图片描述](http://blog.cyang.top/IR_Tim_Capture.png)

> 之前已经写过了一个使用定时器普通计时功能来识别红外遥控数据的[文章](http://cyang.tech/2017/08/09/IR_Timer/)。本次是使用定时器输入捕获来实现，这种方法比起定时器普通计数来说要更加复杂一些，不过效果会更好。

<!-- more -->

# 一、原理

## 1、红外发射协议

- 红外发射协议已经在之前的[文章](http://cyang.tech/2017/08/09/IR_Timer/)中写过，在此就不赘述。

## 2、定时器计数和输入捕获

- 定时器就是按照一个特定的频率对计数值进行加一或减一操作，当数值溢出时则产生一个标志或中断。

- 定时器的输入捕获就是可以测量输入信号的脉冲宽度。

- 本次就是通过普通计数和输入捕获的结合来实现的。

## 3、实现方法

- 利用定时器记录输入信号高脉冲的时间，通过该时间来判断数据是否是同步头信息、数据 1 或者数据 0。

# 二、实现

## 1、配置定时器2输入捕获通道

- 示例代码中使用 PA1 管脚，配置为上拉输入模式，复用功能为定时器2的通道2。

- 定时器采用普通定时器，定时器2，该定时器具有输入捕获功能。

- 配置定时器的两种工作模式，一个是普通计数器`TIM_TimeBaseInit`，一个是输入捕获模式`TIM_ICInit`。

- 配置定时器2的中断源，有两个中断源，一个是更新中断`TIM_IT_Update`，一个是输入捕获中断`TIM_IT_CC2`。

- 配置代码如下:

```c
/*
* Ir input pin
* mode:floating input
* pin: PA1
* GPIO_AF: TIM2_CH2
*/
void Ir_Pin_Init()
{
    GPIO_InitTypeDef GPIO_InitStructure;

    GPIO_PinAFConfig(GPIOA,GPIO_PinSource1,GPIO_AF_2);

    GPIO_InitStructure.GPIO_Pin  =  GPIO_Pin_1;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);
    GPIO_SetBits(GPIOA,GPIO_Pin_1); //output 1
}

//PA1 TIM2_CH2
//使用GPIO输入捕获实现红外接收
void Remote_Init()
{
    NVIC_InitTypeDef                NVIC_InitStructure;
    TIM_TimeBaseInitTypeDef         TIM_TimeBaseStructure;
    TIM_ICInitTypeDef               TIM_ICInitStructure;

    /*使能TIM1时钟,默认时钟源为PCLK1(PCLK1未分频时不倍频,否则由PCLK1倍频输出),可选其它时钟源*/
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);

    Ir_Pin_Init();

    TIM_ClearITPendingBit(TIM2,TIM_IT_Update|TIM_IT_CC2); //清除中断和捕获标志位

    TIM_TimeBaseStructure.TIM_Period = 1000; //设定计数器自动重装值 最大10ms溢出
    TIM_TimeBaseStructure.TIM_Prescaler = 480-1;   //预分频器,0.1M的计数频率,10us加1.
    TIM_TimeBaseStructure.TIM_ClockDivision = TIM_CKD_DIV1; //设置时钟分割:TDTS = Tck_tim
    TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;  //TIM向上计数模式
    TIM_TimeBaseInit(TIM2, &TIM_TimeBaseStructure); //根据指定的参数初始化TIMx

    TIM_ICInitStructure.TIM_Channel = TIM_Channel_2;  // 选择输入端 IC2映射到TI2上
    TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;   //上升沿捕获
    TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;
    TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;  //配置输入分频,不分频
    TIM_ICInitStructure.TIM_ICFilter = 0x03;//IC4F=0011 配置输入滤波器 8个定时器时钟周期滤波
    TIM_ICInit(TIM2, &TIM_ICInitStructure);//初始化定时器输入捕获通道

    NVIC_InitStructure.NVIC_IRQChannel = TIM2_IRQn;  //TIM2中断
    NVIC_InitStructure.NVIC_IRQChannelPriority = 2;  //优先级0级
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE; //IRQ通道被使能
    NVIC_Init(&NVIC_InitStructure);  //根据NVIC_InitStruct中指定的参数初始化外设NVIC寄存器

    TIM_ITConfig(TIM2,TIM_IT_Update|TIM_IT_CC2,ENABLE);//允许更新中断 ,允许CC2IE捕获中断
    TIM_Cmd(TIM2,ENABLE);    //使能定时器2
}
```

## 2、添加定时器2的中断服务函数

- 使用了两种定时器中断源，分别为计数溢出中断和输入捕获中断。但是这两种方式触发中断的中断服务函数是同一个，即`void TIM2_IRQHandler(void)`。

- 定时器使用的是 TIM2 通用定时器，模式为向上计数。在该模式中，计数器从 0 计数到自动加载值 (TIMx_ARR计数器的内容) ，然后重新从 0 开始计数并且产生一个计数器溢出事件。定时器计数溢出的周期为10ms，该中断的产生说明在10ms内都没有输入捕获来清空计数值，也就是输入信号没有发生变化，说明 10ms 没有收到红外信号了，因此可判断为接收完成。

- 输入捕获是为了测量高电平的持续时间，因此采用上升沿触发中断，对计数值清零，切换下一次为下降沿触发；在下降沿触发中断时，记下计数值，切换下一次为上升沿触发。因此在下降沿记下的时间即为高电平的时序时间。记录高电平持续时间的原因，是因为红外信号在表示逻辑0、逻辑1时低电平的持续时间的相同的，而高电平的持续时间不同的。

- 示例代码如下：

```c
//遥控器接收状态
//[7]:收到了引导码标志
//[6]:得到了一个按键的所有信息
//[5]:保留
//[4]:标记上升沿是否已经被捕获
//[3:0]:溢出计时器
uint8_t  RmtSta = 0;
uint16_t Dval;         //下降沿时计数器的值
uint32_t RmtRec = 0;   //红外接收到的数据
uint8_t  RmtCnt = 0;   //按键按下的次数

//定时器2中断服务程序
void TIM2_IRQHandler(void)
{
    if(TIM_GetITStatus(TIM2,TIM_IT_Update) != RESET)  //计数溢出中断
    {
        if(RmtSta & 0x80) //上次有数据被接收到了
        {
            RmtSta &= ~0x10;  //取消上升沿已经被捕获标记
            if((RmtSta & 0x0f) == 0x00) RmtSta |= 1<<6; //电平没有变化后延时10ms，可标记已经完成一次按键的信息采集

            if((RmtSta & 0x0f) < 14) RmtSta++;  //若进入14，意味着定时器计数溢出了14次，即140ms
            else
            {
                RmtSta &= ~(1<<7); //清空引导标识
                RmtSta &= 0xf0;   //清空计数器 ，意味着可以下一次的检测

                //RmtCnt = 0;
                //RmtSta &= ~(1<<6);
            }
        }
    }

    if(TIM_GetITStatus(TIM2,TIM_IT_CC2) != RESET)    //输入捕获中断
    {
        if(RDATA)
        {
            TIM_OC2PolarityConfig(TIM2,TIM_ICPolarity_Falling); //设置下降沿触发捕获
            TIM_SetCounter(TIM2,0);    // 清零计数值
            RmtSta |= 0x10;
        }
        else
        {
            Dval = TIM_GetCapture2(TIM2);  // 获取计数值，该计数值代表高电平持续时间
            TIM_OC2PolarityConfig(TIM2,TIM_ICPolarity_Rising);  // 设置上升沿触发
            if(RmtSta & 0x10)
            {
                if(RmtSta & 0x80)
                {
                    if((Dval > 30) && (Dval < 80))
                    {
                        RmtRec <<= 1;
                        RmtRec |= 0;
                    }else if((Dval > 140) && (Dval < 180))
                    {
                        RmtRec <<= 1;
                        RmtRec |= 1;
                    }else if((Dval > 200) && (Dval < 250))
                    {
                        RmtCnt++;    // 重复码
                        RmtSta &= 0xf0;
                    }
                }else if((Dval > 420) && (Dval < 470))
                {
                    RmtSta |= 1<<7; //表示接收到同步头
                    RmtCnt = 0;    //清零按键次数
                }
            }
            RmtSta &= ~0x10;
        }
    }
    TIM_ClearFlag(TIM2,TIM_IT_Update|TIM_IT_CC2);
}
```

## 3、红外按键扫描函数

- 该函数放在主循环中，轮训判断按键是否接收完成。如果接收完成则开始分析键值。

- 该函数返回一个16位的数值，其中低八位表示键值，高八位表示按下的次数，依次来分析长按键和短按键。这一点主要是通过红外协议中重复码的规定来实现的。

- 红外协议中规定，若按下一个键后没有放开，则会以 108ms 为一个周期发送重复码。重复码表现为2.25ms的高电平。

- 示例代码如下：

```c
/*SystemKeybuf
*处理红外键盘
*返回值: keybuf
*   0xffff,没有任何按键按下
*   其他,按下的按键键值.
*   keybuf[15:8] : key cnt
*   keybuf[7:0]  : key code
*/
uint16_t Remote_Scan(void)
{
    uint16_t keybuf = 0xffff;
    uint8_t sta = 0xff;
    uint8_t t1,t2;

    if(RmtSta & (1<<6))//得到一个按键的所有信息了
    {
        t1 = RmtRec >> 24;            //得到地址码
        t2 = (RmtRec >> 16) & 0xff; //得到地址反码
        //printf("rmtrec = %#x\n",RmtRec);

        if((t1 == (uint8_t)~t2) && (t1 == REMOTE_ID))//检验遥控识别码(ID)及地址
        //if(t1 == (uint8_t)~t2)//检验遥控识别码(ID)及地址
        {
            //printf("t1 = %d\n",t1);
            t1 = RmtRec >> 8;
            t2 = RmtRec;
            if(t1 == (uint8_t)~t2) sta = t1;//键值正确
        }

        if((sta != 0xff))//按键数据正确
        {
            if(RmtCnt < 10) // short key
            {
                if((RmtSta & 0x80) == 0) // release
                {
                    keybuf = RmtCnt;
                    keybuf <<= 8;
                    keybuf |= sta;

                    RmtSta &= ~(1<<6); //清除接收到有效按键标识
                    RmtCnt = 0;       //清除按键次数计数器
                }
            }
            #if 0
            else if(RmtCnt == 10)// && RmtCnt < 15)  // long key
            {
                keybuf = RmtCnt;
                keybuf <<= 8;
                keybuf |= sta;
            }
            else if(RmtCnt > 10)
            {
                if(((RmtCnt - 10) % 2) == 0)   //每5次重复码处理一次按键 
                {
                    keybuf = RmtCnt;
                    keybuf <<= 8;
                    keybuf |= sta;
                }
            }
            #else
            else if((sta == KEY_NEXT) || (sta == KEY_PREV) || (sta == KEY_MENU) || (sta == KEY_SELECT)) //仅这四个按键支持连续键功能
            {
                if(((RmtCnt - 10) % 2) == 0)   //每2次重复码处理一次按键
                {
                    keybuf = RmtCnt;
                    keybuf <<= 8;
                    keybuf |= sta;
                }

                if((RmtSta & 0x80) == 0) // release
                {
                    RmtSta &= ~(1<<6); //清除接收到有效按键标识
                    RmtCnt = 0;       //清除按键次数计数器
                }
            }
            else //长按键
            {
                //if((RmtSta & 0x80) == 0)  //release ，并非等到按键释放才处理
                {
                    keybuf = RmtCnt;
                    keybuf <<= 8;
                    keybuf |= sta;

                    RmtSta &= ~(1<<6); //清除接收到有效按键标识
                    RmtCnt = 0;       //清除按键次数计数器

                    RmtRec = 0;       //清除接收值
                }
            }
            #endif
        }
    }
    return keybuf;
}
```

## 4、主函数

- 在 main 函数中，对 IO 口和 定时器进行初始化。

- 主循环中，通过判断接收完成标志位，对接收完成的按键控制码进行打印。

- `SystemKeyHandle()`函数处理每一个按键的操作逻辑。

- 示例代码如下：

```c
void main()
{
    uint16_t SystemKeybuf;
    ...

    //Ir Remote
    Remote_Init();

    while(1)
    {
        SystemKeybuf = Remote_Scan();    //红外按键扫描
        if(SystemKeybuf != 0xffff) //如果有按键未处理
        {
            //main_printf("SystemKeyBuf = %#x\n",SystemKeybuf);
            SystemKeyHandle();
            SystemKeybuf = 0xffff;
        }
    }
}
```

# 三、演示

如下图为串口打印出接收的红外按键值信息：
![这里写图片描述](http://blog.cyang.top/20170911164650633?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

说明1：这篇文章所使用的方法主要参考自[这篇文章](http://blog.csdn.net/qq_16255321/article/details/43602535)，代码仅做了部分修改，并在源代码基础上添加了部分代码，用于实现连续键。

> 本文档基于 STM32 F1 系列 MCU，固件库版本 3.5。其他 MCU 及固件库仅需要对库函数略作修改。

> [参考链接1：cortex_m3_stm32嵌入式学习笔记（二十三）：红外遥控实验（输入捕捉+解码）](http://blog.csdn.net/qq_16255321/article/details/43602535)

> [参考链接2：STM32 红外线实验](http://cyang.tech/2016/07/19/STM32%20%E7%BA%A2%E5%A4%96%E7%BA%BF%E5%AE%9E%E9%AA%8C/)

> [参考链接3：STM32 红外线实验](http://cyang.tech/2017/08/09/IR_Timer/)

> [更多关于 STM32 的文章](http://cyang.tech/tags/STM32/)

