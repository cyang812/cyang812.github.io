title: Source Insight 更新4.0版本
date: 2017/3/19 20:10:23
tags:
- source insight
categories:
- 总结
thumbnail: http://blog.cyang.top/20170311100050765?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


## 一、使用体验
![这里写图片描述](http://blog.cyang.top/20170311100050765?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
source insight 终于大版本更新了。我开始使用 SI 是去年，那时候是 3.5 的版本，就觉得这是一款神器，搭配 Keil 和 IAR 等编译软件使用，可以提高单片机编程的效率。SI 有很多的特性是 keil 和 IAR 不具备的，作为一个代码编辑软件来说，很多功能确实很强大。只不过官方久久不更新，所以很多现在主流 IDE 或者代码编辑器的一些实用功能都不支持。

<!-- more -->

这次更新，整合了一些新功能，界面也有所改进。之前的版本有的设置界面很小，字都不能完全显示完，这次更新修复了这些问题。而且，可以很方便的将旧版本的配置文件导出，并导入进新版本，所以惯用的快捷键和代码配色都可以和之前保持一样，升级后也不需要做过多改动。值得一提的是，这次版本内置了多套主题，尽管都不是那么好看。

以下是一些新特性的展示：

- 1、文件对比
 
    这个功能在->Tools 中，可以支持当前文件和备份文件对比，也支持两个文件对比。甚至支持文件夹内容对比。简单使用后发现效果好不错，这是替代 UltraCompare 的节奏啊，不过应该不支持文件信息的二进制版本对比。
    ![这里写图片描述](http://blog.cyang.top/20170311095522944?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 2、代码 Reformat

    这个功能也在->Tools 中，可以支持几种常见的代码风格，例如：ANSI、GNU、K&R，也支持自定义，这和Eclipse 中的一样。代码风格是非常个人化的东西，看着舒服就好。这个功能在拷贝粘贴代码的时候很实用，设置好自己的代码风格，拷贝代码后 reformat 一下，大括号缩进什么的就可以轻松搞定。
    ![这里写图片描述](http://blog.cyang.top/20170311095742213?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 3、自动补全

    自动补全这个功能本来就有，这也是使用 SI 写代码比直接在 keil IAR 中写代码效率高的一个很重要的原因。但是这次自动补全又增加了新的功能，支持一些关键字的自动补全。例如如下的 for 循环，if else 结构。
    ![这里写图片描述](http://blog.cyang.top/20170311095618672?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、代码折叠

    这个功能在阅读非常长的代码时还是很好用的。
    ![这里写图片描述](http://blog.cyang.top/20170311095819439?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

还有更多的功能例如主题配色，查找等就不演示了，反正都比 3.5 版本更好了。但是好也不是完全的，在使用新版本是出现过一次程序奔溃，再次打开后当前文件就部分出现了乱码。

## 二、修改设置
虽然从 3.5 版本更新到 4.0 可以导入之前的配置文件，很多键盘设置和配色方案都可以很好的过渡，但还是有一些东西会有不同，需要重新设置。不过这也是因人而异的，更多的还是风格的问题。以下是我在使用时做的一些设置修改。

- 1、编码方式

    3.5版本时，默认的编码方式为系统默认的编码方式，即 Windows ANSI，4.0版本的默认编码方式则为 UTF-8，这就导致了在 3.5 版本中可以正常显示的中文注释，在 4.0 版本中变成乱码。修改方式如下：
    在 Options->Preferences->Files 中的最下面，Default enconding 从 UTF-8 修改为 ANSI。
    ![这里写图片描述](http://blog.cyang.top/20170311095850887?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 2、部分快捷键
    这些快捷键可以从 3.5 导入配置文件导过来，不过如果有些和默认中重复，则需要手动修改下。修改方式如下： 在 Options->Key Assignments 中，根据自己的需要进行修改，我一般会改这几个地方。

    ```c
    Symbol: Jump To Definition -> Alt+1  //跳转到定义
    Navigation: Go Back -> Alt+2 //返回
    Symbol: Jump To Caller -> Alt+3 //查看调用
    File: Open -> Alt+Q //打开，其实就是切换下文件，如果已经在标签页中，使用 Ctrl+Tab 也行
    View: Project Window -> Alt+0 //关闭或打开项目文件列表
    ```

- 3、自动补全

    自动补全功能是因为在新版本中默认不使用 Tab 键补全，只能使用回车键，习惯了旧版本可能会有点不适应这一点，不过好在这是可以修改的，在 Options->Typing 中间那栏 Auto Completion 中，勾选 Tab key selects item 即可。
    ![这里写图片描述](http://blog.cyang.top/20170311095924669?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 4、TAB键宽度
    
    由于 SI 只是用来编写代码的，编译还是在 IDE 中，所以 Tab 键的宽度应该和 IDE 中保持一致，这样在 IDE 中查看代码的时候格式才不会错位，我一般习惯的 Tab 键宽度为 2 ，4.0 版本默认为4，所以需要做如下修改：Options->File Type Options 右下一栏中的 Tab Width。
    

- 5、大括号位置
    
    这还是一个代码风格的问题，就是大括号的位置是在 if 后面，下面，下面后两格的问题。我习惯于大括号在正下面，但是 SI 有一个智能缩进，会将大括号自动缩进在下面后两格。这一个可在 Options->File Type Options 右边一栏 Auto Indent 中修改，从 Smart 改为 Simple 即可。
    ![这里写图片描述](http://blog.cyang.top/20170311095942466?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)