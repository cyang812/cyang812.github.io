title: j-link v9 修复记录
date: 2018-9-13 19:19:23
tags:
- j-link
- stm32
categories:
- 嵌入式
thumbnail: http://blog.cyang.top/j-link/pic.jpg?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# j-link v9
主控: stm32f205RC

# 现象
- 1、给 M0 下载固件的过程中经常出错，提示找不到M0。需要反复尝试很多次才可以下载。
- 2、在一次正常的拔线断电后，再也无法识别，灯也不亮了。

<!-- more -->

# 修复方法一
- 1、准备另一个可以使用的 j-link。这里使用的就是这种只有四根线，只支持 SWD 的 j-link OB。
- 2、拆开坏了的 j-link v9， 可以看到 PCB 上留有四个圆孔，分别是 VCC，GND， SCK，SWD。具体的位置要看对应的原理图，因为有很多不同的 j-link 。
- 3、使用 SWD 的方式连接好的 j-link 和 坏的 j-link 。
![这里写图片描述](http://blog.cyang.top/j-link/pic.jpg?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、打开 j-flash， 新建项目，配置芯片为 STM32F205RC，使用 SWD 接口，点击连接。如果无法连接，可能是上一部四条线没有接对，可更改后在尝试。也不可以不用新建项目，直接用 j-flash 打开 `restore.jflash`。
![这里写图片描述](http://blog.cyang.top/j-link/new.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/j-link/choose.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 5、点击 file, 选择 open data file，打开恢复固件 `JLinkAll.hex`。

- 6、下载固件，完成修复。
![这里写图片描述](http://blog.cyang.top/j-link/program.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/j-link/succ.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
![这里写图片描述](http://blog.cyang.top/j-link/end.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 7、完成之后，j-link v9 就修复了，可正常使用了。
![这里写图片描述](http://blog.cyang.top/j-link/download.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 修复方法二
- 1、前面连接的方法和方法一相同，区别在于下载的东西不同。方法一中下载的固件是完整的，版本比较旧，大概是14年的版本，但是也可以用。
- 2、也可以只烧写一个 bootloader 到掉固件的j-link， 烧写方法如上，也是需要一个好的 j-link， 使用 SWD 接口和坏的 j-link 相连， 使用 j-flash 下载。bootloader 文件见末尾方法二附件。
- 3、下载完成后，将旧的 j-link 和电脑连接，打开 j-link commeder 这个软件。会提示固件需要更新，之后就会自动下载并更新固件。
- 4、之后可以看到 j-link 的 SN 为 -1，表示还未配置 SN，可使用如下命令配置。同时可添加一些特性，代码如下。
```
在JLINK的command下依次运行如下命令  

Exec SetSN=XXXXXXXX      ;添加SN
Exec AddFeature GDB      ;添加GDB
Exec AddFeature RDI      ;添加RDI
Exec AddFeature FlashBP  ;添加FlashBP  
Exec AddFeature FlashDL  ;添加FlashDL
Exec AddFeature JFlash   ;添加JFlash
Exec AddFeature RDDI     ;添加RDDI
```

[方法一附件下载](https://download.csdn.net/download/u011303443/10664113)
[方法二附件下载](https://download.csdn.net/download/u011303443/10666798)
