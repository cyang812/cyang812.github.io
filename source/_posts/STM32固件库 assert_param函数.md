title: STM32固件库 assert_param函数
date: 2016/11/2 14:50:23
tags:
- STM32
categories:
- 硬件
thumbnail: http://blog.cyang.top/assert_param.png
---


![](http://blog.cyang.top/assert_param.png)

# 一、知识点
-1、固件函数库通过检查库函书的输入来实现运行时间错误侦测。通过使用宏assert_param来实现运行时间检测。所有要求输入参数的函数都使用这个宏。它可以检查输入参数是否在允许的范围之内。

<!-- more -->

例如通过定义
```c
#define IS_ADC_ALL_PERIPH(PERIPH) (((PERIPH) == ADC1) || \
                                   ((PERIPH) == ADC2) || \
                                   ((PERIPH) == ADC3))
```
这样在每次调用带输入参数的函数时,都会对所输入的参数进行检查，例如
```c
void ADC_ClearFlag(ADC_TypeDef* ADCx, uint8_t ADC_FLAG)
{
  /* Check the parameters */
  assert_param(IS_ADC_ALL_PERIPH(ADCx));
  assert_param(IS_ADC_CLEAR_FLAG(ADC_FLAG));

  /* Clear the selected ADC flags */
  ADCx->SR = ~(uint32_t)ADC_FLAG;
}
```
实际上关于宏`assert_param`的定义如下：
```c
/* Exported macro ------------------------------------------------------------*/
#ifdef  USE_FULL_ASSERT

/**
  * @brief  The assert_param macro is used for function's parameters check.
  * @param  expr: If expr is false, it calls assert_failed function
  *   which reports the name of the source file and the source
  *   line number of the call that failed.
  *   If expr is true, it returns no value.
  * @retval None
  */
  #define assert_param(expr) ((expr) ? (void)0 : assert_failed((uint8_t *)__FILE__, __LINE__))
/* Exported functions ------------------------------------------------------- */
  void assert_failed(uint8_t* file, uint32_t line);
#else
  #define assert_param(expr) ((void)0)
#endif
```
如果在定义了`USE_FULL_ASSERT`的情况下，那么`assert_param`的宏定义就是一个条件运算符,类似于`a=(b>c)?b:c`,即如果b>c，则a=b；否则a=c。如果在没定义`USE_FULL_ASSERT`的情况下，就是一个空函数。

就是说如果输入正确，则`IS_ADC_ALL_PERIPH(ADCx)`值为1，则`assert_param(IS_ADC_ALL_PERIPH(ADCx))`运行(void)0,否则执行后一个函数`assert_failed((uint8_t *)__FILE__, __LINE__)`。

关于`assert_failed((uint8_t *)__FILE__, __LINE__)`，这个函数是用来打印错误信息的，用户可以自己编程，用于打印出程序错误的地方，这个函数是在main.c里面的，函数原型如下：
```c
#ifdef  USE_FULL_ASSERT

/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t* file, uint32_t line)
{
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */

  /* Infinite loop */
  while (1)
  {
  }
}
#endif
```

# 二、说明
- 1、在上述的代码中，出现了`\`符号，这个符号的作用是当程序不在一行时，连接程序几部分。如下:
```c
#define IS_ADC_ALL_PERIPH(PERIPH) (((PERIPH) == ADC1) || \
                                   ((PERIPH) == ADC2) || \
                                   ((PERIPH) == ADC3))
```
表示`PERIPH`可以为`ADC1`或`ADC2`或`ADC3`.
