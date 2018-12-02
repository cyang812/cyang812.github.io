title: Win 10下VM虚拟机安装Win 7
date: 2016/9/11 10:08:23
tags:
- Win10
- Win7
- VM虚拟机
categories:
- 教程
thumbnail: http://blog.cyang.top/win10win7.jpg
---


![](http://blog.cyang.top/win10win7.jpg)

由于主机为64位系统，近期需要用到32为系统，所以想在Win 10 64位下使用虚拟机安装一个32位的Win 7。以下为安装过程。网上的很多教程，很多还要用到PE，分区什么的。实际上在VM12虚拟机下，已经可以全自动的安装Win 7了，很多的Linux系统也都可以全自动安装。

# 1、版本说明
- 1、VM虚拟机
VMware® Workstation 12 Pro
- 2、Win 7
Windows 7 Enterprise (x86) - DVD (Chinese-Simplified) （msdn 上下载的原装镜像文件。）

<!-- more -->

# 2、安装步骤
- 1、下载所需的镜像文件，建议到[MSDN](http://www.itellyou.cn/)上下载官方镜像。64位的系统文件为x64，32位的为x86。
![这里写图片描述](http://blog.cyang.top/20160911100338188?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 2、新建虚拟机，选择典型，这很重要，这会简化很多操作。
![这里写图片描述](http://blog.cyang.top/20160911100359320?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20160911100448898?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 3、设置选项如下。需要填写密钥，这个可以去百度上搜一搜，需要搜对应版的，比如我这里的是企业版，就需要去搜索企业版的。
![这里写图片描述](http://blog.cyang.top/20160911100502180?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、选择安装位置。
![这里写图片描述](http://blog.cyang.top/20160911100538346?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 5、选择分区大小。选择将虚拟磁盘拆分成多个文件。大小建议为60G，这里我只分配了30G。
![这里写图片描述](http://blog.cyang.top/20160911100633238?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 6、等待分区完成。这里的分区和安装Linux一样，并不会一开始就占用所有分配给的分区容量，而是用多少占多少。
![这里写图片描述](http://blog.cyang.top/20160911100753614?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 7、等待全自动的安装，大概需要1小时左右。中途重启3次。
![这里写图片描述](http://blog.cyang.top/20160911100850369?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20160911100901494?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20160911100917760?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20160911100934838?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20160911100951963?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


# 3、结果
![这里写图片描述](http://blog.cyang.top/20160911101039477?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20160911101047479?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
