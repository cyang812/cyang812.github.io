title: PCM 转 WAV pcmToWav
date: 2018/6/7 19:45:23
tags:
- python
- PCM
categories:
- 编程
thumbnail: http://blog.cyang.top/20180607194104253?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---

PCM 数据无法直接通过播放器打开，因为少了 44 字节的文件头，这里面最主要的信息是描述该 PCM 的采样频率，通道数，以及位数。

双击 pcmToWav.exe，拖入待转换的 PCM 数据，输入通道数和采样频率，默认使用 16-bit 表示一个采样点。等待程序运行结束，就会生成一个同名的 .wav 文件。

<!-- more -->

![这里写图片描述](http://blog.cyang.top/20180607194104253?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

python 源码

```python
# -*- coding: utf-8 -*-
# @Author: cyang
# @Date:   2018-06-07 11:30:01
# @Last Modified by:   cyang
# @Last Modified time: 2018-06-07 14:43:23

import os
import wave

def pcmToWav(in_file, out_file):

	in_file = open(in_file, 'rb')
	out_file = wave.open(out_file, 'wb')

	out_file.setnchannels(int(input("plese input channels [1/2]: ")))
	out_file.setframerate(int(input("plese input samplerate [32000]: ")))
	out_file.setsampwidth(2) #16-bit
	out_file.writeframesraw(in_file.read())

	in_file.close()
	out_file.close()
	print('complete')

if __name__ == '__main__':
	IN_FILE = input(r'input the file name: ')
	OUT_FILE = IN_FILE.split('.')[0]+'.wav'

	pcmToWav(IN_FILE, OUT_FILE)
	os.system('pause')
```
