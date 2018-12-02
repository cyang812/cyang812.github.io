title: hexo安装RSS插件
date: 2016/8/27 09:42:23
tags:
- hexo
- RSS
- 插件
categories:
- 教程
thumbnail: http://blog.cyang.top/20160827093755958?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---

# 一、步骤

1、安装插件。进入本地hexo目录，打开git bash。输入以下命令

```
npm install hexo-generator-feed

```

![这里写图片描述](http://blog.cyang.top/20160827093755958?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

2、添加配置。在本地hexo根目录下的_config.yml文件中，添加以下配置。

```
# Extensions
## Plugins: http://hexo.io/plugins/
#RSS订阅
plugin:
- hexo-generator-feed
#Feed Atom
feed:
type: atom
path: atom.xml
limit: 20
```
![这里写图片描述](http://blog.cyang.top/20160827093855258?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

3、添加主题配置，在主题目录下的_config.yml目录下，添加如下配置。

```
rss: /atom.xml
```

![这里写图片描述](http://blog.cyang.top/20160827093903148?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
