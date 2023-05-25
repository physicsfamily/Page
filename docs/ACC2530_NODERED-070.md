---
layout: post
title:  "070_CC2530_NODERED平台控制蜂鸣器实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# CC2530_NODERED平台控制蜂鸣器实验
<!-- ------------------------ -->
## 实验内容


- NODERED平台创建按键应用。
- 在NODERED平台控制蜂鸣器模块。

<!-- ------------------------ -->
## 实验目的


- NODERED平台应用创建。

<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 2个 |  · |
| 3 | 蜂鸣器模块 | 1个 | ·  |
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

[蜂鸣器模块](https://docs.stepiot.com/docs/aiot018)

![蜂鸣器模块](/assets/CC2530_NODERED/BEEP.png)

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

   
① 将WIFI模块、蜂鸣器模块分别安装CC2530底座上，CC Debugger连接电脑与协调器节点底座，如下图所示：

![模块组装](/assets/CC2530_OneNET/73-1.jpg)

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
![NODERED部署](/assets/CC2530_NODERED/NODERED-BEEP.png)

### 打开本次试验的UI界面(输入地址127.0.0.1:1880/ui)
![NODERED图像界面](/assets/CC2530_NODERED/NODERED-UI1.png)

⑥ 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`基于CC2530 NODERED实验\9.NODERED平台控制蜂鸣器实验\Projects\zstack\Samples\SampleApp\CC2530DB\SampleApp.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530_NODERED/6.png) 

⑦ 待工程启动完毕，修改PANID或者信道防止与他人网络冲突，终端与协调器代码共用该配置文件如图：
   
![修改参数](/assets/CC2530_OneNET/4.png)  

⑧ 设置工程配置为`CoordinatorEB`。

![修改参数](/assets/CC2530_OneNET/5.png) 

⑨ 打开`WiFiGate.h`，修改WIFI热点的名字与密码，以及根据自己的NODE RED服务器的IP地址和端口，修改connect_IP，如下图:

![修改参数](/assets/CC2530_NODERED/NODERED-WIFI.png) 

![修改参数](/assets/CC2530_OneNET/6.png) 

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
    
![USB线供电](/assets/CC2530_NODERED/72.jpg) 


⑱ 观察WIFI模块状态灯---长亮表示已经连接到路由器：

![WIFI模块指示灯](/assets/CC2530_NODERED/WIFI-ONLINE3.jpg) 

⑲ NODERED平台显示控制实验。(脚本位于：`基于CC2530 NODERED实验\8.NODERED平台控制风扇实验\风扇控制.json`)。具体操作参考[node red](https://codelabs.stepiot.com/docs/ASTM32_NodeRED-082.html)平台应用手册。

![OneNET平台控制](/assets/CC2530_NODERED/NODERED-UI1.png) 


<!-- ------------------------ -->
## 代码讲解


### 终端节点

① 程序目录结构，源代码文件如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![代码目录结构](/assets/CC2530_OneNET/63.jpg)

② `EndDevice.c`->`SampleApp_Init()`函数是应用代码的入口函数，对蜂鸣器模块初始化、初始化`Point_To_Point_DstAddr`结构，注册端点。

```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID   = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID  = 0; 

        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//用于调试
        Beep_Init();//初始化蜂鸣器控制IO  
        printf("i am end device\r\n");//串口打印   
        // 点对点通讯定义
        Point_To_Point_DstAddr.addrMode       = (afAddrMode_t)Addr16Bit; //点播
        Point_To_Point_DstAddr.endPoint       = SAMPLEAPP_ENDPOINT;      
        Point_To_Point_DstAddr.addr.shortAddr = 0x0000;//发给协调器
        
        // 填写端点
        EndDevice_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        EndDevice_epDesc.task_id    = &SampleApp_TaskID;
        EndDevice_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&EndDevice_SimpleDesc;
        EndDevice_epDesc.latencyReq = noLatencyReqs;
        
        // 注册端点
        afRegister( &EndDevice_epDesc );
    }
```

`EndDevice.c`->`SampleApp_ProcessEvent()`函数是任务处理函数。在该函数中调用`SampleApp_MessageMSGCB()`处理无线信道的指令。

```c
    // Received when a messages is received (OTA) for this endpoint
    case AF_INCOMING_MSG_CMD:
    SampleApp_MessageMSGCB( MSGpkt );
    break;
