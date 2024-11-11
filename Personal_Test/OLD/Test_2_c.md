#include "Test_2.h"


#define Demo_2024_07_02 0
#define Demo_2024_07_03 0
#define Demo_2024_07_04 0
#define Demo_2024_07_11 0
#define Demo_2024_07_16 0


#if Demo_2024_07_02
	int x1_relay_num = -1;
	int x2_relay_num = -2;
#endif

#if Demo_2024_07_04
int read_value(int *pdata1,int *pdata2,int i)
{
	*(pdata1) = *(pdata2+i);
	return *(pdata1);
}
#endif

#if Demo_2024_07_11
/*ÿ������һ������,�����ڳ����а��������в���.�����Ϳ��԰Ѽ�����ͬ�Ķ�����һ���ֽڵĶ�����λ������ʾ.*/

/*C����--λ��(һ�����ݽṹ):
 *��Щ��Ϣ�ڴ洢ʱ,������Ҫռ��һ���������ֽ�,��ֻ��ռ������һ��������λ.
  �����ڴ��һ��������ʱ,ֻ��0��1����״̬,��һλ����λ����.
  Ϊ�˽�ʡ�洢�ռ�,��ʹ�������,C�������ṩ��һ�����ݽṹ,��Ϊ��λ�򡱻�λ�Ρ�.
  ��ν��λ���ǰ�һ���ֽ��еĶ���λ����Ϊ������ͬ������,��˵��ÿ�������λ��.
 */

/*�Խṹ��λ���˵��:
 *1.λ�������˵����ṹ����˵���ķ�ʽ��ͬ.�ɲ����ȶ����˵����ͬʱ����˵������ֱ��˵�������ַ�ʽ.

  eg1:˵��bitValue1ΪBitField1��������ռ1���ֽڡ�λ��bit0~bit7��ռ1λ��

 *2.һ��λ�����洢��ͬһ���ֽ��У����ܿ������ֽڡ���һ���ֽ���ʣ�ռ䲻�������һλ��ʱ��
  	Ӧ����һ��Ԫ���Ÿ�λ��Ҳ��������ʹĳλ�����һ��Ԫ��ʼ��

  eg2:˵��bitValue2ΪBitField2��������ռ2���ֽڡ��ڵ�һ���ֽ��У�λ��aռ7λ��λ��b,c�޷�����ڵ�һ�ֽ���
      ��ʹ��"int  :0;"��ʾ֮���λ�����ڵڶ����ֽ��С���BitField2���λ�����У�aռ��һ�ֽڵ�7λ��
	  ��1λ��0��ʾ��ʹ�ã�b�ӵڶ��ֽڿ�ʼ��ռ��4λ��cռ��4λ��

 *3.λ��ĳ��Ȳ��ܴ���һ��int�ĳ��ȣ�Ҳ����˵���ܳ���32λ��
 * 
 * 
 */
//eg1:
typedef struct BitField1
{
  unsigned char bit0:1;//����1����Ը�","��";"���ǺϷ���.
  unsigned char bit1:1;
  unsigned char bit2:1;
  unsigned char bit3:1;
  unsigned char bit4:1;
  unsigned char bit5:1;
  unsigned char bit6:1;
  unsigned char bit7:1;
}bitValue1;

//eg2:
typedef struct BitField2
{
  int a:7;
  int  :0;
  int b:4;
  int c:4;
}bitValue2;

#endif

#if Demo_2024_07_16
/*
1.����ָ���ָ�뺯��:
  (1)ָ�뺯��:
	 (i)����:����һ������ָ��ĺ������䱾����һ�����������ú�����"����ֵ"��һ��ָ��;
	 (ii)������ʽ:
		a.��ͨ���������Ͷ����ʽ:"�������� + ��������������".��:
		  int fun(int x)
		  {
		  	//��������
			return 0;
		  }
		b.ָ�뺯�������Ͷ����ʽ:"ָ������ + ��������������".��:
		  int* fun(int x,int y)
		  {
		  	//��������
			return 0;
		  }
		c.���䷽������Ϊ��������һ����������д�������Ͼ�����ָ�������������ͨ���������е����;���.
  (2)����ָ��:
     (i)����:ָ������ָ��,������ָ��,ָ�����һ�������ĵ�ַ
	 (ii)������ʽ:������������ + (*������) + (����).ע��:ע��"*"�ͺ�����Ҫ��������������������Ϊ����������ȼ�ԭ��ͱ��ָ�뺯����.��:
		"int (*fun)(int,int);" ���� "int (*fun)(int x,int y);"
		()()()()()()()()
*/


