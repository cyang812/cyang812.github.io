title: WIN10版OneDrive不能登录，显示正在同步其他账户
date: 2017-05-14 10:48:19
tags:
- OneDrive
- Win10
categories:
- 总结
thumbnail: http://blog.cyang.top/20170514103312808?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# 一、错误现象
前几天通过Win10系统的推送进行了小版本的更新，更新之后需要重写登陆OneDrive，但是登陆却出现错误，提示正在同步其他账户，根据系统的指示在设置中更改账号并不能解决。

![这里写图片描述](http://blog.cyang.top/20170514103312808?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

![这里写图片描述](http://blog.cyang.top/20170514103323818?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 二、解决方法

## 1、控制面板卸载OndDrive
不过一般在控制面板是找不到OneDrive的，因此需要先执行安装包安装程序。安装包的路劲为`C:\Windows\SysWOW64`
![这里写图片描述](http://blog.cyang.top/20170514103735970?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170514103746586?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

之后便可以在控制面板卸载程序中找到OneDrive，如下图：

![这里写图片描述](http://blog.cyang.top/20170514103844931?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 2、重装OneDrive
重新执行OneDrive的安装包，安装包路径还是`C:\Windows\SysWOW64`

![这里写图片描述](http://blog.cyang.top/20170514104010239?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170514104028458?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170514104040135?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170514104020432?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 3、正常使用
之后OneDrive便可以正常使用了。

![这里写图片描述](http://blog.cyang.top/20170514104250025?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 三、参考链接：
[百度知道](https://zhidao.baidu.com/question/564899353262025404.html)

[微软社区解答](https://answers.microsoft.com/zh-hans/onedrive/forum/odstart-odsignin/win10%E7%89%88onedrive%E4%B8%8D%E8%83%BD%E7%99%BB/97bf330c-2f19-4a39-8fcc-159922fd1ece)