```

`SampleApp_MessageMSGCB()`，中依据指令参数，控制蜂鸣器。BeepOn=1蜂鸣器响起，BeepOn=0停止蜂鸣器。

```c
    if(pkt->cmd.Data[0] == 0){//停止蜂鸣器
		BeepOn = 0;//BeepOn=0,蜂鸣器停止
	}
	else{//打开蜂鸣器
		BeepOn = 1;//BeepOn=1，蜂鸣器响起				
	}
```

    
### 协调器节点

① 程序目录结构，源代码文件如下图。代码中有大量ZigBee底层的代码，我们只要主要关心下图中标出的文件代码，ZigBee底层的代码会使用即可。

![代码目录结构](/assets/CC2530_OneNET/64.jpg)

② `Coordinator.c`->`SampleApp_Init()`函数是应用代码的入口函数，注册端点。
   
```c
    void SampleApp_Init( uint8 task_id )
    {
        SampleApp_TaskID = task_id;
        SampleApp_NwkState = DEV_INIT;
        SampleApp_TransID = 0;  

        UartInit(HAL_UART_PORT_1,HAL_UART_BR_115200);//初始化串口用于调试
        
        Point_To_Point_DstAddr.addrMode       = (afAddrMode_t)Addr16Bit; //点播
        Point_To_Point_DstAddr.endPoint       = SAMPLEAPP_ENDPOINT;      
        Point_To_Point_DstAddr.addr.shortAddr = 0; 

        //广播通信定义 
        Boardcast_DstAddr.addrMode       = (afAddrMode_t)AddrBroadcast;
        Boardcast_DstAddr.endPoint       = SAMPLEAPP_ENDPOINT;
        Boardcast_DstAddr.addr.shortAddr = 0Xffff;
            
        // 填写端点
        Coordinator_epDesc.endPoint   = SAMPLEAPP_ENDPOINT;
        Coordinator_epDesc.task_id    = &SampleApp_TaskID;
        Coordinator_epDesc.simpleDesc = (SimpleDescriptionFormat_t *)&Coordinator_SimpleDesc;
        Coordinator_epDesc.latencyReq = noLatencyReqs;
        // 注册端点
        afRegister( &Coordinator_epDesc );
        osal_start_timerEx( SampleApp_TaskID, READ_KEY_MSG_EVT,3000); 
    }
