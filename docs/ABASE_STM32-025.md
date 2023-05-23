---
layout: post
title:  "025_基于STM32的继电器模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的继电器模块实验
<!-- ------------------------ -->
## 实验内容


- 搭建实验硬件环境；
- 理解继电器工作原理；
- 通过程序编写开启、关闭继电器。

<!-- ------------------------ -->
## 实验目的


- 熟悉继电器模块实验套件的使用；
- 了解继电器的相关知识；
- 了解继电器的运用场景。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | 继电器模块| 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | 继电器模块实验代码 | 1份 | · |


### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [oneNET](https://codelab.stepiot.com/codelabs/oneNet_080/index.html?index=..%2F..index#0)平台应用手册

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[继电器模块](https://docs.stepiot.com/docs/aiot014)

![继电器模块](/assets/BASE_STM32/100.png)


<!-- ------------------------ -->
## 实验要求


- 掌握继电器的工作原理；
- 熟悉继电器的常开触点（NO），常闭触点（NC），公共端（COM）。
  
<!-- ------------------------ -->
## 实验原理


### 继电器的工作原理

如下图，当开关S闭合时，线圈结构A有电流通过产生电磁场，把衔铁B吸下来使D和E接触，工作电路闭合。工作电路正常工作。

当开关S断开时，线圈结构A内无电流通过失去磁性，衔铁B松开，弹簧把衔铁拉起来，D和E断开，切断工作电路。

总的说，继电器就是利用电磁铁控制工作电路通断的开关。继电器的工作原理，使得继电器非常适合于用低电压控制高电压、小电流控制大电流的场景。

![继电器工作原理](/assets/BASE_STM32/101.png)

### 常开触点（NO）、常闭触点（NC）、公共端（COM）

下图是一个开关，此时控制电路未动作，我们把这时的触点A称为公共端（COM）。触点B称为常闭（NC）触点，触点C称为常开（NO）触。

![开关](/assets/BASE_STM32/102.jpg)

NO英文全称Normal open，NC英文全称Normal close。

### 继电器模块基本电路

![继电器模块基本电路](/assets/BASE_STM32/103.png)

通过对电路的分析，当PB14输出高电平时，RL2继电器工作，此时，LED3亮起，当PB13输出高电平时，RL1继电器，此时，LED2亮起。

<!-- ------------------------ -->

## 实验步骤


① 将继电器模块安装在STM32底座上，ST_LINK连接电脑与STM32底座，如下图所示：

![安装模块](/assets/BASE_STM32/104.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\13.继电器模块\继电器模块程序\USER\Relay.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择工程](/assets/BASE_STM32/105.jpg)

⑥ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 观察实验：可以听到继电器1和继电器2交替开关并伴随嗒嗒声。



<!-- ------------------------ -->
## 代码讲解


① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/BASE_STM32/106.jpg)  

② main.c继电器控制IO进行初始化。每2秒分别依次控制继电器1、继电器2的开关。  

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        Relay_Init();//初始化继电器

        while(1)
        {
            RELAY1_OPEN();//打开继电器1
            delay_ms(2000);
                
            RELAY1_CLOSE();//关闭继电器1
            delay_ms(2000);

            RELAY2_OPEN();//打开继电器2 
            delay_ms(2000);
                
            RELAY2_CLOSE();//关闭继电器2
            delay_ms(2000);
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


<!-- ------------------------ -->
## 实验思考


1. 控制继电器1打开时底座灯亮红色。
   
2. 编写一个函数，可根据参数选择打开那个继电器及打开时。例如：
  void RelayControl(uint8_t relayindex,uint16_t time)。
