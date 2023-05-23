---
layout: post
title:  "014_基于STM32的OLED模块(7x5cm)实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的OLED模块(7x5cm)实验
<!-- ------------------------ -->
## 实验内容


- 在OLED屏上显示中文及英文。

<!-- ------------------------ -->
## 实验目的


- 掌握屏幕显示中文及英文的原理。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 1个 |  · |
| 3 | OLED模块(7x5cm) | 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | OLED实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[OLED模块](https://docs.stepiot.com/docs/aiot003)：OLED模块是一个0.96寸OLED屏，像素128*64。

![OLED模块](/assets/BASE_STM32/67.png)


<!-- ------------------------ -->
## 实验要求


- 重点了解程序如何在指定的点坐标上(x,y)显示一个点。


<!-- ------------------------ -->
## 实验原理


### OLED介绍

#### OLED屏：

OLED显示屏是利用有机电自发光二极管制成的显示屏。由于同时具备自发光有机电激发光二极管，不需背光源、对比度高、厚度薄、视角广、反应速度快、可用于挠曲性面板、使用温度范围广、构造及制程较简单等优异之特性，被认为是下一代的平面显示器新兴应用技术。

#### OLED技术功能：

有机发光二极管 (OLED)显示器越来越普遍，在手机、媒体播放器及小型入门级电视等产品中最为显著。不同于标准的液晶显示器，OLED像素是由电流源所驱动。若要了解 OLED电源供应如何及为何会影响显示器画质，必须先了解 OLED显示器技术及电源供应需求。本文将说明最新的OLED显示器技术，并探讨主要的电源供应需求及解决方案，另外也介绍专为 OLED电源供应需求而提出的创新性电源供应架构。

背板技术造就软性显示器，高分辨率彩色主动式矩阵有机发光二极管 (AMOLED) 显示器需要采用主动式矩阵背板，此背板使用主动式开关进行各像素的开关。液晶 (LC) 显示器非晶硅制程已臻成熟，可供应低成本的主动式矩阵背板，并且可用于OLED。许多公司正针对软性显示器开发有机薄膜晶体管 (OTFT) 背板制程，此一制程也可用于OLED显示器，以实现全彩软性显示器的推出。不论是标准或软性OLED，都需要运用相同的电源供应及驱动技术。若要了解OLED技术、功能及其与电源供应之间的互动，必须深入剖析这项技术本身。OLED显示器是一种自体发光显示器技术，完全不需要任何背光。OLED采用的材质属于化学结构适用的有机材质。OLED技术需要电流控制驱动方法，OLED具有与标准发光二极管 (LED)相当类似的电气特性，亮度均取决于LED电流。若要开启和关闭OLED并控制OLED电流，需要使用薄膜晶体管(TFT)的控制电路。

### 硬件设计

OLED屏与底座模块通过四线SPI总线接口通信。

![OLED模块电路](/assets/BASE_STM32/68.png)

<!-- ------------------------ -->

## 实验步骤


① 将OLED模块安装于STM32底座上，ST-Link连接电脑与STM32底座，如下图：

![安装模块](/assets/BASE_STM32/69.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\2.OLED模块\USER\OLED.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)

![选择文件](/assets/BASE_STM32/70.png)

⑥ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)

![下载成功](/assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 观察实验，程序运行后显示如下：  
![OLED屏显示](/assets/BASE_STM32/71.png)


<!-- ------------------------ -->
## 代码讲解


① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
![程序目录结构](/assets/BASE_STM32/124.jpg)

② main.c中进行OLED屏的初始化及显示“OLED显示实验”，“Hello world!”。

```c
int main(void)
{
    HAL_Init();//初始化HAL库    
    IIC_Init();//初始化OLED屏IIC
    OLED_Init();//初始化OLED
    
    /*在屏幕上显示出 OLED显示实验*/
    OLED_P8x16Str(3,0,"OLED");
    OLED_P16x16Ch(35,0,7);//显
    OLED_P16x16Ch(51,0,8);//示
    OLED_P16x16Ch(67,0,5);//实
    OLED_P16x16Ch(83,0,6);//验
        
    OLED_P8x16Str(3,2,"Hello world!");//显示Hello world  
    while(1);
}
```

③ IIC.c 是OLED的IIC接口驱动，关键函数为Write_IIC_Byte()。显示屏的应用程序最终都会调用该函数将数据写到显示屏。

```c
u8 Write_IIC_Byte(u8 byte)
{
    u8 i, ack;

    IIC_SCL_OUTPUT();
    IIC_SDA_OUTPUT();//设置为输出模式
        
    for(i = 0; i < 8; i++)
    {
        if(byte & 0x80)//一次读取最高位 发送数据
        {
            SDA_HIGH();
        }
        else 
        {
            SDA_LOW();
        }
            
        SCL_HIGH();//时钟线拉高
        IIC_Delay(1);
        SCL_LOW();//时钟线拉低
        IIC_Delay(1);
            
        byte <<= 1;
    }
```

④ OLED.c为OLED显示屏，OLED_Init()屏幕初始化驱动，设置屏幕参数。
```c
    void OLED_Init(void)
    {
        HAL_Delay(1000); /*显示1s等待上电完成*/      
        OLED_WrCmd(0xae);       
        OLED_WrCmd(0x00);      
        OLED_WrCmd(0x10);       
        OLED_WrCmd(0x40);       
        OLED_WrCmd(0x81);      
        OLED_WrCmd(0xCF); 
        OLED_WrCmd(0xa1);       
        OLED_WrCmd(0xc8);       
        OLED_WrCmd(0xa6);      
        OLED_WrCmd(0xa8);       
        OLED_WrCmd(0x3f);       
        OLED_WrCmd(0xd3);       
        OLED_WrCmd(0x00);       
        OLED_WrCmd(0xd5);       
        OLED_WrCmd(0x80);       
        OLED_WrCmd(0xd9);       
        OLED_WrCmd(0xf1);       
        OLED_WrCmd(0xda);       
        OLED_WrCmd(0x12);
        OLED_WrCmd(0xdb);       
        OLED_WrCmd(0x40);       
        OLED_WrCmd(0x20);      
        OLED_WrCmd(0x02); 
        OLED_WrCmd(0x8d);       
        OLED_WrCmd(0x14);       
        OLED_WrCmd(0xa4);      
        OLED_WrCmd(0xa6);       
        OLED_WrCmd(0xaf);       
        OLED_Fill(0x00);        
        OLED_Set_Pos(0,0);
        OLED_Fill(0xff);//全屏亮  
        OLED_CLS();//清屏   
} 
```

并提供显示显示字符串操作接口函数。  
6x8点阵字符操作函数: 

```c
    //函数名称：	OLED_P6x8Str()
    //
    //函数功能：	显示6*8一组标准ASCLL字符串
    //
    //入口参数：	显示坐标(X,Y) y(0~7)
    //
    //返回参数：	无
    //
    //说明：		
```

    16x16点阵字符操作函数:
```c
    //函数名称：	OLED_P16x16Ch_Power()
    //
    //函数功能：	显示16*16点阵-电量显示使用
    //
    //入口参数：	显示坐标(X,Y) y(0~7)
    //
    //返回参数：	无
    //
    //说明：
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


1. 将Hello World！居中显示。