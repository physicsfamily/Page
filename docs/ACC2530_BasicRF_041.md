---
layout: post
title:  "041_CC2530_BasicRF通信实验_无线点灯实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# CC2530 BasicRF 通信实验_无线点灯实验
<!-- ------------------------ -->
## 实验内容


- 搭建实验硬件环境；
- 使用一个底座作为发送端，另一个底座作为接收端使用两个LED模块，发送端的LED模块按键控制接收端LED模块的LED灯。

<!-- ------------------------ -->
## 实验目的


- 熟悉LED模块的使用；
- 了解ZigBee无线传输数据原理；
- 熟悉ZigBee无线收发数据。

<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 2个 |  · |
| 3 | LED模块 | 2个 | ·  |
| 4 | CC Debugger 仿真器和连接线| 1套 | ·  |
| 5 | USB线| 1根 | ·  |
| 6 | 无线点灯实验代码 | 1份 | ·  |

### 实验所需软件

- [IAR](https://codelab.stepiot.com/codelabs/IAR_077/index.html?index=..%2F..index#0) 驱动安装步骤

- [CC Debugger](https://codelab.stepiot.com/codelabs/CC_Debugger_081/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

[CC2530底座](https://docs.stepiot.com/docs/aiot017) 

![实验硬件](/assets/BASE_CC2530/3.png)

CC Debugger 仿真器和连接线

![实验硬件](/assets/BASE_CC2530/4.png)

[LED模块](https://docs.stepiot.com/docs/aiot001)

![LED模块](/assets/BASE_CC2530/6.png)

USB线

![USB线](/assets/CC2530/2.png)

<!-- ------------------------ -->
## 实验要求


- 了解ZigBee无线收发机制；
- 熟悉appSwitch()函数应用；
- 了解下面的关键字：
  - CCM - Counter with CBC-MAC (mode of operation)
  - HAL - Hardware Abstraction Layer (硬件抽象层)
  - PAN - Personal Area Network （个人局域网）
  - RF - Radio Frequency （射频）
  - RSSI - Received Signal Strength Indicator (接收信号强度指示) 


<!-- ------------------------ -->
## 实验原理


### 工程介绍

![BasicRF 软件文件夹架构](/assets/CC2530/37.png)

### Basic RF layer 介绍及其工作过程

在介绍Basic RF之前，来看看这个实验例程设计的大体结构，如图所示，Basic RF例程的软件设计框图就如一座建筑物。

![Basic RF例程软件设计框图](/assets/CC2530/38.png)

Hardware layer放在最底层，是实现数据传输的基础。Hardware Abstraction layer它提供了一种接口来访问TIMER、GPIO、UART、ADC等。这些接口都通过相应的函数进行实现。

Basic RF layer为双向无线传输提供一种简单的协议。

Application layer是用户应用层，它相当于用户使用Basic RF层和HAL的接口，也就是说我们通过在Application layer就可以使用到封装好的Basic RF和HAL的函数。本例程的要求就是读者理解掌握Basic RF。

### Basic RF layer简介

Basic RF由TI公司提供，它包含了IEEE 802.15.4标准的数据包的收发功能但并没有使用到协议栈，它仅仅是是让两个结点进行简单的通信，也就是说Basic RF仅仅是包含着IEEE 802.15.4标准的一小部分而已。其主要特点有：
- 不会自动加入协议、也不会自动扫描其他节点也没有组网指示灯（LED3）；
- 没有协议栈里面所说的协调器、路由器和终端的区分，节点的地位是相等的；
- 没有自动重发的功能。Basic RF layer 为双向无线通信提供了一个简单的协议，通过这个协议能够进行数据的发送和接收。Basic RF 还提供了安全通信所使用的CCM-64身份验证和数据加密，它的安全性读者可以通过在工程文件里面定义SECURITY_CCM在Project->Option里面就可以选择。


<!-- ------------------------ -->
## 实验步骤

   
① 将两个LED模块分别安装在CC2530底座上，CC Debugger连接电脑与按键节点底座，如下图所示：

![模块组装](/assets/CC2530/39.png)

② 轻按CCDebugger复位按键，指示灯变绿，表示连接正常。如下图:

![模块组装](/assets/CC2530/5.png)
    
③ 访问[github](https://github.com/aiotcom/eps),进入github界面后点击Code，Clone HTTPS安全链接，如下图所示：

![操作步骤](/assets/STM32/38.jpg)

④ 打开电脑终端，进入工作目录workspace (workspace 为工程文件夹所在目录)：
   
```shell
$ cd workspace
```

⑤ 运行`clone`命令：

```shell
$ git clone https://github.com/aiotcom/eps.git
```

下载目录至指定文件夹下。  
如果提示“command not found”表示电脑没有安装Git，请至[Git](https://git-scm.com/downloads)官网下载。  
如果电脑没有安装 Git 软件，也可以进入[Github](https://github.com/aiotcom/eps)，点击 `Code` -> `DownLoad ZIP` 下载所有工程代码。如下图所示：  
![下载代码](/assets/STM32/47.jpg)  
如果电脑没有公网，可以进：D盘\实验教程与代码选择相应的代码。

⑥ 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`CC2530_BasicRF通信实验\1.无线点灯实验\key_node\ide\cc2530_sw_examples.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530/40.jpg)  

⑦ 待工程启动完毕，打开main.c，修改RF_CHANNEL或者PAN_ID，避免与其他学员的冲突。
   
![修改参数](/assets/CC2530/41.png)  

⑧ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑨ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑩ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑪ 将CCDebugger连接到灯光节点，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`CC2530_BasicRF通信实验\1.无线点灯实验\Light_node\ide\cc2530_sw_examples.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530/42.jpg)  

⑫ 待工程启动完毕，打开main.c,修改RF_CHANNEL或者PAN_ID,避免与其他学员的冲突，参数必须与步骤7的一致。
    
![修改参数](/assets/CC2530/43.png)  

⑬ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑭ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑮ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑯ 移除`CC Debugger`仿真器，采用USB线供电（接任意底座），底座拼接。
    
![退出仿真](/assets/CC2530/44.png) 

⑰ 开始实验：
    
按下按键节点的S1按键，灯光节点的LED1的状态改变；
    
按下按键节点的S2按键，灯光节点的LED2的状态改变；

按下按键节点的S3按键，灯光节点的LED3的状态改变；

按下按键节点的S4按键，灯光节点的LED4的状态改变。


<!-- ------------------------ -->
## 代码讲解


### 按键节点

① 程序目录结构，源代码文件如下图。其中无线通信为官方提供源代码库。能够掌握其关键API函数即可。

![代码目录结构](/assets/CC2530/45.jpg) 

② main.c代码对LED模块按键的控制IO初始化，无线RF层代码初始化。初始化完成后，调用appSwitch()函数，在函数中进行按键检测，根据按键发出不同的控制指令。

```c
    void main(void)
    {
        //Initalise board peripherals 初始化外围设备
        halBoardInit();
        KEY_Init();//初始化LED模块上的按键
        // Initalise hal_rf 硬件抽象层的 rf 进行初始化
        if(halRfInit()==FAILED)
        {
            HAL_ASSERT(FALSE);
        }
        appSwitch();//节点为按键    
    }
```

appSwitch()中调用KEY_Scan(0)进行按键检测，调用basicRfSendPacket()将数据发出。

```c
    while (TRUE)
    {
        pTxData[1] = KEY_Scan(0);//按键扫描

        switch(pTxData[1])
        {
          case S1_PRES://按键1
            basicRfSendPacket(LIGHT_ADDR, pTxData, APP_PAYLOAD_LENGTH);//发送数据
            break;
          case S2_PRES://按键2
            basicRfSendPacket(LIGHT_ADDR, pTxData, APP_PAYLOAD_LENGTH);//发送数据
            break;
          case S3_PRES://按键3
            basicRfSendPacket(LIGHT_ADDR, pTxData, APP_PAYLOAD_LENGTH);//发送数据
            break;
          case S4_PRES://按键4
            basicRfSendPacket(LIGHT_ADDR, pTxData, APP_PAYLOAD_LENGTH);//发送数据
            break;
          default: break;
        }
        
        delay_ms(100);//100ms检测一次按键
    }
```

### 灯光节点

① 程序目录结构，源代码文件如下图。其中无线通信为官方提供源代码库。能够掌握其关键API函数即可。

![代码目录结构](/assets/CC2530/46.jpg) 

② main.c代码对LED模块LED灯控制IO初始化，无线RF层代码初始化。初始化完成后，调用appLight ()函数，在函数接收无线指令，并控制相应的灯灯。
   
```c
    void main(void)
    {
        halBoardInit();//单片机初始化
            LED_Init();//LED模块上的LED灯控制IO初始化
            
        // Initalise hal_rf 硬件抽象层的 rf 进行初始化
        if(halRfInit()==FAILED)
        {//如果配置失败
            HAL_ASSERT(FALSE);
        }
        
        appLight();//无线控制命令接收处理
    }
```

appLight()中调用basicRfPackeyIsReady()判断是否接收数据，调用basicRfReceive()将数据数据读出。 

```c
    while (TRUE)
    {
        while(!basicRfPacketIsReady());//开启接收

        if(basicRfReceive(pRxData, APP_PAYLOAD_LENGTH, NULL)>0)//接收发送到该地址的数据
        {
			if(pRxData[0] == CMD_LIGHT_CTRL){//是灯控制命令
				switch(pRxData[1])
				{
					case S1_PRES://按键1
					MCU_IO_TGL_PREP(0,2);//P0.2 IO输出反转
					break;
					case S2_PRES://按键2
					MCU_IO_TGL_PREP(0,3);//P0.3 IO输出反转
					break;
					case S3_PRES://按键3
					MCU_IO_TGL_PREP(1,6);//P1.6 IO输出反转
					break;
					case S4_PRES://按键4
					MCU_IO_TGL_PREP(1,7);//P1.7 IO输出反转
					break;
					default: break;
				}
			/*清空缓冲*/
			pRxData[0] = 0;
			pRxData[1] = 0;
		}
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
   - 两个节点的PANID、信道是否相同，灯光节点的地址是否填写正确。

     
<!-- ------------------------ -->
## 实验思考


1. 修改代码实现按键S1控制LED1亮、按键S2控制LED1灭。
