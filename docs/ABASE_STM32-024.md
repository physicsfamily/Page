---
layout: post
title:  "024_基于STM32的蓝牙模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的蓝牙模块实验
<!-- ------------------------ -->
## 实验内容


- 了解蓝牙模块使用，理解蓝牙模块工作原理。
- 编写程序配置蓝牙模块为从机模式，使用手机连接蓝牙模块，蓝牙连接完成以后手机端发送数据到蓝牙模块，蓝牙模块会返回同样的数据。

<!-- ------------------------ -->
## 实验目的


- 熟悉蓝牙模块的使用；
- 了解本次使用的蓝牙AT指令；
- 了解蓝牙通信原理。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | 蓝牙模块| 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | 蓝牙模块实验代码 | 1份 | · |


### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [oneNET](https://codelab.stepiot.com/codelabs/oneNet_080/index.html?index=..%2F..index#0)平台应用手册

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[蓝牙模块](https://docs.stepiot.com/docs/aiot013)

![蓝牙模块](/assets/BASE_STM32/82.png)


<!-- ------------------------ -->
## 实验要求


- 理解蓝牙通信原理；
- 熟悉蓝牙模块使用；
- 能够自主编写程序配置蓝牙模块以及通信。
  
<!-- ------------------------ -->
## 实验原理


### 蓝牙技术

蓝牙（Bluetooth）：是一种无线技术标准，可实现固定设备、移动设备和楼宇个人域网之间的短距离数据交换（使用2.4—2.485GHz的ISM波段的UHF无线电波）。蓝牙技术最初由电信巨头爱立信公司于1994年创制，当时是作为RS232数据线的替代方案。蓝牙可连接多个设备，克服了数据同步的难题。

蓝牙是基于数据包、有着主从架构的协议。一个主设备至多可和同一网中的七个从设备通讯。因此，本次实验中两个蓝牙模块有一个为从，另一个为主。

### 蓝牙为什么要配对

任何无线通信技术都存在被监听和破解的可能，蓝牙SIG为了保证蓝牙通信的安全性，采用认证的方式进行数据交互。同时为了保证使用的方便性，以配对的形式完成两个蓝牙设备之间的首次通讯认证，经配对之后，随后的通讯连接就不必每次都要做确认。所以认证码的产生是从配对开始的，经过配对，设备之间以PIN码建立约定的link key用于产生初始认证码，以用于以后建立的连接。

所以不配对，两个设备之间便无法建立认证关系，无法进行连接及其之后的操作，所以配对在一定程度上保证了蓝牙通信的安全，当然这个安全保证机制是比较容易被破解的，因为现在很多个人设备没有人机接口，所以PIN码都是固定的而且大都设置为通用的0000或者1234之类的，就很容易被猜到并进而建立配对和连接。

### 蓝牙模块电路

![蓝牙模块电路](/assets/BASE_STM32/83.png)

<!-- ------------------------ -->

## 实验步骤


① 将蓝牙模块安装于底座上，ST_LINK连接电脑与STM32底座，如下图所示：

![安装模块](/assets/BASE_STM32/84.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\12.蓝牙模块\BLE\USER\Ble.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择工程](/assets/BASE_STM32/85.png)

⑥ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 手机上安装蓝牙调试工具(只支持安卓手机)，到手机应用商中店，搜索“蓝牙调试器”，如下图：
    
![成功连接OneNET平台](/assets/BASE_STM32/86.png)

⑪ 蓝牙调试器安装完成后，打开手机的蓝牙功能，在手机桌面上打开蓝牙图标，弹出界面如图所示：
![实验结果](/assets/BASE_STM32/87.png)

⑫ 扫描附近蓝牙信号，点击扫描按键如下图，进行扫描。
![实验结果](/assets/BASE_STM32/88.png)

⑬ 发现蓝牙“BLE:341513”(不同的设备这个编号不相同，有BLE:开头就是蓝牙模块的信号)，蓝牙设备的MAC地址(每个蓝牙设备都不一样的)。

![扫描蓝牙设备](/assets/BASE_STM32/89.png)  
![MAC地址](/assets/BASE_STM32/90.png)

⑭ 选择特征值，点击设备按钮，在弹出的界面中选择值如图，确认后返回。

![蓝牙设置](/assets/BASE_STM32/91.png)
![选择特征值](/assets/BASE_STM32/92.png)

⑮ 点击按钮连接蓝牙信号，连接成功后，选择“对话模式”，如图：

![连接蓝牙设备](/assets/BASE_STM32/93.png)  
![切换模式](/assets/BASE_STM32/94.png)

⑯ 在输入框中输入“blue”字符，底座灯亮起蓝色如图：  
![发送blue指令](/assets/BASE_STM32/95.png)   
![底座灯亮蓝色](/assets/BASE_STM32/96.png)  

⑰ 在输入框中输入“red”字符，底座灯亮起红色。
![发送blue指令](/assets/BASE_STM32/97.png)    
![底座灯亮红色](/assets/BASE_STM32/98.png)  



<!-- ------------------------ -->
## 代码讲解


① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。
![程序目录结构](/assets/BASE_STM32/99.jpg)  

② main.c中对蓝牙使用的串口、蓝牙模块、底座灯控制IO进行初始化。连接成功根据发下的命令控制底座灯亮红色还是蓝色。

```c
    int main(void)
    { 
        uint8_t ledstete = 0;
        HAL_Init();//初始化HAL库  
        USART3_Init(115200);
        printf("this is usart3 print\r\n");    
        UART2_Init(9600);//初始化串口2
        Rs485_Init();//初始化485
        TIM3_Init(1000-1 ,64-1);//初始化定时器3
        Ble_Init();//初始化蓝牙
    
        USART2_RX_STA = 0;
        while(!(strstr((const char*)USART2_RX_BUF,(const char*)"Connected")));//蓝牙模块如果被连接，返回字符 Connected
        printf("connected\r\n");
    
        /*底座灯亮起绿色*/
        HAL_GPIO_WritePin(GPIOB,GPIO_PIN_4 ,GPIO_PIN_SET);
        HAL_GPIO_WritePin(GPIOB,GPIO_PIN_3 ,GPIO_PIN_SET);
        HAL_GPIO_WritePin(GPIOA,GPIO_PIN_15,GPIO_PIN_RESET);    
        while(1)
        {
            if(USART2_RX_STA){//接收完成           
                HAL_Delay(50);//延时一会等待接收完成
                
                if(strstr((const char*)USART2_RX_BUF,(const char*)"blue")){
                    /*底座灯亮起蓝色*/
                    HAL_GPIO_WritePin(GPIOB,GPIO_PIN_4 ,GPIO_PIN_RESET);
                    HAL_GPIO_WritePin(GPIOB,GPIO_PIN_3 ,GPIO_PIN_SET);
                    HAL_GPIO_WritePin(GPIOA,GPIO_PIN_15,GPIO_PIN_SET);
                }

                if(strstr((const char*)USART2_RX_BUF,(const char*)"red")){
                    /*底座灯亮起红色*/                
                    HAL_GPIO_WritePin(GPIOB,GPIO_PIN_4 ,GPIO_PIN_SET);
                    HAL_GPIO_WritePin(GPIOB,GPIO_PIN_3 ,GPIO_PIN_RESET);
                    HAL_GPIO_WritePin(GPIOA,GPIO_PIN_15,GPIO_PIN_SET);                
                }            
                USART2_RX_STA=0;
                memset((void*)USART2_RX_BUF,0,USART1_REC_LEN);
            
            }//if(USART2_RX_STA&UART_RX_DONE_MASK)        
        }
    }
```

使用strstr()函数，查找数组中是否有“blue”、“red”指令；使用DZD_ColSet()函数，点亮底座灯。
    
```c
    if(strstr((const char*)USART2_RX_BUF,(const char*)"blue")){
        /*底座灯亮起蓝色*/
        DZD_Colset(DZD_BLUE);
   }
    if(strstr((const char*)USART2_RX_BUF,(const char*)"red")){
         /*底座灯亮起红色*/  
        DZD_Colset(DZD_RED);
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


<!-- ------------------------ -->
## 实验思考


1. 自定义指令，指令内容包括要设置的颜色及亮多长时间，时间到后熄灭。
