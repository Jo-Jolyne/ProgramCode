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

//���ݴ�������
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

#if TEST_2024_07_26 > 0
//��ȡ������
void Get_Float(float f_data, uint8_t* ptr)
{
	uint32_t *f_ptr = (uint32_t *)&f_data;
	for (int i = 3; i >= 0; i--)
	{
		*(ptr + i) = (*f_ptr >> (i * 8)) & 0xFF;
	}
}
#endif

#if TEST_2024_08_26 > 0
int* add(int a, int b)
{
	int sum = 0;
	int *p = &sum;
	*p = a + b;
	printf("*p = %d\r\n", *p);
	return p;
}
#endif

#if TEST_2024_09_02 > 0
#endif

#if TEST_2024_09_03 > 0

#endif

#if TEST_2024_09_06 > 0

#endif
 
#if TEST_2024_09_22 > 0
void ModbusRTU_01_Process(uint8_t *pData, uint16_t dataLen)
{
	uint8_t ReturnCode = MODBUS_OK;
	uint16_t reg = (pData[MODBUSRTU_REG_START_Pos] << 8) | pData[MODBUSRTU_REG_START_Pos + 1];
	printf("reg = %d\r\n",reg);
	uint16_t regLen = (pData[MODBUSRTU_REG_COUNT_Pos] << 8) | pData[MODBUSRTU_REG_COUNT_Pos + 1];
	printf("regLen = %d\r\n",regLen);

	uint8_t i = 0, j = 0, k = 0;
	uint16_t regIndex = 0;
	uint8_t	 byteIndex = 0;
	uint8_t  bitIndex = 0;

	//对于ModbusRTU协议来说,加上CRC校验码,串口发送一帧数据正好8个字节(10指令除外)
	if(dataLen != 8)
	{
		ReturnCode = MODBUS_DAT_ERR;
	}

	if(MODBUS_OK == ReturnCode)
	{
		//ModbusRTU协议功能码到这里的操作都是一样的
		txBuffer[MODBUSRTU_ADD_Pos] = pData[MODBUSRTU_ADD_Pos];
		txBuffer[MODBUSRTU_FUN_Pos] = pData[MODBUSRTU_FUN_Pos];
		txPoint = 2;

		//数据处理部分需要区分
		i = regLen/8;
		if(0!=(regLen%8))
		{
			i += 1;
		} 

		// printf("i = %d\r\n",i);


		txBuffer[MODBUSRTU_ACK_BYTE_Pos] = i;
		// printf("txBuffer[%d] = %#x\r\n",MODBUSRTU_ACK_BYTE_Pos,txBuffer[MODBUSRTU_ACK_BYTE_Pos]);
		txPoint += 1;
		// printf("txPoint = %d\r\n",txPoint);

		// for (j = 0; j < i; j++)
		// {
		// 	txBuffer[MODBUSRTU_ACK_DATA_Pos + j] = 0;
		// }

		for (k = 0; k < regLen; k++)
		{
			regIndex = (reg + k) / 16;
			byteIndex = ((reg + k) % 16)/8;	//1 - (((reg + j) % 16)/8);
			bitIndex = (reg + k) % 8;

			if(aucModbusData[0 + regIndex][byteIndex] & (0x01 << bitIndex))
			{
				txBuffer[MODBUSRTU_ACK_DATA_Pos + k/8] |= (0x01 << (k%8));
			}
		}

		printf("\r\n");
		txPoint += i;
		printf("WithoutCRC_txPoint = %d\r\n",txPoint);
		//组包发送长度还要加2
		txPoint += 2;
		printf("Final_txPoint = %d\r\n",txPoint);

		for (int m = 0; m < (txPoint - 2); m++)
		{
			printf("txBuffer[%d] = %#x, ",m,txBuffer[m]);
		}
	}
}
#endif
 
#if TEST_2024_10_08 > 0

