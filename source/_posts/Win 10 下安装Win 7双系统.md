title: Win 10下安装Win 7双系统
date: 2016/9/11 10:08:23
tags:
- Win10
- Win7
- 双系统
categories:
- 教程
thumbnail: http://blog.cyang.top/win10win7%E5%8F%8C%E7%B3%BB%E7%BB%9F.jpg
---

![](http://blog.cyang.top/win10win7%E5%8F%8C%E7%B3%BB%E7%BB%9F.jpg)

上一篇文章说了在Win10下使用VM虚拟机安装双系统的教程。其实虚拟机已经可以解决很多的需求了。但问题在与，网卡的MAC地址不同。在虚拟机里就算使用桥接模式，可以正常链接网络，但还是不能使用正常网卡的MAC地址。

# 一、安装说明
- 1、在Win 10的情况下，将磁盘进行分区，划分一个新的分区，大小依据个人情况而定，最好是50G，我这里只分了30G.
- 2、由于我的整个磁盘分区表是MBR，所以可以直接安装Win7 32位系统。

<!-- more -->

# 二、安装步骤
- 1、建议到[msdn](http://www.itellyou.cn/)下载官网镜像。下载结束会直接进行解压缩。

- 2、双击压缩包里的setup.exe，即可直接进行安装。
![这里写图片描述](http://blog.cyang.top/20160911195907037?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、在选择安装位置时，一定要选择新分出来的区。

- 4、安装过程大概需要30分钟。

- 5、安装过程中如果出现错误，说明这种方法并不可行。那么，可以选择进入PE，使用PE里的恢复选项，选择下载好的Win7系统，选择要安装的位置进行安装。

# 三、可能出项的错误
- 1、Windows安装程序无法将Windows配置为在此计算机的硬件上运行。
![这里写图片描述](http://blog.cyang.top/20160911195651832?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
**解决方法：**
当出项如上提示的时候，按下shift+f10会打开命令窗口，进入到C:\Windows\system32\oobe文件夹，输入msoobe回车然后输入msoobe即可进入下一步操作，但是错误提示框仍在，不用理会按照屏幕提示输入相应的信息直至完成系统的安装。安装好的Win7系统没有任何问题。

- 2、安装过程中要求输入安装密钥，但是找不到正确的密钥。
**解决方法：**
这一步是可以直接跳过的，安装好的系统没有激活，但是有30天的试用期。可进入到系统后再激活。

- 3、安装结束后Win10启动项丢失。
**解决方法：**
下载大白菜装机版的PE。制作U盘PE，进入PE后选择启动项修复。

# 四、安装结果
在win7下的目录结构
![这里写图片描述](http://blog.cyang.top/20160911195941643?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

在win10下的目录结构
![这里写图片描述](http://blog.cyang.top/20160911195950538?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
