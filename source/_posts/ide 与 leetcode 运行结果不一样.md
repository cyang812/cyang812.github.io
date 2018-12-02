title: ide 与 leetcode 运行结果不一样
date: 2018-09-16 12:57:19
tags:
- leetcode
- C语言
categories:
- 编程
thumbnail: http://blog.cyang.top/leetcode/diff.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


在做 leetcode 的[第 15 题](https://leetcode.com/problems/3sum/description/)， `3Sum` 时发现，同样的代码在本地运行的结果是正确的，而在 leetcode 的服务器上结果却是错误的。而且检查了程序中，也并没有使用全局或者静态变量。

![这里写图片描述](http://blog.cyang.top/leetcode/diff.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

通过打印，仔细对比两种环境下的输出发现，原来是代码有一条语句指针指向了数组外边的第一个地址。语句的内容是比较当前地址的值是否和后一个地址的值相同， 由于后一个地址实际上已经发生了溢出，在当前地址为数组最后一个元素时，下一个地址就在数组外边了，这个地址的值是不确定的。在本地调试时，由于两个地址的值不同，所以程序结果正确，而在 leetcode 服务器上运行时，这两个值相同，因此程序最终的结果就错误了。

<!-- more -->

![这里写图片描述](http://blog.cyang.top/leetcode/debug.png?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

下面是完整代码，出错的代码在第 62 行。
```c
/*
* @Author: cyang
* @Date:   2018-08-06 16:26:59
* @Last Modified by:   cyang
* @Last Modified time: 2018-09-15 14:55:12
*/

#include <stdio.h>
#include <stdlib.h>

int cmpfunc (const void * a, const void * b)
{
   return ( *(int*)a - *(int*)b );
}

/**
 * Return an array of arrays of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int** threeSum(int* nums, int numsSize, int* returnSize) {

	int **return_array = (int **)malloc(sizeof(int *) * numsSize * (numsSize-1));

	qsort(nums, numsSize, sizeof(int), cmpfunc);

	int *start, *end;
	int rerurn_size = 0;
	int temp = 0;

	for (int i = 0; i < numsSize-2; ++i)
	{
		if (nums[i-1] == nums[i] && i > 0)
			continue;

		printf("=======================================\n");

		start = &nums[i+1];
		end = &nums[numsSize-1];

		while(start < end)
		{
			printf("%d %d %d %d \n", i, nums[i], *start, *end);
			temp = *start + *end + nums[i];
			if(temp == 0)
			{
				// printf("%d %d %d %d \n", i, nums[i], *start, *end);
				return_array[rerurn_size] = (int *)malloc(sizeof(int) * 3);
				return_array[rerurn_size][0] = nums[i];
				return_array[rerurn_size][1] = *start;
				return_array[rerurn_size][2] = *end;

				rerurn_size++;

				while(*start == *(start+1))
				{
					printf("+\n");
					start++;
				}
				#if 1 //ringht
				while((*end == *(end+1)) && (end != &nums[numsSize-1]))
				#else //wrong
				while(*end == *(end+1))
				#endif
				{
					printf("-\n");
					end--;
				}

				start++;
				end--;
			}
			else if (temp < 0)
			{
				start++;
			}
			else // > 0
			{
				end--;
			}

			printf("%p, %p\n", start, end);		
		}
	}

	*returnSize = rerurn_size;
	printf("rerurn_size = %d\n", rerurn_size);

	return return_array;
}

int nums[] = {1, -4, -4, 2, 0, 0, -2, 3, 3, -3, -4};

int main(int argc, char const *argv[])
{
	// int nums[] = {-1, 0, 1, 2, -1, -4};
	// int nums[] = {0, 0, 0, 0};
	// int nums[] = {1, -1, -1, 0};
	// int nums[] = {-2, 0, 0, 2, 2};
	// int nums[] = {-2, 0, 1, 1, 2};
	// int nums[] = {1, -4, -4, 2, 0, 0, -2, 3, 3, -3, -4};

	int numsSize = sizeof(nums)/sizeof(int);

	int returnSize = 0;
	int **return_array = NULL;

	return_array = threeSum(nums, numsSize, &returnSize);

	printf("\n");
	if(return_array == NULL)
		return 0;

	for (int i = 0; i < returnSize; ++i)
	{
		for (int j = 0; j < 3; ++j)
		{
			printf("%d ", return_array[i][j]);
		}

		printf("\n");
	}

	for (int i = 0; i < returnSize; ++i)
	{
		free(return_array[i]);
	}

	// system("pause");

	return 0;
}
```
