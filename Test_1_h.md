#ifndef _TEST_1_H_
#define _TEST_1_H_
#ifdef __cplusplus
extern "C" {
#endif

/* USER CODE BEGIN Includes(ͷ�ļ�����)*/

/* USER CODE END Includes */

/* USER CODE BEGIN PD(�����)*/
#define TEST_2024_07_16 0
#define TEST_2024_07_17 0
#define TEST_2024_07_23 0
#define TEST_2024_07_24 0
#define TEST_2024_07_25 1

/* USER CODE END PD */

/* USER CODE BEGIN PTD(����ṹ��,ö����,�������)*/
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
  //�������ͱ�ʶλ(���,����)
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