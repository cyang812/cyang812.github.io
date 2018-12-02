title: 树莓派3 安装Win10 IoT 操作系统
date: 2017/1/11 20:09:23
tags:
- 树莓派
- win10 IoT
categories:
- 树莓派
thumbnail: http://blog.cyang.top/win10%20IoT.png
---


![](http://blog.cyang.top/win10%20IoT.png)

# 一、必备工具
- 1、树莓派3
- 2、Windows 10 IoT Core Insider Preview
- 3、Windows 10 IoT 核心版仪表板
- 4、内存卡（推荐8G以上）
- 5、显示器（可通过hdmi转vga线连接到vga显示器）

<!-- more -->

# 二、安装步骤
- 1、下载并安装[Windows 10 IoT 核心版仪表板](https://developer.microsoft.com/zh-cn/windows/iot/Downloads.htm)
![这里写图片描述](http://blog.cyang.top/20170111191012837?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
点击`获取IoT核心版仪表板`，下载之后进行安装。

- 2、下载，解压并安装[Windows 10 IoT Core Insider Preview](https://developer.microsoft.com/zh-cn/windows/iot/Downloads.htm)
![这里写图片描述](http://blog.cyang.top/20170111191154884?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
需要点击红框所示的位置进行下载。这需要登陆微软账号，并且账号需要具有预览版体验资格。如果闲麻烦，也可以去我的百度云下载。
```
链接：http://pan.baidu.com/s/1bpcMusv 密码：wl91
```
下载完成为一个ISO文件，进行解压可以看到一个软件安装包，进行安装，这个安装包里面就包含了win10的安装镜像flash.ffu。

- 3、写入镜像到内存卡
直接使用Windows 10 IoT 核心版仪表板软件进行写入。
![这里写图片描述](http://blog.cyang.top/20170111192717906?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

插入格式化后的内存卡，选择设备为树莓派，设置PC名称和密码，等待下载完成，并自动安装。
![这里写图片描述](http://blog.cyang.top/20170111192529617?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

这需要等待下载完成，在写入，需要很长时间，而且尝试了两次均出现错误。

![这里写图片描述](http://blog.cyang.top/20170111192920733?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

所以，还是使用自定义设备，直接写入下载好的flash.ffu。这个文件一般在`C:\Program Files (x86)\Microsoft IoT\FFU` 目录下。
![这里写图片描述](http://blog.cyang.top/20170111193256706?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/20170111193346724?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

等待写入完成后，内存卡的文件为：

![这里写图片描述](http://blog.cyang.top/20170111193421144?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、将内存卡插入树莓派3并开机
开机的过程可见下面的图片
![这里写图片描述](http://blog.cyang.top/20170111195811869?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170111195824531?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170111195835197?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20170111195930855?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


# 三、注意事项
- 1、flash.ffu显示的是RPi2，也就是为树莓派2制作的，但其实树莓派3也是这个文件。

# 四、参考链接
- 1、[树莓派Win10镜像下载安装教程及使用初体验](http://bbs.ickey.cn/community/forum.php?mod=viewthread&tid=44814)
- 2、[值还是不值？——树莓派3 Win10 IoT系统体验](http://www.eeboard.com/evaluation/raspberry-win10-iot/3/)
