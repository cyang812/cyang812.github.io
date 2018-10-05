title: SPI总线协议
date: 2016/10/6 19:17:23
tags:
- SPI
categories:
- 硬件
thumbnail: http://p7tst3obo.bkt.clouddn.com/SPI%E9%80%9A%E4%BF%A1%E5%8E%9F%E7%90%86.png
---


![](http://p7tst3obo.bkt.clouddn.com/SPI%E9%80%9A%E4%BF%A1%E5%8E%9F%E7%90%86.png)

## 一、知识点
- 1、SPI：Serial Peripheral Interface，串行外围设备接口，是一种高速，半/全双工，同步的通信总线。
- 2、SPI接口主要用于EEPROM，FLASH，实时时钟，AD转换器等外接设备。

<!-- more -->

- 3、SPI通信分为主机和从机，这取决于硬件设计和软件设置。
- 4、SPI总线一般有四根线，分别为MOSI,MISO,SCK,NSS，即主机输出，从机输入，时钟线，片选线。
- 5、如下图所示为SPI总线控制器的内部示意图：
![](http://p7tst3obo.bkt.clouddn.com/SPI%E5%86%85%E9%83%A8%E7%BB%93%E6%9E%84.png)
- 6、下图所示为连接示例：
![](http://p7tst3obo.bkt.clouddn.com/SPI%E8%BF%9E%E6%8E%A5%E5%9B%BE%E7%A4%BA.png)
- 7、SPI数据的传输过程其实就是通过一个一位寄存器来完成的，主机将自己的移位寄存器数据移出，同时从机的移位寄存器数据移入；或者从机将自己的移位寄存器数据移出，同时主机的移位寄存器数据移入。通信的速率取决于主机设置的波特率。
- 8、时钟信号的相位和极性，通过软件编程设置，用来决定数据的采集时刻（上升沿还是下降沿）以及数据传送时刻。
- 9、数据帧格式可以通过寄存器来编程设置，例如8位或者16位。
- 10、关于NSS：标准的SPI总线一般为4根线，其中包括NSS，这根线主要用于确定主机还是从机。从硬件设计的角度讲，如果作为主机，则这一根线接高电平，从机的话，这一根线接低电平。另外也可以弃用主机的这根连接线，直接采用别的GPIO管脚连接从机的NSS脚，通过GPIO输出低电平达到拉低NSS从而片选的功能。
