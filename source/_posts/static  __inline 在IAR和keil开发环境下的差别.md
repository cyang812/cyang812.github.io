title: static  __inline 在IAR和keil开发环境下的差别
date: 2016/11/3 11:19:23
tags:
- STM32
- keil
- IAR
- C语言
categories:
- 硬件
thumbnail: http://blog.cyang.top/20161029185507754?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


static  __inline这条语句在IAR和Keil下的需要写成不同的形式，否则会报错。

<!-- more -->

如下：
1、IAR错误
![这里写图片描述](http://blog.cyang.top/20161029185507754?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
2、IAR正确
![这里写图片描述](http://blog.cyang.top/20161029185541584?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
3、Keil错误
![这里写图片描述](http://blog.cyang.top/20161029185550240?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
4、Keil正确
![这里写图片描述](http://blog.cyang.top/20161029185556693?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

总结就是，在IAR环境下，需要写成`static inline`，而在keil环境下，需要写成`static __inline`
