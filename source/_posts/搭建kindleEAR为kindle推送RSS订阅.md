title:  搭建kindleEAR为kindle推送RSS订阅
date: 2017/1/16 16:54:23
tags:
- kindle
- Google
- RSS
categories:
- 教程
thumbnail: http://blog.cyang.top/kindleEAR.png
---


![](http://blog.cyang.top/kindleEAR.png)

# 一、简介
[kindleEAR](https://github.com/cdhigh/KindleEar)是一个运行在Google App Engine(GAE)上的Kindle个人推送服务应用，生成排版精美的杂志模式mobi/epub格式自动每天推送至您的Kindle或其他邮箱。

<!-- more -->

此应用目前的主要功能有：

- 支持类似Calibre的recipe格式的不限量RSS/ATOM或网页内容收集
- 不限量自定义RSS，直接输入RSS/ATOM链接和标题即可自动推送
- 多账号管理，支持多用户和多Kindle
- 生成带图的杂志格式mobi或带图的有目录epub
- 自动每天定时推送
- 强大而且方便的邮件中转服务和Evernote/Pocket/Instapaper等系统的集成

# 二、搭建过程
- 1、 [申请google账号](https://accounts.google.com/SignUp) 并暂时 [启用不够安全的应用的访问权限](https://www.google.com/settings/security/lesssecureapps) 以便上传程序。
- 2、[创建一个Application](https://console.developers.google.com/iam-admin/projects)，注意不用申请GCE，那个是60天试用的，而GAE是限额范围内永久免费的。
- 3、open cloud shell
![这里写图片描述](http://blog.cyang.top/20170116161614670?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 4、Clone and cd then sh upload.sh
	```
	git clone https://github.com/miaowm5/KeUploader.git
	cd KeUploader
	sh upload.sh
	```

- 5、Set information of your app
![这里写图片描述](http://blog.cyang.top/20170116161824371?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 6、Open yourappid.appspot.com and enjoy. :-)
![这里写图片描述](http://blog.cyang.top/20170116161934455?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 三、使用示例
- 1、使用默认订阅源
![这里写图片描述](http://blog.cyang.top/20170116162512359?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 2、添加自定义订阅源
![这里写图片描述](http://blog.cyang.top/20170116162743485?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 3、设置推送邮箱及时间
![这里写图片描述](http://blog.cyang.top/20170116162803187?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 4、同步到pocket,evernote等第三方应用
![这里写图片描述](http://blog.cyang.top/20170116162829984?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
# 四、注意事项
- 1、需要将你的邮箱添加到亚马逊的[信任列表](https://www.amazon.cn/mn/dcw/myx.html/ref=kinw_myk_redirect#/home/settings/payment)。
- 2、一开始如果发件列表出错，推送可能会出错。类似于`wrong SRC_EMAIL`。解决方法：确定发件邮箱正确。
![这里写图片描述](http://blog.cyang.top/20170116164843839?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)


# 五、参考链接
- 1、[kindleEAR](https://github.com/cdhigh/KindleEar)
- 2、[如何更好的使用kindle](http://zhangyongcun.com/2016/05/15/%E5%A6%82%E4%BD%95%E6%9B%B4%E5%A5%BD%E7%9A%84%E4%BD%BF%E7%94%A8kindle/)
- 3、[cdhigh/KindleEar](https://github.com/cdhigh/KindleEar)
