title: C 语言 查找字符串子串
date: 2017/4/12 19:16:23
tags:
- 编程
- C
categories:
- 编程
thumbnail: http://blog.cyang.top/vc++%20%E6%9F%A5%E6%89%BE%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%AD%90%E4%B8%B2.png
---


![](http://blog.cyang.top/vc++%20%E6%9F%A5%E6%89%BE%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%AD%90%E4%B8%B2.png)

<!-- more -->

主要的思路就是，不断的从母串 str1 中取出和子串长度相等的临时子串 temp_str，与子串 str2 进行比较。没有找到子串，返回 -1；成功找到子串，返回子串首字母在母串中的位置，该位置从 0 开始。



```c
#include <stdio.h>
#include <string.h>

char temp_str[30];    // 临时子串

void ReadStrUnit(char * str,char *temp_str,int idx,int len)  // 从母串中获取与子串长度相等的临时子串
{
	int index = 0;
	for(index; index < len; index++)
	{
		temp_str[index] = str[idx+index];
	}
	temp_str[index] = '\0';
}

int GetSubStrPos(char *str1,char *str2)
{
	int idx = 0;
	int len1 = strlen(str1);
	int len2 = strlen(str2);
	
	if( len1 < len2)
	{
		printf("error 1 \n"); // 子串比母串长
		return -1;
	}

	while(1)
	{
		ReadStrUnit(str1,temp_str,idx,len2);    // 不断获取的从 母串的 idx 位置处更新临时子串
		if(strcmp(str2,temp_str)==0)break;      // 若临时子串和子串一致，结束循环
		idx++;                                  // 改变从母串中取临时子串的位置
		if(idx>=len1)return -1;                 // 若 idx 已经超出母串长度，说明母串不包含该子串
	}

	return idx;    // 返回子串第一个字符在母串中的位置
}

int main()
{
	char *str1 = "abcdefghijk";
	char *str2 = "def";
	int i = 0;
	i = GetSubStrPos(str1,str2);
	if( i<0 )
	{
		printf("not found\n");
	}
	else
	{
		printf("i = %d\n",i);
	}

	return 0;
}

```