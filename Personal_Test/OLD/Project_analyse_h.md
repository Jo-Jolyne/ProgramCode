#include <stdio.h>

typedef unsigned           char uint8_t;
typedef unsigned short     int uint16_t;
typedef unsigned           int uint32_t;

#define APP_MAX_REG_ADD   300//���Ĵ�����ַ

#define CONNECT     0   //��ʾ����Ϊ����
#define LINK        0x01//��ʾ�������ӳɹ�

#define LINK_LOSE_COUNT 	5000	//5s=5000ms,��·����ʱ��

/* Operation mode bits */
#define VDM		0x00//�ɱ䳤������ģʽ
/*�̶���������ģʽ*/
#define FDM1	0x01//FDMģʽ�µ�1�ֽ�����д��SPI֡
#define	FDM2	0x02//FDMģʽ�µ�2�ֽ�����д��SPI֡
#define FDM4	0x03//FDMģʽ�µ�4�ֽ�����д��SPI֡

/* Read_Write control bit */
#define RWB_READ	0x00//
#define RWB_WRITE	0x04//

/* Block select bits */
#define COMMON_R	0x00//

#define GAR		0x0001

#define DEV_ADDR_REG   0x04

//for BootLoader Para
#define START_BOOTLOADER 0x5A	
#define START_APP        0xA5

#define UPDATA_2_UART    0xA3


typedef struct _eth_para{
	volatile uint8_t Eth_Rst_Flag;			//���縴λ��ʶ
	volatile uint8_t Link_Status;			//�������ӱ�ʶ
	volatile uint16_t Link_Count;			//�������Ӽ���
}T_ETH_PARA;

typedef struct _bootpara{
  uint8_t UartADDR;
  uint8_t RSV1;//����λ1
  uint8_t IP1;
  uint8_t IP2;
  uint8_t IP3;
  uint8_t IP4;
  uint8_t PortH;
  uint8_t PortL;
  uint8_t RSV2;//����λ2
  uint8_t RSV3;//����λ3
  uint8_t RunMode;
  uint8_t ID;
  uint8_t UpdateType;
  uint8_t RSV4;//����λ4
  uint8_t EraseFlagH;
  uint8_t EraseFlagL;
  uint32_t CheckFlashLen;
  uint32_t RSV5;
}T_BOOT_PARA;




















