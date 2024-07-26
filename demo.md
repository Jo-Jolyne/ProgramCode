#include <stdio.h>
#include <string.h>
#include "Test_1.h"

#if TEST_2024_07_25 > 0
//定义一个返回值为整型,传入参数为uint8_t指针和uin32_t类型的函数
void Get_32bit_num(uint8_t* Arr_ptr, uint32_t x)
{
	for (int i = 0; i < 4; i++)
	{
		printf("Arr[%d] = %#x\t", i, *(Arr_ptr + i));
		x |= *(Arr_ptr + i) << ((3 - i) * 8);
	}
	printf("\r\n%d\r\n",x);
	printf("%#x\r\n", x);
}

void Get_Float(uint8_t* RecBuf_ptr, uint8_t* Arr_ptr, uint16_t length)
{
	for (int i = 0; i < length; i++)
	{
		*(RecBuf_ptr + i) = *(Arr_ptr + i);
		// printf("%#x\r\n", *(RecBuf_ptr + i));
	}
}

//数据处理函数
void processReceivedData(StudentData data,uint8_t *recvBuffer, uint16_t length)
{
	//数据合理性判断
	if (length < 5) 
	{
		printf("Invalid data length\n");
		return;
    }
	data.DataType = (*recvBuffer == 1)? Stu_Height : Stu_Weight;
	Get_Float(&data.u8_f.u8Data[0], (recvBuffer + 1), length);
	switch (data.DataType)
	{
		case 1:
			printf("Received height: %.2f\n", data.u8_f.f);
			break;

		case 2:
			printf("Received weight: %.2f\n", data.u8_f.f);
			break;

		default:
			break;
	}
}

#endif


void main()
{
	#if TEST_2024_07_16 > 0
	// printf("%d\r\n",sizeof(uint16_t));//2
	// printf("%d\r\n",sizeof(uint8_t));//1
	uint8_t a = 0;
	uint16_t b = 4095;//0xfff000
	uint8_t c = 0;
	a = (uint8_t)b << 4;
	c = (b << 12) >> 8;
	printf("%#x\r\n",a);
	printf("%#x\r\n",a);
	#endif

	#if TEST_2024_07_17 > 0
	//大学生信息(结构体变量初始化)
	static StudentInfo UStInfo = 
	{
		.grade = 1,//大一
		.age = 19,
		.name = "Tom",
		.height = 175.5,
		.weight = 130.0
	};
	/*
	下面两种"初始化"方式有所区别(以初始化"name"变量为例):
	(1)
		StudentInfo UStInfo;
		UStInfo.name[20] = "Tom";

	在这种情况下，你显式地为name数组指定了大小为20。这不是初始化整个数组，
	而是将一个字符串常量 "Tom" 复制到数组name的前20个字符位置。
	这种方式是在已经定义了数组后，对其中一部分进行赋值，而不是对整个结构体进行初始化。

	(2)
		static StudentInfo UStInfo = 
		{
			.name = "Tom"
		};

	C语言中,在结构体初始化时，如果"name"是一个字符数组成员，且初始化时使用了字符串常量"Tom"，
	编译器会自动计算字符串的长度，并分配足够的空间来存储它。
	因此，你不需要显式地指定数组的大小，编译器会根据字符串长度自动处理。
	*/

	/*
	  定义一个StudentInfo_Handle类型的变量,StudentInfo_Handle为结构体指针StudentInfo*,
	  那么UStInfo_Handle就是一个指向UStInfo的结构体指针
	*/
	const StudentInfo_Handle UStInfo_Handle = &UStInfo;
	//使用"->"来输出结构体中的各个变量
	printf("%d\r\n",UStInfo_Handle->grade);




	#endif

	#if TEST_2024_07_23 > 0
	//二维数组相关练习
	int a[2][3] = {{0,1,2},{3,4,5}};
	int b[6] = {0};
	int *p = &a[0][0];
	for (int i = 0; i < 6; i++)
	{
		b[i] = p[i];
		printf("%d\r\n",b[i]);
	}
	#endif

	#if TEST_2024_07_24 > 0
	int Array[5] = {1, 2, 3, 4, 5};
	for (int i = 0; i < 5; i++)
	{
		switch (Array[i])
		{
		case 0 ... 5:
			printf("Array[%d] = %d\r\n", i, Array[i]);
			break;
		default:
			break;
		}
	}
	
	#endif

	#if TEST_2024_07_25 > 0
	//函数指针的应用
	// uint32_t num = 0;
	// uint8_t Arr[4] = {0x12,0x34,0x56,0x78};
	// Func_ptr p = Get_32bit_num;
	// p(Arr, num); 
	// Func_ptr = Get_32bit_num;
	// (*Func_ptr)(Arr, num);

	// u8_f.f = 123.5f;
	// for (int i = 0; i < 4; i++)
	// {
	// 	printf("u8Data[%d] = %#x\r\n", i, u8_f.u8Data[i]);
	// }

	// uint8_t Big_Endian_Arr[4] = {0x42,0xF7,0x00,0x00};//大端模式
	// uint8_t Little_Endian_Arr[4] = {0x00,0x00,0xF7,0x42};//小端模式
	// Func_ptr p = Get_Float;
	// p(Little_Endian_Arr);
	// printf("获取的浮点数为:%.2f\r\n", u8_f.f);

	uint8_t heightBuffer[5] = {0x01, 0x00, 0x00, 0xF7, 0x42};
	uint8_t weightBuffer[5] = {0x02, 0x00, 0x00, 0xC8, 0x42};
	StudentData StuData;
	processReceivedData(StuData, &weightBuffer[0], sizeof(heightBuffer) / sizeof(heightBuffer[0]));
	#endif
}

