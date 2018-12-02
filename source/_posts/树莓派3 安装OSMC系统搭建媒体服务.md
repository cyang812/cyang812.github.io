title: 树莓派3 安装OSMC系统搭建媒体服务
date: 2017/2/7 13:27:23
tags:
- 树莓派
- OSMC
categories:
- 树莓派
thumbnail: http://blog.cyang.top/20170207125146892?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---




![这里写图片描述](http://blog.cyang.top/20170207125146892?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
OSMC是一款基于 Linux 免费开源的媒体播放系统。目前支持树莓派1、2、3、zero,vero,Apple TV这几款硬件平台。


<!-- more -->

# 一、必备工具

- 1、树莓派3
- 2、OSMC OS
- 3、内存卡（推荐8G以上）
- 4、显示器（可通过hdmi转vga线连接到vga显示器）
- 5、2A的电源适配器

# 二、安装步骤
**安装的方法有三种，如下。方法一比较简单，也是常用的为树莓派烧写系统的方式；方法二为树莓派官方推荐的新手入门安装系统的方式；方法三是 OSMC 官方推出的系统烧写器，也很简单。三种方式各有优劣，第一种比较常见，烧写系统的时间最短；第二种烧写时间适中，但是最简单；第三种可以根据硬件型号自动下载系统，也可以很方便地设置 WIFI 连接方式，在没有网线的情况下，使用第三种方式安装系统是最方便的。**

- 方法一 下载安装镜像烧写
1、下载 [OSMC](https://osmc.tv/download/) 系统
![这里写图片描述](http://blog.cyang.top/20170207125232290?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

 2、使用 Etcher 烧录系统
![这里写图片描述](http://blog.cyang.top/20170207125246837?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 方法二 使用树莓派官方推荐的NOOBS安装
 1、下载NOOBS,推荐下载左边完整版，这样在安装时不需要连接网络下载系统。
 ![这里写图片描述](http://blog.cyang.top/20170207125427800?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
 2、解压后将全部文件放入内存卡根目录
![这里写图片描述](http://blog.cyang.top/20170207125552972?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 方法三 使用OSMC官方安装器安装
1、下载OSMC官方安装器，根据你所使用的操作系统选择下载。
![这里写图片描述](http://blog.cyang.top/20170207125657317?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
2、根据提示进行操作
![这里写图片描述](http://blog.cyang.top/20170207125737078?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170207125745207?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170207125751989?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、插入内存卡，开机初始化。第一次启动会配置时间地区语言等，切勿在初始化设置时更改语言为中文，将会出现中文字用框框显示的错误。
之后开机可以到设置中切换中文，先`system–>settings–>apparence–>skin，把 fonts 改成 Arial based；`之后再`到 skin 下面的 international，把 language 改成 Chinese；`
![这里写图片描述](http://blog.cyang.top/20170207131628247?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170207131642430?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170207131742603?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


- 4、安装插件
系统可以安装的插件非常多，但大部分都是国外的媒体服务供应商，所以使用起来不是很方便。如果想要观看国内的视频，就需要安装插件。
推荐安装[xbmc-addons-chinese](https://github.com/taxigps/xbmc-addons-chinese)插件。
首先[下载](https://raw.githubusercontent.com/taxigps/xbmc-addons-chinese/master/repo/repository.xbmc-addons-chinese/repository.xbmc-addons-chinese-1.2.1.zip)，之后将文件拷贝如内存卡，选择从本地安装.zip文件。
或者安装[SuperRepo](https://superrepo.org/)，方法为：
>1、首先在系统-文件管理中添加源，源地址为 http://srp.nu，给源取名 SuperRepo。
>2、完成后在系统-设置-插件中选择 从 zip 文件安装
>3、根据自己的操作系统选择安装项，例如我选择 helix，然后选择 all


	关于从本地安装插件的方法，总结如下：
	- 1、找到ZIP安装界面，主界面下Videos(视频)->Video Add-ons（视频插件）
	- 2、点击Get more(获取更多) -> 出现一个列表，点第一行
	- 3、出来新的列表，选择点击第一行，显示多种导入方式，选择Install From zip file(从ZIP文件导入)
	- 4、选择USB存储盘，选择对应的插件，点击即可安装

# 三、使用体验
- 1、安装插件后可以完美观看国内主流的视频网站上的内容，使用PPTV插件还可以看电视内容，无广告，但是出错的机率比较大。
- 2、音频的输出可以使用耳机，也可以使用HDMI，这可以在系统中设置，但可能是我硬件的问题，使用耳机输出在不播放视频时底噪非常大。由于没有带音频输出的HDMI转接线，所以不知道HDMI输出音频时的效果。
![这里写图片描述](http://blog.cyang.top/20170207131822728?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)![这里写图片描述](http://blog.cyang.top/20170207131833932?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)



# 四、参考链接
- 1、[创客智造](http://www.ncnynl.com/archives/201607/241.html)
- 2、[樹莓派 OpenELEC（XBMC/Kodi）加入中文節目套件，觀看土豆網與優酷等影片](https://blog.gtwang.org/iot/openelec-xbmc-kodi-chinese-addons/)
- 3、[Raspberry Pi 3 Model B 安装 OSMC](http://www.cnblogs.com/ifantastic/p/5672039.html)
