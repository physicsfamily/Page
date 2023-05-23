---
layout: post
title:  "043_CC2530_BasicRF通信实验_温湿度采集实验_点对点"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# CC2530 BasicRF 通信实验_温湿度采集实验_点对点
<!-- ------------------------ -->
## 实验内容
Duration: 2

- 搭建实验硬件环境；
- 完成温湿度数据采集；
- BasicRF点对点通信。
  
<!-- ------------------------ -->
## 实验目的
Duration: 3

- 熟悉OLED模块的使用；
- 了解BasicRF无线传输数据原理；
- 掌握无线数据传输方法。

<!-- ------------------------ -->
## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | CC2530底座模块 | 2个 |  · |
| 3 | OLED模块 | 1个 | ·  |
| 4 | 温湿度模块 | 1个 | ·  |
| 5 | CC Debugger 仿真器和连接线| 1套 | ·  |
| 6 | USB线| 1根 | ·  |
| 7 | 温湿度采集实验_点对点代码 | 1份 | ·  |

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

[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](/assets/BASE_STM32/2.png)

USB线

![USB线](/assets/CC2530/2.png)

<!-- ------------------------ -->
## 实验要求
Duration: 4

- 了解BasicRF无线收发机制；
- 熟悉CC2530底座使用；
- 掌握温湿度模块使用。

<!-- ------------------------ -->
## 实验原理
Duration: 15

### 温度传感器工作原理

利用材料的物理特性随着温度变化的原理设计。比如利用金属膨胀原理设计的传感器，将双金属片由两片不同膨胀系数的金属贴在一起而组成，随着温度变化，材料A比另外一种金属膨胀程度要高，引起金属片弯曲。弯曲的曲率可以转换成一个输出信号。又比如利用金属随着温度变化，其电阻值也发生变化的原理。阻值的变化会导致电流电压的变化，通过测量电流电压。便可知道温度的变化。

### 湿度感器工作原理

湿敏元件是最简单的湿度传感器。湿敏元件主要有电阻式、电容式两大类。湿敏电阻的特点是在基片上覆盖一层用感湿材料制成的膜，当空气中的水蒸气吸附在感湿膜上时，元件的电阻率和电阻值都发生变化，利用这一特性即可测量湿度。湿敏电容一般是用高分子薄膜电容制成的，常用的高分子材料有聚苯乙烯、聚酰亚胺、酪酸醋酸纤维等。当环境湿度发生改变时，湿敏电容的介电常数发生变化，使其电容量也发生变化，其电容变化量与相对湿度成正比。温湿度模块使用SHT20，采用的是电容式湿敏元件。

### 硬件设计

温湿度传感器采用集成化的模块SHT20，主要通过IIC的通信方式进行数据的传输及处理，该芯片性能优异，长期工作也能保持数据稳定。数码管驱动采用TM1640驱动芯片。 TM1640是一种LED(发光二极管显示器)驱动控制专用电路,内部集成有MCU 数字接口、数据锁存器，能有效节省IO。

![温湿度传感器基本电路图](/assets/CC2530/55.png)

![数码管驱动电路图](/assets/CC2530/56.png)


<!-- ------------------------ -->
## 实验步骤
Duration: 15
   
① 将OLED模块、温湿度模块分别安装CC2530底座上，CC Debugger连接电脑与协调器节点CC2530底座，如下图所示：

![模块组装](/assets/CC2530/57.png)

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

