title: leetcode power of three
date: 2017/8/1 22:27:23
tags:
- 编程
- C
categories:
- 编程
thumbnail: http://blog.cyang.top/20170801215151913?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# 一、题目

leetcode 上有这么一道题,[power of three.](https://leetcode.com/problems/power-of-three/description/)

题目如下：
>Given an integer, write a function to determine if it is a power of three.

要求：
> Could you do it without using any loop / recursion?

就是说给出一个数，判断该数是否是 3 的 n 次方。且最好不要使用循环或者迭代来实现。

<!-- more -->


# 二、解法：

## 1、方法一、
使用最基本的循环判断，通过循环判断目标值是否可以对 3 进行整除。代码如下：
```c
while(n)
  {
      if(n==1)return true;
      if(n%3 != 0)
          return false;
      n /= 3;
      if(n == 1)
          return true;
  }
  return false;
```

## 2、方法二、
由于在 int（4字节）的范围内，3 最大的一个次方数为 3^19，即 1162261467，可用该数值对目标值进行取余操作，如果余数为 0，则说明目标值是一个 3 的某次方数。代码如下：
```c
if(n <= 0)return false;
if(1162261467%n == 0)
    return true;
else
    return false;
```

## 3、方法三
通过对目标值取 3 的对数，判断该值是否为整数来判断。利用换底公式，`log3(n) = log10(n) / log10(3)`。利用`a-(int)a == 0` 来判断 a 是否为整数。代码如下：
```
double res;
res = log10(n)/log10(3);
if(res- (int)res == 0)
    return true;
else
    return false;
```

三种解法的代码在 leetcode 网站的运行时间如下图：
- 1、方法一
![这里写图片描述](http://blog.cyang.top/20170801215151913?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 2、方法二
![这里写图片描述](http://blog.cyang.top/20170801215159841?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)
- 3、方法三
![这里写图片描述](http://blog.cyang.top/20170801215207349?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

可见，第二种最好，第一种次之，第三种最差。
类似的题目还有 power of two, power of four，使用上述三种方法略加修改即可。

# 三、附录
全部代码:
```c
/*
*326. Power of Three
* three ways to solution this problem
*/

#include <stdio.h>
#include <stdbool.h>
#include <math.h>

#define solution 3

bool isPowerOfThree(int n) {

  #if solution==1
  //循环迭代
    while(n)
    {
        if(n==1)return true;
        if(n%3 != 0)
            return false;
        n /= 3;
        if(n == 1)
            return true;
    }
    return false;
  #elif solution==2
  //32位数中最大的3次方数
    if(n <= 0)return false;
    if(1162261467%n == 0)
        return true;
    else
        return false;
  #elif solution==3
  //对数换底公式
  //使用 a-(int)a == 0; 来判断a是否为整数
    double res;
    res = log10(n)/log10(3);
    if(res- (int)res == 0)
        return true;
    else
        return false;
  #endif
}

int main()
{
    int num = 4782968;
    bool res = false;

    res = isPowerOfThree(num);
    printf("res = %d\n",res);

    return 0;
}

```