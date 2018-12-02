title: BootLoader浅析
date: 2016-11-28 20:56:19
tags:
- BootLoader
categories:
- 硬件
thumbnail: http://blog.cyang.top/20161128154143013?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


关于Bootloader，从书上的文字描述，很难理解这个名词是什么，有什么用。这次用到了，算是有了更进一步的认识。

## 一、知识点
- 1、BootLoader就是单片机启动时候运行的一段小程序，这段程序负责单片机固件的更新，也就是单片机选择性的自己给自己下程序。可以更新，也可以不更新，更新的话，BootLoader更新完程序后，跳转到新程序运行；不更新的话，BootLoader直接跳转到原来的程序去运行。
- 2、BootLoader更新完程序后并不擦除自己，下次启动后依然先运行BootLoader程序，又可以选择性的更新或者不更新程序，所以BootLoader就是用来管理单片机程序的更新。

<!-- more -->

- 3、在实际的单片机工程项目中，如果加入了BootLoader功能，就可以给单片机日后升级程序留出一个接口，方便日后单片机程序更新。当然，这就需要创建两个工程项目，一个为BootLoader工程，一个为APP工程。
- 4、BootLoader工程生成的.hex或者.bin文件通常下载到ROM或Flash中的首地址，这样可以保证上电后先运行BootLoader程序。而APP工程生成的.hex或者.bin文件则下载到ROM或Flash中BootLoader后面的地址中。也就是说，存在ROM/Flash中的内容是分为两部分的。
- 5、要实现在同一个ROM/Flash中保存两段程序，并且保证不能相互覆盖，则需要在下载程序时指定地址。如在Keil下，可以进行如下的调整。

