title: I2C Bit-Bang 程序分析
date: 2017/3/20 11:19:23
tags:
- STM32
- IIC
categories:
- 硬件
thumbnail: http://blog.cyang.top/20170316182930507?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# 一、Bit Bang
关于 Bit Bang 的解释:Use software to control serial communication at general-purpose I/O pins,简单来讲就是使用软件通过 IO 脚去实现 I2C 的时序从而使用 I2C 协议进行通信。

这样做的好处是可以突破硬件上的限制，例如芯片不具有硬件 I2C 模块，或者硬件 I2C 模块损坏，又或者使用硬件 I2C 模块时布线非常麻烦。坏处是需要写代码模拟时序，根据不同的硬件平台和不同的时钟频率，代码中的部分参数是不一样的。

# 二、代码分析
以下代码基于 STM32 系列 MCU

使用软件模拟 I2C 的步骤如下：

- 1、设置 GPIO 管脚
设置两个管脚作为 SCL 和 SDA，例如 GPIOA1 和 GPIOA2

```c
#define SCL_PORT            GPIOA
#define SCL_PIN             GPIO_Pin_1
#define SCL_HIGH 			GPIOA->BSRR=(uint32_t)GPIO_Pin_1	
#define SCL_LOW 			GPIOA->BRR=(uint32_t)GPIO_Pin_1

#define SDA_PORT            GPIOA
#define SDA_PIN 			GPIO_Pin_2
#define SDA_HIGH 			GPIOA->BSRR=(uint32_t)GPIO_Pin_2
#define SDA_LOW 			GPIOA->BRR=(uint32_t)GPIO_Pin_2
#define SDA_READ 			(uint16_t)(GPIOA->IDR&GPIO_Pin_2)
#define SDA_OUT 			GPIOA->MODER|=(((uint32_t)GPIO_Mode_OUT) << (2 * 2))
#define SDA_IN 				GPIOA->MODER&=~(GPIO_MODER_MODER0<<(2 * 2))
```

- 2、SCL时钟周期

```c
void I2C_Delay(void)
{
	uint8_t i = 200; //根据具体的硬件平台和主频调整
	while(i--);
}
```

- 3、附加设置
这里主要是使用宏定义模拟函数

```c
#define SCL_OUTH() 		SCL_HIGH
#define SCL_OUTL() 		SCL_LOW
#define SDA_OUTH() 		SDA_HIGH
#define SDA_OUTL() 		SDA_LOW
#define SDA_SETIN() 	SDA_IN
#define SDA_READ() 		SDA_READ

void SDA_SETOUT(void) 	
{
	SDA_IN;
	SDA_OUT;   //确保 IO 为输出模式
}

```

