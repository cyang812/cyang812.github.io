title: STM32 延迟函数高级用法
date: 2017/3/2 10:08:23
tags:
- 嵌入式
- STM32
categories:
- STM32
thumbnail: http://blog.cyang.top/STM32%20%E5%BB%B6%E8%BF%9F%E5%87%BD%E6%95%B0.png
---


![](http://blog.cyang.top/STM32%20%E5%BB%B6%E8%BF%9F%E5%87%BD%E6%95%B0.png)

<!-- more -->

# 一、使用场景

第一种情况，在使用普通 STM32 延迟函数，类似于 `HAL_Delay(time)`，由于该函数是使用循环去判断及延时的，所以在执行该函数时整个程序会在此处等待定时器的中断服务函数修改参量使得循环判决条件不成立，从而继续程序的执行，同时也达到延迟时间的效果。由于使用的是系统的定时器进行延迟，所以时间相对准确。

第二种情况，当需要周期性的执行一个任务时，将这个函数放在某个定时器的中断服务函数里，设置好定时器的时间，完成时产生中断，从而进入中断服务函数执行该函数。此时，MCU 执行中断程序，只有更高优先级的中断才能打断当前执行的中断服务函数，进入更高优先级的中断服务函数去执行。需要等所有中断服务函数都执行完成，才会退回到主函数。

第三钟情况，而结合定时器以及相应的标志位，直接在主函数中达到周期任务的效果。原理如下：

- 1、设置一个全局的标志位`flag`，初值为 0。
- 2、在`SysTick`定时器的中断服务函数中，周期性地对改标志位置 1。
- 3、主函数  `while(1)`中,只要使用`if(flag){}`去判断条件是否满足，满足则执行，不满足则跳过。

第三种情况和第二钟情况的主要区别在于，第三种情况的周期任务函数是在主函数中执行的，而第二种则是在中断服务函数里执行的。使用第二种方式去执行周期任务，程序上可能会更好理解一些；使用第三种方式，则在编写程序时更简便一点。

这三种情况的使用场景不一样，第一种是使用 CPU 空操作的方式来延迟固定时间，保证通信时序正确；第二种使用中断的方式适用于比较重要的周期任务，保证周期准确；第三种则适用于周期不那么重要，只要在 `while(1)`循环中，任务函数不断地进行 `if(flag)`的判断，满足就执行。

# 二、代码演示

```c
  while (1)
  { 
    BSP_LED_On(LED1);
	#if 1 //演示1，普通延时函数 5s打印一次时间和follow on
  		printf_time();
    	HAL_Delay(1000); //延时1000ms
    	printf("follow on \n ");
	#else //演示2，周期任务 1s打印一次时间，5s打印一次follow on
		printf_time();
    	HAL_Delay(1000);
		Sys_Delay(5000);
		if(flag)
		{
			flag = 0;
			printf("follow on \n");
		}		
    #endif
  }
```

`printf_time()`函数就是将 MCU RTC 中的时间通过串口打印出来，而`HAL_Delay()`就是普通的延时函数，`Sys_Delay()`是用于设置第三种方式中所提的定时任务的周期，代码如下：

```c
void Sys_Delay(uint32_t time)
{
  Cycle_Time = time;
}
```

而中断服务函数的代码如下：

```c
void SysTick_Handler(void)
{
  HAL_IncTick();
  T1msCount++;
  if(T1msCount>Cycle_Time)
  {
    T1msCount = 0;
	flag = 1;			
  }
}
```

该中断每 1ms 产生一次,对计数值`T1msCount`进行加 1，当大于周期时间时，清零，并对标志位赋 1，此后主函数中`if(flag)`成立，对标志位清零，并执行其中的周期任务。


![这里写图片描述](http://blog.cyang.top/20170301203955373?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20170301204043655?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
图一对于主函数中演示 1，代表延迟一秒，打印时间及“follow on”，
图二对应主函数在 `#if 0` 时的演示2，代表延迟一秒打印一次时间，打印"follow on"的周期为5秒。
