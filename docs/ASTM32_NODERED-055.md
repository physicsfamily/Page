---
layout: post
title:  "055_STM32组合实验_NODERED平台显示超声波测距实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# STM32组合实验_NODERED平台显示超声波测距实验
<!-- ------------------------ -->
## 实验内容


- 使用485总线读取超声波测距数据；
- 通过WiFi模块将超声波测距的数据传输到NODERED平台。
  
<!-- ------------------------ -->
## 实验目的


- 将传感器数据上传到NODERED平台。
- NODERED平台动态显示距离数据。

<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | Wifi模块 | 1个 | ·  |
| 4 | 超声波模块 | 1个 | ·  |
| 5 | ST-Link下载器 | 1个 | · |
| 6 | ST-Link下载器连接线 | 1根 |  · |
| 7 | USB线| 1根 | ·  |
| 8 | 超声波传感器数据采集实验代码 | 1份 | ·  |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [NODE RED](https://codelabs.stepiot.com/codelabs/STM32_NodeRED_082/index.html?index=..%2F..index)平台安装应用手册

- [Git](https://git-scm.com/downloads)软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)

![STM32底座](/assets/STM32/2.png)

[WiFi模块](https://docs.stepiot.com/docs/aiot011)

![WiFi模块](/assets/BASE_CC2530/65.png)

[超声波模块](https://docs.stepiot.com/docs/aiot006)

![超声波模块](/assets/BASE_STM32/8.png)

USB线

![USB线](/assets/CC2530/2.png)

<!-- ------------------------ -->
## 实验原理


### WIFI技术基本概念

WIFI英语全称是Wireless Fidelity，中文译成无线保真。WIFI是建立连接和进行通讯的手段，它对应一套通讯的规则，保证让两个节点能互相连接，设备连接建立后，通过TCP/IP和UDP等协议，传输数据，连接互联网络。WIFI模块的STA模式和AP模式。

AP模式：Access Point，提供无线接入服务，允许其它无线设备接入，提供数据访问，一般的无线路由/网桥工作在该模式下。AP和AP之间允许相互连接。Sta模式（SP模式）：Station类似于无线终端，sta本身并不接受无线的接入，它可以连接到AP，一般无线网卡即工作在该模式。对照表如下。

|分类|AP模式|SP模式|
| :--: | :--: | :--: |
|接入网络|接受|不接受|
|网卡|需要|不需要|
|终端|有线|无线|

### WiFi模块原理图

![WIFI模块硬件电路](/assets/STM32_OneNET/1.png)

### RS485总线标准

RS485采用平衡发送和差分接收方式实现通信：发送端将串行口的ttl电平信号转换成差分信号a,b两路输出，经过线缆传输之后在接收端将差分信号还原成ttl电平信号。由于传输线通常使用双绞线，又是差分传输，所以有极强的抗共模干扰的能力，总线收发器灵敏度很高，可以检测到低至200mv电压。故传输信号在千米之外都是可以恢复。rs-485最大的通信距离约为1219m，最大传输速率为10mb/s，传输速率与传输距离成反比，在100kb/s的传输速率下，才可以达到最大的通信距离，如果需传输更长的距离，需要加485中继器。rs-485采用半双工工作方式，支持多点数据通信。rs-485总线网络拓扑一般采用终端匹配的总线型结构。即采用一条总线将各个节点串接起来，不支持环形或星型网络。如果需要使用星型结构，就必须使用485中继器或者485集线器才可以。rs-485总线一般最大支持32个节点，如果使用特制的485芯片，可以达到128个或者256个节点，最大的可以支持到400个节点。

### UART与RS-485性能对比

**抗干扰性：** RS485 接口是采用平衡驱动器和差分接收器的组合，抗噪声干扰性好。RS232 接口使用一根信号线和一根信号返回线而构成共地的传输形式，这种共地传输容易产生共模干扰。
**传输距离：** RS485 接口的最大传输距离标准值为 1200 米（9600bps 时），实际上可达 3000 米。RS232 传输距离有限，最大传输距离标准值为 50 米，实际上也只能用在 15 米左右。
**通信能力：** RS-485 接口在总线上是允许连接多达128个收发器，用户可以利用单一的 RS-485 接口方便地建立起设备网络。RS-232只允许一对一通信。
**传输速率：** RS-232传输速率较低，在异步传输时，波特率为20Kbps。RS-485 的数据最高传输速率为 10Mbps。
**信号线：** RS485 接口组成的半双工网络，一般只需二根信号线。RS-232 口一般只使用 RXD、TXD、GND 三条线。
**电气电平值：** RS-485的逻辑"1"以两线间的电压差为+（2-6）V表示；逻辑"0"以两线间的电压差为-（2-6）V表示。在 RS-232-C 中任何一条信号线的电压均为负逻辑关系。即：逻辑"1"，-5- -15V；逻辑"0 " +5- +15V。

### 实验底座RS485总线原理

每个底座周边都有RS485总线接口，由底座的485信号由MCU的UART信号+MAX3485 485总线转换芯片组成。

![实验底座RS485总线原理](/assets/STM32_OneNET/2.jpg)

<!-- ------------------------ -->
## 实验步骤

   
① 将WiFi模块、超声波模块分别安装在STM32底座上，ST_LINK连接WIFI节点连接与电脑，如下图所示：

![模块组装](/assets/STM32_OneNET/16.jpg)

    
② 访问[github](https://github.com/aiotcom/eps),进入github界面后点击Code，Clone HTTPS安全链接，如下图所示：

![操作步骤](/assets/STM32/38.jpg)

③ 打开电脑终端，进入工作目录workspace (workspace 为工程文件夹所在目录)：
   
```c
$ cd workspace
```

④ 运行`clone`命令：

```c
$ git clone https://github.com/aiotcom/eps.git
```

⑤下载目录至指定文件夹下。  
如果提示“command not found”表示电脑没有安装Git，请至[Git](https://git-scm.com/downloads)官网下载。  
如果电脑没有安装 Git 软件，也可以进入[Github](https://github.com/aiotcom/eps)，点击 `Code` -> `DownLoad ZIP` 下载所有工程代码。如下图所示：  
![下载代码](/assets/STM32/47.jpg)  
如果电脑没有公网，可以进：D盘\实验教程与代码选择相应的代码。

⑥ 打开已经安装NODERED的电脑：
   
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
⑦ 打开浏览器，输入地址127.0.0.1:1880 打开本机node red 主页：

![NODERED主页](/assets/CC2530_NODERED/NODERED-INPUT0.png)
### 导入本次试验的NODE RED流程
![NODERED导入1](/assets/CC2530_NODERED/NODERED-INPUT1.png)
![NODERED导入2](/assets/CC2530_NODERED/NODERED-INPUT2.png)
![NODERED导入3](/assets/CC2530_NODERED/NODERED-INPUT3.png)

### 部署本次试验NODE RED流程
![NODERED部署](/assets/CC2530_NODERED/NODERED-DIS.png)

### 打开本次试验的UI界面(输入地址127.0.0.1:1880/ui)
![NODERED图像界面](/assets/CC2530_NODERED/NODERED-UI7.png)

⑦ 打开` Keil uVision5 `(即安装的MDK5)工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32 NODERED实验\3.NODERED平台显示超声波测距实验\WiFi模块程序\USER\WIFI.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)

![选择文件](/assets/STM32_OneNET/17.jpg)

⑧ 打WIFI.h，修改WIFI热点的名字与密码。及根据安装NODERED服务的电脑IP和端口，修改并保存，如下图：
   
![修改WIFI信息](/assets/STM32_NODERED/SET-IP.png) 

⑨ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)

![下载成功](/assets/STM32/41.jpg)

⑪ 将STLink连接到超声波节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32 NODERED实验\3.NODERED平台显示超声波测距实验\超声波模块程序\USER\ChaoShengBo.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)

![选择文件](/assets/STM32_OneNET/18.jpg)

⑫ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑬ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑭ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)

![下载成功](/assets/STM32/41.jpg)

⑮ 下载完成后，将温湿度节点与WIFI节点拼接，并将USB线与任意底座进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。WIFI模块上线后状态如下图：

![WIFI模块指示灯](/assets/CC2530_NODERED/WIFI-ONLINE3.jpg) 

⑯  NODERED平台显示实验数据。(脚本位于：`基于STM32 NODERED实验\3.OneNET平台显示超声波测距实验\超声波测距..json`)。具体操作参考[node red](https://codelabs.stepiot.com/codelabs/STM32_NodeRED_082/index.html?index=..%2F..index)平台应用手册。

![OneNET平台显示](/assets/CC2530_NODERED/NODERED-UI7-1.png)


<!-- ------------------------ -->
## 代码讲解


### 超声波节点

① 程序目录结构，源代码文件如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。

![代码目录结构](/assets/STM32_OneNET/21.jpg) 

② `main.c`中对串口、定时器器、超声模块、RS485协议进行初始化。初始化完成后，`HCSR04_StartMeasure ()`读取探测，`send_LED_Display()`显示距离数据。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库
        TM1640_Init();//初始化TM1640，数码管驱动
        HCSR04_Init();//初始化超声波模块
        Rs485_Init();//初始化485
        USART3_Init(115200);//用于调试
        UART1_Init(115200);//初始化串口1，RS485通信
        TIM2_Init(10-1,64-1);//初始化定时器2，(中断周期10us)，超声波测量使用
        /*中断频率20HZ，关联RS485_HandlerCb()回调函数*/
        TIM3_Init(10000-1,320-1,RS485_HandlerCb);    
        while(1)
        {
            Avg =  HCSR04_StartMeasure(10);//探测距离共探测10次
            send_LED_Display(0xC0,Avg,1);//数码管显示距离
            HAL_Delay(500);//每500ms测量一次
            
            /*转换到数组中*/
            SendBuffer[0] = Avg>>8;
            SendBuffer[1] = (uint8_t)Avg;    
        }
    }
```

`main.c`->`RS485_HandlerCb()`(回调函数)，处理WIFI节点的请求，调用函数`Rs485_Send()`返回传感器数据。

```c
    void RS485_HandlerCb(void)
    {
        if(!DataHandling_485(Addr_Ultrasonic)){//是本机期望的485数据处理
            printf("get requery\r\n");
            /*485发送数据*/
            printf("%d,%d\r\n",SendBuffer[0],SendBuffer[1]);
            /*向wifi节点发送数据*/
            Rs485_Send(Addr_Ultrasonic,Addr_WiFi,Ultrasonic_Dis,2,&SendBuffer[0]);
        }
    }
```


### WIFI节点

① 程序目录结构，源代码文件如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。

![代码目录结构](/assets/STM32_OneNET/22.jpg) 

② `main.c`中对串口、RS485协议进行初始化，并WIFI初始化并连接NODERED平台。初始化完成后，定时请求传感器数据。每1秒将传感器传送到NODERED平台。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1，485总线使用
        UART2_Init(115200);//初始化串口2
        USART3_Init(115200);//调试串口 
        printf("this usart3 print\r\n");
        WiFi_Init();//初始化WIFI并连接OneNET
    
        /*中断频率2HZ，关联回调函数RS485_HandlerCb*/
        TIM3_Init(10000-1,3200-1,RS485_HandlerCb);  
        while(1)
        {
            HAL_Delay(1000);//定时1s，1s更新一次
            /*转成字符串*/
            Distance = (SensorData[0]<<8) + SensorData[1];
            sprintf((void*)&SendBuffer[0],"%04d",Distance);
            WiFi_SerialSend(&SendBuffer[0],4);//发送到OneNET
            printf("传感器数据==%d\r\n",Distance);//调试打印
        }
    }
```

`main.c`->`RS485_HandlerCb()`(回调函数)，定时向超声波节点请求数据，有收数据保存到SensorData数组中。最终被传到NODERED平台。

```c
    void RS485_HandlerCb(void)
    {
    static uint8_t state = 0;
    if(state == 0){//发送请求
            Rs485_Send(Addr_WiFi,Addr_Ultrasonic,Ultrasonic_Get_Dis,0,(void*)0);
            printf("请求传感器数据");
            state = 1;
        }
        else{//检测是否返回数据
            if(!DataHandling_485(Addr_WiFi)){//是本机期望的485数据处理
                printf("get data\r\n");
                SensorData[0] = Rx_Stack.Data[0];
                SensorData[1] = Rx_Stack.Data[1];          
            }
            state = 0;
        }
    }
```




<!-- ------------------------ -->
## 常见问题


1. 弹出警告窗口，不能下载程序。

    - 请确认STLink驱动、STM32F103C8的DFP包是否安装。
    - STLink仿真器是否正常接入。

2. 下载代码后程序没观察到实验现象。
   
   - 请重新上电，或者按下底座上的复位按键。
   - 模块没有安装稳妥。
3. NODERED平台设备没有上线。

    - WIFI名字、WIFI密码、IP\PORT，信息是否正确。


<!-- ------------------------ -->
## 实验思考


1. 编写流程在NODERED平台显示最近三次的测量中的最大值。
