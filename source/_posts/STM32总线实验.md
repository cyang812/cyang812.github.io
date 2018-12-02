title: STM32总线实验
date: 2016/7/18 10:08:23
tags:
- STM32
- IIC
categories:
- STM32
thumbnail: http://blog.cyang.top/iic/iic.jpg?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# STM32总线实验

## 项目介绍
通过IIC总线，向EEPROM存储器24c02进行读写操作。在51单片机中，由于没有IIC接口，所以需要通过两个管脚模拟IIC协议的时序，从而进行IIC总线通信。而在STM32中，是具有IIC总线接口的，但是比较复杂，所以这里依然通过GPIO口模拟IIC时序。通过IIC总线与24C02进行通信，并通过串口返回数据。

<!-- more -->

## 知识点
- 1、IIC是飞利浦公司推出的一种串行总线，可以连接多个设备。IIC总线仅包含两根信号线。一根为SDA（数据线），另一根为SCL（时钟线）。
- 2、每一个连接到IIC总线都具有唯一的地址，每一个设备之间为与的关系。
- 3、由主机发送起始s和终止p信号。起始信号后紧跟着的数据为寻址信号字节。通过这个字节可以选择主机想要与之通信的从机。这个字节的高7位表示地址，其中包括固定地址和可编程地址，可编程地址的位数决定了这种类型的芯片最多可以连接到IIC总线的数量。例如24c02芯片的7为地址为1010，后三位可编程为为A2，A1，A0。这三位为外接口，可通过连接高电平或者接地进行编程。寻址字节的最后一位为数据的传送方向，若为0，则表示主机向从机写数据；若为1，则表示主机需要向从机度数据。
- 4、一问一答式。每次数据传送都需要进行应答。每个数据位均为8位，后面会跟一个从机或者主机的应答位，因此每帧数据为9位。
- 5、每次数据传送都是由主机产生终止信号来结束，但是，若主机希望继续占用总线进行新的数据传送，则可以不产生终止信号，马上再次发送起始信号对另从机进行寻址。
- 6、起始信号s；终止信号p；0/应答；1/非应答这四种信号的表示方法如下图。
![image](http://blog.cyang.top/iic/iic.jpg?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


## 部分代码
- 1、由于还是使用STM32 的两个GPIO口进行模拟IIC协议，所以这里的程序主要包括两层，首先就是需用通过GPIO口模拟IIC协议，因此需要进行模拟操作。这部分程序包括了基本的GPIO初始化部分，输入设置部分，输出设置部分，发送四种信号部分，等待从机应答部分，发送一个字节部分，读取一个字节部分。

```c
//产生起始信号
void I2C_Start(void)
{
    I2C_SDA_OUT();

	I2C_SDA_H;
	I2C_SCL_H;
	delay_us(5);
	I2C_SDA_L;
	delay_us(6);
	I2C_SCL_L;
}

//产生停止信号
void I2C_Stop(void)
{
   I2C_SDA_OUT();

   I2C_SCL_L;
   I2C_SDA_L;
   I2C_SCL_H;
   delay_us(6);
   I2C_SDA_H;
   delay_us(6);
}

//主机产生应答信号ACK
void I2C_Ack(void)
{
   I2C_SCL_L;
   I2C_SDA_OUT();
   I2C_SDA_L;
   delay_us(2);
   I2C_SCL_H;
   delay_us(5);
   I2C_SCL_L;
}

//主机不产生应答信号NACK
void I2C_NAck(void)
{
   I2C_SCL_L;
   I2C_SDA_OUT();
   I2C_SDA_H;
   delay_us(2);
   I2C_SCL_H;
   delay_us(5);
   I2C_SCL_L;
}
//等待从机应答信号
//返回值：1 接收应答失败
//		  0 接收应答成功
u8 I2C_Wait_Ack(void)
{
	u8 tempTime=0;

	I2C_SDA_IN();

	I2C_SDA_H;
	delay_us(1);
	I2C_SCL_H;
	delay_us(1);

	while(GPIO_ReadInputDataBit(GPIO_I2C,I2C_SDA))
	{
		tempTime++;
		if(tempTime>250)
		{
			I2C_Stop();
			return 1;
		}
	}

	I2C_SCL_L;
	return 0;
}
//I2C 发送一个字节
void I2C_Send_Byte(u8 txd)
{
	u8 i=0;

	I2C_SDA_OUT();
	I2C_SCL_L;//拉低时钟开始数据传输

	for(i=0;i<8;i++)
	{
		if((txd&0x80)>0) //0x80  1000 0000
			I2C_SDA_H;
		else
			I2C_SDA_L;

		txd<<=1;
		I2C_SCL_H;
		delay_us(2); //发送数据
		I2C_SCL_L;
		delay_us(2);
	}
}

//I2C 读取一个字节

u8 I2C_Read_Byte(u8 ack)
{
   u8 i=0,receive=0;

   I2C_SDA_IN();
   for(i=0;i<8;i++)
   {
   		I2C_SCL_L;
		delay_us(2);
		I2C_SCL_H;
		receive<<=1;
		if(GPIO_ReadInputDataBit(GPIO_I2C,I2C_SDA))
		   receive++;
		delay_us(1);
   }

   	if(ack==0)
	   	I2C_NAck();
	else
		I2C_Ack();

	return receive;
}
```

- 2、第二部分包括对24C02的基本读写函数。而基本的读写函数都需要通过IIC来控制24c02。因此这部分的函数是需要调用上面IIC操作函数的。

```
#include "AT24CXX.h"
//24c02读一个字节地址  数据

u8 AT24Cxx_ReadOneByte(u16 addr)
{
	u8 temp=0;

	I2C_Start();

	if(EE_TYPE>AT24C16)
	{
		I2C_Send_Byte(0xA0);
		I2C_Wait_Ack();
		I2C_Send_Byte(addr>>8);	//发送数据地址高位
	}
	else
	{
	   I2C_Send_Byte(0xA0+((addr/256)<<1));//器件地址+数据地址
	}

	I2C_Wait_Ack();
	I2C_Send_Byte(addr%256);//双字节是数据地址低位
							//单字节是数据地址低位
	I2C_Wait_Ack();

	I2C_Start();
	I2C_Send_Byte(0xA1);
	I2C_Wait_Ack();

	temp=I2C_Read_Byte(0); //  0   代表 NACK
	I2C_NAck();
	I2C_Stop();

	return temp;
}

//24c02读2个字节地址	数据

u16 AT24Cxx_ReadTwoByte(u16 addr)
{
	u16 temp=0;

	I2C_Start();

	if(EE_TYPE>AT24C16)
	{
		I2C_Send_Byte(0xA0);
		I2C_Wait_Ack();
		I2C_Send_Byte(addr>>8);	//发送数据地址高位
	}
	else
	{
	   I2C_Send_Byte(0xA0+((addr/256)<<1));//器件地址+数据地址
	}

	I2C_Wait_Ack();
	I2C_Send_Byte(addr%256);//双字节是数据地址低位
							//单字节是数据地址低位
	I2C_Wait_Ack();

	I2C_Start();
	I2C_Send_Byte(0xA1);
	I2C_Wait_Ack();

	temp=I2C_Read_Byte(1); //  1   代表 ACK
	temp<<=8;
	temp|=I2C_Read_Byte(0); //  0  代表 NACK

	I2C_Stop();

	return temp;
}

/*******************************************************************************
* 函 数 名         : AT24Cxx_WriteOneByte
* 函数功能		   : 24c02写一个字节地址  数据
* 输    入         : addr  dt
* 输    出         : 无
*******************************************************************************/
void AT24Cxx_WriteOneByte(u16 addr,u8 dt)
{
	I2C_Start();

	if(EE_TYPE>AT24C16)
	{
		I2C_Send_Byte(0xA0);
		I2C_Wait_Ack();
		I2C_Send_Byte(addr>>8);	//发送数据地址高位
	}
	else
	{
	   I2C_Send_Byte(0xA0+((addr/256)<<1));//器件地址+数据地址
	}

	I2C_Wait_Ack();
	I2C_Send_Byte(addr%256);//双字节是数据地址低位
							//单字节是数据地址低位
	I2C_Wait_Ack();

	I2C_Send_Byte(dt);
	I2C_Wait_Ack();
	I2C_Stop();

	delay_ms(10);
}

/*******************************************************************************
* 函 数 名         : AT24Cxx_WriteTwoByte
* 函数功能		   : 24c02写2个字节地址  数据
* 输    入         : addr  dt
* 输    出         : 无
*******************************************************************************/
void AT24Cxx_WriteTwoByte(u16 addr,u16 dt)
{
	I2C_Start();

	if(EE_TYPE>AT24C16)
	{
		I2C_Send_Byte(0xA0);
		I2C_Wait_Ack();
		I2C_Send_Byte(addr>>8);	//发送数据地址高位
	}
	else
	{
	   I2C_Send_Byte(0xA0+((addr/256)<<1));//器件地址+数据地址
	}

	I2C_Wait_Ack();
	I2C_Send_Byte(addr%256);//双字节是数据地址低位
							//单字节是数据地址低位
	I2C_Wait_Ack();

	I2C_Send_Byte(dt>>8);
	I2C_Wait_Ack();

	I2C_Send_Byte(dt&0xFF);
	I2C_Wait_Ack();

	I2C_Stop();

	delay_ms(10);
}
```
