title: STM32 IIC EEPROM实验
date: 2016/9/30 20:10:23
tags:
- STM32
- IIC
- EEPROM
categories:
- STM32
thumbnail: http://blog.cyang.top/STM32%20EEPROM.png
---


![](http://blog.cyang.top/STM32%20EEPROM.png)

# 一、项目说明

利用 STM32 的普通 IO 口模拟 IIC 时序，并实现和 24C02 之间的双向通信，并将结果通过串口printf 输出。

<!-- more -->

# 二、知识点
- 1、关于IIC的知识点前面两篇文章已经有多介绍，本文重点从代码的角度讲解IIC的实际应用。
- 2、24C02是一款EEPROM芯片，兼容IIC总线。具有掉电不丢失，反复改写存储内容等特点。具有2K位存储容量，即256个字节，对应于256个地址。
- 3、对24C02的写操作，可以一次写一个字节，也可以一次写一页（一页有8个字节）。在一次寻址后，在同一页的情况下，写完一个字节地址会自动加一。
- 4、对24C02的读操作，不管是不是在同一页，每读完一个字节后，读取地址自动加一。
- 5、24C02的从设备地址为1010ABC，其中ABC代表可通过引脚的高低电平改变。因此可实现8种不同从设备地址，这意味着在同一个IIC总线上，最多可以挂在的24C02设备为8个。
- 6、[时钟频率]

# 三、部分代码
- 1、IIC start
```c
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
```

- 2、24C02 Write
```c
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
							//单字节是数字地址低位
	I2C_Wait_Ack();

	I2C_Send_Byte(dt);
	I2C_Wait_Ack();
	I2C_Stop();

	delay_ms(10);
}
```
- 3、test code
```c
int main()
{
	//u32 i=0;
	u32 j=0;
	u8 value=0;

	printf_init();	//printf初始化
	I2C_INIT();		 //IIC初始化
	delay_ms(1);

	/*
	for(i=0;i<=255;i++){
		AT24Cxx_WriteOneByte(i,data);	  //24c02写数据
		printf("写进去的数据是： %d\r\n",data);
		data--;
	}*/

	for(j=0;j<=255;j++){
		value=AT24Cxx_ReadOneByte(j);
		printf("读出来的数据是：%d\r\n",value);
	}
```
