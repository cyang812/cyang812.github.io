title: 树莓派使用USB摄像头和motion实现监控
date: 2016/9/11 10:08:23
tags:
- 树莓派
- Linux
categories:
- 树莓派
thumbnail: http://blog.cyang.top/%E6%A0%91%E8%8E%93%E6%B4%BE3B.jpg
---


![](http://blog.cyang.top/%E6%A0%91%E8%8E%93%E6%B4%BE3B.jpg)

# 一、工具
- 1、树莓派3B
- 2、USB摄像头

<!-- more -->

# 二、操作步骤
- 1、安装motion
```
sudo apt-get install motion
```

- 2、配置motion

(1)

```
sudo nano /etc/default/motion
```
将里面的no修改成yes，让motion可以一直在后台运行：`start_motion_daemon=yes`

![这里写图片描述](http://blog.cyang.top/20160912231833758?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

(2)

```
sudo nano /etc/motion/motion.conf
```
修改配置文件，这个文件比较长，请确保一下参数的配置。在nano编辑器下，可以使用^w快速查找到如下配置内容。也可以使用^v向下翻页。

![这里写图片描述](http://blog.cyang.top/20160912231847133?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20160912231855620?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20160912231906196?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20160912232402108?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

![这里写图片描述](http://blog.cyang.top/20160912232619513?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、启动motion
```
sudo motion
```

- 4、查看视频数据
在局域网内的设备，不管是手机还是电脑，均可打开浏览器访问`树莓派IP:8081`

![这里写图片描述](http://blog.cyang.top/20160912232736471?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 5、退出motion
```
killall -TERM motion
```


# 三、可能出现的问题
- 1、配置错误
出现`Unknown config option "sdl_threadnr"`
![这里写图片描述](http://blog.cyang.top/20160912232922786?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
解决方法：
在配置文件中，直接将这一行内容进行注释。不是下图光标所在处，是光标下面`sdl_threadnr 0`这一行，注释成`# sdl_threadnr 0`即可。
![这里写图片描述](http://blog.cyang.top/20160912232932546?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 2、8081页面无法显示
在8081端口，无法显示数据，但是在8080端口可以看到motion的信息。
![这里写图片描述](http://blog.cyang.top/20160912233127366?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
**解决方法：**
这可能是摄像头没有被识别，可以将摄像头拔下重新插入。
