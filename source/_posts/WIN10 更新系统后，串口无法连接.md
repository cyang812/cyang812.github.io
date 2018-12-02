title: WIN10 更新系统后，串口无法连接
date: 2017-04-26 15:41:19
tags:
- UART
- Win10
categories:
- 总结
thumbnail: http://blog.cyang.top/20170426093429829?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---

   
# 一、问题
- 1、WIN10 更新系统后，无法连接 ST 开发板上 USB 转串口，但可以正常的下载程序。
![这里写图片描述](http://blog.cyang.top/20170426093429829?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

# 二、解决方法
- 1、尝试更改 COM 口，例如从 COM3 转为 COM4。
![这里写图片描述](http://blog.cyang.top/20170426094024912?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 2、尝试更新 ST-LINK 的固件版本，如下：
![这里写图片描述](http://blog.cyang.top/20170426093638831?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


之后便可正常使用了：
![这里写图片描述](http://blog.cyang.top/20170426094105647?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)