title: 装逼编辑器Atom+activiate-power-mode插件效果配置
date: 2016/7/27 10:08:23
tags:
- Atom
- 插件
- activiate-power-mode
categories:
- 教程
thumbnail: http://blog.cyang.top/20160727220824261?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


## 插件效果展示
![这里写图片描述](http://blog.cyang.top/20160727220824261?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 本文说明
#### activiate-power-mode是一款非常 **装逼** 的Atom编辑器插件。具体的安装方法就不多说，网上有很多的教程。这里只将插件的震动效果的配置。

<!-- more -->

#### 以下三种方法均为该插件的震动效果展示。第一种比较强烈，屏幕晃动厉害，辣眼睛；第二种为默认，文件多时可能也比较晃眼；第三种为完全不震动。可根据个人喜好，自由设置震动效果。

- 1、超震
![这里写图片描述](http://blog.cyang.top/20160727220734518?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 2、默认
![这里写图片描述](http://blog.cyang.top/20160727220802081?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、不震
![这里写图片描述](http://blog.cyang.top/20160727220812120?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 方法一

- 1、打开安装目录。一般为 `C:\Users\你的用户名\.atom\packages\activate-power-mode\lib`

- 2、打开该目录下的配置文件`config-schema.coffee`
![这里写图片描述](http://blog.cyang.top/20160727221236317?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、修改15行和第23行的数值。范围均为0~100,如上面三幅图片所示的数值分别为：超震10，30；默认1.3；不震0，0.
![这里写图片描述](http://blog.cyang.top/20160727221248673?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 方法二

- 1、打开Atom，点击file，点击setting。
- 2、找到activate-power-mode，选择setting。
![这里写图片描述](http://blog.cyang.top/20160829124045539?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、找到如下图所示的设置框，根据自己的喜好设置。我这里为0/0，即不振的。
![这里写图片描述](http://blog.cyang.top/20160829124055848?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
