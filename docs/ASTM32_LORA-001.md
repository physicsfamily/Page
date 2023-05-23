---
layout: post
title:  "基于STM32的LORA有源模块传感器数据传输实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# 基于STM32的LORA有源模块传感器数据传输实验
<!-- ------------------------ -->
## 实验内容


- 编写程序实现利用两个LORA模块进行温湿度传感器数据传输。
  
<!-- ------------------------ -->
## 实验目的


- 了解如何使用LORA模块进行传感器数据传输。

<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | PC机 | 1台 | PC机安装有MDK5，ST_LINK驱动 |
| 2 | STM32底座模块 | 4个 |  · |
| 3 | LORA有源模块模块 | 2个 | ·  |
| 4 | 温湿度传感器模块 | 1个 | ·  |
| 5 | TFT显示器模块 | 1个 | · |
| 6 | ST_LINK下载器 | 1个 |  · |
| 7 | ST_LINK下载器连接线| 1根 | ·  |
| 8 | LORA有源模块实验代码 | 1份 | ·  |
| 9 | 433短天线  | 2根 | ·  |

![STM32底座、电池盒](/assets/STM32-LORA/1.png)
图7.3.1 STM32底座、电池盒
![实验模块](/assets/STM32-LORA/2.png)
图7.3.2 实验模块
![STLink仿真器](/assets/STM32-LORA/3.png)
图7.3.3 STLink仿真器
### 实验要求

- 了解LORA通信基本原理。

- 能够编写程序使用两个LORA通信。

<!-- ------------------------ -->
## 实验原理


### 什么是LoRa

LoRa是semtech公司创建的低功耗局域网无线标准，低功耗一般很难覆盖远距离，远距离一般功耗高，要想马儿不吃草还要跑得远，好像难以办到。LoRa的名字就是远距离无线电（Long Range Radio），它最大特点就是在同样的功耗条件下比其他无线方式传播的距离更远，实现了低功耗和远距离的统一，它在同样的功耗下比传统的无线射频通信距离扩大3-5倍。

### LoRa的特性

传输距离：城镇可达2-5 Km ， 郊区可达15 Km 。工作频率：ISM 频段 包括433、868、915 MH等。标准：IEEE 802.15.4g。调制方式：基于扩频技术，线性调制扩频（CSS）的一个变种，具有前向纠错（FEC）能力，semtech公司私有专利技术。容量：一个LoRa网关可以连接上千上万个LoRa节点。电池寿命：长达10年。安全：AES128加密。传输速率：几百到几十Kbps，速率越低传输距离越长。

### LoRa和LoRaWan

LoRa和LoRaWan很容易混淆。

![LoRaWan协议栈](/assets/STM32-LORA/4.png)
图 7.3.4 LoRaWan协议栈

上图可以看出，LoRa是LoRaWan的一个子集，LoRa仅仅包括物理层定义，LoRaWan还包括了链路层。
![LoRaWan网络架构](/assets/STM32-LORA/5.png)
图 7.3.5 LoRaWan网络架构

这张图片是LoRaWan的网络架构图，左边是各种应用传感器，包括智能水表，智能垃圾桶，物流跟踪，自动贩卖机等，它右边是LoRaWan网关，网关转换协议，把LoRa传感器的数据转换为TCP/IP的格式发送到Internet上。LoRa网关用于远距离星型架构，是多信道、多调制收发、可多信道同时解调。由于LoRa的特性可以同一信道上同时多信号解调。网关使用不同于终端节点的RF器件，具有更高的容量，作为一个透明网桥在终端设备和中心网络服务器间中继消息。网关通过标准IP连接连接到网络服务器，终端设备使用单播的无线通信报文到一个或多个网关。
其实LoRaWan并不是一个完整的通信协议，因为它只定义了物理层和链路层，网络层和传输层没有，功能也并不完善，没有漫游，没有组网管理等通信协议的主要功能。

### Lora物理帧结构

LoRa的报文分为上行和下行。上行是从传感器到LoRa网关的，下行是LoRa网关到传感器的，仅仅作为回复。
![上行报文](/assets/STM32-LORA/6.png)
图 7.3.6 上行报文

上图是上行报文，包括一个前导码，包头和包头的CRC值，后面是数据，最后是CRC校验。

![下行报文](/assets/STM32-LORA/7.png)
图 7.3.7 下行报文

