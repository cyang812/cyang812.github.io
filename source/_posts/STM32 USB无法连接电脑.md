title: STM32 USB无法连接电脑
date: 2017/12/6 22:06:23
tags:
- STM32
- USB
categories:
- STM32
thumbnail: http://blog.cyang.top/20171213122640655?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


![](http://blog.cyang.top/20171213122640655?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 一、说明

在调试 STM32 USB device MSC 功能时，使用官方提供的库和示例项目，电脑可以正确识别设备，也可以正常操作。但是将 USB 部分的代码移植到自己的工程后，发现电脑无法正确识别设备，有时会在右下角显示无法识别设备。

<!-- more -->

# 二、解决方法

在main.c中添加 hal_delay() 函数的实现方式。

在默认的模板工程里，一般使用如下的方式实现延迟函数。

```c

__weak uint32_t HAL_GetTick(void)
{
  return uwTick;
}

__weak void HAL_Delay(__IO uint32_t Delay)
{
  uint32_t tickstart = 0U;
  tickstart = HAL_GetTick();
  while((HAL_GetTick() - tickstart) < Delay)
  {
  }
}
```

而在 USB 项目中，需要使用如下的方式实现延时函数。

```c
void HAL_Delay(__IO uint32_t Delay)
{
  while(Delay) 
  {
    if (SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk) 
    {
      Delay--;
    }
  }
}
```

这两种方式都可以实现基本的延时功能，都是使用的 SysTick 定时器来实现，也都是使用 `while()` 来进行条件判断，条件不满足，即计时到了指定的延时后退出 `while()` 。区别在于，第一种方式，进行条件判断的变量 `uwTick` 在 SysTick 的中断函数中进行加一操作，即如下代码：

```c
__weak void HAL_IncTick(void)
{
  uwTick++;
}

void SysTick_Handler(void)
{
  HAL_IncTick();
}
```

而第二种方式，进行 `while()` 条件判断的变量 `Delay` 是不依赖于 SysTick 中断函数进行改变的，而是直接在这个函数中进行判断，等待寄存器数值改变，延时 1ms 后，对 `Delay` 进行减一操作。

默认的实现方式依赖于 `SysTick` 中断函数`void SysTick_Handler(void)`，而在使用 USB 功能时，USB的操作本身就是需要在中断函数 `void OTG_FS_IRQHandler(void)` 中进行的。可能由于对不同中断函数的处理，导致了时间上的错误，从而电脑无法正确进行枚举操作。

USB 部分是在 `usbd_conf.c` 文件中的 `void USBD_LL_Delay(uint32_t Delay)` 函数中进行延时函数的调用的，如下所示：

```c
void USBD_LL_Delay(uint32_t Delay)
{
  HAL_Delay(Delay);
}
```
而且在底层的 USB 库中，也有直接调用到`hal_delay`的，如下：
![这里写图片描述](http://blog.cyang.top/20171213122640655?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
 
# 2018.1.3修改
后来发现好像不是这个问题，不这样修改也可以连接。讲道理 SysTick 的中断优先等级比 USB 的高，因此应该是不会受影响的。可是当时对比了两个工程的代码，好像也就这点区别。不过在官方库中，都是使用第二种方式的。