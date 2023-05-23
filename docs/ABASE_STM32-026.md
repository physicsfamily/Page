---
layout: post
title:  "026_基于STM32的风扇模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的风扇模块实验
<!-- ------------------------ -->
## 实验内容
Duration: 4

- 熟练应用PWM，并控制风扇转动。
- 编写程序，使得风扇能够以不同的速度转动。

<!-- ------------------------ -->
## 实验目的
Duration: 5

- 熟悉风扇模块的使用；
- 了解PWM的概念；
- 能够编程实现用PWM控制风扇；
- 了解霍尔元件的工作原理。

<!-- ------------------------ -->

## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | 风扇模块| 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | 风扇模块实验代码 | 1份 | · |


### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [oneNET](https://codelab.stepiot.com/codelabs/oneNet_080/index.html?index=..%2F..index#0)平台应用手册

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[风扇模块](https://docs.stepiot.com/docs/aiot015)

![风扇模块](/assets/BASE_STM32/107.png)


<!-- ------------------------ -->
## 实验要求
Duration: 6

- 理解PWM相关概念，能够计算PWM频率、周期和占空比等相关参数；
- 能够用IO口来模拟生成PWM波形并控制电机；
- 能够画出程序流程图。
  
<!-- ------------------------ -->
## 实验原理
Duration: 25

### 风扇模块相关知识介绍

**PWM以及占空比：**

PWM是Pulse Width Modulation（脉冲宽度调制）的简称，图3.5.5所展示的是PWM的波形，PWM从波形上看就是一系列周期变化的脉冲波形。

![PWM的波形](/assets/BASE_STM32/108.png)

所谓的占空比（duty）即Ton脉冲持续的时间与周期T的比值，计算公式如下图所示。

![占空比与频率f计算公式](/assets/BASE_STM32/109.png)

占空比的变化范围可以从0%到100%，当占空为0%时，输出一直为低，当占空为100%时，输出一直为高。在本次的实验中，PWM波通过CC2530的IO口模拟生成，生成PWM的基本原理如下图所示：

![利用IO口模拟生成PWM的基本原理](/assets/BASE_STM32/110.png)

**霍尔传感器介绍：**

霍尔器件是一种基于霍尔效应的磁传感器。霍尔效应简单的说是通电的半导体薄片，加上和薄片表面垂直的磁场B，在薄片的横向两侧会出现一个电压，如下图（图中的Vh称为霍尔电压）。

当电机转速加快时，流过线圈的电流势必加大，霍尔电压也会随之增加，所以通过采集电机输出的霍尔电压，就能知道风扇转速。

![霍尔效应示意图](/assets/BASE_STM32/111.png)

### 硬件设计

电机的电源V-通过N-MOS管Q1接地，要使电机转动，PWM需输出高电平。当PWM的占空比增大时电机的转速也会随之增大。

![硬件图](/assets/BASE_STM32/112.png)


<!-- ------------------------ -->

## 实验步骤
Duration: 15

① 将风扇模块安装在STM32底座上，ST_LINK连接电脑与STM32底座，如下图所示：

![安装模块](/assets/BASE_STM32/113.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\14.风扇模块\风扇模块程序\USER\FAN.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择工程](/assets/BASE_STM32/114.jpg)

⑥ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 观察实验：程序运行后，风扇全速转动。



<!-- ------------------------ -->
## 代码讲解
Duration: 15

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
    
![程序目录结构](/assets/BASE_STM32/115.jpg)

② main.c风扇PWM信号初始化。每隔5秒改变一次风扇转速度。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        TIM4_PWM_Init(64-1,2000-1);//初始化定时器

        while(1)
        {
            PWM_SetTIM4Compare2(500);//高速
            delay_ms(5000);
            PWM_SetTIM4Compare2(1200);//中速
            delay_ms(5000);
            PWM_SetTIM4Compare2(1500);//低速
            delay_ms(5000);
            PWM_SetTIM4Compare2(2000);//关闭风扇
            delay_ms(5000);
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


<!-- ------------------------ -->
## 实验思考
Duration: 5

1. 修改代码使用GPIO输出高低电平，控制风扇转动。
   
2. 修改代码使用GPIO模拟PWM信号实现风扇转速控制。
