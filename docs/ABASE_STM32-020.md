---
layout: post
title:  "020_基于STM32的HF_RFID读写卡实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的HF_RFID读写卡实验
<!-- ------------------------ -->
## 实验内容


- 编写程序读S50卡片ID号，并在LCD显示屏上显示;
- 编写程序读/写S50卡片内的内容并显示。

<!-- ------------------------ -->
## 实验目的


- 了解S50卡片内的存储结构;
- 了解HF_RFID原理及应用。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | HF_RFID模块 | 1个 |  · |
| 4 | OLED模块 | 1个 | · |
| 5 | ST-Link下载器 | 1个 | · |
| 6 | ST-Link下载器连接线 | 1根 |  · |
| 7 | HF_RFID卡片 | 1张 | · |
| 8 | HF_RFID卡片实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelabs.stepiot.com/docs/AMDK5-078.html) 安装步骤

- [ST-LINK](https://codelabs.stepiot.com/docs/AST_LINK-079.html) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[OLED模块](https://docs.stepiot.com/docs/aiot003)  & [HF_RFID模块](https://docs.stepiot.com/docs/aiot009)
![OLED模块 & HF_RFID模块](/assets/BASE_STM32/16.png)


<!-- ------------------------ -->
## 实验要求


- 掌握OLED模块基本操作；
- 了解在RFID的工作频段分布。

<!-- ------------------------ -->
## 实验原理


### HF_RFID技术

RFID（Radio Frequency Identification）技术，又称无线射频识别，是一种通信技术，可通过无线电讯号识别特定目标并读写相关数据，而无需识别系统与特定目标之间建立机械或光学接触。应用分布在身份证件和门禁控制、供应链和库存跟踪、汽车收费、防盗、生产控制、资产管理。

RFID的使用频段分布有低频（125KHz到135KHz），高频（13.56MHz）和超高频（860MHz到960MHz）之间。

#### RFID技术与NFC技术的区别
  
NFC（Near Field Communication）技术，即近距离无线通讯技术。NFC是在RFID的基础上发展而来，NFC从本质上与RFID没有太大区别，从名字可以看出NFC技术增加了点对点通信（Communication）功能，NFC设备彼此寻找对方并建立通信连接，而RFID通信的双方设备是主从关系。 

其余还有一些技术细节方面。NFC相较于RFID技术，具有距离近、带宽高、能耗低等一些特点：

+ NFC只是限于13.56MHz的频段。而RFID的频段有低频（125KHz到135KHz），高频（13.56MHz）和超高频（860MHz到960MHz之间。
+ 工作有效距离：NFC（小于10cm，所以具有很高的安全性），RFID距离从几米到几十米都有。
+ 因为同样工作于13.56MHz，NFC与现有非接触智能卡技术兼容，所以很多的厂商和相关团体都支持NFC，而RFID标准较多，统一较为复杂，只能在特殊行业有特殊需求下，采用相应的技术标准。
+ 应用：RFID更多的被应用在生产、物流、跟踪、资产管理上，而NFC则在门禁、公交、手机支付等领域内发挥着巨大的作用。

#### 高频RFID系统介绍

典型的高频HF（13.56MHz）RFID系统包括阅读器（Reader）和电子标签（Tag，也称应答器Responder）。电子标签通常选用非接触式IC卡，又称智能卡具有可读写，容量大，加密及数据记录可靠等功能。IC卡目前已经大量使用在校园一卡通系统、消费系统、考勤系统、公交消费系统等。目前市场上使用最多的是PHILIPS的Mifare系列IC卡。读写器（也称为阅读器）包含有高频模块（发送器和接收器）、控制单元以及与卡连接的耦合元件。由高频模块和耦合元件发送电磁场，以提供非接触式IC卡所需要的工作能量以及发送数据给卡，同时接收来自卡的数据。IC卡由主控芯片ASIC（专用集成电路）和天线组成，标签的天线只由线圈组成，很适合封状到卡片中，常见IC卡内部结构如图 ：

![IC卡内部结构图](/assets/BASE_STM32/17.jpg)

较常见的高频RFID应用系统如图所示，IC卡通过电感耦合的方式从读卡器处获得能量。

![RFID高频应用系统组成](/assets/BASE_STM32/18.png)

下面以典型的IC卡MIARE 1为例说明电子标签获得能量的整个过程。读卡器向IC卡发送一组固定频率的电磁波，标签内有一个LC串联谐振电路（图），其谐振频率与读写器发出的频率相同，这样当标签进入读写器范围时便产生电磁共振，从而使电容内有了电荷，在电容的另一端接有一个单向通的电子泵，将电容内的电荷送到另一个电容内储存，当储存积累的电荷达到2V时，此电源可作为其他电路提供工作电压，将标签内数据发射出去或接收读写器的数据。

![IC功能示意图](/assets/BASE_STM32/19.png)

##### ISO 14443协议标准

ISO 14443协议是超短距离智慧卡标准，该标准定义出读取距离7~15公分的短距离非接触智能卡的功能及运作标准，ISO 14443为TYPE A和TYPE B两种。TYPE A产品具有更高的市场占有率，如Philips公司的MIFARE系列占有了当前约80%的市场，且在较为恶劣的工作环境下有很高的优势。而TYPE B在安全性、高速率和适应性方面有很好的前景，特别适合于CPU卡。这里重点介绍MIFARE1符合ISO 14443 TYPE A标准。

1. ISO 14443 TYPE A标准中规定的基本空中接口基本标准：  
+ PCD到PICC（数据传输）调制为：ASK，调制指数100%
+ PCD到PICC（数据传输）位编码为：改进的Miller编码
+ PICC到PCD（数据传输）调制为：频率为847kHz的副载波负载调制
+ PICC到PCD位编码为：曼彻斯特编码，数据传输速率为106kbps。射频工作区的载波频率为13.56MHz。
+ 最小未调制工作场的值是1.5A/mrms（以Hmin表示），最大未调制工作场的值是7.5A/mrms（以Hmax表示），邻近卡应持续工作在Hmin和Hmax之间PICC的能量是通过发送频率为13.56MHz的阅读器的交变磁场来提供。由阅读器产生的磁场必须在1.5A/m~7.5A/m之间。

2. ISO 14443 TYPE A标准中规定的PICC标签状态集，读卡器对进入其工作范围的多张IC卡的有效命令有：
+ REQA：TYPE A请求命令
+ WAKE UP：唤醒命令
+ ANTICOLLISION：防冲突命令
+ SELECT：选择命令
+ HALT：停止命令

下图为PICC（IC卡）接收到PCD（读卡器）发送命令后，可能引起状态的转换图。传输错误的命令（不符合ISO 14443 TYPE A协议的命令）不包括在内。

![PICC状态转化图](/assets/BASE_STM32/20.png)

掉电状态（POWER OFF）：在没有提供足够的载波能量的情况下，PICC不能对PCD发射的命令做出应答，也不能向PCD发送反射波；当PICC进入耦合场后，立即复位，进入闲置状态。

闲置状态（IDLE STATE）：当PICC进入闲置状态时，标签已经上电，能够解调PCD发射的信号；当PICC接收到PCD发送的有效的REQA（对A型卡请求的应答）命令后，PICC将进入就绪状态。

就绪状态（READY STATE）：在就绪状态下，执行位帧防碰撞算法或其他可行的防碰撞算法；当PICC标签处于就绪状态时，采用防冲突方法，用UID（惟一标识符）从多张PICC标签中选择出一张PICC；然后PCD发送含有UID的SEL命令，当PICC接收到有效的SEL命令时，PICC就进入激活状态（ACTIVE STATE）。

激活状态（ACTIVE STATE）：在激活状态下，PICC应该完成本次应用所要求的所有操作（例如，读写PICC内部存储器）；当处于激活状态的PICC接收到有效的HALT命令后，PICC就立即进入停止状态。

停止状态（HALT STATE）：PICC完成本次应用所有操作后，应进入停止状态；当处于停止状态的PICC接收到有效的WAKE_UP命令时，PICC立即进入就绪状态。注意：当PICC处于停止状态下时，在重新进入就绪状态和激活状态后，PICC接受到相应命令，不再是进入闲置状态，而是进入停止状态。

##### 非接触式IC卡

目前市面上有多种类型的非接触式IC卡，它们按照遵从的不同协议大体可以分为三类，各类IC卡特点及工作特性如表 1所示。PHILIPS的Mifare 1卡（简称M1卡）属于PICC卡，该类卡的读写器可以称为PCD。

| IC卡 | 读写器 | 国际标准 | 读写距离 | 工作频率 |
| --- | --- | --- | --- | --- |
| CICC | CCD | ISO/IEC 10536 | 密耦合（0~1cm）| 0~30MHz |
| PICC |PCD	| ISO/IEC 14443	| 近耦合（7~10cm）| 135MHz，6.75MHz，13.56MHz，27.125MHz |
| VICC | VCD | ISO/IEC 15693 | 疏耦合（<1m） | 135MHz，6.75MHz，13.56MHz，27.125MHz |

高频RFID系统选用PICC类IC卡作为其电子标签，这里以Philips公司典型的PICC卡Mifare1为例，详细讲解IC卡内部结构。Philips是世界上最早研制非接触式IC卡的公司，其Mifare技术已经被制定为IS0 14443 TYPE A国际标准。本平台选用Mifare1（S50）卡作为电子标签，其内部原理如图 所示：

![M1卡内部原理](/assets/BASE_STM32/21.png)

射频接口部分主要包括有波形转换模块。它可将读写器发出的13.56MHz的无线电调制频率接收，一方面送调制/解调模块，另一方面进行波形转换，将正弦波转换为方波，然后对其整流滤波，由电压调节模块对电压进行进一步的处理，包括稳压等，最终输出供给卡片上的各电路。数字控制单元主要针对接收到的数据进行相关处理，包括选卡、防冲突等。Mifare1卡片采取EEPROM作为存储介质，其内部可以分为16个扇区，每个扇区由4块组成，（我们也将16个扇区的64个块按绝对地址编号为0-63，存贮结构如下图所示：

![MF1卡存储结构](/assets/BASE_STM32/22.jpg)

第0扇区的块0（即绝对地址0块），它用于存放厂商代码，已经固化不可更改。其中：第0～3个字节为卡片的序列号；第4个字节为序列号的校验码；第5个字节为卡片内容“size”字节，第6~7个字节为卡片的类型字节。

每个扇区的块0、块1、块2为数据块，可用于存贮数据。数据块可作两种应用：用作一般的数据保存，可以进行读、写操作。用做数据值，可以进行初始化加值、减值、读值操作。

每个扇区的块3为控制块，包括了密码A、存取控制、密码B。具体结构如下：

![控制块结构](/assets/BASE_STM32/23.jpg)

其中：

A0—A5代表密码A的六个字节；

FF 07 80 69为四字节存取控制字的默认值，FF为低字节；

B0—B5代表密码B的六个字节。

每个扇区的密码和存取控制都是独立的，可以根据实际需要设定各自的密码及存取控制。存取控制为4个字节，共32位，扇区中的每个块（包括数据块和的存取条件是由密码和存取控制共同决定的，在存取控制中每个块都有相应的三个控制位，定义如下：

块0：C10 C20 C30

块1：C11 C21 C31

块2：C12 C22 C32

块3：C13 C23 C33

三个控制位以正和反两种形式存在于存取控制字节中，决定了该块的访问权限（如进行减值操作必须验证KEY A，进行加值操作必须验证KEY B，等等）。三个控制位在存取控制字节中的位置，以块0为例，如下所示：_b表示取反。

控制字结构

|   | Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|字节6||||C20_b||||C10_b|
|字节7||||C10||||C30_b|
|字节8||||C30||||C20|
|字节9|||||||||

控制字结构

|   | bit 7 | bit 6 | bit 5 | bit 4 | bit 3 | bit 2 | bit 1 | bit 0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|字节6|C23_b|C22_b|C21_b|C20_b|C13_b|C12_b|C11_b|C10_b|
|字节7|C13|C12|C11|C10|C33_b|C32_b|C31_b|C30_b|
|字节8|C33|C32|C31|C30|C23|C22|C21|C20|
|字节9|||||||||

数据块的存取控制

![数据块的存取控制](/assets/BASE_STM32/24.jpg)

上图中KeyA|B表示密码A或密码B，Never表示任何条件下不能实现。如：

当块0的存取控制位C10 C20 C30=1 0 0时，验证密码A或密码B正确后可读。验证密码B正确后可写；不能进行加值、减值操作。

控制块块3的存取控制与数据块（块0、1、2）不同，它的存取控制如下图：

控制块3的存取控制

![控制块3的存取控制](/assets/BASE_STM32/25.jpg)

例如：当块3的存取控制位C13 C23 C33=1 0 0时，表示：密码A：不可读，验证KEYA或KEYB正确后，可写（更改）。存取控制：验证KEYA或KEYB正确后，可读、可写。密码B：验证KEYA或KEYB正确后，可读、可写。

通过以上控制器的描述可知，如果控制字为0xFF 0x07 0x80 0x69时，即如下图：

![控制字（0xFF 0x07 0x80 0x69）](/assets/BASE_STM32/26.jpg)

块0：C10 C20 C30------------0 0 0

对每个扇区的块0我们在验证秘钥A或者秘钥B以后可以进行读、写、增加（Increment）、减值（Decrement）等操作，但绝对地址块0只读；

块1：C11 C21 C31------------0 0 0

对每个扇区的块1我们在验证秘钥A或者秘钥B以后可以进行读、写、增加（Increment）、减值（Decrement）等操作；

块2：C12 C22 C32------------0 0 0

对每个扇区的块2我们在验证秘钥A或者秘钥B以后可以进行读、写、增加（Increment）、减值（Decrement）等操作；

块3：C13 C23 C33------------0 0 1

密码A：验证密码A正确后不可读（所以默认情况下我们用秘钥A“FF FF FF FF FF FF”读扇区块3的数据时候返回的秘钥A数据是00 00 00 00 00 00其实就表示空数据不可读的意思），验证秘钥A或者秘钥B正确后，可写（更改）。

存取控制：验证KEYA或KEYB正确后可读、可写。

密码B：验证KEYA或KEYB正确后，可读、可写。

##### RFID模块电路

RFID模块采用NXP RFID芯片MFRC522如图9，MFRC522是高度集成的非接触式（13.56MHz）读写卡芯片，此发送模块利用调制和调节的原理，并将它们完全集成到各种非接触式通信方法和协议中。它支持ISO14443A/MIFARE。MFRC522支持SPI、I2C和UART接口。在本次实验程序采用SPI接口。

![RFID模块电路](/assets/BASE_STM32/27.png)

RFID感应天线采用板载PCB天线。

<!-- ------------------------ -->

## 实验步骤


① 将HF_RFID模块、OLED模块分别安装在STM32底座上，分成两个节点，如下图：

![安装模块](/assets/BASE_STM32/52.png)

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

⑤ 将ST_LINK连接电脑与HF_RFID模块的底座上。

⑥ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\8.HF-RFID模块\HF_RFID\USER\RFID.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择文件](/assets/BASE_STM32/53.jpg)

