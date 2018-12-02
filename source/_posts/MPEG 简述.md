title: MPEG 简述
date: 2018-08-12 16:18:19
tags:
- Audio
- MPEG
- MP3
categories:
- 总结
thumbnail: http://blog.cyang.top/MPEG/bitrate.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---

# MPEG AUDIO 简介
MP3 是 MPEG Layer3 音频压缩技术的简写，这种技术可在音质极少损伤的情况下获取更好的压缩性能。MP3文件可以被压缩成不同的速率，文件压缩的越小，音质损伤越大。标准的压缩比例为10：1，一段3分钟的音频数据压缩后只需4MB大小。
<!-- more -->

MPEG 音频压缩算法由联合图像专家组开发，作为高质量数字音频数据压缩的国际标准。MPEG-1 音频压缩算法基于两种机理来减少音频信号码率额，一是利用统计相关性，去除音频信号的冗余，二是利用人耳的心理声学现象如频率掩蔽和时间掩蔽等，去除听觉冗余。分为三个层次，分别是 layer1,layer2,layer3，每个层次针对不同的应用，但是三个层次的基本模型是相同的，每个后继的层都有更高的压缩比，但是需要更加复杂的编解码器。

更新的版本是 MPEG-2，这个版本扩展了MPEG-1的采样率标准，在 MPEG-1 的 32kHz,44.1kHz, 48kHz 的基础上，支持16kHz,22.05kHz,24kHz 采样率。并且MPEG-2还支持多通道。

有一个非官方的标准MPEG-2.5，支持更低的采样率，支持8KHz，11.025kHz 和 12KHz。


# MP3 bitstream 概述

广泛使用的MP3指的是不论MPEG哪个版本的layer3的编码，然而，有时候MP3也泛指了所有的MPEG算法。
MPP3 的音频流具有固定可选的码率。根据不同的采样率和 layer 可选的 bitrate 不同，具体见下表。也允许对流中不同的部分使用不同的码率，称为“VBR”。
![](http://blog.cyang.top/MPEG/bitrate.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

MP3编码压缩后的数据是由一系列固定大小的块组成的，这些块叫做帧。每一帧具有多少的PCM采样数据是固定的，见下表。解码时必须从帧头开始解，但可以只解一部分，可解的最小单元数见下表。
![](http://blog.cyang.top/MPEG/sample.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

帧的格式见下图，帧头是32位的数据，用于同步一些基本的信息。例如采样率，通道数，比特率之类。16位的CRC校验值是可选的。注意，CRC校验的并不是整个帧的数据，仅仅是帧头数据的后16位和辅助信息的校验。
![](http://blog.cyang.top/MPEG/frame.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

MPEG的音频文件没有主要的文件头。一个MPEG文件是由一系列的标准帧组成的。帧与帧之间不能有任何的垃圾数据。帧的大小可从帧头信息中获得，因此如果知道前一帧头位置可以很容易得到下一帧的帧头位置。为了支持更多的自定义格式，解码器必须连续读取几个帧头，以便计算出该音频一帧的长度。在使用自定义模式时，不可使用VBR模式，且不可以和固定大小的帧混合。

在Layer1和Layer2中，帧是相对独立的。每一帧都包含了足够解码使用的信息，所以解码器只要在数据流中正确解出一个帧头后就可以完成工作。但是在Layer3中，帧不总是独立的。