### 软件设计

![数据流向](/assets/STM32-LORA/8.png)
图 7.3.8 数据流向

要完成这个实验需要四个底座，故会有四个子部分代码。
温湿度数据通过485总线传输给LORA模块发送节点，由该通过无线方式传输致LORA模块接收节点，节点接收完成后通过485总线传输给TFT显示器模块节点。
温湿度模块代码：

```c
//485总线通过处理函数，由定时器3中断调用，在该函数内数据处理时间不能过长
void RS485_HandlerCb(void); 
uint8_t SensorData[5];      //485总线数据发送缓冲
int main(void)
{
    HAL_Init();             					//初始化HAL库  
	ADC_Init();									//初始化ADC
    USART3_Init(115200);
	SHT2x_Init();								//初始化SHT20
	TM1640_Init();								//初始化TM1640
Rs485_Init();
//初始化定时器中断周期64M/64/10000/ = 100HZ
	TIM3_Init(10000-1,64-1,RS485_HandlerCb);	
    
    printf("sht20:usart3 print\r\n");           //调试信息，通过PB10 管脚输出   
	while(1)
	{
		Display_Data();						    //获取温湿度并显示到数码管上

        /*将传感器的值转换成数组，方便通过485总线发送*/
        SensorData[0] = g_sht2x_param.TEMP_POLL;        
        SensorData[1] = g_sht2x_param.HUMI_HM;
        SensorData[2] = (uint8_t)LDR_Data;
        SensorData[3] = (uint8_t)(LDR_Data>>8);          
	}
}
//==========================================================
//	函数名称：	void RS485_HandlerCb(void)
//
//	函数功能：	通过485总线获取数据处理函数
//
//	入口参数：	void
//
//	返回参数：	无
//
//	说明：		接收通过485总线发送过来的数据请求
//==========================================================
void RS485_HandlerCb(void)
{  
    if((!DataHandling_485(Addr_SHT20))&&(SHT20_Get_All==Rx_Stack.Command))
    {
        Rs485_Send(Addr_SHT20,Addr_LORA,SHT20_All,4,SensorData);
    }        
}
```
LORA模块发送节点:
```
uint8_t version;
uint8_t IRQ_RegValue=0;
uint8_t LoraSendBuffer[32],UpdataFlg=0;
//485总线通过处理函数，由定时器3中断调用，在该函数内数据处理时间不能过长
void RS485_HandlerCb(void);  
int main(void)
{
	uint8_t LedState = 1;

	HAL_Init();    //初始化HAL库    
	LORA_Init();   //初始化LORA芯片

	Rs485_Init();				    //初始化485
//中断频率64M/640/10000=10HZ，中断频率10HZ
    TIM3_Init(10000-1,640-1,RS485_HandlerCb);
	UART1_Init(115200);				//初始化串口1
    USART3_Init(115200);            //printf 打印信息，PB10->TX,PB11->RX,通常只使用，TX功能
	
	ModLed_Init(); //初始化LORA模块上的LED灯控制管脚
	ModKey_Init(); //初始化LORA模块上的按键管脚
    version = SX1276ReadBuffer(REG_LR_VERSION);
    if(version == 0x12)
    {//如果模块安放稳妥，亮500ms
        MOD_LED_ON();
        HAL_Delay(500);
        MOD_LED_OFF();
    }
	while(1)
	{
        if(UpdataFlg == 1)
        {//得到一次传感器数据，发送一次，RS485_HandlerCb()函数中更新UpdataFlg
            UpdataFlg = 0;
            Sx1278SendLong(LoraSendBuffer,4);            
        }           
	}
}
//==========================================================
//	函数名称：	void RS485_HandlerCb(void)
//
//	函数功能：	通过485总线获取数据处理函数
//
//	入口参数：	void
//
//	返回参数：	无
//
//	说明：		定时器3中断，回调函数，每100ms调用一次
//==========================================================
void RS485_HandlerCb(void)
{
    static uint32_t PollCounter=0;
    static uint8_t state = 0;
    
    if(state == 0)
    {
        if((++PollCounter) >= 20)
        {//2000ms发送一次轮询指令
            PollCounter = 0;
            Rs485_Send(Addr_LORA,Addr_SHT20,SHT20_Get_All,  0,(void*)0); 
            state = 1;
        }
    }
    else if(state == 1)
    {
        if(!DataHandling_485(Addr_LORA))
        {
            memcpy((void*)LoraSendBuffer,Rx_Stack.Data,4);
            UpdataFlg = 1;
        }
        state = 0;    
    }
}
```
LORA模块接收节点：
```
uint8_t version;
uint8_t IRQ_RegValue=0;
uint8_t LoraRecvBuffer[32],UpdataFlg=0;
void RS485_HandlerCb(void);
int main(void)
{
	uint8_t LedState = 1;

	HAL_Init();    //初始化HAL库    
	LORA_Init();   //初始化LORA芯片

	Rs485_Init();				    //初始化485
    TIM3_Init(10000-1,640-1,RS485_HandlerCb);//中断频率64M/640/10000=10HZ，中断频率10HZ
	UART1_Init(115200);				//初始化串口1
    USART3_Init(115200);            //printf 打印信息，PB10->TX,PB11->RX,通常只使用，TX功能
	
	ModLed_Init(); //初始化LORA模块上的LED灯控制管脚
	ModKey_Init(); //初始化LORA模块上的按键管脚
    version = SX1276ReadBuffer(REG_LR_VERSION);
    if(version == 0x12)
    {//如果模块安放稳妥，亮500ms
        MOD_LED_ON();
        HAL_Delay(500);
        MOD_LED_OFF();
    }
	while(1)
	{
        IRQ_RegValue = SX1276ReadBuffer( REG_LR_IRQFLAGS );
        if((IRQ_RegValue)&&(IRQ_RegValue != 0XFF))
        {//读 LORA中断寄存器 REG_LR_IRQFLAGS，判断是否有中断发生
            if(INT_FLG_RX_DONE == SX1278_InteruptHandler(LoraRecvBuffer))
            {//是接收中断，将接收的数据保存到LoraRecvBuffer中
                Rs485_Send(Addr_LORA,Addr_LCD,SHT20_All,4,LoraRecvBuffer);
            }
        }         
	}
}
//==========================================================
//	函数名称：	void RS485_HandlerCb(void)
//
//	函数功能：	通过485总线获取数据处理函数
//
//	入口参数：	void
//
//	返回参数：	无
//
//	说明：		定时器3中断，回调函数，每100ms调用一次
//==========================================================
void RS485_HandlerCb(void)
{

}
```
TFT显示器模块代码：
```c
extern void TIM3_Init(uint16_t arr,uint16_t psc,void (*cb)(void));
void Master_do_RS485Bus_Poll(void);
void RS485_HandlerCb(void);

struct _SHT20{
    uint8_t   temp;  //温度
    uint8_t   humi;  //湿度
    uint16_t  LightIntensity; //光强
};
struct _SHT20 SensorData;

uint32_t SensorDataUpdateIntervalCounter=0;
uint32_t TouchIntervalCounter;
uint8_t  update = 1;
int main(void)
{
    
    HAL_Init();             	//初始化HAL库
	LCD_Init();             	//初始化LCD显示器端口
	Lcd_Init();								//初始化LCD
	Rs485_Init();							//初始化485
	UART1_Init(115200);				//初始化串口一
	UART2_Init(115200);				//初始化串口一		
    USART3_Init(115200);				//初始化串口一	
	LCD_Clear(WHITE);								//清屏
    TIM3_Init(10000-1,64-1,RS485_HandlerCb);	//初始化定时器中断周期64M/64/10000/ = 100HZ

	POINT_COLOR = RED;
	GUI_DrawTempTitle(1,10);
    GUI_DrawHumiTitle(1,68);
    GUI_DrawLinghtIntensityTitle(1,126);
    
    printf("TFT_LCD:usart3 printf\r\n");
	while(1)
	{	
        if(update)
        {
            update = 0;
            //显示温度
            GUI_DrawTempData(106-32,10,SensorData.temp);
            //显示湿度
            GUI_DrawHumiData(106-32,68,SensorData.humi);
            //显示光强值
            GUI_DrawLinghtIntensityData(106-32,126,SensorData.LightIntensity);            
        }
	}
}
//==========================================================
//	函数名称：	void RS485_HandlerCb(void)
//
//	函数功能：	通过485总线获取数据处理函数
//	入口参数：	void
//	返回参数：	无
//
//	说明：		定时器3中断，回调函数，每100ms调用一次
//==========================================================
void RS485_HandlerCb(void)
{  
    if(!DataHandling_485(Addr_LCD))
    {
        SensorData.temp = Rx_Stack.Data[0];
        SensorData.humi = Rx_Stack.Data[1];
        SensorData.LightIntensity = Rx_Stack.Data[2] + (Rx_Stack.Data[3]<<8);
        update = 1;
    }     
}
```

