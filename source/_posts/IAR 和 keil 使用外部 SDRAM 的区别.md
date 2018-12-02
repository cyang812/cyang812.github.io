title: IAR 和 keil 使用外部 SDRAM 的区别
date: 2018/3/26 19:35:23
tags:
- STM32
- keil
- IAR
categories:
- 硬件
thumbnail: http://blog.cyang.top/20180326193029821?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


当芯片内部的 SRAM 不够用时，就需要在外部扩展 SDRAM，然后在写程序时将一些比较大的 buffer 定义在外部内存中。在进行正确的配置之后，对外部 SDRAM 的使用，和芯片内部的 SRAM 是一样的，可以直接对 SDRAM 的地址进行读写访问。

<!-- more -->

因此，最简单的方法就是，如下所示的代码，直接使用指针指到外部 SDRAM 的地址，之后对指针进行移动，便可以对全部 SDRAM 进行读写。使用这种方法需要特别小心，要确保指针指向的地址在 SDRAM 的地址空间。
```c
uint8_t *sdram_buf = (uint8_t*)SDRAM_ADDR;
uint8_t *sdram_ptr = sdram_buf;
uint32_t sdram_useSize = 0;
uint8_t sdram_full = 0;
```

另外的一种方法就是，将 buffer 数组定义在外部 SDRAM 中，这样可将指针操作改为对数组的操作。也就是在定义数组时，不是由编译器自动分配地址，而是手动指定数组的地址。不同的 IDE 语法不一样，在 IAR 下，需要使用如下的语句，

```c
IAR 在外部SDRAM定义数组的方法
#pragma location = SDRAM_ADDR
uint8_t sdram_buffer[0x700000];
```

编译后生成的 map 文件可以看出，有 7MB 的空间是使用绝对地址定义的。
![这里写图片描述](http://blog.cyang.top/20180326193029821?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


keil 下使用如下语句：
```c
keil 在外部SDRAM定义数组的方法
uint8_t sdram_buffer[0x700000] __attribute__( (at(SDRAM_ADDR)) ); 
```