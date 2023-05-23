---
layout: post
title:  "042_CC2530_BasicRF通信实验_RSSI采集实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# CC2530 BasicRF 通信实验_RSSI采集实验
<!-- ------------------------ -->
## 实验内容
Duration: 2

- 搭建实验硬件环境；
- 下载程序；
- 完成RSSI采集。
  
<!-- ------------------------ -->
## 实验目的
Duration: 3

- 熟悉OLED模块的使用；
- 了解ZigBee无线传输数据原理；
- 了解RSSI获得方法。

<!-- ------------------------ -->
## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 2个 |  · |
| 3 | OLED模块 | 1个 | ·  |
| 4 | CC Debugger 仿真器和连接线| 1套 | ·  |
| 5 | USB线| 1根 | ·  |
| 6 | RSSI采集实验代码 | 1份 | ·  |

### 实验所需软件

- [IAR](https://codelab.stepiot.com/codelabs/IAR_077/index.html?index=..%2F..index#0) 驱动安装步骤

- [CC Debugger](https://codelab.stepiot.com/codelabs/CC_Debugger_081/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

[CC2530底座](https://docs.stepiot.com/docs/aiot017) 

![实验硬件](/assets/BASE_CC2530/3.png)

CC Debugger 仿真器和连接线

![实验硬件](/assets/BASE_CC2530/4.png)

[OLED模块](https://docs.stepiot.com/docs/aiot002)

![OLED模块](/assets/BASE_CC2530/5.png)

USB线

![USB线](/assets/CC2530/2.png)

<!-- ------------------------ -->
## 实验要求
Duration: 4

- 理解实验原理；
- 了解ZigBee无线收发机制。

<!-- ------------------------ -->
## 实验原理
Duration: 15

### RSSI讲解

RSSI：Received Signal Strength Inducation 接收的信号强度指示，无线发送层的可选部分，用来判定连接质量，以及是否增大广播发送强度。

RSSI技术是通过接收到的信号强弱测定信号点与接收点的距离，进而根据相应数据进行定位计算的一种定位技术，如无线传感的ZigBee网络CC2431芯片的定位引擎就采用这种技术、算法。

接收机测量电路所得到的接收机输入的平均信号强度指示。这一测量值一般不包括天线增益或传输系统的损耗。RSSI的实现是在反向通道基带接收滤波器之后进行的。

为了获取反向信号的特征，在RSSI的具体实现中做出了如下处理：在104us内进行基带lQ功率积分得到RSSI的瞬时值，即RSSI(瞬时) = sum（l2+Q2）；然后在约1秒对8192个RSSI的瞬时值进行平均得到RSSI的平均值，即RSSI（平均） = sum（RSSI（瞬时）/8192），同时给出1秒内RSSI瞬时值的最大值和RSSI瞬时值大于某一门限时的比率（RSSI瞬时值大于某一门限的个数/8192）。由于RSSI是通过在数字域进行功率积分而后反推到天线口得到的，反向通道信号传输特性的不一致会影响RSSI的精度。

CC2530芯片中有专门读取RSSI值的寄存器，当数据包接收后，CC2530芯片中的处理器将该数据包的RSSI值写入寄存器。如图所示：

![RSSI产生过程](/assets/CC2530/47.png)

RSSI值和接收信号功率的换算关系如下：P=RSSI_VAL+RSSI_OFFSET[dBm]其中，RSSI_OFFSET是经验值，一般取-45，在收发节点距离固定的情况下，RSSI值随发射功率线性增长，如下图所示：

![RSSI随发射功率变化曲线](/assets/CC2530/48.png)

CC2530芯片有一个内置的接收信号强度指示器，其数值为8位有符号的二进制补码，可以从寄存器RSSIL.RSSI_VAL读出，RSSI值总是通过8个符号周期内，取平均值得到的，此为获得RSSI的一种方法，但是当数据接收以后这个寄存器没有被锁定，因此不宜把寄存器RSSIL.RSSI_VAL的值作为RSSI值，另外当MDMCTRL0L.AUTOCRC已经设置为1时（这在初始化的函数BOOL halRfConfig(UINT8 channel)中已通过MDMCTRL0L|=AUTO_CRC；设定），两个FCS字节被RSSI值、平均相关值（用于链路质量提示LQI）和CRC OK/not OK所取代，第一帧校验序列（FCS）字节被8位的RSSI值取代。可以在接收数据时读出。最后将接收的数据和RSSI值打印输出。

<!-- ------------------------ -->
## 实验步骤
Duration: 15

① 将OLED模块安装在其中一个CC2530底座模块上，CC Debugger连接电脑与接收节点底座，如下图所示：

![模块组装](/assets/CC2530/49.png)

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

⑥ 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`CC2530_BasicRF通信实验\2.RSSI采集实验\receiver_node\ide\cc2530_sw_examples.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530/50.png)  

⑦ 点击`Make`按钮，重新编译文件，显示没有错误。
   
    ![文件编译](/assets/CC2530/8.jpg) 

⑧ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑨ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑩ 将CCDebugger连接到发送节点，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`CC2530_BasicRF通信实验\2.RSSI采集实验\transmitter_node\ide\cc2530_sw_examples.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530/42.jpg)  

⑪ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑫ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑬ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑭ 移除`CC Debugger`仿真器，采用USB线供电（接任意底座），底座拼接，按下底座上的复位键。
    
![退出仿真](/assets/CC2530/51.png) 

⑮ OLED屏显示采集结果：

![显示屏显示](/assets/CC2530/52.png) 

<!-- ------------------------ -->
## 代码讲解
Duration: 15

### 发送节点

① 程序目录结构，源代码文件如下图。其中无线通信为官方提供源代码库。能够掌握其关键API函数即可。

![代码目录结构](/assets/CC2530/53.jpg) 

② main.c代码无线RF层代码初始化。初始化完成后，调用appTransmitter()函数，定时向接收节点发送数据。

```c
    void main (void)
    {
        //初始化外围硬件
        halBoardInit();
        appStarted = TRUE;//应用程序开始
        // 初始化 hal_rf
        if(halRfInit()==FAILED) {
            HAL_ASSERT(FALSE);
        }  

        halMcuWaitMs(350);//延时350ms

        appTransmitter();//发射模式
    }
```


### 接收节点

① 程序目录结构，源代码文件如下图。其中无线通信为官方提供源代码库。能够掌握其关键API函数即可。

![代码目录结构](/assets/CC2530/54.jpg) 

② per_test.c代码无线RF层代码初始化，初始化完成后，调用appReceiver()函数测量出误码率及RSSI。
      
```c
    void main (void)
    { 
        // 初始化外围硬件
        halBoardInit();  
            
        IIC_Init();//IIC初始化
        OLED_Init();//初始化OLED
        OLED_Init_UI();//开机界面
        
        appStarted = TRUE;//应用程序运行模块

        // 初始化 hal_rf
        if(halRfInit()==FAILED) {
            HAL_ASSERT(FALSE);
        }
        
        halMcuWaitMs(350);//350ms
        
        appReceiver();//接收模式,接收数据并测量出误码率及RSSi
    }
```

③ appReceiver()中调用basicRfReceive()函数，传入变量rssi指针，可读取到当前数据的RSSI。

```c
    while(!basicRfPacketIsReady());// 等待新的数据包
    if(basicRfReceive((uint8*)&rxPacket, MAX_PAYLOAD_LENGTH, &rssi)>0) {
         halLedSet(3);//P1_4
			
      UINT32_NTOH(rxPacket.seqNumber);// 改变接收序号的字节顺序
      segNumber = rxPacket.seqNumber;
            
     // 若统计被复位，设置期望收到的数据包序号为已经收到的数据包序号     
      if(resetStats)
      {
        rxStats.expectedSeqNum = segNumber;
        
        resetStats=FALSE;
      }      

      rxStats.rssiSum -= perRssiBuf[perRssiBufCounter];// 从sum中减去旧的RSSI值

      perRssiBuf[perRssiBufCounter] =  rssi;// 存储新的RSSI值到环形缓冲区，之后它将被加入sum

      rxStats.rssiSum += perRssiBuf[perRssiBufCounter];// 增加新的RSSI值到sum
      if(++perRssiBufCounter == RSSI_AVG_WINDOW_SIZE) {
        perRssiBufCounter = 0;    
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
   - 两个节点的PANID、信道是否相同，灯光节点的地址是否填写正确。

     
<!-- ------------------------ -->
## 实验思考
Duration: 5

1. 影响RSSI、误码率的原因有哪些？

