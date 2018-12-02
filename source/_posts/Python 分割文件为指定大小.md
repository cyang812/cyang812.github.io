title: python分割文件为指定大小
date: 2016/11/3 14:14:23
tags:
- python
categories:
- 教程
thumbnail: http://blog.cyang.top/Python%20split.png
---


![](http://blog.cyang.top/Python%20split.png)

本文参考[python学习--大文件分割与合并](http://www.cnblogs.com/shenghl/p/3946656.html)
# 一、说明
1、使用python将文件分割成指定大小，便于传输。例如，在文件大小大于U盘大小时，可使用改程序将数据进行切割。
2、本程序在每一个切割文件的前四个字节，添加了表示分割后的文件序号以及分割后的文件总数。

<!-- more -->

# 二、代码
```
# coding=utf-8

import sys,os

kilobytes = 1024 #1K byte
megabytes = kilobytes*1000 #1M byte
chunksize = int(200*megabytes) #default chunksize

def getPartSum(fromfile,chunksize):
	'''
	get the total number of part
	'''
    if os.path.getsize(fromfile)%chunksize != 0:
        return int(os.path.getsize(fromfile)/chunksize)+1
    else:
        return int(os.path.getsize(fromfile)/chunksize)

def split(fromfile,todir,chunksize=chunksize):
	'''
	split files by the chunksize
	'''
    if not os.path.exists(todir):#check whether todir exists or not
        os.mkdir(todir)          #make a folder
    else:
        for fname in os.listdir(todir):
            os.remove(os.path.join(todir,fname))
    partnum = 0  # the number of part
    partsum = getPartSum(fromfile,chunksize)  # the sum of parts
    inputfile = open(fromfile,'rb')# open the fromfile
    while True:
        chunk = inputfile.read(chunksize)
        if not chunk:             # check the chunk is empty
            break
        partnum += 1
        filename = os.path.join(todir,('part%04d'%partnum)) # make file name
        fileobj = open(filename,'wb')  # create partfile
        fileobj.write(bytes.fromhex('%04x'%partnum)) #write the serial number
        fileobj.write(bytes.fromhex('%04x'%partsum)) #write the sum of parts
        fileobj.write(chunk)         #write data into partfile
        fileobj.close()
    return partnum
if __name__=='__main__':
        fromfile  = input('File to be split?')
        todir     = input('Directory to store part files?')
        chunksize = int(input('Chunksize to be split?'))
        absfrom,absto = map(os.path.abspath,[fromfile,todir])
        print('Splitting',absfrom,'to',absto,'by',chunksize)
        try:
            parts = split(fromfile,todir,chunksize)
        except:
            print('Error during split:')
            print(sys.exc_info()[0],sys.exc_info()[1])
        else:
            print('split finished:',parts,'parts are in',absto)
```

# 三、示例
![](http://blog.cyang.top/python%20split%20demo.png)
