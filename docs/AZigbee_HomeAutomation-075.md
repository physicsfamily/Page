---
layout: post
title:  "075_Zigbee_HomeAutomation"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# 综合实验_基于Zigbee的智能家居系统实验
<!-- ------------------------ -->
## 实验内容


- 通过ZigBee网络传输数据；
- 通过ZigBee网络对硬件进行控制。

<!-- ------------------------ -->
## 实验目的


- 熟练使用ZigBee协议读数据及发送控制指令。


<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 3个 |  · |
| 3 | 温湿度模块 | 1个 | ·  |
| 4 | LED模块 | 1个 | ·  |
| 5 | OLED模块 | 1个 | 7*5cm  |
| 6 | ST-Link下载器 | 1个 | · |
| 7 | ST-Link下载器连接线 | 1根 |  · |
| 8 | ZigBee智能家居实验代码 | 4份 | ·  |

### 实验所需软件

- [IAR](https://codelab.stepiot.com/codelabs/IAR_077/index.html?index=..%2F..index#0) 驱动安装步骤

- [CC Debugger](https://codelab.stepiot.com/codelabs/CC_Debugger_081/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

[CC2530底座](https://docs.stepiot.com/docs/aiot017) 

![实验硬件](/assets/BASE_CC2530/3.png)

[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](/assets/BASE_STM32/2.png)

[LED模块](https://docs.stepiot.com/docs/aiot001)

![LED模块](/assets/STM32_OneNET/37.png)

[OLED模块](https://docs.stepiot.com/docs/aiot003)

![OLED模块](/assets/BASE_STM32/67.png)

CC Debugger 仿真器和连接线

![实验硬件](/assets/BASE_CC2530/4.png)

USB线

![USB线](/assets/CC2530/2.png)

<!-- ------------------------ -->
## 实验原理
0

### ZigBee基本通信方式

ZigBee的通讯方式主要有三种点播、组播、广播。点播顾名思义就是点对点通信，也是2个设备之间的通讯，不允许有第三个设备收到信息；组播就是把网络中的节点分组，每一个组员发出的信息只有相同组号的组员才能收到。广播最广泛的也就是1个设备上发出的信息所有设备都能接收到。这也是ZigBee通信的基本方式。

### ZigBee网络地址类型

|序号|地址类型|说明|
|----|----|----|
|1|AddrNotPresent|依照绑定表|
|2|Addr16Bit|16位地址，常用于点播|
|3|Addr64Bit|Addr64Bit|
|4|AddrGroup|组播|
|5|AddrBroadcast|广播|

以上的的地址类型均保存在afAddrMode_t。在实际使用过程中考虑功耗，常用使用16位地址，原因16位地址是通过路由算法计算出来的，在传输路径上通过的节点较少最大的节省了网络功耗。16位地址可分成如下几类如表:

|序号|16位地址类型|说明|
|----|----|----|
|1|0xFFFF|对所有设备广播，包括睡眠|
|2|0xFFFE|间接传输，通过绑定表寻找网络短地址|
|3|0xFFFD|对没休眠的设备广播|
|4|0xFFFC|给协调器和路由器广播|
|5|0x0000|给协调器通信|
|6|0x0001-0xFFFB|用户自设定的目标地址|

<!-- ------------------------ -->
## 实验步骤


1. 将温湿度模块、OLED模块、LED模块分别安装在CC2530底座上，确认各个节点，如下图所示，ST_LINK连接OLED节点连接。

    ![搭建实验硬件平台](/assets/HomeAutomation/12.png)

2. CC Debugger连接PC机与OLED节点CC2530底座，轻按CC Debugger复位按键，指示灯变绿，表示连接正常。

3. 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`综合实验\ZigBee智能家居\Projects\zstack\Samples\SampleApp\CC2530DB\SampleApp.eww` 并打开。

    ![启动工程](/assets/HomeAutomation/14.jpg)

4. 修改PANID或者信道防止与他人网络冲突，终端与协调器代码共用该配置文件如图:

    ![修改PANID或信道](/assets/HomeAutomation/15.png)

5. 设置工程配置为`Coordinator`。

    ![工程配置为coordinatorEB](/assets/HomeAutomation/16.jpg)

6. 点击`Make`按钮，编译程序，点击`Download and Debug`按钮，将程序下载到底座中。

    ![编译并下载程序](/assets/HomeAutomation/17.png)

7. 点击`X`，退出仿真模式。

    ![退出仿真模式](/assets/HomeAutomation/18.png)

8. 将CC Debuger连接到温湿度节点，选择工程的配置为`EndDeviceEB-TempHumi`如图。

    ![工程配置为coordinatorEB](/assets/HomeAutomation/19.png)

9. 编译工程，下载程序，方法参考以上步骤6~7。

10. 将CC Debuger连接到LED节点，选择工程的配置为`EndDeviceEB-LED`如图：

    ![工程配置为coordinatorEB](/assets/HomeAutomation/20.png)

11. 编译工程，下载程序，方法参考以上步骤6~7。

12. 移除CC Debugger，采用USB线供电，如图：

    ![USB线供电](/assets/HomeAutomation/21.png)

13. 观察OLED屏温湿度数据，如图：

    ![显示传感器数据](/assets/HomeAutomation/22.png)

14. 单击OLED上的按键，选择灯1、灯2、灯3、灯4(灯1、灯2、灯3、灯4指的是LED模块上的四个灯)，选择选后双击OLED的按键进行控制操作：

    ![图片](/assets/HomeAutomation/23.png)

<!-- ------------------------ -->
## 代码讲解


### LED节点

① 程序目录结构，如下图。代码中有大量ZigBee底层的代码，我们主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![程序目录结构](/assets/HomeAutomation/24.jpg)

② EndDevice-LED.c->SampleApp_Init()函数是应用代码的入口函数，对LED模块初始化、初始化Point_To_Point_DstAddr结构，注册端点。

```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID   = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID  = 0; 

        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//用于调试
        LedMode_Init();//初始化LED模块  
        printf("i am end device\r\n");//串口打印   
        //点对点通讯定义
        Point_To_Point_DstAddr.addrMode = (afAddrMode_t)Addr16Bit;//点播
        Point_To_Point_DstAddr.endPoint = SAMPLEAPP_ENDPOINT;      
        Point_To_Point_DstAddr.addr.shortAddr = 0x0000;//发给协调器
        
        //填写端点
        EndDevice_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        EndDevice_epDesc.task_id    = &SampleApp_TaskID;
        EndDevice_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&EndDevice_SimpleDesc;
        EndDevice_epDesc.latencyReq = noLatencyReqs;
        
        //注册端点
        afRegister( &EndDevice_epDesc );
    }
```

EndDevice.c->SampleApp_ProcessEvent()函数是任务处理函数。在该函数中调用SampleApp_MessageMSGCB()处理无线信道的指令。

```c
    // Received when a messages is received (OTA) for this endpoint
    case AF_INCOMING_MSG_CMD:
    SampleApp_MessageMSGCB( MSGpkt );
    break;
```

### 温湿度节点

① 程序目录结构，如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![程序目录结构](/assets/HomeAutomation/25.jpg)

② EndDevice_TempHumi.c->SampleApp_Init()函数是应用代码的入口函数，对温湿度模块初始化、初始化Point_To_Point_DstAddr结构，注册端点、启动传感器数据采集。

```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID   = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID  = 0; 

        SHT2x_Init();//初始化温湿度传感器
        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//初始化串口1，用于printf  
        printf("i am end device\r\n");//串口打印   
        //点对点通讯定义
        Point_To_Point_DstAddr.addrMode = (afAddrMode_t)Addr16Bit; //点播
        Point_To_Point_DstAddr.endPoint = SAMPLEAPP_ENDPOINT;      
        Point_To_Point_DstAddr.addr.shortAddr = 0x0000;//发给协调器
        
        //填写端点
        EndDevice_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        EndDevice_epDesc.task_id    = &SampleApp_TaskID;
        EndDevice_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&EndDevice_SimpleDesc;
        EndDevice_epDesc.latencyReq = noLatencyReqs;
        
        //注册端点
        afRegister( &EndDevice_epDesc );
        
        //启动定时器，开始本地显示温湿度数值
        osal_start_timerEx( SampleApp_TaskID, READ_TEMP_HUMI_MSG_EVT,1000);
    }
```

③ EndDevice.c->SampleApp_ProcessEvent()函数是任务处理函数。在该函数中调用SHT2x_DisplayHumi()、SHT2x_DisplayTemp()函数读取传感器数据显示在数码管上。定时调用SendTemperatureHumityToCoordinator()函数向协调器发送传感器数据。

```c
    if(events & READ_TEMP_HUMI_MSG_EVT)
    {
        TempHumiShowSwitch = 1 - TempHumiShowSwitch;//滚动显示标志 
            /*读取温湿并显示*/
        TempHumiShowSwitch?SHT2x_DisplayHumi():SHT2x_DisplayTemp();
            /*1000ms 后触发READ_TEMP_HUMI_MSG_EVT事件*/
        osal_start_timerEx( SampleApp_TaskID, READ_TEMP_HUMI_MSG_EVT,1000);
    }
    if(events & SEND_TEMP_HUMI_MSG_EVT)
    {
        SendTemperatureHumityToCoordinator();//发送温湿度到OLED节点   
        /*2000ms 后触发SEND_TEMP_HUMI_MSG_EVT事件*/
        osal_start_timerEx( SampleApp_TaskID, SEND_TEMP_HUMI_MSG_EVT,2000);
    }
```

### OLED节点

① 程序目录结构，如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![程序目录结构](/assets/HomeAutomation/26.jpg)

② Coordinator.c->SampleApp_Init()函数是应用代码的入口函数，对OLED屏，注册端点。

```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID = 0;  

        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);  
        printf("我是协议器\r\n");//串口打印
        OLEDM_Init();//初始化OLED
        OLED_PowerUpDisp();//开机启动界面
            
        //点对点通讯定义，本次实验中协调器指向风扇节点发送数据，而不会向湿湿度发送，
        //Point_To_Point_DstAddr 结构体用于向风扇节点发送指令使用
        Point_To_Point_DstAddr.addrMode       = (afAddrMode_t)Addr16Bit;//点播
        Point_To_Point_DstAddr.endPoint       = SAMPLEAPP_ENDPOINT;      
        Point_To_Point_DstAddr.addr.shortAddr = 0;

        //广播通信定义 
        Boardcast_DstAddr.addrMode       = (afAddrMode_t)AddrBroadcast;
        Boardcast_DstAddr.endPoint       = SAMPLEAPP_ENDPOINT;
        Boardcast_DstAddr.addr.shortAddr = 0Xffff;
        
        //填写端点
        Coordinator_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        Coordinator_epDesc.task_id    = &SampleApp_TaskID;
        Coordinator_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&Coordinator_SimpleDesc;
        Coordinator_epDesc.latencyReq = noLatencyReqs;
        //注册端点
        afRegister( &Coordinator_epDesc );
            
        /*3000ms后触发 READ_KEY_MSG_EVT */
        osal_start_timerEx( SampleApp_TaskID, READ_KEY_MSG_EVT,3000);
    }
```

Coordinator.c ->SampleApp_ProcessEvent()函数是任务处理函数。当接收到温湿度数据时，进入SampleApp_MessageMSGCB()函数。

```c
    // Received when a messages is received (OTA) for this endpoint
    case AF_INCOMING_MSG_CMD:
    SampleApp_MessageMSGCB( MSGpkt );
    break;
```

SampleApp_MessageMSGCB()函数中将传感器的数据解析显示在OLED显示屏上。

```c
    case TEMP_HUMI_CLUSTERID:        
	Sensor_Temp = pkt->cmd.Data[0];
	Sensor_Humi = pkt->cmd.Data[1];
	OLED_Disp_Temp(0,0,Sensor_Temp);//更新温度
	OLED_Disp_Humi(0,2,Sensor_Humi);//更新湿度
    break;
```

Coordinator.c ->SampleApp_ProcessEvent()函数是任务处理函数，其中调用OLED_KeyPoll()，获取当前按键是单击还是双击。按键单击选择控制对象(灯1、灯2、灯3、灯4)。双击进行控制操作(亮/灭)。

```c
    switch(OLED_KeyPoll())//OLED_KeyPoll()检测按键
	{
		case KV_DOUBLE://双击按键
		printf("KV_DOUBLE\r\n");
		LEDCtrl   = 1;
		LEDCmd[0] = DevItem-1;//控制哪个灯。
		LED_State[LEDCmd[0]] = 1 - LED_State[LEDCmd[0]];                  
		LEDCmd[1] = LED_State[LEDCmd[0]];//控制动作(亮/灭)。
		Send_LEDCtrl(LEDCmd[0],LEDCmd[1]);//发送LED节点
		break;
		case KV_SINGLE://单击按键
		printf("KV_SINGLE\r\n"); 
		DevItem++;
		if(DevItem>4){//只有4个选项，灯1 灯2 灯3 灯4
			DevItem = 1;
		}
		OLED_SelectDev(DevItem);//更新选项反显。
		break;
	}
```

<!-- ------------------------ -->
## 常见问题


1. 弹出警告窗口，不能下载程序。

    - 请确认CC Debugger的驱动否安装。
    - 轻按CC Debugger仿真器的按键，指示灯不是绿色表示连接有问题。
    - CC Debugger仿真器是否正常接入到底座，参考实验步骤第一步。

2. 下载代码后程序没观察到实验现象。

    - 请重新上电，或者按下底座上的复位按键。
    - 模块没有安装稳妥。
    - 节点之间的PANID、信道是否相同。

     
<!-- ------------------------ -->
## 实验思考


1. 将继电器模块加入其中并实现对继电器的控制。