//ָ�뺯����ʹ��:
//����һ��ָ�뺯��,����һ������ָ��
void (* getFunc(void))(void);

//����func����
void func(void)
{
	printf("Hello World\r\n");
}

// ����ָ�뺯�� getFunc������һ������ָ��
void (*getFunc(void))(void) {
    // ֱ�ӷ��� func �����ĵ�ַ
    return &func;
}

//����ָ���ʹ��:
//������һ������ָ��:
void (* fun)(void);

/*
2.ʹ�ýṹ�������Լ�����ָ�����Ż�STM32F407���ִ���
  (1)����ָ�������:����ָ���������C������һ�ַǳ����õ����ݽṹ��������һ������.
	 ��������洢�������ָ�룬���ҿ���ͨ��������������Щ������
  (2)������ʽ: ����ָ������ + ������ + ����Ԫ��.��:
	 // ����һ������ָ������
	 typedef void (*func_ptr)(void);

	 // һ��������������������
	 func_ptr func_array[] = {func1, func2};
*/

int FlagArray[6] = {1,0,1,0,0,1};

typedef enum {
 TGChannel1,
 TGChannel2,
 TGChannel3,
 TGChannel4,
 TGChannel5,
 TGChannelNum
}TGNUM;

typedef struct num
{
	int x;
 	int y;
}CTRL_NUM;

CTRL_NUM Target_num[] = 
{
	{2,3},
	{3,4},
	{3,9},
	{9,5},
	{35,48},
	{31,45}
};

void add(int x, int y)
{
	printf("%d\r\n",(x + y));
}

void minus(int x, int y)
{
	printf("%d\r\n",(x - y));
}

void (*OperationNum[])(int x, int y) = {add, minus};

void Ctrl_Single_num(CTRL_NUM targetnum,int flag)
{
	OperationNum[flag](targetnum.x,targetnum.y);
}


void Ctrl_All_num(CTRL_NUM *targetnum,int *flag)
{
	TGNUM TargetNum;
	for (TargetNum = TGChannel1; TargetNum < TGChannelNum; TargetNum++)
	{
		Ctrl_Single_num(targetnum[TargetNum],flag[TargetNum]);
	}
}

#endif



void main()
{
	#if Demo_2024_07_02
	int a,b;
	printf("x1_relay_num = %d\tx2_relay_num = %d\r\n",x1_relay_num,x2_relay_num);
	scanf("%d%d",&a,&b);
	x1_relay_num = a;
	x2_relay_num = b;
	if (x1_relay_num == x2_relay_num)
	{
		printf("scanf is ok.\r\n");
		printf("x1_relay_num = %d\r\nx2_relay_num = %d\r\n",x1_relay_num,x2_relay_num);
	}
	else
	{
		printf("scanf is not ok.\r\n");
		printf("x1_relay_num = %d\r\nx2_relay_num = %d\r\n",x1_relay_num,x2_relay_num);
	}
	#endif

	#if Demo_2024_07_03
	int a = 0;
	a |= (0 << 0);
	a |= (1 << 1);
	a |= (0 << 2);
	a |= (1 << 3);
	printf("%#x",a);
	#endif

	#if Demo_2024_07_04
	//��ȡ���������еľ���ĳһλ��ֵ
    int a = 0x10;
	int bit_value = (a >> (5 - 1)) & 1;
	printf("%d",bit_value);



	int array1[5] = {1,1,1,1,1};
	int array2[5] = {0};
	int tmp = 0;
	//int data = read_value(array2,array1,2);
	// printf("%d",data);
	for (int i = 0; i < 5; i++)
	{
		array2[0] |= (read_value(&tmp,&array1[0],i) << i);
		// printf("%#x\t",array2[0]);
	}
	printf("\r\n");
	for (int i = 0; i < 5; i++)
	{
		array2[1] |= (1 << i);
		// printf("%#x\t",array2[1]);
	}
	#endif

	#if Demo_2024_07_11
	//������Ƕ�׽ṹ��
	printf( "Memory size occupied by bitValue2 : %d\n", sizeof(bitValue2));//8���ֽ�

	#endif

	#if Demo_2024_07_16
	/*
	1.����ָ��:
	2.ʹ�ýṹ�������Լ�����ָ�����Ż�STM32F407���ִ���
	*/

	//ָ�뺯����ʹ��
	// ����һ������ָ�������ͨ������ָ�뺯����ȡ func �����ĵ�ַ
    void (*funcPtr)(void) = getFunc();

    // ʹ�ú���ָ����� func ����
    funcPtr();


	//����ָ���ʹ��:
	fun = func;
	fun();

	Ctrl_All_num(Target_num,FlagArray);
	#endif









}












