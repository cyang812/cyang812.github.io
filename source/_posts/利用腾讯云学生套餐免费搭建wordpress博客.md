title: 利用腾讯云学生套餐免费搭建WP博客
date: 2016/7/8 10:08:23
tags:
- 腾讯云
- Wordpress
- 服务器
- Linux
- 博客
categories:
- 教程
thumbnail: http://blog.cyang.top/20160708191226398?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


本文是一片个人博客建站教程，利用腾讯云学生套餐搭建免费的WP博客。

# 一、前期准备

## 1、申请[学生套餐](http://www.qcloud.com/event/qcloudSchool)中免费的腾讯云服务器。首先需要通过学信网验证学生身份，之后腾讯每月会赠送一个满65元减64元的代金券，一直赠送到大学毕业。云服务器建议选择安装cent OS 6.x版本。安装时候需要记住系统镜像的密码，后续通过SSH连接服务器时需要填写这个密码。
![这里写图片描述](http://blog.cyang.top/20160708191226398?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

## 2、购买域名。这个可以在万网购买，很多后缀都只要6元一年。
![这里写图片描述](http://blog.cyang.top/20160708191334898?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


# 二、安装步骤

----------
-------面板安装-----------

## **1、通过SSH连接云服务器**
1.1建议使用X shell这个软件。在主机框内填写云服务器的公网IP地址。这个地址从腾讯云服务器管理后天得到。
![这里写图片描述](http://blog.cyang.top/20160708191504284?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
1.2 填写后点击连接，需要填写服务器的用户名和密码。如果是cent OS ,默认用户名为root，密码是你安装镜像是填写的密码。如果忘记，需要去腾讯云服务器后台重新设置，重新设置后需要重启服务器才能生效。
![这里写图片描述](http://blog.cyang.top/20160708192152214?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
**补充：** 如果使用的是ubuntu系统，则默认不提供管理员权限。需要通过用户名ubuntu登陆，登陆后通过输入`sudo passwd root`获取权限，之后输入`sudo -s -H`提升用户权限至管理员。

## **2、安装AMH面板。**

2.1 安装面板需要获取管理员权限。具体安装步骤查看[官网](
https://amh.sh/install.htm)。
![这里写图片描述](http://blog.cyang.top/20160708192914577?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

 2.2  amh4.2版本安装过程比较慢，大概需要20分钟。如果是5.2版本就很快，不过需要支付每月6元的费用。这里选择4.2版本就可以。
 ![这里写图片描述](http://blog.cyang.top/20160708193102156?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## **3、安装面板成功后，就可以登陆面板，之后通过面板安装博客程序。**
面板安装完成后，就可以通过访问 `主机ip:8888`登陆到amh面板
![这里写图片描述](http://blog.cyang.top/20160708193313235?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

----------
-------博客程序安装---------

## **4、新建一个虚拟主机**
点击“虚拟主机”选项卡，并按照下图填入你想要绑定的域名（这里以cyang.top 这个域名为例，不要忘了将域名的A记录解析到你VPS的IP上），然后点击保存。
![这里写图片描述](http://blog.cyang.top/20160708193501750?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

域名解析直接使用万网提供的解析就好。
![这里写图片描述](http://blog.cyang.top/20160708200217435?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## **5、安装AMFTP–在线文件管理工具**
点击“扩展模块”选项卡，找到AMFTP-2.0并点击“安装”。
![这里写图片描述](http://blog.cyang.top/20160708193915193?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## **6. 添加FTP账户**
点击“FTP”选项卡，如下图输入你想要设置的FTP的账号密码，并将主机根目录选择为我们刚才新建的虚拟主机的根目录。然后点击“保存”即可完成新建。
![这里写图片描述](http://blog.cyang.top/20160708194023675?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


## **7. 上传Wordpress程序**
点击“FTP”选项卡，在我们刚才新建的FTP账户的最后面点击“管理”并输入上一步中我们设置的FTP的账号密码，登录之后即可进入在线文件管理系统。
![这里写图片描述](http://blog.cyang.top/20160708200422215?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


7.1 选择并删除默认自带的 index.html 文件

7.2 点击“上传”按钮将Wordpress程序的zip压缩包上传到根目录中。
WordPress程序请从这里下载https://cn.wordpress.org/wordpress-4.2.2-zh_CN.zip
![这里写图片描述](http://blog.cyang.top/20160708194303557?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

7.3 上传完成之后，点击“刷新”即可看到我们刚刚上传的压缩包，选择它，并且点击“智能解压”。

7.4 点击进入刚刚解压的wordpress目录，点击复选框，全选目录下的所有文件，移动到目录中输入“/”，然后点击确定。这样就把所有文件都移动到根目录中了。
全选应该移动的项目应该为19项，这里和截图不一样。
![这里写图片描述](http://blog.cyang.top/20160708194455294?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


## **8. 新建Wordpress数据库**

点击选项卡“MYSQL”–“快速建库”，在弹出的页面“数据库名称”中输入你想要建立的数据库名字，AMH会自动生成“数据库用户名”以及“数据库用户密码”，点击“创建”，将这三组数据记下来，一会要用到。
![这里写图片描述](http://blog.cyang.top/20160708194655319?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20160708194801112?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## **9. 配置Wordpress**

打开你的域名，本例子中为 http://www.cyang.top

点击“现在就开始”，输入刚才的三组数据（数据库名、数据库用户名、数据库密码）然后点击“进行安装”。

9.1 输入站点相关信息，然后点击“确定”就完成安装啦。
![这里写图片描述](http://blog.cyang.top/20160708195146446?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


原文链接 [csdn博客](http://blog.csdn.net/u011303443/article/details/51863282)

# 参考链接
1、[安装面板的视频教程](http://yun.baidu.com/share/home?uk=288436188&third=1&view=share#category/type=0)

2、*[安装博客文件的教程](http://www.izcv.com/379.html)*
