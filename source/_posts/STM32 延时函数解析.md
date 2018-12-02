title: STM32 延迟函数解析
date: 2017/2/28 10:08:23
tags:
- 嵌入式
- STM32
categories:
- STM32
thumbnail: http://blog.cyang.top/20170228152910421?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# 一、函数原型

STM32官方提供的函数库中，可以找到类似于 `HAL_Delay()` 这样的函数。这个函数的就是通过使用定时器，达到一个较为精确的时间延迟，提供给用户调用。

这个函数一般包含在类似于 `stm32f4xx_hal.c` 这样的函数中。函数原型如下：

```c
__weak void HAL_Delay(__IO uint32_t Delay)
{
  uint32_t tickstart = 0U;
  tickstart = HAL_GetTick();
  while((HAL_GetTick() - tickstart) < Delay)
  {
  }
}
```

<!-- more -->

输入参数为需要延时的时间，单位为毫秒（ms）。其中调用的 `HAL_GetTick()` 函数为获取计数值 `uwTick`,该计数值在中段服务函数中进行加一操作。

```c
__weak uint32_t HAL_GetTick(void)
{
  return uwTick;
}
```

在中断服务函数如下：

```c
void SysTick_Handler(void)
{
  uwTick++;
}
```

该中断服务函数为系统定时器SysTick的中断响应。而该定时器的初始化函数 `HAL_InitTick()` 是在 `stm32f4xx_hal.c`文件里定义,并在 `HAL_Init()` 函数中被调用。

查看其初始化函数  `HAl_InitTick()` ,内容如下：

```c
__weak HAL_StatusTypeDef HAL_InitTick(uint32_t TickPriority)
{
  /*Configure the SysTick to have interrupt in 1ms time basis*/
  HAL_SYSTICK_Config(SystemCoreClock/1000U);

  /*Configure the SysTick IRQ priority */
  HAL_NVIC_SetPriority(SysTick_IRQn, TickPriority ,0U);

  /* Return function status */
  return HAL_OK;
}
```

这个函数首先是为该定时器设置中断产生的周期，例如当前情况下为1ms，也就是没一毫秒都要产生一次中断。其次是为该定时器设置中断优先级。

# 二、函数说明

用户在使用延时时，直接调用函数 `HAl_Delay(time)`,填入需要延时的时长，单位为毫秒，例如填入5000，则代表延迟5秒，这段时间MCU会产生5000次中断，进5000次中断服务函数对计数值进行加一操作。

延时函数的核心语句为 `while循环`，如下：

```c
 while((HAL_GetTick() - tickstart) < Delay)
  {
  }
```

这个函数在条件满足时会一直循环，但是由于循环体为空，所以实际上循环是不产生任何操作的，直到循环不满足，也就是计数值在不断加一操作后的值减去开始延迟时值已经大于延时值时。此时条件不满足，循环结束，程序继续向下执行。

关于上面这个`while循环`，还可以用采用`for循环`写的版本，如下：

```c
for(  ;(HAL_GetTick() - tickstart) < Delay;  );
```

即仅使用`for`循环的一个条件，这段代码等同于如下代码：

```c
for(;;)
 {
 	if((HAL_GetTick() - tickstart) > Delay)
 		break;
 }
```

# 三、更多说明

- 1、关于 SysTick 定时器，数据手册的说明如下：
![这里写图片描述](http://blog.cyang.top/20170228152910421?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 2、关于`for`和`while`循环的效率说明，可以看[这篇文章](http://blog.csdn.net/coolbacon/article/details/7469044)。
