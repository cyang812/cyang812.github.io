title: STM32 SysTick定时器
date: 2017/3/2 10:08:23
tags:
- 嵌入式
- STM32
categories:
- STM32
thumbnail: http://blog.cyang.top/STM32%20SysTick.png
---


![](http://blog.cyang.top/STM32%20SysTick.png)

<!-- more -->

# 一、说明

SysTick 定时器是实时操作系统专用的，但是也可以作为一个标准的递减计数器使用。它具有以下特点：

- 1、24位递减计数器（16777216）
- 2、自动装填能力
- 3、计数器达到 0 时，有可屏蔽的系统中断产生。
- 4、可编程时钟源 （HCLK 或者 HCLK/8）

该定时器具有四个寄存器，如下表所示：

![这里写图片描述](http://blog.cyang.top/20170302190239935?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170302190300373?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

一般，该定时器的计数周期为一毫秒，则寄存器`LOAD`中的值根据 MCU 主频的不同而不同。例如，主频为 16MHz,则填入该寄存器的值为16 000。也就是，每一秒执行16 000 000 次，那么每毫秒执行 16 000 次。

# 二、代码

使用该定时器非常容易，官方的演示代码中几乎后使用了这个定时器。这是因为延时函数 `HAL_Delay()`底层就是调用的这个定时器。所以对这个定时器其实不用添加或修改任何代码的。以 `STM32Cube_FW_F4_V1.14.0`固件库为例，说明这个定时器的配置代码。

- 1、芯片上电或者复位后，运行 `HAL_Init()`这个函数，用于一些初始化的设置，这 其中就包括对 SysTick 定时器的设置的`HAL_InitTick()`。

  ![init](http://blog.cyang.top/20170302190605046?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

  ![inittick](http://blog.cyang.top/20170302190616640?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 2、在`HAL_InitTick()`中，设置了定时器计数值，开启定时器，设置定时器的中断优先级。

  ![hal_InitTick](http://blog.cyang.top/20170302190814373?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 3、由于芯片默认使用的时钟为内部高速时钟，此时定时器的时钟源`HCLK`的大小，即`SystemCoreClock`的值，为内部高速时钟的频率。下一步就会去配置系统时钟，为了达到更高的主频，此时可能会使用外部高速时钟。

  ![cube](http://blog.cyang.top/20170302190850796?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、随着时钟的改变，主频会发生变化，相应的定时器的值也要根据主频做出调整，以保障定时器的计数周期始终保持不变，为 1ms。但是这个不必我们再次去设置，因为每次调用 `HAL_RCCC_ClockConfig()` 去配置时钟后，都会自动的重新计算`SystemCoreClock`的值，并调用 `HAL_InitTicK()` 设置定时器的计数值。

  ![update](http://blog.cyang.top/20170302190906671?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 三、总结

- 1、SysTick 定时器是非常好用的定时器，而且几乎不用配置，因为演示代码里基本上都有。而且会自动根据主频重新设置计数值，保证计数周期为 1ms。
- 2、一开始走了一些弯路，觉得这个定时器在任何主频下都会定时 1ms ，和设置的主频没关系，其实是因为每次修改主频都都会重新设置计数值。注释里说的很明白。如下：
>@note This function is called  automatically at the beginning of program after reset by HAL_Init() or at any time when clock is reconfigured  by HAL_RCC_ClockConfig().
