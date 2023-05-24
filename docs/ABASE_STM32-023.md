---
layout: post
title:  "023_基于STM32的NB-IoT模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# 基于STM32的NB-IoT模块实验
<!-- ------------------------ -->
## 实验内容


- 搭建实验硬件环境；
- 下载程序；
- 使用NB-IoT模块接入OneNET云平台。

<!-- ------------------------ -->
## 实验目的


- 熟悉NB-IoT模块的使用；
- 了解CoAP（LWM2M）协议；
- 掌握NB-IoT连接OneNET平台方法。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | NB-IoT模块 | 1个 |  · |
| 4 | NB-IoT卡 | 1张 |  · |
| 5 | 天线 | 1根 |  · |
| 6 | ST-Link下载器 | 1个 | · |
| 7 | ST-Link下载器连接线 | 1根 |  · |
| 8 | NB-IoT 连接OneNET实验代码 | 1份 | · |


### 实验所需软件

- [MDK5](https://codelabs.stepiot.com/docs/AMDK5-078.html) 安装步骤

- [ST-LINK](https://codelabs.stepiot.com/docs/AST_LINK-079.html) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[NB-IoT模块](https://docs.stepiot.com/docs/aiot012)

![NB-IoT模块](/assets/BASE_STM32/72.png)


<!-- ------------------------ -->
## 实验要求


- 理解实验原理；
- 了解OneNET云平台；
- 熟悉连接云平台流程；
- 掌握OneNET云平台创建应用方法。
  
<!-- ------------------------ -->
## 实验原理


### NB-IoT简介

NB-IoT（Narrow Band Internet of Things，窄带物联网）成为万物互联网络的一个重要分支。NB-IoT构建于蜂窝网络，NB-IoT射频带宽为200kHz。下行速率：大于160kbps，小于250kbps。上行速率：大于160kbps，小于250kbps(Multi-tone)/200kbps(Single-tone)。可直接部署于GSM网络、UMTS网络或LTE网络，以降低部署成本、实现平滑升级。

NB-IoT是IoT领域一个新兴的技术，支持低功耗设备在广域网的蜂窝数据连接，也被叫作低功耗广域网（LPWAN）。NB-IoT支持待机时间长、对网络连接要求较高设备的高效连接。NB-IoT设备电池寿命可以提高至少10年，同时还能提供非常全面的室内蜂窝数据连接覆盖。

### NB-IoT无线技术特点

1. 广覆盖。相比现有的GSM、宽带LTE等网络覆盖增强了20dB，信号的传输覆盖范围更大（GSM基站目前理想状况下能覆盖35km），能覆盖到深层地下GSM网络无法覆盖到的地方。其原理主要依靠：
    - 缩小带宽，提升功率谱密度；
    - 重复发送，获得时间分集增益。
2. 大连接。相比现有无线技术，同一基站下增多了50-100倍的接入数，每小区可以达到50K连接，真是实现万物互联所必须的海量连接。其原理在于：
    - 基于时延不敏感的特点，采用话务模型，保存更多接入设备的上下文，在休眠态和激活态之间切换；
    - 窄带物联网的上行调度颗粒小，资源利用率更高；
    - 减少空口信令交互，提升频谱密度。  
3. 低功耗。终端在99%的时间内均处在休眠态，并集成多种节电技术，待机时间可达10年。1、PSM低功耗模式，即在idle空闲态下增加PSM态 ，相当于关机，由定时器控制呼醒，耗能更低；2、eDRX扩展的非连续接收省电模式，采用更长的寻呼周期，eDRX是DRX耗电量的1/16。
4. 低成本。硬件可剪裁，软件按需简化，确保了NB-IoT的成本低廉，NB-IoT通信单模块成本不足5美元。
5. NB-IoT因其适用的场景，还具有低速率和低移动性的特点。
    - 低速率。多点上行速率仅为56kbps，理想下行速率为21.25kbps；
    - 低移动性。仅支持终端设备在30km/h的移动速率下实现小区切换，远低于4G支持250km/h的速率（高铁专网可达450km/h）。

### NB-IoT模块电路图

![NB-IoT模块电路图](/assets/BASE_STM32/73.png)

### NB-IoT模块介绍（M5310）

M5310 模块是一款工业级的两频段 NB-IoT 无线模块， 其工作频段是 Band 5 或Band 8， 它主要应用于低功耗的数据传输业务， 满足 3gpp Release13 标准。M5310是 LCC 封装的贴片式模块， 30 个管脚，尺寸仅有 19mm×18mm×2.2mm。 M5310 内置 UDP/CoAP 等数据传输协议及扩展的 AT 命令， 模块采用了低功耗技术，电流功耗在深度睡眠模式低至 5uA。模块实物正面图如下图所示：

![NB-IoT模块实物正面图](/assets/BASE_STM32/74.png)

<!-- ------------------------ -->

## 实验步骤


① OneNET平台上创建上应用，参考[oneNET平台应用手册]()注册帐号并创建应用。
   
② 将NBIOT卡片安装到NBIOT模块上，NBIOT模块安装天线，然后安装于底座，STLink连接STM32底座与电脑，如下图所示：  
![安装模块](/assets/BASE_STM32/75.png)  

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

⑥ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\11.NB-IOT模块\NB-IoT模块程序\USER\NB-IoT.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择工程](/assets/BASE_STM32/76.jpg)

⑦ 工程启动后，打开main.c，修改其中的IMEI码，其中IMEI码从模组上读取，然后保存。

![读取IMEI](/assets/BASE_STM32/77.png)


⑧ 点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑨ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑩ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑪ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上），等待约30s（如果信号差等待时间会更长一些）。

⑫ 创建应用完成后。 点击“应用管理”查看应用中仪表盘的变化。在程序中模拟了一组温湿度参数，传送到平台，如下图：
![实验结果](/assets/BASE_STM32/79？.png)
![实验结果](/assets/BASE_STM32/80？.png)



<!-- ------------------------ -->
## 代码讲解


① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
![程序目录结构](/assets/BASE_STM32/81.jpg)  

② main.c中对串口、NBIOT模组M5310进行初始化,发起OneNET登陆请求。完成登陆后每5秒向OneNET平台发送一次数据。

```c
    {
        HAL_Init();//初始化HAL库  
        Rs485_Init();//初始化485
        Platform_LED_Init();
        M5310_Power_Init();//M5310的复位控制IO初始化
        UART1_Init(115200);//初始化串口1
        UART2_Init(9600);//初始化串口2
        netdev_init();//初始化M5310
        init_miplconf(86400,coap_uri,endpoint_name);//上报注册码
        Subscription_esources();//订阅 Object 组配置
        m5310_register_request();//发出登陆请求
        delay_ms(1000);
        TIM2_Init(1000-1,64-1);//初始化定时器2(1ms)
        Ticks_SendData = HAL_GetTick() + 10000; 
        Platform_LED_Green();
        while(1)
        {
            msgcode = Msg_Handler();//返回数据解析
            USART_Data_Send();//串口数据发送
            if(HAL_GetTick() > Ticks_SendData){
                memset((void*)SendOneNET,0,10);
                sprintf((char *)SendOneNET,"%0.2f",temp);
                m5310_notify_upload(&temp_uri,NBIOT_FLOAT,(char*)&SendOneNET[0],1,0);//上传温度数据
                
                memset((void*)SendOneNET,0,10);
                sprintf((char *)SendOneNET,"%0.2f",humi);
                m5310_notify_upload(&humi_uri,NBIOT_FLOAT,(char*)&SendOneNET[0],1,0);//上传湿度数据
                Ticks_SendData = HAL_GetTick() + 5000;//5秒后在次发送
                humi++;
                if(humi>50){
                    humi = 30;
                }
                temp++;
                if(temp>60){
                    temp = 10;
                    }
                }      
            }
    }
```

m5310_notify_upload()函数将数据传送到OneNET平台上：
    
```c
    sprintf((char *)SendOneNET,"%0.2f",humi);
	m5310_notify_upload(&humi_uri,NBIOT_FLOAT,(char*)&SendOneNET[0],1,0);//上传湿度数据
    Ticks_SendData = HAL_GetTick() + 5000;//5秒后在次发送
    humi++;
    if(humi>50){
        humi = 30;
    }
        temp++;
    if(temp>60){
        temp = 10;
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
  
3. OneNET平台设备没有上线。

    - 代码中IMEI码、创建的设备IMEI及模组的IMEI是否一致。

<!-- ------------------------ -->
## 实验思考


1. 当温湿度数据变化时才向OneNET平台发送数据。