⑦ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑧ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑨ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)
    
⑩ 将STLink连接到OLED模块底座上，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\8.HF-RFID模块\OLED\USER\OLED.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择文件](/assets/BASE_STM32/120.png)

⑪ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑫ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑬ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑭ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。
    
⑮ 将HF_RFID卡片，放置于HF_RFID模块的线圈上，观察显示器上显示的卡号(采用16进制)

![HF_RFID 感应线圈位置](/assets/BASE_STM32/55.png)
![OLED显示RFID卡号](/assets/BASE_STM32/56.png)



<!-- ------------------------ -->
## 代码讲解


① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
    ![程序目录结构](/assets/BASE_STM32/121.jpg)  

② main.c中对RC522RFID芯片、串口、定时驱动进行初始初始化，当检测到卡号将卡号通过485总线发送到OLED显示。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库

        USART3_Init(115200);//初始化串口,用于调试    
        printf("usart3 print\r\n");
        RC522_Init ();//RC522模块初始化    
        HAL_Delay(200);//初始化完成RC522延时一会	

        Rs485_Init();//初始化485协议
        UART1_Init(115200);//初始化串口，用于485通信
        
        TIM3_Init(10000-1,640-1,RS485_HandlerCb);//初始化定时器中断周期64M/64/10000/ = 10HZ
        while(1)
        {
            if(IC_Card_Search(SendBuf)){//IC卡检测  
                printf("%d,%d,%d,%d\r\n",SendBuf[0],SendBuf[1],SendBuf[2],SendBuf[3]);//打印调试
            }
            else{
                memset(SendBuf,0,4);//清空数组
            }
        }
    }
