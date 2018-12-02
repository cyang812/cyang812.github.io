title: UI设计实验一
date: 2016/5/17 10:08:23
tags:
- 编程
- java
- Android
- UI
categories:
- Android
thumbnail: http://blog.cyang.top/ui/3.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---

本实验基于 **郭霖《第一行代码》**第三章内容。

## 一、实验内容

<!-- more -->

### 1、常见控件实验

> 控件包括 Button、TextView、EditView、ImageView、ProcessBar、AlertDialog、ProgressDialog

**程序截图：**
>![](http://blog.cyang.top/ui/3.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

>![](http://blog.cyang.top/ui/4.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

>![](http://blog.cyang.top/ui/5.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

>![](http://blog.cyang.top/ui/6.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

>![](http://blog.cyang.top/ui/7.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)



**程序说明：**

1、从上到下分别有5个控件。TextView显示一句话；Button控件添加一个点击响应事件；EditView控件可以输入文字；ImageView控件可以显示图片。
2、Button控件可支持多种功能，当然需要替换内部代码。例如，点击将EditView输入的内容通过Toast显示出来；或者点击切换ImageView控件显示的图片；或者点击对进度条加10%；或者点击弹出AlertDialog或ProgressDialog提示框。
3、控件详解
- TextView控件
 ```android
    android:gravity="center"     //设置文字居中显示
    android:textSize="30sp"      //设置文字的大小
    android:textColor="#00aa00"  //设置文字的颜色
```
- EditView控件
```android
    android:hint="type something here" //设置提示文字
    android:maxLines="2"               //设置最大内容显示行数
```
- ImageView控件
```android
    android:src="@mipmap/ic_launcher" //设置图片文件位置
    android:layout_gravity="center"   //设置图片显示位置
```
- ProcessBar控件
```android
    style="?android:attr/progressBarStyleHorizontal" //设置控件风格
    android:max="100"                                //设置最大量
```
- Button控件
```android
    android:layout_width="wrap_content"   //设置为包裹内容
    android:layout_height="wrap_content"  //设置为由父布局控制
```
4、Button监听事件
- Toast显示EditView输入的内容
```android
String inputText = editText.getText().toString(); //获取输入转换为字符串
Toast.makeText(MainActivity.this, inputText, Toast.LENGTH_SHORT).show();//短时间显示内容
```
- 切换图片
```android
imageView.setImageResource(R.drawable.cyang); //切换显示该位置处的图片
```
- 切换是否显示进度条
```android
if (progressBar.getVisibility() == View.VISIBLE){
    progressBar.setVisibility(View.GONE);
}else {
    progressBar.setVisibility(View.VISIBLE);}
//通过getVisibility获取控件属性，判断后通过setVisibility设置
//控件的三种熟悉为visibility(可见)，invisibility(透明占地方)，gone(完全消失).
```
- 对程序进度条加10
```android
int progress = progressBar.getProgress(); //获取属性
    progress = progress + 10;           //点击加10
    progressBar.setProgress(progress);  //设置属性
```
- 进入一个activity
```android
Intent intent = new intent(FisrtActivity.this,SecondActivity.class);
startActivity(intent);
//第一个参数为启动活动的上下文，第二个参数为指定想要启动的活动
```
- 启动AlertDialog
```android
AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
    dialog.setTitle("这是一个对话框");
    dialog.setMessage("something important.");
    dialog.setCancelable(false);
    dialog.setPositiveButton("ok", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int which) {}});
    dialog.setNegativeButton("no",new DialogInterface.OnClickListener(){
    @Override
    public void onClick(DialogInterface dialog,int which){}});
    dialog.show();
//前面设置title，message，设置不可返回，之后可设置两种选项的触发事件，这里没有设置
```
- 启动ProgressDialog
```android
ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);
progressDialog.setTitle("this is ProgressDialog");
progressDialog.setMessage("Loading...");
progressDialog.setCancelable(true);
progressDialog.show();
//设置名称，内容，是否可通过返回键返回当前活动

```


## 二、Errors&solutions
1、 layout_gravity和gravity的区别
> 前者用于指定控件在布局中的对齐方式，后者用于指定文字在控件中的对齐方式。

2、 可通过使用匿名类的方式来注册监听器，也可以使用实现接口的方式来进行注册。但是，使用前者在实现具体逻辑时程序运行出错，使用第二种时运行正常。

## 三、关键词
> Button、TextView、EditView、ImageView、ProcessBar、AlertDialog、ProgressDialog