⑥ 打开 `IAR Embedded Workbench` 工程软件，点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`CC2530_BasicRF通信实验\3.温湿度采集实验_basicrf\Display_Node\ide\cc2530_sw_examples.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530/58.jpg)  

⑦ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑧ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑨ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑩ CCDebugger连接电脑与终端节点底座，轻按CCDebugger复位按键，指示灯变绿，表示连接正常。点击工具栏： `File` -> `Open` -> `Workspace`，选择工程文件：`CC2530_BasicRF通信实验\3.温湿度采集实验_basicrf\Tempareture_Node\ide\cc2530_sw_examples.eww` 并打开。
   
![打开工程](/assets/CC2530/6.jpg)
    
![选择文件](/assets/CC2530/60.jpg)  

⑪ 点击`Make`按钮，重新编译文件，显示没有错误。
   
![文件编译](/assets/CC2530/8.jpg) 

⑫ 点击`Download and Debug`按钮，将程序下载到模块中。

![下载程序](/assets/CC2530/9.jpg) 

![代码下载成功](/assets/CC2530/10.jpg) 

⑬ 点击`X`退出仿真模式。

![退出仿真](/assets/CC2530/11.jpg) 

⑭ 移除`CC Debugger`仿真器，采用USB线供电（接任意底座），底座拼接。
    
![退出仿真](/assets/CC2530/62.png) 

⑮ OLED屏显示数据：

![显示屏显示](/assets/CC2530/63.png) 

<!-- ------------------------ -->
## 代码讲解
Duration: 15

### 终端节点

① 程序目录结构，源代码文件如下图。其中无线通信为官方提供源代码库。能够掌握其关键API函数即可。

![代码目录结构](/assets/CC2530/64.jpg) 

② main.c代码对温湿度模块初始化、无线RF层代码初始化、初始化完成后，调用SHT20_Node()函数，定时向协议器发送温湿度数据。

```c
    void main(void)
    {        
        //Initalise board peripherals 初始化外围设备
        halBoardInit();
        IIC_Init();//初始化IIC 
        TM1640_Init();//初始化TM1640
        // Initalise hal_rf 硬件抽象层的 rf 进行初始化
        if(halRfInit()==FAILED)
        {
            HAL_ASSERT(FALSE);
        }
        SHT20_Node();//温湿度节点
    }
```

SHT20_Node()函数中调用SHT2x_GetTempHumi()，获取温度度，调用basicRfSendPacket()函数将数据发送到协调器节点。

```c
    while (TRUE)
    {
	    /*获取温湿度值.分别保存g_sht2x_param.TEMP_HM，g_sht2x_param.HUMI_HM*/
        SHT2x_GetTempHumi(); 
        send_LED_Display(0xC0,(uint16_t)g_sht2x_param.TEMP_HM,1);//显示温度
				delay_ms(500);//延时1秒    
        send_LED_Display(0xC0,(uint16_t)g_sht2x_param.HUMI_HM,2);//显示湿度
				delay_ms(500);//延时1秒   

		/*温湿度转化成字符串*/
        sprintf((char *)pTxData,"Temp:%d,\tHumi:%d.\r\n", 
								(uint16_t)g_sht2x_param.TEMP_HM,(uint16_t)g_sht2x_param.HUMI_HM);
				
        basicRfSendPacket(Coordinator_ADDR, pTxData,APP_PAYLOAD_LENGTH);//发送温湿度数据到协调器节点

        memset(pTxData,0,APP_PAYLOAD_LENGTH);//清楚缓冲
    }
```


### 终端节点

① 程序目录结构，源代码文件如下图。其中无线通信为官方提供源代码库。能够掌握其关键API函数即可。

![代码目录结构](/assets/CC2530/65.jpg) 

② main.c代码对初始化OLED屏、无线RF层代码初始化、初始化完成后，OLED屏开机界面，调用Coordinator_Node ()函数，接收无线数码并显示数据。
      
```c
    void main(void)
    {        
        //Initalise board peripherals 初始化外围设备
        halBoardInit();
        IIC_Init();//初始化IIC 
        TM1640_Init();//初始化TM1640
        // Initalise hal_rf 硬件抽象层的 rf 进行初始化
        if(halRfInit()==FAILED)
        {
            HAL_ASSERT(FALSE);
        }
        SHT20_Node();//温湿度节点
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

1. 修改终端节点的代码，实现当温湿数据变化时才向协调器发送传感器数据。(要修改的代码CC2530 BasicRF通信实验\3.温湿度采集实验_basicrf\Tempareture_Node)。