```

IC_Card_Search()函数检测卡号，当检测到卡号，卡号保存在SendBuf数组。

```c
    if(IC_Card_Search(SendBuf)){//IC卡检测  
        printf("%d,%d,%d,%d\r\n",SendBuf[0],SendBuf[1],SendBuf[2],SendBuf[3]);//打印调试
        }
        else{
            memset(SendBuf,0,4);//清空数组
            }
```

在485总线的定义中RFID节点为从设备，故需要处理主设备的请求。如下图。RS485_HandlerCb()为回调函数，由定时器中断处理函数中调用。
    
```c
    void RS485_HandlerCb(void)
    {
    if(!DataHandling_485(Addr_HF_RFID)){//接收数据
        if(Rx_Stack.Src_Adr == Addr_LCD){//来自OLED显示屏的请求
            printf("get req\r\n");//打印调试信息
            
            /*将数据发送到OLED显示*/
            Rs485_Send(Addr_HF_RFID,Addr_LCD,HF_RFID_ID,4,SendBuf);           
            }
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
  
3. 卡号读不出来。

    - 是否拿错卡片，HF_RFID的卡片正反两面均为空白。
    
<!-- ------------------------ -->
## 实验思考


1. 显示屏显示最近三次输入的卡号，如果卡号相同只显示一次。
