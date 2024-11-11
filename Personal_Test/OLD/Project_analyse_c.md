#include <Project_analyse.h>




const T_BOOT_PARA DefaultBootPara = {
	.UartADDR = 1,//���ڵ�ַ
	.IP1 = 192,
	.IP2 = 168,
	.IP3 = 2,
	.IP4 = 199,
	.PortH = 0x13,
	.PortL = 0x88,
	.RunMode = START_APP,
	.ID		= 0,
	.UpdateType = UPDATA_2_UART,
	.EraseFlagH = 0,
	.EraseFlagL = 0,
	.CheckFlashLen = 0,
	.RSV1 = 0,
	.RSV2 = 0,
	.RSV3 = 0,
	.RSV4 = 0,
	.RSV5 = 0
};







uint8_t S_State[8] = {0,0,0,0,0,0,0,0};	//���˿�״̬��¼,1:�˿���ɳ�ʼ��,2�˿��������(����������������)

uint8_t S_Data[8] = {0,0,0,0,0,0,0,0};	//���˿ڽ��պͷ������ݵ�״̬,1:�˿ڽ��յ�����,2:�˿ڷ����������

uint8_t Gateway_IP[4];//����IP��ַ
uint8_t aucModbusData[APP_MAX_REG_ADD][2];//�弶��ʼ����������

