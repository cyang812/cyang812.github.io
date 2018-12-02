title: 树莓派使用DHT11和yeelink实现远程获取环境信息
date: 2017/1/2 10:08:23
tags:
- 树莓派
- DHT11
categories:
- 树莓派
thumbnail: http://blog.cyang.top/DHT11-yeelink.png
---

![DHT11](http://blog.cyang.top/DHT11-yeelink.png)

## 原理：
树莓派使用DHT11传感器获取环境温湿度信息，上传至yeelink服务器。

<!-- more -->

## 工具：
- 1、树莓派
- 2、DHT11温湿度传感器
- 3、yeelink账号

## 实现；
- 1、注册yeelink账号，获取用户Api-key。
- 2、添加设备，添加数值型传感器。
- 3、树莓派使用Adafruit库函数获取温湿度信息。
- 4、将信息通过python-curl的方式上传。

## 附录：
- 1、树莓派需要安装python-curl库。可使用pip安装。
- 2、树莓派需要安装Adafruit函数库。

## 代码：
```bash
cd /home/pi/class/DHT11/Adafruit_Python_DHT/examples
#pwd
#ls
sudo python AdafruitDHT.py 11 16 >/home/pi/class/DHT11/DHT11_Data.txt
#cat /home/pi/class/DHT11/DHT11_Data.txt
cd /home/pi/class/DHT11
sudo python Post_DHT11_data.py &
```

- 1、首先是使用python运行AdafruitDHT.py程序，这个程序需要两个运行参数，一个是11，代表DHT11,一个是16，代表传感器的数据是通过16管脚接入的。
- 2、然后是通过输出重定向，将结果保存在一个文件中。
- 3、之后使用上传函数将传感器数据上传至服务器。
- 4、本实验所使用的代码均可在[github](https://github.com/cyang812/Raspberry-Pi)找到。

## 参考链接；
- 1、https://github.com/adafruit/Adafruit_Python_DHT
- 2、http://blog.csdn.net/xukai871105/article/details/38349519
