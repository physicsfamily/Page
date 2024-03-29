---
layout: post
title:  "062_CC2530_NODERED平台显示温湿度实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# CC2530_NODERED平台显示温湿度实验
<!-- ------------------------ -->
## 实验内容


- 通过zigbee读取温湿度传感器数据；
- 通过WiFi模块将温湿度数据传输到NODERED平台。

<!-- ------------------------ -->
## 实验目的


- 将传感器数据上传到NODE RED平台；
- NODE RED平台部署相关流程，并图形化显示温湿度数据。

<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 2个 |  · |
| 3 | 温湿度模块 | 1个 | ·  |
| 4 | WIFI模块 | 1个 | ·  |
| 5 | CC Debugger 仿真器和连接线| 1套 | ·  |
| 6 | USB线| 1根 | ·  |
| 7 | 实验代码 | 1份 | ·  |

### 实验所需软件

- [IAR](https://codelabs.stepiot.com/docs/AIAR-077.html) 驱动安装步骤
- [CC Debugger](https://codelabs.stepiot.com/docs/ACC_Debugger-081.html) 驱动安装步骤

- [NODE RED](https://codelabs.stepiot.com/docs/ASTM32_NodeRED-082.html)平台安装应用手册

- [Git](https://git-scm.com/downloads)软件下载(可选)

[CC2530底座](https://docs.stepiot.com/docs/aiot017) 

![实验硬件](/assets/BASE_CC2530/3.png)



[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](/assets/BASE_STM32/2.png)

[WiFi模块](https://docs.stepiot.com/docs/aiot011)

![WiFi模块](/assets/BASE_CC2530/65.png)

CC Debugger 仿真器和连接线

![实验硬件](/assets/BASE_CC2530/4.png)

USB线

![USB线](/assets/CC2530/2.png)


<!-- ------------------------ -->
## 实验原理


### ZigBee基本通信方式

ZigBee的通讯方式主要有三种点播、组播、广播。点播顾名思义就是点对点通信，也是2个设备之间的通讯，不允许有第三个设备收到信息；组播就是把网络中的节点分组，每一个组员发出的信息只有相同组号的组员才能收到。广播最广泛的也就是1个设备上发出的信息所有设备都能接收到。这也是ZigBee通信的基本方式。

### ZigBee网络地址类型

| **序号** | **地址类型** | **说明** |
| --- | --- | --- |
|1|AddrNotPresent|依照绑定表|
|2|Addr16Bit|16位地址，常用于点播|
|3|Addr64Bit|Addr64Bit|
|4|AddrGroup|组播|
|5|AddrBroadcast|广播|

以上的的地址类型均保存在afAddrMode_t。在实际使用过程中考虑功耗，常用使用16位地址，原因16位地址是通过路由算法计算出来的，在传输路径上通过的节点较少最大的节省了网络功耗。16位地址可分成如下几类如表。

| **序号** | **地址类型** | **说明** |
| --- | --- | --- |
|1|0xFFFF|对所有设备广播，包括睡眠|
|2|0xFFFE|间接传输，通过绑定表寻找网络短地址|
|3|0xFFFD|对没休眠的设备广播|
|4|0xFFFC|给协调器和路由器广播|
|5|0x0000|给协调器通信|
|6|0x0001-0xFFFB|用户自设定的目标地址|

### WIFI技术基本概念

WIFI英语全称是Wireless Fidelity，中文译成无线保真。WIFI是建立连接和进行通讯的手段，它对应一套通讯的规则，保证让两个节点能互相连接，设备连接建立后，通过TCP/IP和UDP等协议，传输数据，连接互联网络。WIFI模块的STA模式和AP模式。

AP模式：Access Point，提供无线接入服务，允许其它无线设备接入，提供数据访问，一般的无线路由/网桥工作在该模式下。AP和AP之间允许相互连接。Sta模式（SP模式）：Station类似于无线终端，sta本身并不接受无线的接入，它可以连接到AP，一般无线网卡即工作在该模式。对照表如下：

| **分类** | **AP模式** | **SP模式** |
| --- | --- | --- |
|接入网络|接受|不接受|
|网卡|需要|不需要|
|终端|有线|无线|

### WiFi模块原理图

![WIFI模块硬件电路](/assets/CC2530_OneNET/1.png)

<!-- ------------------------ -->
## 实验步骤

   
① WIFI模块、温湿度模块分别安装CC2530底座上，CC Debugger连接电脑与协调器节点底座，如下图所示：

![模块组装](/assets/CC2530_OneNET/2-1.png)

② 轻按CCDebugger复位按键，指示灯变绿，表示连接正常。如下图:

![模块组装](/assets/CC2530/5.png)
    
③ 访问[github](https://github.com/aiotcom/eps),进入github界面后点击Code，Clone HTTPS安全链接，如下图所示：

![操作步骤](/assets/STM32/38.jpg)


进入[Github](https://github.com/aiotcom/eps)，点击 `Code` -> `DownLoad ZIP` 下载所有工程代码。如下图所示：  
![下载代码](/assets/STM32/47.jpg)  
如果电脑没有公网，可以进：U盘\实验教程与代码选择相应的代码。

④ 打开已经安装NODERED的电脑：
   
```c
D:\> ipconfig /all   //查看本机IP
```
### 本机IP
![本机IP](/assets/CC2530_NODERED/NODERED-LED-GETIP.png)
```c
D:\> NODE-RED  //启动本机nodered服务
```
### 启动NODE RED服务
![NODERED服务](/assets/CC2530_NODERED/NODERED-START.png)
⑤ 打开浏览器，输入地址127.0.0.1:1880 打开本机node red 主页：

![NODERED主页](/assets/CC2530_NODERED/NODERED-INPUT0.png)
### 导入本次试验的NODE RED流程
![NODERED导入1](/assets/CC2530_NODERED/NODERED-INPUT1.png)
![NODERED导入2](/assets/CC2530_NODERED/NODERED-INPUT2.png)
![NODERED导入3](/assets/CC2530_NODERED/NODERED-INPUT3.png)

### 部署本次试验NODE RED流程
![NODERED部署](/assets/CC2530_NODERED/NODERED-TEMPHUM.png)

### 打开本次试验的UI界面(输入地址127.0.0.1:1880/ui)
![NODERED图像界面](/assets/CC2530_NODERED/NODERED-UI0.png)

⑥ 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`基于CC2530 NODERED实验\1.NODERED平台显示温湿度实验\Projects\zstack\Samples\SampleApp\CC2530DB\SampleApp.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530_OneNET/3.jpg) 

⑦ 待工程启动完毕，修改PANID或者信道防止与他人网络冲突，终端与协调器代码共用该配置文件如图(一个教室多组同时实验室时，必须修改PANID!!!)：
   
![修改参数](/assets/CC2530_OneNET/4.png)  

⑧ 设置工程配置为`CoordinatorEB`。

![修改参数](/assets/CC2530_OneNET/5.png) 

⑨ 打开`WiFiGate.h`，修改WIFI热点的名字与密码，以及根据自己的NODE RED服务器的IP地址和端口，修改connect_IP，如下图:

![修改参数](/assets/CC2530_NODERED/NODERED-WIFI.png) 

⑩ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑪ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg)

![代码下载成功](/assets/CC2530/10.jpg) 

⑫ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑬ 将CCDebugger连接到终端节点，选择工程的配置为`EndDeviceEB`，如图：
       
![选择文件](/assets/CC2530_OneNET/7.png)  

⑭ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑮ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑯ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑰ 移除`CC Debugger`仿真器，采用USB线供电，接协调器节点的底座。
    
![USB线供电](/assets/CC2530_OneNET/8.png) 

⑱ 观察WIFI模块状态灯---长亮表示已经连接到路由器：

![WIFI模块指示灯](/assets/CC2530_NODERED/WIFI-ONLINE3.jpg) 

⑲ NODE RED平台操作。(流程位于：`基于CC2530 NODERED实验\1.NODERED平台显示温湿度实验、`)。具体操作参考[node red](https://codelabs.stepiot.com/docs/ASTM32_NodeRED-082.html)平台应用手册。

![NODERED数据呈现](/assets/CC2530_NODERED/TEMP-HUM.png)


<!-- ------------------------ -->
## 代码讲解


### 终端节点(EndDeviceEB)

① 程序目录结构，源代码文件如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![代码目录结构](/assets/CC2530_OneNET/11.jpg)

② EndDevice.c->SampleApp_Init()函数是应用代码的入口函数，对温湿度模块初始化、初始化`Point_To_Point_DstAddr`结构，注册端点、启动传感器数据采集。

```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID   = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID  = 0; 
        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//调试串口初始化
        SHT2x_Init();//初始化温湿度传感器
        printf("i am end device\r\n");//串口打印
        //点对点通讯定义
        Point_To_Point_DstAddr.addrMode = (afAddrMode_t)Addr16Bit;//点播
        Point_To_Point_DstAddr.endPoint = SAMPLEAPP_ENDPOINT;//目标端点号
        /*发给协调器,协调器地址固定为0x0000*/
        Point_To_Point_DstAddr.addr.shortAddr = 0x0000;
        //填写端点
        EndDevice_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        EndDevice_epDesc.task_id    = &SampleApp_TaskID;
        EndDevice_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&EndDevice_SimpleDesc;
        EndDevice_epDesc.latencyReq = noLatencyReqs;
        //注册端点
        afRegister( &EndDevice_epDesc );
        //开始本地显示温湿度数值
        osal_start_timerEx( SampleApp_TaskID, READ_TEMP_HUMI_MSG_EVT,1000);
    }
```

`EndDevice.c`->`SampleApp_ProcessEvent()`函数是任务处理函数。在该函数中调用`SHT2x_DisplayHumi()`、`SHT2x_DisplayTemp()`函数读取传感器数据显示在数码管上。定时调用`SendTemperatureHumityToCoordinator()`函数向协调器发送传感器数据。

```c
    if(events & READ_TEMP_HUMI_MSG_EVT)//READ_TEMP_HUMI_MSG_EVT
    {//读取传感器数据并显示在数码管上
        TempHumiShowSwitch = 1 - TempHumiShowSwitch;
        /*读取温湿度并数码管显示*/
        TempHumiShowSwitch?SHT2x_DisplayHumi():SHT2x_DisplayTemp();
        /*启动定时器1000ms后超时,触发一次READ_TEMP_HUMI_MSG_EVT事件*/
        osal_start_timerEx( SampleApp_TaskID, READ_TEMP_HUMI_MSG_EVT,1000);
    }
    
    if(events & SEND_TEMP_HUMI_MSG_EVT)//是SEND_TEMP_HUMI_MSG_EVT事件
    {
        SendTemperatureHumityToCoordinator();//向协调器发送温湿度数据  
        /*启动定时器2000ms后超时，触发一次SEND_TEMP_HUMI_MSG_EVT事件*/    
        osal_start_timerEx( SampleApp_TaskID, SEND_TEMP_HUMI_MSG_EVT,2000);
    }
```

### 协调器节点(CoordinatorEB)

① 程序目录结构，源代码文件如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![代码目录结构](/assets/CC2530_OneNET/12.jpg)

② `Coordinator.c`->`SampleApp_Init()`函数是应用代码的入口函数。
   
```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID = 0;  
        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//调试串口初始化
        //OLED_Init();//初始化OLED 
        //OLED_P8x16Str(0,0,"coordinator");//OLED显示提示信息
        
        //填写端点
        Coordinator_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        Coordinator_epDesc.task_id    = &SampleApp_TaskID;
        Coordinator_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&Coordinator_SimpleDesc;
        Coordinator_epDesc.latencyReq = noLatencyReqs;
        //注册端点
        afRegister( &Coordinator_epDesc );
    }
```

`Coordinator.c` ->`SampleApp_ProcessEvent()`函数是任务处理函数。当前接收无线信道的数据时，进入`SampleApp_MessageMSGCB()`函数，如图：

```c
    uint16 SampleApp_ProcessEvent( uint8 task_id, uint16 events )
    {
        afIncomingMSGPacket_t *MSGpkt;
        (void)task_id;//Intentionally unreferenced parameter

        if ( events & SYS_EVENT_MSG )
        {
            MSGpkt = (afIncomingMSGPacket_t *)osal_msg_receive( SampleApp_TaskID );
            while ( MSGpkt )
            {
                switch ( MSGpkt->hdr.event )
                {        
                    //Received when a messages is received (OTA) for this endpoint
                    case AF_INCOMING_MSG_CMD:
                    SampleApp_MessageMSGCB( MSGpkt );
                    break;

                    //Received whenever the device changes state in the network
                    case ZDO_STATE_CHANGE:
                    SampleApp_NwkState = (devStates_t)(MSGpkt->hdr.status);
                    if(SampleApp_NwkState == DEV_ZB_COORD)
                    {
                        printf("coord ready!");
                    }
                    break;

                    default:
                    break;
                }

            //Release the memory
            osal_msg_deallocate( (uint8 *)MSGpkt );

            //Next - if one is available
            MSGpkt = (afIncomingMSGPacket_t *)osal_msg_receive( SampleApp_TaskID );
            }

            //return unprocessed events
            return (events ^ SYS_EVENT_MSG);
        }//if ( events & SYS_EVENT_MSG )

        //Discard unknown events
        return 0;
    }
```

`SampleApp_MessageMSGCB()`函数中将传感器的数据解析并通过`SendToWiFiNetwork()`函数发送到NODE RED平台。

```c
    void SampleApp_MessageMSGCB( afIncomingMSGPacket_t *pkt )
    { 
        uint8 ShowTemperatureBuf[ ]="temperture=xxC";
        uint8 ShowHumiBuf[ ]="humidity=xx%";
        switch ( pkt->clusterId )
        {
            case TEMP_HUMI_CLUSTERID://是温湿度数据命令码
                ShowTemperatureBuf[11] = pkt->cmd.Data[0]/10 + 0x30;//HEX 转成ASCII
                ShowTemperatureBuf[12] = pkt->cmd.Data[0]%10 + 0x30;//HEX 转成ASCII 
                printf("Temperature=%dC\r\n",pkt->cmd.Data[0]);//串口打印
                OLED_P8x16Str(0,4,ShowTemperatureBuf);//OLED屏显示 
                
                ShowHumiBuf[9]  = pkt->cmd.Data[1]/10 + 0x30;//HEX 转成ASCII
                ShowHumiBuf[10] = pkt->cmd.Data[1]%10 + 0x30;//HEX 转成ASCII 
                printf("Humidity=%d%%\r\n",pkt->cmd.Data[1]);//串口打印        
                OLED_P8x16Str(0,6,ShowHumiBuf);//OLED屏显示  
                        
                //转成字符字串
                sprintf((void*)tempbuf,"%02d%02d",pkt->cmd.Data[0],pkt->cmd.Data[1]);
                SendToWiFiNetwork(tempbuf,strlen((const char*)tempbuf));//发送到OneNET
            break;
        }
    }
```

`WiFiGate.c`中`WiFiGate_Init()`函数初始化WIFI模块的IO及模块的通信串口。

```c
    void WiFiGate_Init( uint8 task_id )
    {
        WiFiGate_TaskId = task_id;//保存任务ID号
        /*2s后触发WIFI_PROCESS_PRODIC*/
        osal_start_timerEx( WiFiGate_TaskId, WIFI_PROCESS_PRODIC,2000); 
        UartInit(HAL_UART_PORT_0,HAL_UART_BR_115200);//初始化WIFI模块串口
            
        /*初始化WIFI模块控制IO*/
        P1SEL &= ~(BV(5)|BV(6));
        P1DIR |= BV(5)|BV(6);
        P1_5 = 1;
        P1_6 = 0;
            
        printf("wifi connect start\r\n");//打印调试
    }
```

`WiFiGate.c`中`WiFiGate_ProcessEvent()`，调用`WiFiGate_InitProcess()`初始化WIFI模块。初始化完成`WiFiModeInitDone`置1。

```c
    uint16 WiFiGate_ProcessEvent( uint8 task_id, uint16 events )
    {
        (void)task_id;//Intentionally unreferenced parameter 没有使用到
            
        if(events & WIFI_PROCESS_PRODIC){
            /*100ms后触发一次WIFI_PROCESS_PRODIC事件*/
            osal_start_timerEx( WiFiGate_TaskId, WIFI_PROCESS_PRODIC,100);
                    
            if((ConnectState==0)&&(WiFi_InitProcess())){//初始化WIFI
                /*如果初始化完成*/
                ConnectState = 1;
                WiFiModeInitDone = 1;//WIFI模块初始化完成
                }
            else if(ConnectState == 1){
                    
            }
            return (events ^ WIFI_PROCESS_PRODIC);
        }
        return 0;
    } 
```


<!-- ------------------------ -->
## 常见问题


1. 弹出警告窗口，不能下载程序。

   - 请确认CCDebugger驱动否安装。
   - 轻按CCDebugger仿真器的按键，指示灯不是绿色连接有问题。
   - CCDebugger仿真器是否正常接入到底座，参考实验步骤第一步。

2. 下载代码后程序没观察到实验现象。
   
   - 请重新上电，或者按下底座上的复位按键。
   - 模块没有安装稳妥。
   - 两个节点的PANID、信道是否相同。

3. WIFI模块状态指示灯没有亮。

    - WIFI名字、WIFI密码、NODERED服务器IP地址、端口是否正确。



<!-- ------------------------ -->
## 实验思考


1. 解读一下设备与NODE RED的通讯协议，并尝试修改一下通讯协议，并使得NODE RED的UI界面，依然可以正常显示温湿度数据。

