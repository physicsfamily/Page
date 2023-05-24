---
layout: post
title:  "022_基于STM32的WiFi模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的WiFi模块实验
<!-- ------------------------ -->
## 实验内容
Duration: 4

- 使用WiFi模块连接OneNET云平台；
- 使用485总线进行数据通信；
- 通过WiFi模块将温湿度数据传输到云平台。

<!-- ------------------------ -->
## 实验目的
Duration: 5

- 熟悉WIFI模块的使用；
- 熟悉常用AT指令的功能；
- 熟悉多模块组合使用；
- 了解OneNET云平台使用。

<!-- ------------------------ -->

## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | WiFi模块 | 1个 |  · |
| 4 | 温湿度模块 | 1个 | · |
| 5 | ST-Link下载器 | 1个 | · |
| 6 | ST-Link下载器连接线 | 1根 |  · |
| 7 | WiFi实验代码 | 1份 | · |


### 实验所需软件

- [MDK5](https://codelabs.stepiot.com/docs/AMDK5-078.html) 安装步骤

- [ST-LINK](https://codelabs.stepiot.com/docs/AST_LINK-079.html) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)


ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[WiFi模块](https://docs.stepiot.com/docs/aiot011)

![WiFi模块](/assets/BASE_STM32/28.jpg)

[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](/assets/BASE_STM32/2.png)


<!-- ------------------------ -->
## 实验要求
Duration: 6

- 了解如何使用AT指令建立SP站点；
- 了解AP与SP通信要设置哪些关键参数；
- 掌握WiFi模块连接OneNET平台；
- 熟悉多模块联合使用。
  
<!-- ------------------------ -->
## 实验原理
Duration: 25

### WIFI技术基本概念

WIFI英语全称是Wireless Fidelity，中文译成无线保真。WIFI是建立连接和进行通讯的手段，它对应一套通讯的规则，保证让两个节点能互相连接，设备连接建立后，通过TCP/IP和UDP等协议，传输数据，连接互联网络。WIFI模块的STA模式和AP模式。

AP模式：Access Point，提供无线接入服务，允许其它无线设备接入，提供数据访问，一般的无线路由/网桥工作在该模式下。AP和AP之间允许相互连接。Sta模式（SP模式）：Station类似于无线终端，sta本身并不接受无线的接入，它可以连接到AP，一般无线网卡即工作在该模式。对照表如下。

AP模式和SP模式对照表

|分类|AP模式|SP模式|
| -- | -- | -- |
|接入网络|接受|不接受|
|网卡|需要|不需要|
|终端|有线|无线|

### WiFi模块原理图

WIFI模块硬件电路

![WIFI模块硬件电路](/assets/BASE_STM32/29.png)

### 温度传感器工作原理

利用材料的物理特性会随着湿度变化的原理设计。比如利用金属膨胀原理设计的传感器，将双金属片由两片不同膨胀系数的金属贴在一起而组成，随着温度变化，材料A比另外一种金属膨胀程度要高，引起金属片弯曲。弯曲的曲率可以转换成一个输出信号比如利用金属随着温度变化，其电阻值也发生变化的原理。阻值的变化会导致电流，电压的变化通过测量电流电压。便可知道温度的变化。

### 湿度传感器工作原理

湿敏元件是最简单的湿度传感器。湿敏元件主要有电阻式、电容式两大类。湿敏电阻的特点是在基片上覆盖一层用感湿材料制成的膜，当空气中的水蒸气吸附在感湿膜上时，元件的电阻率和电阻值都发生变化，利用这一特性即可测量湿度。湿敏电容一般是用高分子薄膜电容制成的，常用的高分子材料有聚苯乙烯、聚酰亚胺、酪酸醋酸纤维等。当环境湿度发生改变时，湿敏电容的介电常数发生变化，使其电容量也发生变化，其电容变化量与相对湿度成正比。温湿度模块使用STH20，采用的是电容式湿敏元件。

### 光敏传感器工作原理

光敏传感器的工作原理是基于内光电效应。在半导体光敏材料两端装上电极引线，将其封装在带有透明窗的管壳里就构成光敏电阻，为了增加灵敏度，两电极常做成梳状，当受到一定波长的光线照射时，电阻会随着光照强度的增大而减小，电流也会随电阻的减小而变大，从而实现光电转换。当光强减小后光敏电阻值增大。

### 温湿度模块电路图

温湿度传感器基本电路图

![温湿度传感器基本电路图](/assets/BASE_STM32/30.png)

数码管驱动电路图

![数码管驱动电路图](/assets/BASE_STM32/31.png)

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

![RS485总线原理](/assets/BASE_STM32/32.jpg)


<!-- ------------------------ -->

## 实验步骤
Duration: 15

① 将WiFi模块、温湿度模块分别安装在STM32底座上，并将2个底座拼接，ST_LINK连接PC机与WiFi节点的STM32底座,如下图所示：

![安装模块](/assets/BASE_STM32/62.png)

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

下载目录至指定文件夹下。  
如果提示“command not found”表示电脑没有安装Git，请至[Git](https://git-scm.com/downloads)官网下载。  
如果电脑没有安装 Git 软件，也可以进入[Github](https://github.com/aiotcom/eps)，点击 `Code` -> `DownLoad ZIP` 下载所有工程代码。如下图所示：  
![下载代码](/assets/STM32/47.jpg)  
如果电脑没有公网，可以进：D盘\实验教程与代码选择相应的代码。  

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\10.WIFI模块\WiFi模块程序\USER\WIFI.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择工程](/assets/BASE_STM32/63.png)

⑥ 工程启动后，打开WiFi.h文件，修改路由器名称和密码，然后修改OneNET个人识别信息，从左到右分别是“产品ID”、“鉴权信息”、“脚本名称”，然后保存。

![修改WiFi数据](/assets/BASE_STM32/64.png)

*此处需要在oneNET平台进行设置，请参考[oneNET]()平台应用手册进行设置。*

⑦ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑧ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑨ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑩ 将ST_LINK连接到电脑与温湿度节点的STM32底座，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\10.WIFI模块\温湿度模块程序\USER\SHT20.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择工程](/assets/BASE_STM32/65.jpg)

⑪ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑫ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑬ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑭ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。


  
<!-- ------------------------ -->
## 代码讲解
Duration: 15

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。  
    ![程序目录结构](/assets/BASE_STM32/129.jpg)  

② main.c中对串口、WIFI模块、WS2812B全彩灯灯进行初始化。完成初始化后每2秒向OneNET平台发送一次数据。由于WIFI模块采用透传的模式发送，只需将数据发送到串口上就实现数据传到OneNET平台。
```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1
        UART2_Init(115200);//初始化串口2
        USART3_Init(115200);//用做调试串口
        printf("this is usart3 print\r\n");
        WS2812B_Init();//初始化全彩灯WS2812B
        
        WiFi_Init();//初始化WiFi
        TIM2_Init(1000-1,64-1);//初始化定时器2(1ms)
        Ticks_SendOnenET = HAL_GetTick() + 2000;
        Temp = 16;
        Humi = 60;
        while(1)
        {
            if(HAL_GetTick() > Ticks_SendOnenET){
                sprintf((char *)Send_OneNET_Buffer,"%d%d",Temp,Humi);
                HAL_UART_Transmit(&UART2_Handler,Send_OneNET_Buffer,strlen((const char*)Send_OneNET_Buffer),1000);//发送数据到OneNET
                
                Temp++;
                if(Temp>60){
                    Temp = 16;
                }
                Humi++;
                if(Humi>90){
                    Humi = 60;
                }
                printf("send onenet again\r\n");
                Ticks_SendOnenET = HAL_GetTick() + 2000;
            }
        }
    }
```

<!-- ------------------------ -->
## 常见问题
Duration: 5

1. 弹出警告窗口，不能下载程序。

    - 请确认STLink驱动、STM32F103C8的DFP包是否安装。
    - STLink仿真器是否正常接入。

2. 下载代码后程序没观察到实验现象。

    - 请重新上电，或者按下底座上的复位按键。
    - 模块没有安装稳妥。

3. OneNET平台设备没有上线。

    - WIFI名字、WIFI密码、OneNET脚本，权鉴信息是否正确。


<!-- ------------------------ -->
## 实验思考
Duration: 5

1. 向OneNET平台发送两组湿度、两组温度数据并显示。