![这里写图片描述](http://blog.cyang.top/20161128154143013?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 6、实际上，在STM32系列的单片机中，Flash本身就是分扇区的，一个扇区16KB的样子，具体可以查看手册。那么就可以用从第一个扇区的首地址开始下载BootLoader的程序，而从第二个扇区的起始地址开始下载APP程序。如下为STM32F4系列芯片的Flash模块。

![这里写图片描述](http://blog.cyang.top/20161128154224828?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 7、单片机上电之后开始执行BootLoader程序，这是单片机会检测用户是否有升级应用程序（APP）的请求，具体表现有很多种，例如检测内存卡，Nand Flash中是否包含升级文件，串口/I2C/SPI等外设接口是否传来升级文件。据说还有使用GSM来升级的。
- 8、所谓的升级，就是将ROM/Flash中存储APP程序的扇区内容擦除并写入新文件。例如一次固件升级的过程可以是：1、单片机上电执行BootLoader，2、BootLoader查找升级文件，3、若找到文件，擦除Flash中的部分扇区（存APP的），4、在擦除的扇区写入升级的文件，5、写入完成，读取数据检验是否出错，6、若数据一致，升级成功，删除升级文件，7、BootLoader程序跳转到APP程序执行。删除升级文件是为了下次上电后不再进行升级。
- 9、所谓的跳转，可以理解程序指针的改变，变为指向APP程序扇区的起始地址。

## 二、部分代码
- 1、主函数

```c
int main(void)
{
	HAL_Init();//STM32初始化
	SystemClock_Config();//时钟配置
	System_GPIOInit();//IO口配置
	
	#ifdef BOOTLOAD_DISPLAY_ENABLE
	SystemColorInit();//显示屏配置
	#endif
	
	System_LoadUpdateFile();//升级函数
	while (1)
	{
		
	}
}
```

- 2、升级函数

```c
void System_LoadUpdateFile(void)
{
	uint8_t res;	
	if(bNandFlash_Error)//如果NandFlash错误，串口打印错误信息，跳转到用户程序
	{
		d_printf("NandFlash_Error jump\n");
		BootLoad_Jump();//跳转函数
		return;
	}
	if(bNo_FileSystem)//如果没有文件系统，串口打印错误信息，跳转到用户程序
	{
		d_printf("no file system jump\n");
		BootLoad_Jump();//跳转函数
		return;
	}
	if(f_open(&File, (char *)UPDATE_FILE_PATH, FA_READ)==FR_OK)//如果存在升级文件，开始执行升级
	{
		d_printf("update\n");
		if(BootLoad_Program())//是否写入成功
		{
			f_close(&File);//关闭升级文件
			res=f_unlink((char *)UPDATE_FILE_PATH);//删除升级文件
			d_printfhex(res);d_printf("\n");
			res=f_unlink((char *)UPDATE_DIR_PATH);//删除升级目录
			d_printfhex(res);d_printf("\n");

			BootLoad_Jump();//跳转函数
		}
		else
		{
			HAL_FLASH_Lock();//锁定Flash
			d_printf("update fail\n");
			f_close(&File);//关闭升级文件
			BootLoad_Jump();//跳转函数
		}		
	}
	else
	{
		d_printf("jump\n");
		f_close(&File);
		BootLoad_Jump();
	}
}
```

- 3、重写Flash函数

```c
uint8_t BootLoad_Program(void)
{
	uint32_t BaseAddress=APPLICATION_ADDRESS;//APP地址
	uint32_t i,br,datacnt=0;
	uint8_t data8;
	GlobalPtr32=(uint32_t *)BootBuff;
	HAL_FLASH_Unlock();//解锁Flash
	if(BootLoad_Erase()==false)//擦除Flash
	{
		return false;
	}
	d_printf("size:");d_printfhex32(File.fsize);d_printf("\n");
	while(1)
	{
		f_read(&File,BootBuff,8192,(void *)&br);//读取升级文件
		for (i=0;i<(br>>2);i++)
		{
			if (HAL_FLASH_Program(FLASH_TYPEPROGRAM_WORD, BaseAddress, GlobalPtr32[i]) == HAL_OK)//写入升级文件
			{
				BaseAddress = BaseAddress +4;
			}
			else
			{ 
				d_printf("program err\n");
				return false;
			}
		}
		datacnt+=br;
		if(datacnt>=File.fsize)//写入完成
		{
			break;
		}
	}
	d_printf("verify\n");		//验证Flash中的内容与升级文件是否一致
	f_lseek(&File,0);			//若一致代表升级成功
	datacnt=0;					//若不一致代表升级失败
	BaseAddress=APPLICATION_ADDRESS;
	while(1)
	{
		f_read(&File,BootBuff,8192,(void *)&br);
		for (i=0;i<br;i++)
		{
			data8 = *(__IO uint8_t*)BaseAddress;
			if (data8 != BootBuff[i])
			{
				d_printf("error!\n");
				return false;
			}
			BaseAddress ++;
		}
		datacnt+=br;
		if(datacnt>=File.fsize)
		{
			break;
		}
	}
	HAL_FLASH_Lock();//锁定Flash
	return true;
}
```

- 4、跳转函数（从BootLoader中跳转到APP的main函数）

```c
void BootLoad_Jump(void)
{
	/* Check Vector Table: Test if user code is programmed starting from address 
	"APPLICATION_ADDRESS" */
	d_printfhex32((*(__IO uint32_t*)APPLICATION_ADDRESS));d_printf("\n");
	if (((*(__IO uint32_t*)APPLICATION_ADDRESS) & 0x2FFE0000 ) == 0x20000000)
	{
	JumpAddress = *(__IO uint32_t*) (APPLICATION_ADDRESS +4);
	d_printfhex32(JumpAddress);d_printf("\n");
	HAL_Delay(100);
	Jump_To_Application = (pFunction) JumpAddress;
	/* Initialize user application's Stack Pointer */
	__set_MSP(*(__IO uint32_t*) APPLICATION_ADDRESS);
	Jump_To_Application();
	}
}
```