---
layout: post
title:  "021_基于STM32的LF_RFID读写卡实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的LF_RFID读写卡实验
<!-- ------------------------ -->
## 实验内容
Duration: 4

- 编写程序实读LRF_RFID卡片ID号，并在LCD显示屏上显示。

<!-- ------------------------ -->
## 实验目的
Duration: 5

- 了解LF_RFID 原理及应用。

<!-- ------------------------ -->

## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | LF_RFID模块 | 1个 |  · |
| 4 | OLED模块 | 1个 | · |
| 5 | ST-Link下载器 | 1个 | · |
| 6 | ST-Link下载器连接线 | 1根 |  · |
| 7 | LF_RFID卡片 | 1张 | · |
| 8 | LF_RFID卡片实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](assets/STM32/2.png)

[OLED模块](https://docs.stepiot.com/docs/aiot0003) & [LF_RFID模块](https://docs.stepiot.com/docs/aiot010)

![OLED模块 & LF_RFID模块](assets/BASE_STM32/116.png)


<!-- ------------------------ -->
## 实验要求
Duration: 6

- 掌握OLED模块基本操作。

<!-- ------------------------ -->
## 实验原理
Duration: 25

### LF_RFID原理及应用

按照工作频率的不同，RFID标签可以分为低频(LF)、高频(HF)、超高频(UHF)和微波等不同种类。不同频段的RFID工作原理不同，LF和HF频段RFID电子标签一般采用电磁耦合原理，而UHF及微波频段的RFID一般采用电磁发射原理。目前国际上广泛采用的频率分布于4种波段，低频(LF 125KHz)、高频(HF 13.54MHz）、超高频（UHF 850MHz～910MFz）和微波（2.45GHz）。每一种频率都有它的特点，被用在不同的领域，因此要正确使用就要先选择合适的频率。

低频卡是指频段在 30kHz 到 300kHz 的无线电波，一般的卡的频率在 125/134kHz，主要原因是在这个频率下不存在任何功能性，也就是说不会存在 ID 识别丶读取和写入等。他具有操作简单丶快捷丶可靠丶寿命长丶不怕卡面污染等优点，一般常见的低频卡有：HID 丶T55xx 丶 EM410x 等这些型号的低频卡。

低频 ID 卡的编码原理:125kHzID 卡通常都是使用彻斯特编码（Manchester Encoding），也叫做相位编码 (PE)，是一个同步时钟编码技术，被物理层使用来编码一个同步位流的时钟和数据。曼彻斯特编码被用在以太网媒介系统中。曼彻斯特编码提供一个简单的方式给编码简单的二进制序列而没有长的周期没有转换级别，因而防止时钟同步的丢失，或来自低频率位移在贫乏补偿的模拟链接位错误。在这个技术下，实际上的二进制数据被传输通过这个电缆，不是作为一个序列的逻辑 1 或 0 来发送的（技术上叫做反向不归零制 (NRZ)）。相反地，这些位被转换为一个稍微不同的格式，它通过使用直接的二进制编码有很多的优点。

而 ID 卡的在工作状态下，只要射频电路不断点，非接触的 ID 卡就会不断的循环发送 64 位数据。

ID卡号格式:由于厂家的 ID 卡号读卡器的译码格式不一样，在输出是读取的二进制或者十六进制的结果因该是一样的结果也是唯一的。

低频卡的典型应用有：动物识别、容器识别、工具识别、电子闭锁防盗（带有内置应答器的汽车钥匙），门禁、考勤等。

<!-- ------------------------ -->

## 实验步骤
Duration: 15

① 将LF_RFID模块、OLED模块分别安装在STM32底座上，分成两个节点，如下图：

![安装模块](assets/BASE_STM32/57.png)

② 访问[github](https://github.com/aiotcom/eps),进入github界面后点击Code，Clone HTTPS安全链接，如下图所示：

![操作步骤](assets/STM32/38.jpg)

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
![下载代码](assets/STM32/47.jpg)  
如果电脑没有公网，可以进：D盘\实验教程与代码选择相应的代码。 

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`STM32基础实验\9.LF-RFID模块\LF_RFID模块程序\USER\LF_RFID.uvprojx` 并打开。
   
![打开工程](assets/STM32/39.jpg)
![选择文件](assets/BASE_STM32/58.png)

⑥ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](assets/STM32/18.jpg)
![下载成功](assets/STM32/41.jpg)

⑨ 将STLink连接到TFT模块底座上，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`STM32基础实验\9.LF-RFID模块\OLED\USER\OLED.uvprojx` 并打开。
   
![打开工程](assets/STM32/39.jpg)
![选择文件](assets/BASE_STM32/122.jpg)

⑩ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](assets/STM32/16.jpg)

⑪ 编译成功，如下图：

![编译成功](assets/STM32/17.jpg)

⑫ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](assets/STM32/18.jpg)
![下载成功](assets/STM32/41.jpg)


⑬ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑭ 将LF_RFID卡片，放置于LF_RFID模块的线圈上，观察显示器上的显示。如图：  
![LF_RFID 感应线圈位置](assets/BASE_STM32/60.png)
![显示卡号](assets/BASE_STM32/61.png)




<!-- ------------------------ -->
## 代码讲解
Duration: 15

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
    ![程序目录结构](assets/BASE_STM32/123.jpg)  

② main.c中对EM4095 RFID芯片、串口、定时驱动进行初始初始化，当检测到卡号将卡号通过485总线发送到OLED显示。

```C
    int main(void)
    {
    HAL_Init();//初始化HAL库  
    Rs485_Init();//初始化485
    EM4095_Init();//初始化EM4095
    UART1_Init(115200);//初始化串口1，用于485通信
    TIM2_Init(2000-1,64-1);//初始化定时器2(2ms中断)，读EM4095芯片数据
    UART2_Init(115200);//初始化串口1
    USART3_Init(115200);//调试串口
    printf("usart3 print\r\n");
    TIM3_Init(10000-1,6400-1,RS485_HandlerCb);//初始化定时器中断周期64M/64/10000/ = 10HZ
        while(1)
        {
            GetId = EM4095_SearchHdr(CardID);//读取卡
            if(GetId){
            printf("===%d\r\n",*(uint32_t*)CardID);//调试打印
            }
        }
    }
```

EM4095_SearchHdr ()函数检测卡号，当检测到卡号，卡号保存在数组。

```c
    while(1)
        {
            GetId = EM4095_SearchHdr(CardID);//读取卡
            if(GetId){
            printf("===%d\r\n",*(uint32_t*)CardID);//调试打印
            }
```
    
处理获取卡号请求。返回卡号。RS485_HandlerCb()为回调函数，由定时器中断处理函数中调用。

```c
    void RS485_HandlerCb(void)
    {
        if(!DataHandling_485(Addr_LF_RFID))//接收数据
        {
            if(Rx_Stack.Src_Adr == Addr_LCD)//来自LF-RFID的数据
            {
                printf("get req\r\n");//调试打印
                Rs485_Send(Addr_LF_RFID,Addr_LCD,LF_RFID_ID,4,CardID);//发送ID数据
                
                /*清空数据*/
                CardID[0] = 0;
                CardID[1] = 0;
                CardID[2] = 0;
                CardID[3] = 0;
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
  
3. 卡号读不出来。

    - 是否拿错卡片，HF_RFID的卡片正反两面均为空白。

<!-- ------------------------ -->
## 实验思考
Duration: 5

1. 显示屏显示最近三次输入的卡号，如果卡号相同只显示一次。
