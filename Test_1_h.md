#ifndef _TEST_1_H_
#define _TEST_1_H_
#ifdef __cplusplus
extern "C" {
#endif

/* USER CODE BEGIN Includes(头文件包含)*/

/* USER CODE END Includes */

/* USER CODE BEGIN PD(定义宏)*/
#define TEST_2024_07_16 0
#define TEST_2024_07_17 0
#define TEST_2024_07_23 0
#define TEST_2024_07_24 0
#define TEST_2024_07_25 1

/* USER CODE END PD */

/* USER CODE BEGIN PTD(定义结构体,枚举体,共用体等)*/
typedef unsigned char           uint8_t;
typedef unsigned short int      uint16_t;
typedef unsigned int            uint32_t;

#if TEST_2024_07_17 > 0
//先定义一个学生信息结构体
typedef struct Info
{
    int grade;
    int age;
    char name[20];
    float height;
    float weight;
}StudentInfo;

/*
  将结构体指针"StudentInfo*"定义为"StudentInfo_Handle"类型，
  StudentInfo_Handle则成为了一个指向StudentInfo结构体的指针类型的别名
*/
typedef StudentInfo* StudentInfo_Handle;

#endif

#if TEST_2024_07_23 > 0
//定义一个枚举类型
typedef enum NET_Mode
{
  NETINFO_STATIC = 1,
  NETINFO_DHCP
}DHCP_Mode;

/*
定义一个结构体数组
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
// 声明一个指向同样参数、返回值的函数指针类型(两种不同的定义方式)
// typedef void(*Func_ptr)(uint8_t* a, uint32_t b);
// void(*Func_ptr)(uint8_t* a, uint32_t b);

typedef struct SData
{
  //数据类型标识位(身高,体重)
  enum datatype
  {
    Stu_Height = 1,
    Stu_Weight = 2
  }DataType;

  //声明一个联合体
  union MyUnion
  {
    float f;
    uint8_t u8Data[4];
  }u8_f;

  //定义一个函数指针
  // void(*Func_ptr)(uint8_t* a);

}StudentData;

#endif

/* USER CODE END PTD */

/* USER CODE BEGIN PV(定义变量) */

/* USER CODE END PV */

/* USER CODE BEGIN PFP(存放函数原型)*/
#if TEST_2024_07_25 > 0
void Get_32bit_num(uint8_t* Arr_ptr, uint32_t x);
void Get_Float(uint8_t* RecBuf_ptr, uint8_t* Arr_ptr, uint16_t length);
#endif

/* USER CODE END PFP */

#ifdef __cplusplus
}
#endif

#endif