/*  1.STM32单片机数据存储方式--小端存储  */
// uint8_t aucModbusData[48][2];
uint8_t aucModbusData[20][2];
const T_BOOT_PARA DefaultBootPara = {
	.UartADDR = 1,
	.IP1 = 192,
	.IP2 = 168,
	.IP3 = 2,
	.IP4 = 199,
	.PortH = 0x13,
	.PortL = 0x88,
	.RunMode = 0xA5,
	.ID	= 0,
	.UpdateType = 0xA3,
	.EraseFlagH = 0,
	.EraseFlagL = 0,
	.CheckFlashLen = 0,//只是验证VSCode中数据的存储方式(小端存储)
	.UartStopBits = 1,//默认1个停止位
  	.UartParity = 1,//默认无奇偶校验位
  	.UartBaudRate = 115200,//默认串口通信波特率为115200
	.RSV1 = 0,
	.RSV2 = 0,
	.RSV3 = 0,
	.RSV4 = 0,
	.RSV5 = 0,
	.RSV6 = 0,
	.RSV7 = 0,
};

/*  2.C语言中结构体强制类型转换  */



#endif


int data = 10;

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
	//��ѧ����Ϣ(�ṹ�������ʼ��?)
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
	���ַ�ʽ�����Ѿ������������?������һ���ֽ��и�ֵ�������Ƕ������ṹ����г�ʼ����?

	(2)
		static StudentInfo UStInfo = 
		{
			.name = "Tom"
		};

	C������,�ڽṹ���ʼ��ʱ�����"name"��һ���ַ������Ա���ҳ�ʼ��ʱʹ�����ַ�������?"Tom"��
	���������Զ������ַ����ĳ��ȣ��������㹻�Ŀռ����洢����
	��ˣ��㲻���?��ʽ��ָ������Ĵ�С��������������ַ��������Զ�������
	*/

	/*
	  ����һ��StudentInfo_Handle���͵ı���,StudentInfo_HandleΪ�ṹ��ָ��StudentInfo*,
	  ��ôUStInfo_Handle����һ��ָ��UStInfo�Ľṹ��ָ��
	*/
	const StudentInfo_Handle UStInfo_Handle = &UStInfo;
	//ʹ��"->"������ṹ���еĸ�������?
	printf("%d\r\n",UStInfo_Handle->grade);




	#endif

	#if TEST_2024_07_23 > 0
	//��ά����������?
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
	//����ָ���Ӧ��?
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

	// uint8_t Big_Endian_Arr[4] = {0x42,0xF7,0x00,0x00};//���ģ�?
	// uint8_t Little_Endian_Arr[4] = {0x00,0x00,0xF7,0x42};//С��ģʽ
	// Func_ptr p = Get_Float;
	// p(Little_Endian_Arr);
	// printf("��ȡ�ĸ�����Ϊ:%.2f\r\n", u8_f.f);

	uint8_t heightBuffer[5] = {0x01, 0x00, 0x00, 0xF7, 0x42};
	uint8_t weightBuffer[5] = {0x02, 0x00, 0x00, 0xC8, 0x42};
	StudentData StuData;
	processReceivedData(StuData, &weightBuffer[0], sizeof(heightBuffer) / sizeof(heightBuffer[0]));
	#endif

	#if TEST_2024_07_26 > 0
	//������������?
	float f_data;
	ModBusTCPData MB_TCPData;
	/*һ��float���͵�����ͨ��ռ4�ֽ�(32λ).
	����IEEE 754��׼,float������1λ����λ,23λβ��λ(��Ч����),�Լ�8λָ��λ���?.
	*/

	printf("Please enter a floating-point number:");
	scanf("%f",&f_data);
	Sleep(1000);
	Get_Float(f_data, MB_TCPData.u8_f.u8Data);
	// printf("The input floating-point data is:%f\r\n",MB_TCPData.u8_f.f);
	if (((MB_TCPData.u8_f.f - f_data) > -EPS)&&((MB_TCPData.u8_f.f - f_data) < EPS))
	{
		printf("The input floating-point data is:%.3f\r\n",MB_TCPData.u8_f.f);
	}
	else
	{
		printf("Data Error.");
	}
	#endif

	#if TEST_2024_08_01 > 0
	// printf("%d\r\n",(0x01 << 8)|(0x03));
	printf("%#x\r\n", (0x20 & 0x01));
	// printf("%#x\r\n", (0x2A & 0x01));
	#endif

	#if TEST_2024_08_09 > 0
	uint8_t a = 0xA0;
	uint8_t b = 0xBD;
	uint16_t c = ((a << 8) | b);
	//printf("%#x\r\n", (a << 8) | b);
	// printf("%#x", c);
	float Torque = c*0.003f - 98.304f;
	printf("Torque = %.3f\r\n", Torque);
	int pwm_fre = (1 + Torque/100)*6000;
	printf("pwm_fre = %d\r\n", pwm_fre);
	printf("%f\r\n", fabsf(3.1415));
	#endif

	#if TEST_2024_08_10 > 0
	printf("%f\r\n",ceil(8888.21));
	printf("%f",round(8888.21));
	#endif
	
	#if TEST_2024_08_14 > 0
	uint16_t a = 10;
	uint16_t b = 1000;
	float e = (float)(10000*a)/(3*b);
	printf("e = %f\r\n", e);
	#endif

	#if TEST_2024_08_15 > 0
	printf("%#x",0x50 & ~0x10);
	#endif

	#if TEST_2024_08_20 > 0
	uint8_t arr[8] = {0x10,0x03,0x00,0x0A,0x12,0x34,0x00,0x00}; 
	uint16_t reg_addr = ((uint16_t)arr[2] << 8) + arr[3];
	// printf("%#x", reg_addr);
	uint16_t data = (arr[4] << 8) + arr[5];
	// printf("%d", data);
	uint16_t temp = 0x1234;
	arr[6] = (uint8_t)(temp >> 8);
	arr[7] = (uint8_t)temp;
	// printf("arr[6] = %#x,arr[7] = %#x",arr[6],arr[7]);

	union MyUnion
	{
		float f;
		uint8_t u8Data[4];
	}u8_f;

	u8_f.f = 123.5;
	for (int i = 0; i < 4; i++)
	{
		printf("u8_f.u8Data[%d] = %#x\r\n",i , u8_f.u8Data[i]);
	}
	//123.5转换成二进制位42 F7 00 00
















	#endif

	#if TEST_2024_08_22 > 0
	uint8_t tmod = 0x06;
	tmod &= ~(0x07) | (0 | 0x04);
	printf("%#x", tmod);
	#endif
		
	#if TEST_2024_08_23 > 0
	union MyUnion
	{
	float f;
	uint8_t u8Data[4];
	}u8_f;
	uint8_t getdata[4] = {0x3C,0xC4,0xA0,0x00};
	// uint8_t getdata[4] = {0x42,0x9E,0x6B,0x84};
	// uint8_t getdata[4] = {0x42,0x13,0x00,0x00};
	for (int i = 0; i < 4; i++)
	{
		u8_f.u8Data[i] = getdata[3 - i];
		printf("u8Data[%d] = %#x\t",i,u8_f.u8Data[i]);
	}
	printf("\r\nf = %.8f",u8_f.f);
	
	
	#endif

	#if TEST_2024_08_24 > 0
	uint16_t RegAddr = APP_TORCAL_B2_REG;
	uint32_t EE_ADD = 0x0200 + ((RegAddr - APP_TORCAL_K2_REG) << 4);
	printf("EE_ADD = %#x",EE_ADD);
	#endif

	#if TEST_2024_08_26 > 0
	int add_data = 0;
	uint8_t temp_arr[20] = {0x30,0x2A,0x89,0x3F,0xE0,0x2D,0x10,0xBF,0xB3,0x7B,0x82,0x3F,0x3C,0x4E,0x51,0x3E};
	CAL_FLOAT_DATA Tor_Cal_Data[4];
	
	/*
	单精度浮点数(float)转换为十六进制数的极限为:0x7F800000
	float k1 = 1.0716;//0x3F,0x89,0x2A,0x30
	float b1 = -0.5632;//0xBF,0x10,0x2D,0xE0
	float k2 = 1.0194;//0x3F,0x82,0x7B,0xB3
	float b2 = 0.2044;//0x3E,0x51,0x4E,0x3C

	未交准函数系数:
	float Original_k1 = 1.0//0x3F,0x80,0x00,0x00
	float Original_b1 = 0.0//0x00,0x00,0x00,0x00
	float Original_k2 = 1.0//0x3F,0x80,0x00,0x00
	float Original_b2 = 0.0//0x00,0x00,0x00,0x00
	*/
	//接下来实现利用for循环来给结构体中的数组赋值
	for (int i = 0; i < 4; i++)
	{
		for (int j = 0; j < 4; j++)
		{
			Tor_Cal_Data[i].u8data[j] = temp_arr[i*4 + j];
			/*
			0	0	0		1	0	4		2	0	8		3	0	12
			0	1	1		1	1	5		2	1	9		3	1	13
			0	2	2		1	2	6		2	2	10		3	2	14
			0	3	3		1	3	7		2	3	11		3	3	15
			*/
		}
	}
	// for (int m = 0; m < 4; m++)
	// {
	// 	printf("Tor_Cal_Data[%d].fdata = %.5f\r\n",m , Tor_Cal_Data[m].fdata);
	// }
	//对函数取地址
	memcpy(&add_data,add(3,4),1);
	printf("add_data = %d", add_data);
	#endif

	#if TEST_2024_08_27 > 0	
	/*
	float k1 = 1.0716;//0x3F,0x89,0x2A,0x30
	float b1 = -0.5632;//0xBF,0x10,0x2D,0xE0
	float k2 = 1.0194;//0x3F,0x82,0x7B,0xB3
	float b2 = 0.2044;//0x3E,0x51,0x4E,0x3C
	*/

	// uint8_t temp_arr[20] = {0x30,0x2A,0x89,0x3F,0xE0,0x2D,0x10,0xBF,0xB3,0x7B,0x82,0x3F,0x3C,0x4E,0x51,0x3E};
	// CAL_FLOAT_DATA Tor_Cal_Data[4];
	// CAL_FLOAT_DATA* Tor_Cal_Data_ptr = &Tor_Cal_Data[0];
	// for (int i = 0; i < 4; i++)
	// {
	// 	memcpy((Tor_Cal_Data_ptr+i),&temp_arr[i*4],4);
	// }
	// for (int j = 0; j < 4; j++)
	// {
	// 	printf("%.5f\r\n",(Tor_Cal_Data_ptr + j)->fdata);
	// }


	CAL_TORQUE_DATA TORQUE[2];
	// uint8_t* TORQUE_u8data_ptr = TORQUE.tor_u8data;
	// float* TORQUE_tor_fdata = TORQUE.tor_fdata;
	CAL_TORQUE_DATA* TORQUE_ptr = &TORQUE[0];
	float tmpdata1 = 123.5;
	float *tmpdata_ptr = &tmpdata1;
	float tmpdata2 = 10.5;
	uint8_t temp_arr1[20] = {0x30,0x2A,0x89,0x3F,0xE0,0x2D,0x10,0xBF,0xB3,0x7B,0x82,0x3F,0x3C,0x4E,0x51,0x3E};
	uint8_t temp_arr2[20] = {0x00,0x00,0x80,0x3F,0x00,0x00,0x00,0x00,0x00,0x00,0x80,0x3F,0x00,0x00,0x00,0x00};
	
	// for (int i = 0; i < 4; i++)
	// {
	// 	memcpy((TORQUE_u8data_ptr+i*4),(&temp_arr[i*4]),4);
	// 	// printf("TORQUE.tor_fdata[%d] = %.5f\r\n",i,TORQUE.tor_fdata[i]);
	// }
	// memcpy(TORQUE_ptr->tor_u8data,&tmpdata,4);
	// for (int i = 0; i < 4; i++)
	// {
	// 	printf("%#x\r\n",*(TORQUE_ptr->tor_u8data + i));
	// }
	// printf("%.5f",*(TORQUE_ptr->tor_fdata));
	// printf("%d\r\n",4*sizeof(uint8_t));
	// printf("%d",sizeof(*tmpdata_ptr));
	// for (int i = 0,j = 0; i < 4,j < 4; i++,j++)
	// {
	// 	memcpy((TORQUE_ptr->tor_u8data + i*4),(temp_arr1 + i*4),4);
	// 	memcpy(((TORQUE_ptr + 1)->tor_u8data + j*4),(temp_arr2 + j*4),4);
	// }
	// for (int m = 0,n = 0; m < 4,n < 4; m++,n++)
	// {
	// 	// printf("TORQUE[0].tor_fdata[%d] = %.5f\r\n",m , *(TORQUE_ptr->tor_fdata + m));
	// 	// printf("TORQUE[1].tor_fdata[%d] = %.5f\r\n",n , *((TORQUE_ptr + 1)->tor_fdata + n));
	// 	// printf("\r\n");
	// }

	*(TORQUE_ptr->tor_fdata + 1) = tmpdata1;
	*((TORQUE_ptr + 1)->tor_fdata) = tmpdata2;


	for (int i = 0; i < 4; i++)
	{
		printf("%#x\r\n",*(TORQUE_ptr->tor_u8data + 4 + i));
	}
	
	for (int j = 0; j < 4; j++)
	{
		printf("%#x\r\n",*((TORQUE_ptr+1)->tor_u8data + j));
	}
	
	// printf("%#x\r\n",*((TORQUE_ptr+1)->tor_u8data + 5));
	printf("%#x\r\n",(0x3E - 0x3A));
	#endif

	#if TEST_2024_08_30 > 0

	#endif

	#if TEST_2024_09_02 > 0
	#endif

	#if TEST_2024_09_03 > 0
		float temp = 100.5;
		ModBusDATA_U82F data = {{{0}}, {{0}}, {{0}}, {{0}}, {{0}}};
		ModBusDATA_U82F *pData = &data;
		printf("%d\r\n",pData[0]->fdata[0]);
		printf("%d",sizeof((*pData)[0].u8data));
		printf("%d",sizeof(data));
	#endif

	#if TEST_2024_09_05 > 0
	uint32_t a = 0x12345678;
	a = Swap32(a);
	printf("%08x",a);

	#endif

	#if TEST_2024_09_06 > 0
	T_CAL_PARA tCalPara[MAX_CAL_RANGE];
	printf("sizeof(ModBusData) = %d\r\n\r\n",sizeof(ModBusData));
	printf("ThresValue.u8data's address is:%d\r\n\r\n",&tCalPara[0].ThresValue.u8data[0]);
	printf("KValue.u8data's address is:%d\r\n\r\n",&tCalPara[0].KValue.u8data[0]);
	printf("BValue.u8data's address is:%d\r\n\r\n",&tCalPara[0].BValue.u8data[0]);
	#endif

	#if TEST_2024_09_10 > 0
	#endif

	#if TEST_2024_09_13 > 0
	//for循环嵌套switch语句
	uint8_t arr[6] = {1,2,3,4,5,6};
	for (int i = 0; i < 6; i++)
	{
		switch (arr[i])
		{
		case 1:
			printf("arr[0] = %d\r\n",arr[0]);
			continue;
			// break;
		case 2:
			printf("arr[1] = %d\r\n",arr[1]);
			continue;
			// break;
		case 3:
			printf("arr[2] = %d\r\n",arr[2]);
			continue;
			// break;
		case 4:
			printf("arr[3] = %d\r\n",arr[3]);
			continue;
			// break;
		case 5:
			printf("arr[4] = %d\r\n",arr[4]);
			continue;
			// break;
		default:
			printf("hello world\r\n");
			continue;
			// break;
		}
	}
	#endif

	#if TEST_2024_09_15 > 0
	int n;
	int flag = 10;
	do
	{
		Sleep(1);
		n++;
		flag -= 3;
	} while ((n < 10)&&(1 == flag));
	printf("n = %d, flag = %d\r\n",n ,flag);
	if ((n < 10) && (1 == flag))
	{
		printf("OK");
	}
	else
	{
		printf("Error");
	}

	#endif

	#if TEST_2024_09_18 > 0
	uint8_t arr[5] = {0x00,0x00,0x00,0x00,0x00};
	arr[2] = 0x33;
	for (int i = 0; i < sizeof(arr); i++)
	{
		printf("arr[%d] = %#x\r\n",i,arr[i]);
	}
	

	#endif

	#if TEST_2024_09_20 > 0
	uint8_t num = 0x00;
	do
	{
		printf("num = %#x\r\n",num);
		for (int i = 0; i < 8; i++)
		{
			printf("%d ",(num >> i)&0x01);
		}
		printf("\r\n");
	} while (num++ < 15);

	uint16_t IO_Index = (0x01 << 8) | 0x01;
	printf("IO_Index = %#x\r\n", IO_Index);
	for (int i = 0; i < 16; i++)
	{
		printf("%d ",(IO_Index >> i)&0x01);
		if (i == 7)
		{
			printf("\r\n");
		}
	}

	printf("%d ",1 & 0x01);
	#endif

	#if TEST_2024_09_22 > 0
    // ModbusRTU_01_Process(&rxBuffer[0], rxPoint);

	uint8_t pModbus_In_Data[] = {0x0F,0x00,0x14,0x00,0x0A,0x02,0xFF,0x03};
	uint16_t reg = (pModbus_In_Data[MODBUSTCP_START_REG_H_Pos] << 8) | pModbus_In_Data[MODBUSTCP_START_REG_L_Pos];
	uint16_t regLen = (pModbus_In_Data[MODBUSTCP_REG_LEN_H_Pos] << 8) | pModbus_In_Data[MODBUSTCP_REG_LEN_L_Pos];
	uint8_t	DataNum = pModbus_In_Data[MODBUSTCP_SETDATA_NUM_Pos];

	uint8_t i = 0;
	uint8_t  bitIndex = 0;
	uint8_t TempValue = 0;

	for(i = 0; i < regLen; i++)
	{
		bitIndex = i % 8;
		printf("bitIndex = %d ",bitIndex);
		TempValue = (pModbus_In_Data[MODBUSTCP_START_DATA_Pos + i/8] & (0x01<<bitIndex)) >> bitIndex;
		printf("TempValue(%d) = %d\r\n",(reg + i),TempValue);
	}







	#endif

	#if TEST_2024_09_23 > 0
	int bits[] = {0, 0, 1, 0, 1, 0, 0, 0};
	unsigned char result = 0;
    for (int i = 0; i < 8; i++) {
        result |= (bits[i] << i);  // 左移对应的位数并进行或操作
    }
	printf("Result: %#x\r\n", result);
	#endif

	#if TEST_2024_09_29 > 0
	#if 0
		int a;
		printf("Input:");
		while (scanf("%d",&a) == 1)
		{
			if (a > 0)
			{
				printf("a = %d\r\n",a);
				if ((a % 2) == 0)
				{
					a -= 1;
				}
				printf("Odd Numbers:");
				for (int i = 1; i <= a; i++)
				{
					if ((i % 2) == 0)
					{
						// printf("Space");
						printf(" ");
					}
					else
					{
						printf("%d",i);
					}
				}
			}
			else
			{
				printf("data error.");
			}
			printf("\r\n\r\n");
			printf("Input:");
		}
	#endif

	#if 0
	int a;//因为变量a需要手动输入,这里不需要初始化赋值
	int count = 1;//相当于for循环里的变量i,从1开始计数
	printf("Input:");
	while (scanf("%d",&a) == 1)
	{
		if (a > 0)
		{
			printf("a = %d\r\n",a);
			if ((a % 2) == 0)
			{
				a -= 1;
			}
			printf("Odd Numbers:");
			while (count <= a)
			{
				if ((count % 2) == 0)
				{
					// printf("Space");
					printf(" ");
				}
				else
				{
					printf("%d",count);
				}
				count++;
			}
			printf("\r\ncount = %d",count);
			/*
			  执行完"while (count <= a)"循环后,count的值实际上就等于a,
			  注意这里"a -= 1",scanf输入的a值并不一定是while循环判断里的a
			  如果想重新输出下一组奇数需要将count复位,即重新赋值count = 1;
		    */
			count = 1;
		}
		else
		{
			printf("data error.");
		}
		printf("\r\n\r\n");
		printf("Input:");
	}
	#endif
	
	#if 0
	int a;//因为变量a需要手动输入,这里不需要初始化赋值
	int count = 1;//相当于for循环里的变量i,从1开始计数
	printf("Input:");
	while (scanf("%d",&a) == 1)
	{
		if (a > 0)
		{
			printf("a = %d\r\n",a);
			if ((a % 2) == 0)
			{
				a -= 1;
			}
			printf("Odd Numbers:");
			do
			{
				if ((count % 2) == 0)
				{
					// printf("Space");
					printf(" ");
				}
				else
				{
					printf("%d",count);
				}
				count++;
			} while (count <= a);
			printf("\r\ncount = %d",count);
			count = 1;
		}
		else
		{
			printf("data error.");
		}
		printf("\r\n\r\n");
		printf("Input:");
	}
	#endif
	
	#endif

	#if TEST_2024_09_30 > 0
	uint8_t u8data[10][2];
	uint8_t arr[10] = {0x41,0x31,0x30,0x33,0x00,0x00};
	memcpy(&u8data[4][0],&arr[0],6);
	for (int i = 0; i < 3; i++)
	{
		printf("%#x, ",u8data[4+i][0]);
		printf("%#x\r\n",u8data[4+i][1]);
	}
	#endif

	#if TEST_2024_10_03 > 0

	int tmpdata1 = 0;
	int tmpdata2 = 0;
	// for (int i = 0; i < 8; i++)//i相当于LEDNum
	// {
	// 	tmpdata1 = (0x01 << i);
	// 	// tmpdata2 = ~(0x01 << i);
	// 	// P2 = ~(0x01 << 8)
	// 	// 0001 0000 0000
	// 	printf("tmpdata1_%d = %#x\r\n",i,tmpdata1);
	// }


	/*
	(i = 0)
	tmpdata1 = (0x01 << 0) = 0000 0001
	(i = 1)
	tmpdata1 = (0x01 << 1) = 0000 0010
	(i = 2)
	tmpdata1 = (0x01 << 2) = 0000 0100
	(3)
	(4)
	(5)
	(6)
	(i = 7)
	tmpdata1 = (0x01 << 7) = 1000 0000
	*/
	

	printf("int size = %d\r\n",sizeof(int));
	printf("float size = %d\r\n",sizeof(float));
	printf("double size = %d\r\n",sizeof(double));

	#endif

	#if TEST_2024_10_08 > 0
	//对DIDO板卡的源程序解读
	/*  1.STM32单片机数据存储方式--小端存储  */
	if (0)
	{
		uint32_t tempdata = 0x12345678;
		memcpy(&aucModbusData[4][0], (const uint8_t *)&DefaultBootPara, sizeof(T_BOOT_PARA));
		printf("sizeof(T_BOOT_PARA) = %d\r\n",sizeof(T_BOOT_PARA));
		for (int i = 4; i < 20; i++)
		{
			printf("auc[%d][0] = %#x, ",i,aucModbusData[i][0]);
			printf("auc[%d][1] = %#x\r\n",i,aucModbusData[i][1]);
		}
		printf("\r\n");
		for (int j = 0; j < 4; j++)
		{
			//基地址存放低位数据的这种方式叫做小端模式
			printf("add_%d = %d, ",j,((uint8_t *)(&tempdata) + j));
			printf("value_%d = %#x\r\n",j,*((uint8_t *)(&tempdata) + j));//VSCode中数据的存储方式:小端存储
		}
	}
	/*  2.C语言中结构体强制类型转换  */
	//(1)结构体对齐问题
	if (0)
	{
		test1_str A;
		test2_str B;
		test3_str C;
		//部分数据类型所占字节大小
		printf("sizeof(int) = %d\r\n",sizeof(int));
		printf("sizeof(float) = %d\r\n",sizeof(float));
		printf("sizeof(char) = %d\r\n",sizeof(char));
		printf("sizeof(short) = %d\r\n",sizeof(short));
		printf("\r\n");
		printf("sizeof(A) = %d\r\n",sizeof(A));//sizeof(A) = 12
		printf("str_A_add = %d\r\n",&A);//地址为6422032
		printf("str_A_a_add = %d\r\n",&A.a);//地址为6422032
		printf("str_A_b_add = %d\r\n",&A.b);//地址为6422034
		printf("str_A_c_add = %d\r\n",&A.c);//地址为6422036
		printf("str_A_d_add = %d\r\n",&A.d);//地址为6422040
		printf("\r\n");
		printf("sizeof(B) = %d\r\n",sizeof(B));//sizeof(B) = 8
		printf("str_B_add = %d\r\n",&B);//地址为6422024
		printf("str_B_a_add = %d\r\n",&B.a);//地址为6422024
		printf("str_B_b_add = %d\r\n",&B.b);//地址为6422028
		printf("\r\n");
		printf("sizeof(C) = %d\r\n",sizeof(C));//sizeof(C) = 4
		printf("str_C_add = %d\r\n",&C);//地址为6422020
		printf("str_C_a_add = %d\r\n",&C.a);//地址为6422020
	}
	#endif

	#if TEST_2024_10_09 > 0
	if (1)
	{
		uint32_t tempdata = 0x0001C200;
		printf("\r\n");
		printf("tempdata = %d\r\n",tempdata);
		for (int i = 0; i < 4; i++)
		{
			//基地址存放低位数据的这种方式叫做小端模式
			printf("add_%d = %d, ",i,((uint8_t *)(&tempdata) + i));
			printf("value_%d = %#x\r\n",i,*((uint8_t *)(&tempdata) + i));//VSCode中数据的存储方式:小端存储
		}
	}
	#endif

	#if TEST_2024_10_15 > 0

	#endif



	
}

