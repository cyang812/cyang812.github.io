title: 从Nexus5说Android系统的打开方式
date: 2016/12/18 23:21:23
tags:
- Nexus5
- Android
- BootLoader
categories:
- 总结
thumbnail: http://blog.cyang.top/android%207.0.jpg
---

![](http://blog.cyang.top/android%207.0.jpg)

谷歌官方不在对Nexus5手机升级最新的Android7.0系统，但是在XDA论坛上已经有开发者制作了第三方刷机包。刷机之后体验了一段时间，就没怎么用了。后来出现了闪屏的现象，无法判断是否是因为升级系统导致的，也不排除是硬件的原因。

<!-- more -->

只好刷回官方匹配的6.1系统，刚刷回时依然出现，用了几次后发现居然好了。下面是重新恢复官方系统后做的必不可少的工作。下图为闪屏现象。
![这里写图片描述](http://blog.cyang.top/20161219085110866?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
本文虽针对nexus5手机，但对基于Android系统的手机均有一定参考性。


# 一、恢复官方系统
- 1、恢复官方系统非常简单，只需要去nexus手机系统官网下载对应机型的工厂固件包（.zip格式），大约560M，[下载地址在这里](https://developers.google.com/android/images#hammerhead)。
![这里写图片描述](http://blog.cyang.top/20161219083750668?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
注意：请下载factory image,而不是full ota image

- 2、下载完成后解压，可看到里面的文件结构如下图。
![这里写图片描述](http://blog.cyang.top/20161219083930500?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 3、按住音量下键和电源键，手机进入线刷模式（fastboot），如下图。
![这里写图片描述](http://blog.cyang.top/20161219084703849?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 4、连接电脑（需先保证电脑正确安装了驱动）。
- 5、在windows系统下，可直接双击`flash-all.bat`文件，之后手机会自动通过USB的方式进行升级，此时请勿断开连接。.bat文件中的内容如下图：
![这里写图片描述](http://blog.cyang.top/20161219084924856?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


# 二、刷Recovery
- 1、安装Recovery的为twrp，[下载地址在这里](https://twrp.me/devices/lgnexus5.html)。
![这里写图片描述](http://blog.cyang.top/20161219085439463?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)



- 2、安装的过程可以通过使用如下命令
   ```
  adb devices
  adb reboot bootloader
  fastboot flash recovery twrp.img
  fastboot reboot
  ```
如下图
![这里写图片描述](http://blog.cyang.top/20161219085523409?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

也可以使用机锋论坛大神提供的一键安装工具，下载地址和使用方法[在这里](http://bbs.gfan.com/android-7529229-1-1.html)。

# 三、获取Root
- 1、获取Root的方式为通过recovery模式的install模式刷入压缩包。
- 2、压缩包为SuperSU官方文件，[下载地址在这里](https://download.chainfire.eu/1016/SuperSU/UPDATE-SuperSU-v2.79-20161211114519.zip)。
![这里写图片描述](http://blog.cyang.top/20161219085655083?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、进入recovery模式。首先进入fastboot（按住音量下键和电源键），之后通过音量上下键选择模式，电源键确定。
![这里写图片描述](http://blog.cyang.top/20161219085800230?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、通过recovery模式的install安装压缩包，如下图所示。
![这里写图片描述](http://blog.cyang.top/20161219090125934?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20161219090232278?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20161219090244794?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 5、安装成功后，重启手机进入系统。此时可看见应用程序列表多出一个SuperSU的应用，可通过该应用管理root权限。
![这里写图片描述](http://blog.cyang.top/20161219090528845?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)



# 四、刷Xposed框架
- 1、关于安卓6.0系统安装xposed框架，之前已经写过一篇比较完整的教程，发布在CSDN的博客，[点这里查看](http://blog.csdn.net/u011303443/article/details/51031170)。
- 2、具体来说就是通过recovery中的install，刷入xposed的.zip文件。下载地址在XDA论坛，详情见[这篇帖子](http://forum.xda-developers.com/showthread.php?t=3034811)。简单说，nexus5手机使用基于ARM32位架构的CPU，现在安装的系统为Android6.0,SDK23，因此需要下载的xposed的压缩文件名字应该类似于`xposed-v87-sdk23-arm.zip`。
![这里写图片描述](http://blog.cyang.top/20161219090824562?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 3、进入recovery模式。首先进入fastboot（按住音量下键和电源键），之后通过音量上下键选择模式，电源键确定。


- 4、通过recovery模式的install安装压缩包，如下图所示。
![这里写图片描述](http://blog.cyang.top/20161219091150105?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20161219091204293?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20161219091221325?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 5、安装成功后，重启手机进入系统。
- 6、此时，开机会自动对应用程序进行优化，所以开机需要一段时间。

![这里写图片描述](http://blog.cyang.top/20161219091250919?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 7、最后一步是安装xposed的应用程序，这个应有程序是搭配xposed框架使用的。在6.0的系统中，安装框架不会自动安装应用程序，需要手动安装该程序，可直接去酷安应用商店搜索下载。
![这里写图片描述](http://blog.cyang.top/20161219091609299?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 8、打开应用程序，此时可能需要安装更新包，然后重启手机。之后便可以安装插件了，插件安装成功后，需要到该应用程序勾选启用，并且要在重启后才可以正常使用。
![这里写图片描述](http://blog.cyang.top/20161219091625925?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
