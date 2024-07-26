#include <stdio.h>
#include <string.h>
#include "Test_1.h"

#if TEST_2024_07_25 > 0
//����һ������ֵΪ����,�������Ϊuint8_tָ���uin32_t���͵ĺ���
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

//���ݴ�����
void processReceivedData(StudentData data,uint8_t *recvBuffer, uint16_t length)
{
	//���ݺ������ж�
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
	//��ѧ����Ϣ(�ṹ�������ʼ��)
	static StudentInfo UStInfo = 
	{
		.grade = 1,//��һ
		.age = 19,
		.name = "Tom",
		.height = 175.5,
		.weight = 130.0
	};
	/*
	��������"��ʼ��"��ʽ��������(�Գ�ʼ��"name"����Ϊ��):
	(1)
		StudentInfo UStInfo;
		UStInfo.name[20] = "Tom";

	����������£�����ʽ��Ϊname����ָ���˴�СΪ20���ⲻ�ǳ�ʼ���������飬
	���ǽ�һ���ַ������� "Tom" ���Ƶ�����name��ǰ20���ַ�λ�á�
	���ַ�ʽ�����Ѿ�����������󣬶�����һ���ֽ��и�ֵ�������Ƕ������ṹ����г�ʼ����

	(2)
		static StudentInfo UStInfo = 
		{
			.name = "Tom"
		};

	C������,�ڽṹ���ʼ��ʱ�����"name"��һ���ַ������Ա���ҳ�ʼ��ʱʹ�����ַ�������"Tom"��
	���������Զ������ַ����ĳ��ȣ��������㹻�Ŀռ����洢����
	��ˣ��㲻��Ҫ��ʽ��ָ������Ĵ�С��������������ַ��������Զ�����
	*/

	/*
	  ����һ��StudentInfo_Handle���͵ı���,StudentInfo_HandleΪ�ṹ��ָ��StudentInfo*,
	  ��ôUStInfo_Handle����һ��ָ��UStInfo�Ľṹ��ָ��
	*/
	const StudentInfo_Handle UStInfo_Handle = &UStInfo;
	//ʹ��"->"������ṹ���еĸ�������
	printf("%d\r\n",UStInfo_Handle->grade);




	#endif

	#if TEST_2024_07_23 > 0
	//��ά���������ϰ
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
	//����ָ���Ӧ��
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

	// uint8_t Big_Endian_Arr[4] = {0x42,0xF7,0x00,0x00};//���ģʽ
	// uint8_t Little_Endian_Arr[4] = {0x00,0x00,0xF7,0x42};//С��ģʽ
	// Func_ptr p = Get_Float;
	// p(Little_Endian_Arr);
	// printf("��ȡ�ĸ�����Ϊ:%.2f\r\n", u8_f.f);

	uint8_t heightBuffer[5] = {0x01, 0x00, 0x00, 0xF7, 0x42};
	uint8_t weightBuffer[5] = {0x02, 0x00, 0x00, 0xC8, 0x42};
	StudentData StuData;
	processReceivedData(StuData, &weightBuffer[0], sizeof(heightBuffer) / sizeof(heightBuffer[0]));
	#endif
}