int main()
{
    //һ��������vApp_ModbusTCP_Process����
    static uint8_t linkFlag = 0;//���ӱ�־λ
    T_ETH_PARA eth_para;
    eth_para.Link_Status = 0x01;
    /*
        1.eth_para.Link_Count�ں���vNet_LinkLose_Count()�лᱻ����;
        2.vNet_LinkLose_Count()��������HAL_Tick_SysCall()�����б�����;
        3.HAL_Tick_SysCall()��������SysTick_Handler()�����б�����,ͬʱҲ������С�Ƶ���˸;
            if(RunLedCnt++ > 500)
            {
                RunLedCnt = 0;
                HAL_GPIO_TogglePin(LED_RUN_GPIO_Port, LED_RUN_Pin);
            }
            ÿ1ms,RunLedCnt+1,ֱ��RunLedCnt = 501,����if����ڲ�,RunLedCnt��������0,С�Ƹı�һ��״̬.
        4.SysTick_Handler()����ÿ1ms�ᱻϵͳ�Զ�����.
        SysTick_Handler-->HAL_Tick_SysCall-->vNet_LinkLose_Count-->eth_para.Link_Count

        5.vNet_LinkLose_Count�ڲ������eth_para.Link_Status��������״̬����eth_para.Link_Count���в���.
        (1)��������ʱ(eth_para.Link_Status = 0),eth_para.Link_Count++ֱ���Լ�ΪLINK_LOSE_COUNTֹͣ,��ʾ����δ������
        (2)�������ӳɹ�ʱ(eth_para.Link_Status = 1),eth_para.Link_Count = 0
        �ص�vApp_ModbusTCP_Process��������,
        6.�ж�eth_para.Link_Count �Ƿ�>= LINK_LOSE_COUNT/2,��eth_para.Link_Count�Ƿ�>=2500
        (1)eth_para.Link_Count>=2500,����if�ж����,�ֱ��W5500��8���׽���(Socket0~Socket7)��״̬λ������λ��0,
            ��ʾû���쳣״̬��û���յ�����.ͬʱ��W5500���г�ʼ������:
            void W5500_Initialization(void)
            {
                SOCKET s;
                
                W5500_Init();		//��ʼ��W5500�Ĵ�������
                Detect_Gateway();		//������ط����� 

                for(s=0;s<SOCK_NUM;s++)
                {
                    Socket_Init(s);	//Socket ��ʼ��
                }
            }
            (i)��ʼ��W5500�Ĵ���W5500_Init():
                ��Write_W5500_1Byte(MR, RST);//������λW5500,��1��Ч,��λ���Զ���0
                 void Write_W5500_1Byte(unsigned short reg, unsigned char dat)
                 {
                     HAL_GPIO_WritePin(W5500_SPI_CS_GPIO_Port, W5500_SPI_CS_Pin, GPIO_PIN_RESET);	//��W5500��SCSΪ�͵�ƽ

                     SPI_Send_Short(reg);							//ͨ��SPIд16λ�Ĵ�����ַ
                     SPI_Send_Byte(FDM1|RWB_WRITE|COMMON_R);	//ͨ��SPIд�����ֽ�,1���ֽ����ݳ���,д����,ѡ��ͨ�üĴ���
                     SPI_Send_Byte(dat);							//д1���ֽ�����

                     HAL_GPIO_WritePin(W5500_SPI_CS_GPIO_Port, W5500_SPI_CS_Pin, GPIO_PIN_SET);	//��W5500��SCSΪ�ߵ�ƽ
                 }
                 reg = MR(0x0000ģʽ�Ĵ���),dat = RST(0x80 = 0x1000 0000)
                 ��д����Ҫ���ƵļĴ�����ƫ�Ƶ�ַ,����д����ƶε������Ϣ:ƫ�Ƶ�ַ����������(BSB[4:0])����/дģʽ(RWB)��SPI����ģʽ�������򲻶�������ģʽ(OM[1:0]),
                 �����Ĵ���д����Ӧ�����Ѵﵽ��Ҫ��Ŀ��.

                 FDM1|RWB_WRITE|COMMON_R�ļ�����Ϊ0x05(00000 1 01)
                 00000:ƫ�Ƶ�ַ0x0000��������λ�����Ĵ���
                 1:дģʽ
                 01:��������ģʽ,���ݳ���Ϊ1�ֽ�

                 ģʽ�Ĵ���:
                 7         6         5         4         3         2         1         0
                RST       Res       WOL        PB      PPPoE      Res       FARP      Res(����λ)
                0x80�պý�RSTλ��1���Ѵﵽ��ʼ��������
                
                ��Write_W5500_nByte(GAR, Gateway_IP, 4);//��������(Gateway)��IP��ַ
                 void Write_W5500_nByte(unsigned short reg, unsigned char *dat_ptr, unsigned short size)
                 {
                     //reg:16λ�Ĵ�����ַ,*dat_ptr:��д�����ݻ�����ָ��,size:��д������ݳ���
                     HAL_GPIO_WritePin(W5500_SPI_CS_GPIO_Port, W5500_SPI_CS_Pin, GPIO_PIN_RESET);	//��W5500��SCSΪ�͵�ƽ
                        
                     SPI_Send_Short(reg);						//ͨ��SPIд16λ�Ĵ�����ַ
                     SPI_Send_Byte(VDM|RWB_WRITE|COMMON_R);	//ͨ��SPIд�����ֽ�,N���ֽ����ݳ���,д����,ѡ��ͨ�üĴ���

                     HAL_SPI_Transmit(&hspi2, dat_ptr, size, 1000);

                     HAL_GPIO_WritePin(W5500_SPI_CS_GPIO_Port, W5500_SPI_CS_Pin, GPIO_PIN_SET);	//��W5500��SCSΪ�ߵ�ƽ
                 }

                 reg = GAR(0x0001) 
                 Gateway_IP�����е�Ԫ����Load_Net_Parameters�����б���ֵ
                 Load_Net_Parameters��������vApp_Net_Init�����б�����
                 vApp_Net_Init�����Ĵ���ΪaucModbusData[IP_START_REG][0]�ĵ�ַ
                 ��aucModbusData[5][0]�ĵ�ַ

                 void vApp_Modbus_DataInit(void)
                 {
                    //�弶��ʼ��
                    vBoard_Init();

                    //������������
                    if(OK!=ucLoad_DevPara(&aucModbusData[DEV_ADDR_REG][0], (const uint8_t *)&DefaultBootPara, sizeof(T_BOOT_PARA)))
                    {
                        // Sys_Err.i2cerr.xFlag = TRUE;
                    }

                    //����У׼����
                    ucLoad_CalData(&aucModbusData[APP_CAL_START_REG][0], sizeof(tSysCalData));
                    memcpy(&tSysCalData[0].uForceData[0].u8data[0], &aucModbusData[APP_CAL_START_REG][0], sizeof(tSysCalData));
                    
                    for(int j=0; j<MAX_CAL_NUM; j++)
                    {
                        for(int k=0; k < CAL_FLAG_MAX; k++)
                        {
                            my_printf("Index:%d Force k&b=%f ", j, tSysCalData[j].uForceData[k].fdata);
                        }
                        my_printf("\n");

                        for(int k=0; k < CAL_FLAG_MAX; k++)
                        {
                            my_printf("Type:%d mease k&b=%f ", j, tSysCalData[j].uMeaseData[k].fdata);
                        }
                        my_printf("\n");

                    }
                    
                    //Verion
                    aucModbusData[HARD_VER_REG][0] = aucFWVerion[0];
                    aucModbusData[HARD_VER_REG][1] = aucFWVerion[1];
                    aucModbusData[SOFT_VER_REG][0] = aucFWVerion[2];
                    aucModbusData[SOFT_VER_REG][1] = aucFWVerion[3];
                 }

                 ucLoad_DevPara����:
                 int8_t ucLoad_DevPara(uint8_t *pucLoadPara, const uint8_t *pucInitPara, const uint16_t usParaLen)
                 {
                    uint8_t i = 0;
                    uint8_t ucRetCode = OK;
                    BOOL ucHasParaFlag = FALSE;
                    uint8_t *pucTempData = malloc(usParaLen + 1);	//����ѿռ�(�ñ�����Ҫ�����ǲ�����2��I2C�������������ʡ��)
                    if(NULL==pucTempData)
                    {
                        return ERR;
                    }

                    for(i = 0; i < 3; i++)	//��ೢ��3�ζ�ȡ
                    {
                        if(OK==EEPROM_Read(&hi2c1, DEV_I2C_ADD, PARA_FLAG_BASE, pucTempData, usParaLen + 1))
                        {
                            if(pucTempData[0]==PARAM_FLAG)
                            {
                                memcpy(pucLoadPara, &pucTempData[1], usParaLen);
                                ucHasParaFlag = TRUE;
                                break;
                            }
                        }
                        HAL_Delay(10);
                        ucHasParaFlag = FALSE;
                    }

                    if(FALSE==ucHasParaFlag)
                    {
                        //��Ĭ��ֵд��EEPROM
                        pucTempData[0] = PARAM_FLAG;
                        memcpy(&pucTempData[1], pucInitPara, usParaLen);
                        ucRetCode = EEPROM_Write(&hi2c1, DEV_I2C_ADD, PARA_FLAG_BASE, pucTempData, usParaLen + 1);

                        //��Ĭ��ֵ��ֵ���豸����
                        memcpy(pucLoadPara, pucInitPara, usParaLen);
                    }

                    //�ͷŶѿռ�
                    free(pucTempData);
                    pucTempData = NULL;

                    return ucRetCode;
                 }























                 void Load_Net_Parameters(uint8_t *pucNetPara)
                 {
                     uint8_t i = 0;
                     uint16_t portData = 0;

                     for(i=0;i<4;i++)
                     {
                         Sub_Mask[i] = netConst[4+i];
                     }
                    
                     for(i=0;i<2;i++)
                     {
                         Phy_Addr[i] = netConst[8+i];
                     }

                     for(i=0;i<4;i++)
                     {
                         IP_Addr[i] = pucNetPara[i];
                         Phy_Addr[i+2] = pucNetPara[i];
                     }

                     for(i=0;i<3;i++)
                     {
                         Gateway_IP[i] = IP_Addr[i];
                     }
                    
                     Gateway_IP[3] = 1;

                     portData =  (pucNetPara[4]<<8) + pucNetPara[5];

                     for(i = 0; i < SOCK_NUM; i++)
                     {
                         S_Port[i] = portData;
                         S_Mode[i] = TCP_SERVER;
                     }
                 }



                ��Write_W5500_nByte(SUBR,Sub_Mask,4);//������������(MASK)ֵ
                ��Write_W5500_nByte(SHAR,Phy_Addr,6);//����������ַ
                ��Write_W5500_nByte(SIPR,IP_Addr,4);//���ñ�����IP��ַ
                ��//���ý��ջ������Ĵ�С���ο�W5500�����ֲ�
                
            (ii)������ط�����Detect_Gateway():
            (iii)��ÿһ��W5500���׽��ֽ��г�ʼ��Socket_Init(s):






        (2)
    */
    /**//**//**//**//**//**//**//**//**//**//**//**/





    return 0;
}





















































