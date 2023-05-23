---
layout: post
title:  "050_Zigbee通信实验_网络控制继电器实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# Zigbee通信实验_网络控制继电器实验
<!-- ------------------------ -->
## 实验内容
Duration: 2

- 搭建实验硬件环境；
- 下载程序；
- 终端节点定时向协调器发送LED灯控制数据。
  
<!-- ------------------------ -->
## 实验目的
Duration: 3

- 熟悉Z-Stack协议栈；
- 熟悉Z-Stack协议栈的源文件架构；
- 熟悉Z-Stack常见接口函数调用；
- 熟悉基于Z-Stack的无线通信；
- 掌握点播通信中的关键参数设置。

<!-- ------------------------ -->
## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 2个 |  · |
| 3 | LED模块 | 1个 | ·  |
| 4 | 继电器模块 | 1个 | ·  |
| 5 | CC Debugger 仿真器和连接线| 1套 | ·  |
| 6 | USB线| 1根 | ·  |
| 7 | 实验代码 | 1份 | ·  |

### 实验所需软件

- [IAR](https://codelab.stepiot.com/codelabs/IAR_077/index.html?index=..%2F..index#0) 驱动安装步骤

- [CC Debugger](https://codelab.stepiot.com/codelabs/CC_Debugger_081/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

[CC2530底座](https://docs.stepiot.com/docs/aiot017) 

![实验硬件](/assets/BASE_CC2530/3.png)

CC Debugger 仿真器和连接线

![实验硬件](/assets/BASE_CC2530/4.png)

[LED模块](https://docs.stepiot.com/docs/aiot003)

![LED模块](/assets/Zigbee/49.png)

[继电器模块](https://docs.stepiot.com/docs/aiot014)

![继电器模块](/assets/Zigbee/48.png)

USB线

![USB线](/assets/CC2530/2.png)

<!-- ------------------------ -->
## 实验要求
Duration: 4

- 了解ZigBee通过程中网络地址类型；
- 了解afAddrType_t结构体；
- 掌握ZigBee的网络地址类型afAddrMode_t枚举类型；
- 掌握AF_DataRequest()发送函数的参数定义。

<!-- ------------------------ -->
## 实验原理
Duration: 15

### ZigBee基本通信方式

ZigBee的通讯方式主要有三种点播、组播、广播。点播顾名思义就是点对点通信，也是2个设备之间的通讯，不允许有第三个设备收到信息；组播就是把网络中的节点分组，每一个组员发出的信息只有相同组号的组员才能收到。广播最广泛的也就是1个设备上发出的信息所有设备都能接收到。这也是ZigBee通信的基本方式。

### ZigBee网络地址类型

|序号|地址类型|说明|
| :--: | :--: | :--: |
|1|AddrNotPresent|依照绑定表|
|2|Addr16Bit|16位地址，常用于点播|
|3|Addr64Bit|Addr64Bit|
|4|AddrGroup|组播|
|5|AddrBroadcast|广播|

以上的的地址类型均保存在afAddrMode_t。在实际使用过程中考虑功耗，常用使用16位地址，原因16位地址是通过路由算法计算出来的，在传输路径上通过的节点较少最大的节省了网络功耗。16位地址可分成如下几类：


|序号|16位地址分类|说明|
| :--: | :--: | :--: |
|1|0xFFFF|对所有设备广播，包括睡眠|
|2|0xFFFE|间接传输，通过绑定表寻找网络短地址|
|3|0xFFFD|对没休眠的设备广播|
|4|0xFFFC|给协调器和路由器广播|
|5|0x0000|给协调器通信|
|6|0x0001-0xFFFB|用户自设定的目标地址|

<!-- ------------------------ -->
## 实验步骤
Duration: 15
   
① 将LED模块安装在CC2530底座上，继电器模块暂时不安装，CC Debugger连接电脑与协调器节点的底座，如下图所示：

![模块组装](/assets/Zigbee/50.jpg)

② 轻按CCDebugger复位按键，指示灯变绿，表示连接正常。如下图:

![模块组装](/assets/CC2530/5.png)
    
③ 访问[github](https://github.com/aiotcom/eps),进入github界面后点击Code，Clone HTTPS安全链接，如下图所示：

![操作步骤](/assets/STM32/38.jpg)

④ 打开电脑终端，进入工作目录workspace (workspace 为工程文件夹所在目录)：
   
```c
$ cd workspace
```

⑤ 运行`clone`命令：

```c
$ git clone https://github.com/aiotcom/eps.git
```

下载目录至指定文件夹下。  
如果提示“command not found”表示电脑没有安装Git，请至[Git](https://git-scm.com/downloads)官网下载。  
如果电脑没有安装 Git 软件，也可以进入[Github](https://github.com/aiotcom/eps)，点击 `Code` -> `DownLoad ZIP` 下载所有工程代码。如下图所示：  
![下载代码](/assets/STM32/47.jpg)  
如果电脑没有公网，可以进：D盘\实验教程与代码选择相应的代码。

⑥ 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`ZigBee通信实验\7.ZigBee网络控制继电器实验\Projects\zstack\Samples\SampleApp\CC2530DB\SampleApp.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/Zigbee/51.jpg)  

⑦ 修改PANID或者信道防止与他人网络冲突，终端与协调器代码共用该配置文件如图，修改后保存。
   
![修改PANID和信道](/assets/Zigbee/25.png)  

⑧ 设置工程配置为“coordinatorEB”如图：

![工程配置为coordinatorEB](/assets/Zigbee/14.png) 

⑨ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑩ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑪ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑫ 将CC Debuger连接到终端节点，选择工程的配置“EndDeviceEB”如图。
       
![选择文件](/assets/Zigbee/15.png)  

⑬ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑭ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑮ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑯ 移除`CC Debugger`仿真器，将继电器模块安装于CC2530底座，采用USB线供电（接任意底座），底座拼接。
    
![退出仿真](/assets/Zigbee/52.png) 

⑰ 按下LED模块上的S1按键，继电器开启，再按一次继电关闭。


<!-- ------------------------ -->
## 代码讲解
Duration: 15

### 终端节点

① 程序目录结构，源代码文件如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码。ZigBee底层的代码会使用即可。

![代码目录结构](/assets/Zigbee/53.jpg) 

② `EndDevice.c`->`SampleApp_Init()`函数是应用代码的入口函数，对继电器模块初始化、初始化`Point_To_Point_DstAddr`结构，注册端点。

```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID   = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID  = 0; 

        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//用于调试
        Relay_Init();//初始化继电器模块控制管脚。  
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
    }
```

`EndDevice.c`->`SampleApp_ProcessEvent()`函数是任务处理函数。在该函数中调用`SampleApp_MessageMSGCB()`处理无线信道的指令。

```c
    //Received when a messages is received (OTA) for this endpoint
    case AF_INCOMING_MSG_CMD:
    SampleApp_MessageMSGCB( MSGpkt );
    break;
```

`SampleApp_MessageMSGCB()`，中依据指令参数，控制继电开启、关闭。`Relay1_OFF()`关闭继电器，`Relay1_ON()`，打开继电器。

```C
    case RELAY_CONTROL_CLUSTERID:
		printf("get cmd:%d,%d\r\n",pkt->cmd.Data[0],pkt->cmd.Data[1]);
		if(pkt->cmd.Data[0] == 1){//继电器1
			if(pkt->cmd.Data[1] == 0){//关闭继电器
				Relay1_OFF();
			}
			else{//开启继电器
				Relay1_ON();
            }
```

### 协调节点

① 程序目录结构，源代码文件如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码。ZigBee底层的代码会使用即可。

![代码目录结构](/assets/Zigbee/54.jpg) 

② `Coordinator.c`->`SampleApp_Init()`函数是应用代码入口函数，对LED模块初始化。
      
```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID = 0;  

        LED_Init();
        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//初始化串口用于调试
        
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
        //启动读按键
        osal_start_timerEx( SampleApp_TaskID, READ_KEY_MSG_EVT,1000); 
    }
```

`Coordinator.c` ->`SampleApp_ProcessEvent()`函数是任务处理函数，在其中检测按键S1是否按下，按键S1按键下，发送继电器控制指令。

```c
    if(events & READ_KEY_MSG_EVT){//是READ_KEY_MSG_EVT事件 
		if(KEY_Scan()&S1_PRES){//检测按键
			printf("S1_PRES\r\n");
			LED_State = 1 - LED_State;
			Send_RelayCtrl(1,LED_State);//发送控制命令
		}
        /*100ms 后触发READ_KEY_MSG_EVT*/
        osal_start_timerEx(SampleApp_TaskID,READ_KEY_MSG_EVT,100);
    }
```




<!-- ------------------------ -->
## 常见问题
Duration: 5

1. 弹出警告窗口，不能下载程序。

   - 请确认CCDebugger驱动否安装。
   - 轻按CCDebugger仿真器的按键，指示灯不是绿色连接有问题。
   - CCDebugger仿真器是否正常接入到底座，参考实验步骤第一步。

2. 下载代码后程序没观察到实验现象。
   
   - 请重新上电，或者按下底座上的复位按键。
   - 模块没有安装稳妥。
   - 两个节点的PANID、信道是否相同。




<!-- ------------------------ -->
## 实验思考
Duration: 5

1. 编写代码实现按键S2控制继电器2，继电器在开启2秒后，自动关闭。

