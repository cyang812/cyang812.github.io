title:  38块搞定一年的vps和shadowsocks服务
date: 2017/1/8 21:14:23
tags:
- vps
- shadowsocks
- 梯子
categories:
- 教程
thumbnail: http://blog.cyang.top/CSDN%E5%9B%9E%E6%94%B6%E7%AB%99.png
---


这是一篇半年前写的教程，之前放在CSDN，最近不知道为什么突然被管理员删除了。现在放在个人博客。

![](http://blog.cyang.top/CSDN%E5%9B%9E%E6%94%B6%E7%AB%99.png)

# 原文如下：

<!-- more -->

无意间看到一篇教程，利用VPS搭建SS服务的。试着搭了一遍，使用了两天感觉还不错。VPS也很便宜，38块一年，每个月250G流量，网速还可以，1080p的youtube视频没问题。搭建的教程也很简单。下面简单记录一下。我是参考这篇文章搭建的。

## 一、前期准备
1、购买vps，我使用的是由[openVZVPS](https://www.50vz.net/)提供的38元一年的服务。使用支付宝付款。
![](http://blog.cyang.top/20160630220341378?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

2、SecureCRT，使用ssh连接服务器。或者使用xshell，这个软件个人免费使用。
![](http://blog.cyang.top/20160630220558149?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 二、安装步骤
1、重装VPS系统。默认安装的系统是cent OS,这里的教程是基于Ubuntu的。所以需要重装系统。重装时需要设置系统密码。这个密码很重要。
![](http://blog.cyang.top/20160630220613614?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

2、连接ssh服务器。主机名就是你vps的ip，还有端口号，这些都可以到你的vps查看，用户名是root,然后点连接，点连接后会让你输入密码，密码就是你安装系统的时候设置的密码。连上服务器之后就要在服务器上安装shadowsocks了。

3、依次输入以下指令完成安装。sudo不用输入。
```bash
$ sudo apt-get update
$ sudo apt-get install python-gevent python-pip
$ sudo pip install shadowsocks
$ apt-get install python-m2crypto
```

4、创建配置文件。
```bash
vi /etc/shadowsocks.json
```

5、修改配置文件。配置一下你的服务器IP和密码就可以了。
```
{
    "server":"0.0.0.0",
    "server_port":8388,
    "local_port":1080,
    "password":"password",
    "timeout":600,
    "method":"aes-256-cfb"
}
```

6、最后，输入指令开启SS服务。当然也可以输入指令关闭SS服务。
这样就结束了整体的安装。在手机和电脑上配置后就可以使用了。
```bash
$ su -
ssserver -c /etc/shadowsocks.json -d start //开启
ssserver -c /etc/shadowsocks.json -d stop  //关闭
```
