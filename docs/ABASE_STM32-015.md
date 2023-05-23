---
layout: post
title:  "015_基于STM32的温湿度传感器实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的温湿度传感器实验
<!-- ------------------------ -->
## 实验内容
Duration: 4

- 点亮温湿度模块上数码管。
- 编程读取温湿度模块上的温度、湿度和光照强度数据并在数码管上显示。

<!-- ------------------------ -->
## 实验目的
Duration: 5

- 熟练使用IIC总线。 
- 熟悉温湿度模块的使用。
- 掌握温湿度基本电路及驱动原理（即如何点亮数码管、如何获取温湿度）。
- 掌握温湿度模块上光照传感器使用。

<!-- ------------------------ -->

## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 1个 |  · |
| 3 | 温湿度模块 | 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | 温湿度实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](assets/STM32/2.png)

[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](assets/BASE_STM32/2.png)


<!-- ------------------------ -->
## 实验要求
Duration: 6

- 熟悉实验程序流程图；
- 认识温度湿度传感器；
- 能够编程读取传感器的温湿度数据；
- 能够将读取的数据在数码管上显示。


<!-- ------------------------ -->
## 实验原理
Duration: 25

### 温度传感器工作原理

利用材料的物理特性会随着湿度变化的原理设计。比如利用金属膨胀原理设计的传感器，将双金属片由两片不同膨胀系数的金属贴在一起而组成，随着温度变化，材料A比另外一种金属膨胀程度要高，引起金属片弯曲。弯曲的曲率可以转换成一个输出信号比如利用金属随着温度变化，其电阻值也发生变化的原理。阻值的变化会导致电流，电压的变化通过测量电流电压。便可知道温度的变化。

### 湿度传感器工作原理

湿敏元件是最简单的湿度传感器。湿敏元件主要有电阻式、电容式两大类。湿敏电阻的特点是在基片上覆盖一层用感湿材料制成的膜，当空气中的水蒸气吸附在感湿膜上时，元件的电阻率和电阻值都发生变化，利用这一特性即可测量湿度。湿敏电容一般是用高分子薄膜电容制成的，常用的高分子材料有聚苯乙烯、聚酰亚胺、酪酸醋酸纤维等。当环境湿度发生改变时，湿敏电容的介电常数发生变化，使其电容量也发生变化，其电容变化量与相对湿度成正比。温湿度模块使用STH20，采用的是电容式湿敏元件。

### 温湿度模块电路图

![温湿度传感器基本电路图](assets/BASE_STM32/3.png)

![数码管驱动电路图](assets/BASE_STM32/4.png)

<!-- ------------------------ -->

## 实验步骤
Duration: 15

① 将温湿度模块模块安装于STM32底座上。ST-Link连接电脑与STM32底座，如下图：

![安装模块](assets/BASE_STM32/5.png)

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

⑤ 打开 `MDK5` 工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\3.温湿度模块\温湿度模块程序\USER\SHT20.uvprojx` 并打开。
   
![打开工程](assets/STM32/39.jpg)

![选择文件](assets/BASE_STM32/36.jpg)

⑥ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](assets/STM32/18.jpg)
![下载成功](assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 程序运行后读取温湿度模块获取环境温湿度，显示到数码管上。

![显示湿度值](assets/BASE_STM32/37.png)

![显示温度值](assets/BASE_STM32/38.png)

⑪ 用手轻按温湿传感器，观察温度和湿度的变化。
    

<!-- ------------------------ -->
## 代码讲解
Duration: 15

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](assets/BASE_STM32/125.jpg)

② main.c中对SHT20传感器、TM1640(数码管驱动)进行初始化。

```C
    uint32_t Tick_Disp;
    int main(void)
    {
    uint8_t state = 0;//显示温度 还是湿度
    
    HAL_Init();//初始化HAL库
        SHT2x_Init();//初始化温湿度传感器SHT20
        TM1640_Init();//初始化数码管驱动芯片TM1640
    
    /*HAL_GetTick(),返回值为单片机开机后运行时间值，单位为ms*/
    Tick_Disp = HAL_GetTick(); //HAL_GetTick(),HAL库函数，返回值以ms为单位。
        while(1)
        {
        if((state == 0) && (HAL_GetTick()>(Tick_Disp))){//更新温度
            state = 1;
            SHT2x_GetTempHumi();//读取一次温湿度，保存在g_sht2x_param.TEMP_HM
            send_LED_Display(0xC0,g_sht2x_param.TEMP_HM,1);//数码管显示
            
            Tick_Disp = HAL_GetTick()+1000/*1000ms*/;//设置下一次更新数据的时间点
        }
        else if((state == 1) && (HAL_GetTick()>(Tick_Disp))){//更新湿度
            state = 0;
            SHT2x_GetTempHumi();//读取一次温湿度,保存在g_sht2x_param.HUMI_HM        
            send_LED_Display(0xC0,g_sht2x_param.HUMI_HM,2);
            Tick_Disp = HAL_GetTick()+1000/*1000ms*/;//设置下一次更新数据的时间点        
            }
        }
    }
```

通过调用SHT2x_GetTempHumi(),读取一次温湿度，分别保存在g_sht2x_param.TEMP_HM，g_sht2x_param.HUMI_HM；调用send_LED_Display()，显示温湿度。 
     
```C
    if((state == 0) && (HAL_GetTick()>(Tick_Disp))){//更新温度
          state = 1;
          SHT2x_GetTempHumi();//读取一次温湿度，保存在g_sht2x_param.TEMP_HM
          send_LED_Display(0xC0,g_sht2x_param.TEMP_HM,1);//数码管显示
          
          Tick_Disp = HAL_GetTick()+1000/*1000ms*/;//设置下一次更新数据的时间点
      }
      else if((state == 1) && (HAL_GetTick()>(Tick_Disp))){//更新湿度
          state = 0;
          SHT2x_GetTempHumi();//读取一次温湿度,保存在g_sht2x_param.HUMI_HM        
          send_LED_Display(0xC0,g_sht2x_param.HUMI_HM,2);
          Tick_Disp = HAL_GetTick()+1000/*1000ms*/;//设置下一次更新数据的时间点        
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

1. 修改代码实验实现温湿度数据2秒更新一次。
   
2. 每200ms采集一次温湿度数据。采集5次，取平均数，再显示。