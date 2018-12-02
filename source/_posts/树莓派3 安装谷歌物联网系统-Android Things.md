title: 树莓派3 安装谷歌物联网系统-Android Things
date: 2017/1/12 11:48:23
tags:
- 树莓派
- Android Things
categories:
- 树莓派
thumbnail: http://blog.cyang.top/Android%20Things.png
---


![](http://blog.cyang.top/Android%20Things.png)

# 一、必备工具
- 1、树莓派3
- 2、Android Things安装镜像
- 3、Windows 10 IoT 核心版仪表板
- 4、内存卡（推荐8G以上）
- 5、显示器（可通过hdmi转vga线连接到vga显示器）
- 6、Android Studio

<!-- more -->

# 二、安装步骤
- 1、[下载Android Things](https://developer.android.com/things/preview/download.html)
![这里写图片描述](http://blog.cyang.top/20170112112357082?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
下载之后解压出ISO文件。
- 2、使用Win32DiskImager刷入镜像
![这里写图片描述](http://blog.cyang.top/20170112112428676?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170112112436598?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
写入的过程比较慢，大概需要10分钟。
之后内存卡文件如下：
![这里写图片描述](http://blog.cyang.top/20170112112522945?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 3、将内存开插入树莓派并开机
开机过程如下面的图片所示
![这里写图片描述](http://blog.cyang.top/20170112113244868?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170112113306134?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
下面的小字写着树莓派的IP地址。
- 4、使用ADB连接树莓派。
执行如下语句
  ```
  adb connect <ip>
  ```
  查看是否连接
  ```
  adb devices
  ```
  ![这里写图片描述](http://blog.cyang.top/20170112114039011?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 三、第一个程序
- 1、下载或更新最新的Android Studio。
![这里写图片描述](http://blog.cyang.top/20170112114125574?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 2、获取工程模板，到[这里克隆代码](https://github.com/androidthings/new-project-template)。
![这里写图片描述](http://blog.cyang.top/20170112114142231?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 3、使用Android Studio编译运行。编译时可能需要先下在必须的编译工具。
![这里写图片描述](http://blog.cyang.top/20170112114234841?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170112114327419?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
下载运行可看到显示器显示正在执行的程序
![这里写图片描述](http://blog.cyang.top/20170112114446391?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
同时可通过查看Studio中的logcat查看程序执行情况。
![这里写图片描述](http://blog.cyang.top/20170112114544767?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170112114600889?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
# 四、参考链接
- 1、[树莓派+Android Things](http://blog.csdn.net/joe544351900/article/details/54314333)
- 2、[Android Things参考文档](https://developer.android.com/things/hardware/raspberrypi.html)
- 3、[更多关于树莓派的文章](http://cyang.tech/categories/%E6%A0%91%E8%8E%93%E6%B4%BE/)