```

`WiFiGate.c`中`WiFiGate_Init()`函数初始化WIFI模块的IO及模块的通信串口。

```c
void WiFiGate_Init( uint8 task_id )
{
  WiFiGate_TaskId = task_id;
  osal_start_timerEx( WiFiGate_TaskId, WIFI_PROCESS_PRODIC,2000); 
  UartInit(HAL_UART_PORT_0,HAL_UART_BR_115200);
    P1DIR |= 0x60;      //P1.5、P1.6定义为输出
    P1SEL &= ~0x80;     //设置P1.7为普通IO口  
    P1DIR &= ~0x80;     //按键接在P1.7口上，设P1.7为输入模式 
    P1INP &= ~0x80;     //打开P1.7上拉电阻
  printf("wifi connect start\r\n");
  WiFi_RES = 0;
  delay_ms(1);
  WiFi_RES = 1;
  WiFi_LED=0;
  //WiFi_LED_REST();
}
```

`WiFiGate.c`中`WiFiGate_ProcessEvent()`，调用`WiFiGate_InitProcess()`初始化WIFI模块。初始化完成`WiFiModeInitDone`置1。`WiFi_ReadCommand()`，获取OneNET下发的指令。解析指令调用`Send_FANCtrl()`函数发送到终端节点。

```c
  /*
wifi网关处理任务，包括初始化WiFi模块，接收数据
*/
static uint8 ConnectState = 0,len,*cptr;
static uint8 FAN_Cmd;
uint16 WiFiGate_ProcessEvent( uint8 task_id, uint16 events )
{

  (void)task_id;  // Intentionally unreferenced parameter

  if ( events & SYS_EVENT_MSG )
  {//如果是系统任务
    return (events ^ SYS_EVENT_MSG);
  }
  else
  {//如果是用户自定义任务
	if(events & WIFI_PROCESS_PRODIC){
	    osal_start_timerEx( WiFiGate_TaskId, WIFI_PROCESS_PRODIC,100);
		switch(ConnectState)
		{
			case 0:
				P1_5 = 0;
				ConnectState++;
				break; 
			case 1:
				P1_5 = 1;
				ConnectState++; 
				break;
			case 2:
			case 3:
			case 4:
				ConnectState++; 
				break;
			break;	  
			case 5:
				  switch(WiFi_Send_ATCommand("AT\r\n",20,5,"OK"))
				  {
					case WIFI_RSP_OK:
						 ConnectState++;
					break;
					case WIFI_RSP_TIMEOUT:
						 ConnectState = 0xff;
					break;  				  
				  }
				break;
			case 6:
				  switch(WiFi_Send_ATCommand("AT+CWMODE=3\r\n",20,5,"OK"))
				  {
					case WIFI_RSP_OK:
						 ConnectState++;
					break;
					case WIFI_RSP_TIMEOUT:
						 ConnectState = 0xff;
					break;  				  
				  }
				break;	
			case 7:
				  switch(WiFi_Send_ATCommand(WIFI_AP,30,10,"OK"))
				  {
					case WIFI_RSP_OK:
						 ConnectState++;
					break;
					case WIFI_RSP_TIMEOUT:
						 ConnectState = 0xff;
					break;  				  
				  }
				break;
			case 8:
				  switch(WiFi_Send_ATCommand(OneNET_IP,20,5,"OK"))
				  {
					case WIFI_RSP_OK:
						 ConnectState = 10;
					break;
					case WIFI_RSP_TIMEOUT:
						 ConnectState = 0xff;
					break;  				  
				  }
				break;		
	
			case 10:
				  switch(WiFi_Send_ATCommand(CIPMODE,20,0,"OK"))
				  {
					case WIFI_RSP_OK:
						 ConnectState++;
					break;
					case WIFI_RSP_TIMEOUT:
						 ConnectState = 0xff;
					break;  				  
				  }
				break;	
			case 11:
				  switch(WiFi_Send_ATCommand(CIPSEND,20,0,"OK"))
				  {
					case WIFI_RSP_OK:
						 ConnectState++;
					break;
					case WIFI_RSP_TIMEOUT:
						 ConnectState = 0xff;
					break;  				  
				  }
				break;		
			case 12:
				  //WiFi_Send_ATCommand(CONNECT_ONENET_KEYSTRING,20,0,"");   //跳过注册到ONENET的操作
				  ConnectState++;
				  WiFiRecvLenght = 0;
				  WiFiModeInitDone = 1;
				  memset((void*)WiFiRecvDataBuffer,0,WIFI_RECV_DATA_BUFFER_LEN);
                                  WiFi_LED_SET(0xFE0000);
				break;	
			case 13:
				  len = GET_RECV_LENGHT();
				  if(len){
					if((WiFiRecvLenght+len) >= (WIFI_RECV_DATA_BUFFER_LEN-1)){
						WiFiRecvLenght = 0;
						memset((void*)WiFiRecvDataBuffer,0,WIFI_RECV_DATA_BUFFER_LEN);
						printf("overflow\r\n");
					}						
					GET_RECV_DATA(&WiFiRecvDataBuffer[WiFiRecvLenght],len);
					WiFiRecvLenght = WiFiRecvLenght + len;			
				  }
				  if((WiFiRecvLenght)&&(!strstr((const char*)WiFiRecvDataBuffer,(const char*)"$"))){//没有~这个符号
				  	WiFiRecvLenght = 0;
					memset((void*)WiFiRecvDataBuffer,0,WIFI_RECV_DATA_BUFFER_LEN);
				  }
			          cptr = (uint8*)strstr((const char*)WiFiRecvDataBuffer,(const char*)"$BEEP,");				
				  if(cptr){
                                        FAN_Cmd = WiFiRecvDataBuffer[6]-0x30;//是打开还是关闭。
					printf("cmd=%d\r\n",FAN_Cmd);//串口调试
                                        if(FAN_Cmd>0)
                                        {
                                          Send_LEDCtrl(1,1);//发送控制命令
                                        }
                                        else
                                        {
                                          Send_LEDCtrl(1,0);//向终端节点发送命令
                                        }
					//Send_FANCtrl(FAN_Cmd);
					WiFiRecvLenght = 0;
					
				  }	
				  memset((void*)WiFiRecvDataBuffer,0,WIFI_RECV_DATA_BUFFER_LEN); 		  
			    break;
		}		
		return (events ^ WIFI_PROCESS_PRODIC);
	}
  }//if(events & WIFI_PROCESS_PRODIC){
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

3. NODERED平台设备没有上线。

    - WIFI名字、WIFI密码、NODERED服务器IP和端口等信息是否正确。


<!-- ------------------------ -->
## 实验思考


1. 在NODERED平台增加一个按键控制蜂鸣器以另一种频率响起。
