#ifndef _TEST_1_H_
#define _TEST_1_H_
#ifdef __cplusplus
extern "C" {
#endif

/* USER CODE BEGIN Includes(ͷ�ļ�����)*/
#include<math.h>
#include <stdio.h>
#include <string.h>
#include <windows.h>
/* USER CODE END Includes */

/* USER CODE BEGIN PD(�����)*/
#define TEST_2024_07_16 0
#define TEST_2024_07_17 0
#define TEST_2024_07_23 0
#define TEST_2024_07_24 0
#define TEST_2024_07_25 0
#define TEST_2024_07_26 0
#define TEST_2024_08_01 0
#define TEST_2024_08_09 0
#define TEST_2024_08_10 0
#define TEST_2024_08_14 0
#define TEST_2024_08_15 0
#define TEST_2024_08_20 0
#define TEST_2024_08_22 0
#define TEST_2024_08_23 0
#define TEST_2024_08_24 0
#define TEST_2024_08_26 0
#define TEST_2024_08_27 0
#define TEST_2024_08_30 0
#define TEST_2024_09_02 0
#define TEST_2024_09_03 0
#define TEST_2024_09_05 0
#define TEST_2024_09_06 0
#define TEST_2024_09_10 0
#define TEST_2024_09_13 0
#define TEST_2024_09_15 0
#define TEST_2024_09_18 0
#define TEST_2024_09_20 0
#define TEST_2024_09_22 0
#define TEST_2024_09_23 0
#define TEST_2024_09_29 0
#define TEST_2024_09_30 0
#define TEST_2024_10_03 0
#define TEST_2024_10_08 0
#define TEST_2024_10_09 0
#define TEST_2024_10_15 1

#if TEST_2024_07_26 > 0
#define REC_DATA_LENGTH 4
//�жϸ������Ƿ�λ��0��һ����С��������[-EPS,EPS]��
#define EPS 1e-7
#endif

#if TEST_2024_08_01 > 0
#define DATA_LENGTH 4
#endif

#if TEST_2024_08_24 > 0
#define APP_TORCAL_K1_REG 0x20
#define APP_TORCAL_B1_REG 0x22
#define APP_TORCAL_K2_REG 0x24
#define APP_TORCAL_B2_REG 0x26
#define APP_TORCAL_K3_REG 0x28
#define APP_TORCAL_B3_REG 0x2A
#endif

#if TEST_2024_09_05 > 0
#define Swap32(A) ((((uint32_t)(A) & 0xff000000) >> 24) | \
				           (((uint32_t)(A) & 0x00ff0000) >>  8) | \
				           (((uint32_t)(A) & 0x0000ff00) <<  8) | \
				           (((uint32_t)(A) & 0x000000ff) << 24))
#endif

#if TEST_2024_09_06 > 0
#define MAX_CAL_RANGE 4
#endif

#if TEST_2024_09_22 > 0
//RTU
#define APP_MAX_REG_ADD         0x30//48
#define	MODBUSRTU_ADD_Pos	      0
#define MODBUSRTU_FUN_Pos	      1
#define MODBUSRTU_REG_START_Pos	2
#define MODBUSRTU_SINGL_SET_DATA_Pos  4
#define MODBUSRTU_REG_COUNT_Pos 4
#define MODBUSRTU_SET_BYTE_Pos  6
#define MODBUSRTU_MULTI_SET_DATA_Pos  7
#define MODBUSRTU_ACK_BYTE_Pos  2
#define MODBUSRTU_ACK_DATA_Pos  3
#define MODBUSRTU_ERR_Pos 2

//TCP
#define   MODBUSTCP_FUNC_Pos   0
#define   MODBUSTCP_START_REG_H_Pos  1
#define   MODBUSTCP_START_REG_L_Pos  2
#define   MODBUSTCP_REG_LEN_H_Pos    3
#define   MODBUSTCP_REG_LEN_L_Pos    4
#define   MODBUSTCP_SETDATA_NUM_Pos  5
#define   MODBUSTCP_START_DATA_Pos   6

#define   MODBUSTCP_DATA_LEN_Pos     1
#define   MODBUSTCP_DATA_START_Pos   2

#define   MODBUSTCP_SET_SINGLE_DATA_H_Pos    3
#define   MODBUSTCP_SET_SINGLE_DATA_L_Pos    4

#endif

/* USER CODE END PD */

/* USER CODE BEGIN PTD(����ṹ��,ö����,�������)*/
/*
#if TEST_2024_xx_xx > 0
#endif
*/
typedef unsigned char           uint8_t;
typedef unsigned short int      uint16_t;
typedef unsigned int            uint32_t;

#if TEST_2024_07_17 > 0
//�ȶ���һ��ѧ����Ϣ�ṹ��
typedef struct Info
{
    int grade;
    int age;
    char name[20];
    float height;
    float weight;
}StudentInfo;

/*
  ���ṹ��ָ��"StudentInfo*"����Ϊ"StudentInfo_Handle"���ͣ�
  StudentInfo_Handle���Ϊ��һ��ָ��StudentInfo�ṹ���ָ�����͵ı���
*/
typedef StudentInfo* StudentInfo_Handle;

#endif

#if TEST_2024_07_23 > 0
//����һ��ö������
typedef enum NET_Mode
{
  NETINFO_STATIC = 1,
  NETINFO_DHCP
}DHCP_Mode;

/*
����һ���ṹ������
*/
typedef struct NETInfo
{
  uint8_t W5500_MAC[6];
  uint8_t W5500_IP[4];
  uint8_t W5500_MASK[4];
  uint8_t W5500_GW[4];
  uint8_t W5500_DNS[4];
  DHCP_Mode DHCP;
}W5500_NET_INFO;