<!-- ------------------------ -->
## 实验步骤

   
① 将两个LORA有源模块、温湿度模块、TFTLCD模块分别安装在STM32底座上，分成两个节点，如下图7.3.9所示**(注意两个部分要隔离分开)**。

![搭建实验硬件平台](/assets/STM32-LORA/9.png)

图 7.3.9 搭建实验硬件平台

② 确认上图中各个节点，避免将程序下载到错误的底座上。
    
③ ST_LINK连接PC机与温湿度传感器节点底座。

④ 打开目录：\LORA_Sensor_xfer\SHT20\USER\ 找到SHT20 MDK工程文件，如图7.3.10，双击启动工程。

![启动工程](/assets/STM32-LORA/10.png)

图 7.3.10 启动工程

⑤ 待工程启动完毕，对工程进行编译、下载。如图7.3.11。

![编译、下载](/assets/STM32-LORA/11.png)

图 7.3.12 编译、下载

⑥ 打开目录：\LORA_Sensor_xfer\LORA_transmitter_STH20\USER 找到 LORA MDK工程文件，如图7.3.13，双击启动工程。

![启动工程文件](/assets/STM32-LORA/12.png)

图 7.3.13 启动工程文件

⑦ 待工程启动完毕，修改信道：main.c 第73行，修改宏定义RF_Freqency,如下图7.3.14，要保证接收与其一致，否则不能进行通信。