- 4、I2C 启动
![这里写图片描述](http://blog.cyang.top/20170316182930507?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
```c
void I2C_Start(void)               
{ 
	SCL_OUTH();
	SDA_OUTH();
    I2C_Delay();
    SDA_OUTL();
    I2C_Delay();
    SCL_OUTL();
	I2C_Delay();
}
```

- 5、I2C停止
![这里写图片描述](http://blog.cyang.top/20170316183008820?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
```c
void I2C_Stop(void)              
{
	SCL_OUTL();
	SDA_OUTL();
    I2C_Delay();	
    SCL_OUTH();
    I2C_Delay();		
    SDA_OUTH();
	Delay(Delay5ms);	//Delay() 为系统延时，用于确保数据传输正确
}
```

- 6、发送 8 位数据，返回值为从响应 ACK 标志
```c
uint8_t I2C_WriteByte(uint8_t Data)
{
    uint8_t i,bAck=0;

    for(i=0;i<8;i++)    //循环加移位，不断地将数据通过 SDA 管脚的高低电平发送出去
    {
        SCL_OUTL();			
        if (Data & 0x80)	
            SDA_OUTH();
        else				
            SDA_OUTL();
        I2C_Delay();
        SCL_OUTH();			
        I2C_Delay();
        Data <<= 1;
    }
    
    SCL_OUTL();				
    I2C_Delay();
    SCL_OUTH();				
    I2C_Delay();		
    SDA_SETIN();       //设置 SDA 管脚为输入模式
    if(SDA_READ())     //判断从机响应
        bAck = 1;
    else
        bAck = 0;
    
    SCL_OUTL();				
    SDA_SETOUT();
    SDA_OUTH();
    I2C_Delay();	
    return ((uint8_t)(!bAck));	
}
```
 
- 7、接收 8 位数据
```c
uint8_t I2C_ReadByte(uint8_t bLSByte)
{
	uint8_t i,Data=0;
    SDA_SETIN();
	for(i=8; i!=0; i--) //循环加移位接收 8 位数据
	{
		SCL_OUTL();			
		Data = Data << 1;
		I2C_Delay();
		SCL_OUTH();			
		I2C_Delay();
		if(SDA_READ())
			Data |= 0x01;
		else
			Data &= 0xfe;
	}	
	
	SCL_OUTL();				
	SDA_SETOUT();	
	if(bLSByte)
		SDA_OUTH();	// for Nack
	else
		SDA_OUTL();	// for ACK
    I2C_Delay();		
	SCL_OUTH();				
  	I2C_Delay();	

    SCL_OUTL();				
    I2C_Delay();	
	return(Data);
}
```

# 三、操作实例

以下代码为通过调用上面的基本代码来实现 I2C 通信

- 1、设置 DAC 寄存器的值

	三个参数分比为从机地址，寄存器地址，8 位数据

```c
uint8_t DAC_Write_1byte(uint8_t Slave,uint8_t Regis_Addr,uint8_t Data)
{
    uint8_t succ,time=0;

    I2C_Start();
    succ=I2C_WriteByte(Slave);
    while((succ!=1)&&(time<3)) //从机没有响应，重试三次
    {
        I2C_Stop();
        I2C_Start();
        succ=I2C_WriteByte(Slave);      
        time++;
    }
    succ=I2C_WriteByte(Regis_Addr); //发送寄存器地址
    succ=I2C_WriteByte(Data);	//发送数据
    I2C_Stop();	
    return succ;
}
```

- 2、读取 DAC 寄存器的值

	两个参数分别为从机地址，寄存器地址，返回数据为 16 位。这是由于某些器件的硬件设计，采用 7 位表示寄存器地址，而每个寄存器包含 9 位数据。更常见的方式为 8 位寄存器地址，一个寄存器 8 位数据，这种方式的代码仅返回 8 位数据，见代码 2。 

代码 1，返回 16 位数据，不常见
```c
uint16_t DAC_Read_1byte(uint8_t Slave,uint8_t Regis_Addr)
{ 
    uint8_t Data[2];
    uint8_t succ,time=0;
    uint16_t retData=0;
    
    I2C_Start();
    succ=I2C_WriteByte(Slave);
    while((succ!=1)&&(time<3))
    {
        I2C_Stop();
        I2C_Start();
        succ=I2C_WriteByte(Slave);
        time++;
    }
    succ=I2C_WriteByte(Regis_Addr);
    I2C_Start();	
    succ=I2C_WriteByte((Slave|0x01));
    Data[0] = I2C_ReadByte(0);
    Data[1] = I2C_ReadByte(1);
    I2C_Stop();
	retData = (uint16_t)Data[0]<<8;
	retData += (uint16_t)Data[1];
    return retData;
}
```

代码 2，返回 8 位数据

```c
uint8_t DAC_Read_1byte(uint8_t Slave,uint8_t Regis_Addr)
{
    uint8_t succ,time=0;
    uint8_t dat;

    I2C_Start();
    succ=I2C_WriteByte(Slave+1);  //加 1 代表读数据 
    while((succ!=1)&&(time<3)) //从机没有响应，重试三次
    {
        I2C_Stop();
        I2C_Start();
        succ=I2C_WriteByte(Slave+1);      
        time++;
    }
    succ=I2C_WriteByte(Regis_Addr); //发送寄存器地址
    dat=I2C_ReadByte(0);	//发送数据
    I2C_Stop();	
    return dat;
}
```