typedef W5500_NET_INFO* W5500_NET_INFO_HANDLE;
#endif

#if TEST_2024_07_24 > 0
#endif

#if TEST_2024_07_25 > 0
// ����һ��ָ��ͬ������������ֵ�ĺ���ָ������(���ֲ�ͬ�Ķ��巽ʽ)
// typedef void(*Func_ptr)(uint8_t* a, uint32_t b);
// void(*Func_ptr)(uint8_t* a, uint32_t b);

typedef struct SData
{
  //�������ͱ�ʶλ(����,����)
  enum datatype
  {
    Stu_Height = 1,
    Stu_Weight = 2
  }DataType;

  //����һ��������
  union MyUnion
  {
    float f;
    uint8_t u8Data[4];
  }u8_f;

  //����һ������ָ��
  // void(*Func_ptr)(uint8_t* a);

}StudentData;

#endif

#if TEST_2024_07_26 > 0
typedef struct Data
{
  //����һ��������
  union MyUnion
  {
    float f;
    uint8_t u8Data[4];
  }u8_f;

  //����һ������ָ��
  // void(*Func_ptr)(uint8_t* a);

}ModBusTCPData;

#endif

#if TEST_2024_08_26 > 0
//定义一个联合体,用来存储扭矩值校准函数的系数
typedef union Cal_Data
{
	float fdata;
	uint8_t u8data[4];
}CAL_FLOAT_DATA;

#endif

#if TEST_2024_08_27 > 0
typedef union Cal_Data
{
	float fdata;
	uint8_t u8data[4];
}CAL_FLOAT_DATA;

typedef union Cal_Tor_Data
{
	float tor_fdata[4];
	uint8_t tor_u8data[16];
}CAL_TORQUE_DATA;









#endif

#if TEST_2024_08_30 > 0
//结构体中嵌套联合体
typedef union CAN_DATA
{
  float fdata[3];
  uint8_t u8data[12];
}CANDATA_U82F;


typedef union CAL_DATA
{
  float fdata[20];
  uint8_t u8data[80];
}CALDATA_U82F[3];


typedef struct ModBusRTU_DATA
{
  CANDATA_U82F ModBusRTU_CANDATA_U82F;
  CALDATA_U82F ModBusRTU_CALDATA_U82F;
}ModBusRTU_U82F;

ModBusRTU_U82F ModBusRTU_DATA_U82F;
ModBusRTU_U82F *ModBusRTU_DATA_U82F_PTR = &ModBusRTU_DATA_U82F;


#endif

#if TEST_2024_09_03 > 0
  typedef union ModBusDATA
  {
    float fdata[3];
    uint8_t u8data[12];
  }ModBusDATA_U82F[5];
#endif

#if TEST_2024_09_05 > 0
//联合体大端转小端测试
typedef union ModBusDATA
{
  float fdata;
  uint32_t u32data;
  uint8_t u8data[4];
}ModBusDATA_U82F;

#endif

#if TEST_2024_09_06 > 0
typedef union modbusrtu_data
{
  float   fdata;
  uint32_t  u32data;
  uint32_t   u8data[4];
}ModBusData;

typedef struct cal_data
{
  ModBusData ThresValue;
  ModBusData KValue;
  ModBusData BValue;
}T_CAL_PARA;


#endif

#if TEST_2024_09_22 > 0
typedef enum
{
  MODBUS_OK       = 0x00,
  MODBUS_FUN_ERR  = 0x01,   
  MODBUS_ADD_ERR  = 0x02,   
  MODBUS_DAT_ERR  = 0x03,   
  MODBUS_DEV_ERR  = 0x04,   
}E_MODBUS_ERRORCODE;


#endif

#if TEST_2024_10_08 > 0
/*  1.STM32单片机数据存储方式--小端存储  */
typedef struct _bootpara{
  uint8_t UartADDR;
  uint8_t RSV1;
  uint8_t IP1;
  uint8_t IP2;
  uint8_t IP3;
  uint8_t IP4;
  uint8_t PortH;
  uint8_t PortL;
  uint8_t RSV2;
  uint8_t RSV3;
  uint8_t RunMode;
  uint8_t ID;
  uint8_t UpdateType;
  uint8_t RSV4;
  uint8_t EraseFlagH;
  uint8_t EraseFlagL;
  uint32_t CheckFlashLen;
  uint32_t RSV5;
  uint8_t UartStopBits;
  uint8_t RSV6;
  uint8_t UartParity;
  uint8_t RSV7;
  uint32_t UartBaudRate;
}T_BOOT_PARA;
/*  2.C语言中结构体强制类型转换  */
//(1)结构体对齐问题
typedef struct str1
{
  char a;
  short b;
  int c;
  float d;
}test1_str;
typedef struct str2
{
  char a;
  int b;
}test2_str;
typedef struct str3
{
  short a;
}test3_str;
#endif

#if TEST_2024_10_09 > 0
#endif

extern int data;

/* USER CODE END PTD */

/* USER CODE BEGIN PV(�������) */

/* USER CODE END PV */

/* USER CODE BEGIN PFP(��ź���ԭ��)*/
#if TEST_2024_07_25 > 0
void Get_32bit_num(uint8_t* Arr_ptr, uint32_t x);
void Get_Float(uint8_t* RecBuf_ptr, uint8_t* Arr_ptr, uint16_t length);
#endif

/* USER CODE END PFP */

#ifdef __cplusplus
}
#endif

#endif