![修改频率](/assets/STM32-LORA/13.png)

图7.3.14 修改频率

⑧ 将STLink仿真器连接到LORA发送节点的底座。

⑨ 对工程进行编译、下载。如图7.3.15。

![编译并下载程序](/assets/STM32-LORA/14.png)

图 7.3.15 编译并下载程序

⑩ 打开目录：LORA_Sensor_xfer\LORA_receiver_TFT\USER 找到 LORA MDK工程文件，如图7.3.16，双击启动工程。

![启动工程文件](/assets/STM32-LORA/15.png)

图 7.3.16 启动工程文件

⑪ 待工程启动完毕，修改信道：main.c 第73行，修改宏定义RF_CHANNEL,如下图，要保证与步骤7设置的信道一致，否则不能进行通信。

![修改频率](/assets/STM32-LORA/16.png)

图 7.3.17 修改频率

⑫ 将STLink仿真器连接LORA接收节点的底座。

⑬ 对工程进行编译、下载。如图7.3.18。

![编译并下载程序](/assets/STM32-LORA/17.png)

图 7.3.18 编译并下载程序

⑭ 打开目录：LORA_Sensor_xfer\TFT_LCD\USER 找到 LCD MDK工程文件，如图7.3.19，双击启动工程。

![启动工程](/assets/STM32-LORA/18.png)

图 7.3.19 启动工程

⑮ 将STLink仿真器连接到TFT显示器节点的底座。

⑯ 对工程进行编译、下载。如图7.3.20。

![编译并下载程序](/assets/STM32-LORA/19.png)

图 7.3.20 编译并下载程序

⑰程序下载完成后，重新上电，对这两个节点分别进行供电，可使用带microUSB接口的USB线供电，也可以使用电池盒。注意 这两个节点要隔离分开。 如下图7.3.21。

![实验结果](/assets/STM32-LORA/20.png)

图 7.3.21 实验结果

在TFT显示器模块下，可观察传感器数据如下图7.3.22。

![TFT显示器模块显示传感器数据](/assets/STM32-LORA/21.png)

图 7.3.22 TFT显示器模块显示传感器数据

将手放置在温湿度传感器芯片上可观察到温度、湿度的变化，用手遮挡照射到光敏电阻的光，可以看到光照值的变化小，手不遮挡时变大。如图7.3.23。
![温湿传感器传感器位置](/assets/STM32-LORA/21.png)

图 7.3.23 温湿传感器传感器位置