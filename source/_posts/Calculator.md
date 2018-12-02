title: Calculator 简易计算器开发
date: 2016/7/28 19:34:23
tags:
- 编程
- java
- Android
categories:
- Android
thumbnail: http://blog.cyang.top/calculator/calculator.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# Calculator 简易计算器开发

## 项目说明
使用Android平台开发一款简易的计算器应用。

<!-- more -->

## 知识点
- 1、布局采用一个TextView来显示计算数据，采用TabRow制作一个4*4的控件,分别对应与10个数字和4中基本算数操作，以及等于号和清零号。
- 2、实现计算效果的基本逻辑是，每一个运算必须保证三个元素，前两个为数字，中间一个为运算符号。在一个列表里检测到有三个元素后，提取1和3作为两个运算数，提取中间的操作符号，通过switch语句选择具体的运算操作。
- 3、每一次通过屏幕点击的10个数字，以及四个基本运算符号，均为一个Item对象，这个对象具有一个构造函数，需要输入值，以及类型。其中类型有五种，分别为数字以及4种运算符。可以通过新建的类，并且通过使用 ` public static final int ADD = 0 `之类的操作来实现类似于宏定义的效果。
- 4、数字按键的点击逻辑：通过监听，在点击这个控件之后，通过使用`TextView.append("1")`之类的语句，来实现将数字暂存起来。
- 5、加法控件的点击逻辑：首先需要将第一个数字从`TextView.getText()`转化成一个Item对象并添加到列表里面，之后会执行一个`checkAndComputer`函数，这个函数用于列表里面是否有大于或等于三个元素，从而对其进行挑选并计算结果，最终显示出来。
- 6、`checkAndComputer`这个函数在检测到有三个元素后就会进行计算，并且删除了之前列表里的三个元素，将计算的结果重新添加进列表作为结果，因此在展示结果的时候可以直接显示列表的第一个元素。
- 7、基本流程图
![计算器流程图](http://blog.cyang.top/calculator/calculator.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

## 部分代码
- 1、基本元素Item类
```java
public class Item {

    public Item(double value,int type){
        this.value = value;
        this.type = type;
    }

    public double value = 0;

    public int type = 0;
}
```

- 2、java中类似宏定义的用法

```java
public class Types {

    public static final int ADD = 1;
    public static final int SUB = 2;
    public static final int MUT = 3;
    public static final int DIV = 4;
    public static final int NUM = 5;

}
```
- 3、加法控件逻辑

```java
case R.id.btn_add:
                Log.e("TAG","this is add");
                items.add(new Item(Double.parseDouble(tvScreen.getText().toString()),Types.NUM));
                Log.e("TAG","this is add");
                checkAndComputer();
                items.add(new Item(0,Types.ADD));
                tvScreen.setText("");
                break;
```

- 4、checkAndComputer函数
```java
public void checkAndComputer(){

        if (items.size()>=3){

            double a = items.get(0).value;
            double b = items.get(2).value;
            int opt = items.get(1).type;

            items.clear();

            switch (opt){
                case Types.ADD:
                    items.add(new Item(a+b,Types.NUM));
                    break;
                case Types.SUB:
                    items.add(new Item(a-b,Types.NUM));
                    break;
                case Types.MUT:
                    items.add(new Item(a*b,Types.NUM));
                    break;
                case Types.DIV:
                    items.add(new Item(a/b,Types.NUM));
                    break;
            }
        }

    }
```
