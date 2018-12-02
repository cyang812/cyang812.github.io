title: STM32 硬件CRC和软件CRC速度比较
date: 2018/3/12 22:49:23
tags:
- 编程
- IAR
- STM32
categories:
- 嵌入式
thumbnail: http://blog.cyang.top/20180312224132752?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


# 一、测试条件
硬件： STM32L432KC
主频： 80MHz
编译器： IAR 8.20.1
编译选项： High Speed no size constraints
CRC 生成多项式： 0x782f


# 二、测试方法
软件提前生成CRC表，用于查询。分别使用软件CRC算法和硬件CRC外设对一个缓存进行计算，目的是从该缓存中找到同步头。同步头共11字节，前两个字节为后九个字节的CRC校验值。通过迭代算法依次对11字节进行计算和比较，当找到同步头后返回同步头偏移量。通过时间比较两者之间的速度。

# 三、测试结果
迭代24464次后，从缓存中找到同步头。
不开启编译时间优化时，软件算法用时238ms，硬件CRC用时220ms。
![这里写图片描述](http://blog.cyang.top/20180312224132752?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

开启编译时间优化后，软件算法用时159ms，硬件CRC用时186ms。
![这里写图片描述](http://blog.cyang.top/20180312224305220?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 四、附测试代码

```c
#include "user_crc.h"
#include "stm32l4xx_hal.h"

#define SOFT_CRC  1
#define HARD_CRC  2

CRC_HandleTypeDef   CrcHandle;
uint16_t crc_tab[256];

void crc_init()
{
	/*##-1- Configure the CRC peripheral #######################################*/
	CrcHandle.Instance = CRC;

	/* The default polynomial is not used. It is required to defined it in CrcHandle.Init.GeneratingPolynomial*/	
	CrcHandle.Init.DefaultPolynomialUse 	 = DEFAULT_POLYNOMIAL_DISABLE;

	/* Set the value of the polynomial */
	CrcHandle.Init.GeneratingPolynomial 	 = CRC_POLYNOMIAL_16B;

	/* The user-defined generating polynomial generates a
		 16-bit long CRC */
	CrcHandle.Init.CRCLength				 = CRC_POLYLENGTH_16B;

	/* The default init value is used */
	CrcHandle.Init.DefaultInitValueUse		 = DEFAULT_INIT_VALUE_DISABLE;

	/* The input data are not inverted */
	CrcHandle.Init.InputDataInversionMode    = CRC_INPUTDATA_INVERSION_NONE;

	/* The output data are not inverted */
	CrcHandle.Init.OutputDataInversionMode   = CRC_OUTPUTDATA_INVERSION_DISABLE;

	/* The input data are 8-bit long */
	CrcHandle.InputDataFormat 				 = CRC_INPUTDATA_FORMAT_BYTES;

	if (HAL_CRC_Init(&CrcHandle) != HAL_OK)
	{
		/* Initialization Error */
		Error_Handler();
	}
}

void crc_buildTab(uint16_t gen_polynom)
{
	for(int value = 0; value < 256; value++) 
	{
		uint16_t crc = value << 8;

		for(int i = 0; i < 8; i++) 
		{
			if(crc & 0x8000)
				crc = (crc << 1) ^ gen_polynom;
			else
				crc = crc << 1;
		}

		crc_tab[value] = crc;
	}

}

uint16_t soft_crc_calc(const uint8_t *data, uint16_t len) 
{
	uint16_t crc = 0x0000;

	for(uint16_t offset = 0; offset < len; offset++)
	{
		crc = (crc << 8) ^ crc_tab[(crc >> 8) ^ data[offset]];
	}

	return crc;
}

uint16_t hard_crc_calc(const uint8_t *data, uint16_t len)
{
	uint16_t crc = 0x0000;

	crc = HAL_CRC_Calculate(&CrcHandle, (uint32_t *)data, len);

	return crc;
}

uint16_t find_sync_word(uint8_t *data, uint32_t data_len, uint8_t crc_type)
{
	uint8_t *ptr;
	uint16_t crc_stored,crc_calced;

	ptr = data;

	for(uint32_t i=0; i<data_len-9; i++)
	{
		crc_stored = ptr[0]<<8 | ptr[1];
		if(crc_type == SOFT_CRC)
		{
			crc_calced = soft_crc_calc((uint8_t *)&ptr[2], 9);
		}
		else if(crc_type == HARD_CRC)
		{
			crc_calced = hard_crc_calc((uint8_t *)&ptr[2], 9);
		}

		if( (crc_stored != 0x0000) && (crc_stored == crc_calced) )
		{
			printf("crc check ok! crc1 = 0x%04x,crc2 = 0x%04x\n", crc_stored,crc_calced);
			return i;
		}

		ptr++;
	}

	return 0xffff;
}


void crc_test()
{
	uint32_t tick1,tick2;
	uint32_t find_cnt = 0;
	uint16_t gen_polynom = 0x782f;

	crc_init();
	crc_buildTab(gen_polynom);
	
	tick1 = HAL_GetTick();
	find_cnt = find_sync_word((uint8_t *)superFrameBuf, sizeof(superFrameBuf), SOFT_CRC);
	tick2 = HAL_GetTick();
	printf("use soft_crc find sync word after %d iteration, use time %d\n", find_cnt, tick2 - tick1);

	printf("\n");

	tick1 = HAL_GetTick();
	find_cnt = find_sync_word((uint8_t *)superFrameBuf, sizeof(superFrameBuf), HARD_CRC);
	tick2 = HAL_GetTick();
	printf("use hard_crc find sync word after %d iteration, use time %d\n", find_cnt, tick2 - tick1);

}

```

 
 