title: 关于路由器，锐捷破解，mentohust的使用
date: 2016/8/24 23:23:23
tags: 
- 锐捷
- mentohust
- 路由器
- linux
categories:
- 总结
thumbnail: http://blog.cyang.top/20160820192331858?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---

# 一、意图
最近想要通过路由器直接通过锐捷的认证，然后连接VPN，然后发送WIFI信号。这篇博客主要是讲一下这之间出现的问题，以及解决情况。

# 二、基本情况
锐捷 V4.96

# 三、通过路由器认证锐捷
通过路由器认证锐捷的方式有两种，一种是使用mentohust这款软件认证，另一种就是通过校园锐捷提供的Linux版本认证客户端。

<!-- more -->

## 3.1 mentohust软件使用步骤
1、这款软件是开源的，可以使用RPM包安装，也可以直接获取源码后编译。这里选择使用RPM包安装的方式。
2、安装过程很简单，值得说明的是，安装包是通过电脑连接路由器的WIFI后，使用WinSCP这款软件上传到路由器系统的。
3、安装结束后，直接使用命令打开软件就好。但是，由于本学校的限制，需要将锐捷安装包内的8021x.exe，SuConfig.dat，W32N55.dll这三个文件上传到/etc/mentohust这个目录下。这个步骤并不是所有学校都需要的，视情况而定。
![这里写图片描述](http://blog.cyang.top/20160820192331858?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
4、软件的具体选项可以查看网站，或者执行`mentohust --help` 来获取。类似于：
```
mentohust -u认证账号 -p密码 -w -neth0（网卡） -iIP地址 -m255.255.248.0（掩码） -g网关 -s114.114.114.114（DNS） -a1 -d2 -b2
```
关于每一个具体的选项说明见下图：
![这里写图片描述](http://blog.cyang.top/20160820191946087?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

幸运的话，就可以通过锐捷的认证了。但是，一般来说，第一次不太可能会成功，因为上面的步骤，选项的参数肯定会出错。下面是我遇到的问题以及解决方案：
## 3.2 遇到的问题以及解决方案
1、认证参数的获取。认证账号以及密码不用说，IP，掩码，网关这三个参数非常的重要，这个可以到学校的锐捷管理中心网页中的个人信息出查询。但是一般不准确，或者可能会变。最准确的方式就是看锐捷的认证界面上的信息。因为网关和掩码基本不会变化，但是IP可能过个几天会有一点点变化，这种情况只要先在Windows客户端登陆一遍就可以获取到，近几天就不会变。
2、有的学校是绑定MAC地址的，这一点非常的恶心，但是在linux系统下，更改MAC地址非常的容易，仅仅是一条命令的事情。
3、使用管理员不认可的客户端登录。对于这一点，只需要保证在/etc、mentohust目录下有8021x.exe，SuConfig.dat，W32N55.dll，这三个文件一般就不会有什么问题。也就是步骤中的第三步。
4、最后一点，就是提示认证客户端版本过低。关于这一点，也是我目前没有解决的地方。网上很多教程都说，只要保证上一条中的三个文件拷贝到/etc/mentohust目录下就不会提示版本过低，但是我没有成功。学校锐捷的版本是	v4.96。

## 3.3 通过校园网提供的Linux版本的客户端认证
1、学校是提供Linux版本的锐捷客户端的，并且在虚拟机中运行是可以正常登录的，说明学校并没有禁止Linux系统的锐捷认证。
2、将认证客户端通过WinSCP的方式上传进路由器，执行登录脚本报错。开始以为是路由器的精简的Linux缺少某些必要的文件造成的。
3、所以尝试在树莓派上运行，发现在树莓派上脚本依然无法运行，但是在虚拟机上可以正常运行。
4、最后发现是，学校提供的版本是编译过的客户端程序，并不是源码，而文件里只有X86和X64架构的认证程序，并没有ARM架构的程序。路由器和树莓派均为ARM架构，所以无解。
























