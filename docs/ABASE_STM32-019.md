---
layout: post
title:  "019_基于STM32的人体红外检测实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的人体红外检测实验
<!-- ------------------------ -->
## 实验内容
Duration: 4

- 检测到人体活动后，底座的LED灯会闪烁红色，未检测到人体活动LED灯熄灭。

<!-- ------------------------ -->
## 实验目的
Duration: 5

- 熟悉人体红外感应模块的使用；
- 了解人体红外传感器工作原理；
- 拓展该模块组合性使用。

<!-- ------------------------ -->

## 实验环境
Duration: 4

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 1个 |  · |
| 3 | 人体红外模块 | 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | 人体红外模块实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](assets/STM32/2.png)

[人体红外模块](https://docs.stepiot.com/docs/aiot008)

![人体红外模块](assets/BASE_STM32/13.png)


<!-- ------------------------ -->
## 实验要求
Duration: 6

- 了解菲涅尔透镜的使用目的；
- 由于人体红外传感器很容易受周围环境变化影响，应该注意环境变化对实验结果的影响；
- 应注意人体红传感器RE200B属于被动式传感器只有物体运行才能检测到，静止无法检测。

<!-- ------------------------ -->
## 实验原理
Duration: 25

### 人体红外传感器工作原理

首先，普通人体会发射10um左右的特定波长红外线，当人体红外线照射到传感器上后，因热释电效应将向外释放电荷信号，此信号很微弱需后续电路检测处理后变成足于检测的信号。

### 菲涅尔透镜工作原理

菲涅尔透镜（Fresnel lens 如下图 ），又名螺纹透镜，多是由聚烯烃材料注压而成的薄片，也有玻璃制作的，镜片表面一面为光面，另一面刻录了由小到大的同心圆，它的纹理是根据光的干涉及衍射以及相对灵敏度和接收角度要求来设计的。

![菲涅尔透镜](assets/BASE_STM32/14.png)

菲涅尔透镜广泛使用于在整个被动红外探测器中，其所起的作用是，当有人进入探测的范围，菲涅尔透镜将人体释放的红外光透过镜片被聚集在远距离A区或中距离B区或近距离C区的的同一焦点。而这个焦点位置就是传感器接收的面区。菲涅尔透镜能使人体红外传感器的工作角度更广，工作距离更远。

### 人体红外模块电路

人体红外模块电路

![人体红外模块电路](assets/BASE_STM32/15.png)

U2是人体红传感器的接口，后级信号检测电路由两级运算放大器组成，完成信号放大，最后一级完成信号调理功能。

<!-- ------------------------ -->

## 实验步骤
Duration: 15

① 将人体红外模块安装在STM32底座上，ST_LINK连接电脑与STM32底座如下图：

![安装模块](assets/BASE_STM32/49.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\7.人体红外检测实验\人体红外模块程序\USER\PIR.uvprojx` 并打开。
   
![打开工程](assets/STM32/39.jpg)
![选择文件](assets/BASE_STM32/50.png)

⑥ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](assets/STM32/18.jpg)
![下载成功](assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 检测到人体活动以后，底座红色LED灯亮起，未检测到人体活动时熄灭。
![实验效果](assets/BASE_STM32/51.jpg)

    
<!-- ------------------------ -->
## 代码讲解
Duration: 15

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。   
   ![程序目录结构](assets/BASE_STM32/128.jpg)

② main.c中对人体红外模块驱动、初化底座灯驱动，当检测到人体底亮底座灯。反则之。

```c
    int main(void)
    {
    HAL_Init();//初始化HAL库 
    PIR_Init();//初始化人体红外
    Platform_RGB_LED_Init();//初始化座灯控制IO
        while(1)
        {
            if(PIR_Read())//读检测信号
            {
                PIR_LED_ON();
                Platform_RGB_LED_Red();//底座灯亮红色
            }
            else
            {
                PIR_LED_OFF();
                Platform_RGB_LED_Blackout();//底座灯灭
            }
        }
    }
```

通过调用PIR_Read()获取探测结果。

```c
    if(PIR_Read())//读检测信号
        {
            PIR_LED_ON();
            Platform_RGB_LED_Red();//底座灯亮红色
        }
        else
        {
            PIR_LED_OFF();
            Platform_RGB_LED_Blackout();//底座灯灭
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

1. 修改代码实现每检测到一次人体信号，底座灯依次亮起红、绿、蓝。