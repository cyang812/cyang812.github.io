title: Arduino 按键实验
date: 2017/8/19 20:17:23
tags:
- Arduino
categories:
- Arduino
thumbnail: http://blog.cyang.top/20170819195808163?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# 一、功能

实现按键控制LED亮灭。按下点亮，再次按下熄灭。

# 二、原理图
![这里写图片描述](http://blog.cyang.top/20170819195808163?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

# 三、代码

```c
int ButtonState;
int ButtonLastState;
int ButtonCounter;

void setup() {
  // put your setup code here, to run once:
  pinMode(13,OUTPUT);
  pinMode(11,INPUT_PULLUP);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  ButtonState = digitalRead(11);
  
  if(ButtonState != ButtonLastState)
  {
    if(ButtonState)
    {
        Serial.println("on");
        ButtonCounter++;
    }
    else
    {
      Serial.println("off");  
    }
    delay(100);
  }

  ButtonLastState = ButtonState;
  if(ButtonCounter%2)
  {
    //Serial.println(ButtonCounter);
    digitalWrite(13,1);  
  }
  else
  {
    digitalWrite(13,0);  
  }
}
```

# 四、解析
- 1、开关通过一个数字接口连接到 arduino，端口配置为上拉输入。在默认情况下，端口电平为高，按键按下时，端口被拉低。
- 2、理想情况下，一次按键对应着一个下降沿，一段低电平，一个上升沿。程序通过`ButtonState`和`ButtonLastState`两个标志位来判断电平情况，通过这两个标志位实现了下降沿和上升沿进入第一个判断语句，即`if(ButtonLastState != ButtonState)`，只不过下降沿和上升沿所处理的操作不同，下降沿不做处理，仅打印一条语句，上升沿时候则代表按键被按下并且已经放开了，则对按键次数进行加一。
- 3、要实现，按下点亮，再次按下熄灭，只需要对按键次数进行模2操作。
