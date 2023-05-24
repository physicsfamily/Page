---
layout: post
title:  "018_基于STM32的PM2.5传感器模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的PM2.5传感器模块实验
<!-- ------------------------ -->
## 实验内容


- 了解PM2.5模块工作原理；
- 使用PM2.5模块检测当前环境PM2.5值；
- 编写程序检测环境的PM2.5值并将数据传输到LCD显示屏模块上显示。

<!-- ------------------------ -->
## 实验目的


- 认识PM2.5模块模块；
- 了解PM2.5模块工作原理；
- 掌握PM2.5模块配合其他模块使用技巧。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | PM2.5模块 | 1个 |  · |
| 4 | OLED模块 | 1个 | · |
| 5 | ST-Link下载器 | 1个 | · |
| 6 | ST-Link下载器连接线 | 1根 |  · |
| 7 | PM2.5实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelabs.stepiot.com/docs/AMDK5-078.html) 安装步骤

- [ST-LINK](https://codelabs.stepiot.com/docs/AST_LINK-079.html) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[OLED模块](https://docs.stepiot.com/docs/aiot003) & [PM2.5模块](https://docs.stepiot.com/docs/aiot006)

![OLED模块 & PM2.5模块](/assets/BASE_STM32/11.png)

<!-- ------------------------ -->
## 实验要求


- 理解PM2.5工作原理；
- 能够编写程序使用PM2.5模块检测环境PM2.5值。


<!-- ------------------------ -->
## 实验原理


### 什么是PM2.5

细颗粒物又称细粒、细颗粒、PM2.5。细颗粒物指环境空气中空气动力学当量直径小于等于 2.5 微米的颗粒物。它能较长时间悬浮于空气中，其在空气中含量浓度越高，就代表空气污染越严重。虽然PM2.5只是地球大气成分中含量很少的组分，但它对空气质量和能见度等有重要的影响。与较粗的大气颗粒物相比，PM2.5粒径小，面积大，活性强，易附带有毒、有害物质（例如，重金属、微生物等），且在大气中的停留时间长、输送距离远，因而对人体健康和大气环境质量的影响更大。

### PM2.5模块

pm2.5模块检测大气中粒径小于2.5μm细颗粒物质量的检测仪。虽然细颗粒物只是地球大气成分中含量很少的组分，但它对空气质量和能见度等有重要的影响。细颗粒物粒径小，有些细颗粒物富含大量的有毒、有害物质且在大气中的停留时间长、输送距离远，因而对人体健康和大气环境质量的影响更大。

pm2.5检测模块检测大气中粒径小于2.5μm细颗粒物质量的检测仪。其基本原理是激光经尘埃粒子散射后，对光学传感器输出的脉冲信号进行数字信号处理，测量参数设定。

由专用的激光模块产生一束特定的激光，当颗粒物经过时，其信号会被超高灵敏的数字电路模块检测到，通过对信号数据进行智能识别分析得到颗粒计数和颗粒大小，根据专业的标定技术得到粒径分布与质量浓度转换公式，最终得到跟官方单位统一的质量浓度。

![PM2.5模块工作原理](/assets/BASE_STM32/12.png)

<!-- ------------------------ -->

## 实验步骤


① 将OLED模块和PM2.5模块安装在STM32底座上，确认各个节点，将ST_LINK连接电脑与PM2.5节点的底座上，如下图所示：

![安装模块](/assets/BASE_STM32/46.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\6.PM2.5模块\PM2.5模块程序\USER\PM2.5.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择文件](/assets/BASE_STM32/47.jpg)

⑥ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑨ 将STLink连接到OLED模块底座上节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\6.PM2.5模块\OLED显示屏模块程序\USER\OLED.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)
![选择文件](/assets/BASE_STM32/117.jpg)

⑩ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑪ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑫ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)
![下载成功](/assets/STM32/41.jpg)

⑬ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上），将两个底座拼接在一起。

⑭ 观察可以看到OLED屏上显示的PM2.5数据。  
![实验结果](/assets/BASE_STM32/48.png)



<!-- ------------------------ -->
## 代码讲解


### PM2.5模块

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。     
![程序目录结构](/assets/BASE_STM32/118.jpg)

② main.c中对串口、定时器器、ADC、RS485协议进行初始化。其中定时器为PM2.5传感器数据采集提供时间基准。ADC采集PM2.5的传感器数据进行滤波，并通过485总线发送OLED屏显示。

```c
    int main(void)
    {
    float voltage;
    int i = 0;
  
    HAL_Init();//初始化HAL库
    ADC_Init();//初始化ADC，采集传感器反馈的信号
    Rs485_Init();//初始化485
    UART1_Init(115200);//初始化串口1 485总线使用
    UART3_Init(115200);//初始化串口3，用于调试
    PA_CTRL_Init();
    TIM3_Init(65530,640-1,(void*)0);//计数时钟10us

	while(1)
	{
        for(PM2P5_ADC_Count = 0;PM2P5_ADC_Count < SAMPLE_SIZE;PM2P5_ADC_Count++){
            /*ADC取样*/
            GPIOA->BSRR = GPIO_PIN_1;
            TIM3_DelayUS(28);//280us
            PM2P5_ADC[PM2P5_ADC_Count] = Get_Adc(ADC_CHANNEL_3);
            TIM3_DelayUS(1);//10us
            GPIOA->BRR = GPIO_PIN_1;
            TIM3_DelayUS(980);//9800us 
        }     
        bubbleSort(&PM2P5_ADC[0],SAMPLE_SIZE);//排序，数值小的在前面
        Data = 0;        
        for(i=1;i<(SAMPLE_SIZE-1);i++){//去除采样数据中的最高值及高低值
            Data += PM2P5_ADC[i];
        }
        Data = Data / (SAMPLE_SIZE-2);//减去最大和最小的两个
        voltage = (Data / 4096.0)*3.3;//转换成电压值
        
        PM25_ugm3 = (voltage*0.13)*1000;//单位 ug/m3  
        
        /*转换数据，通过485总线发送数据到显示器显示*/ 
        PM_Data[0] = PM25_ugm3>>8;PM_Data[1] = PM25_ugm3;
        Rs485_Send(Addr_PM2_5,Addr_LCD,PM2_5_Conc,2,PM_Data);	
        
        memset(PM_Data,0,10);//清空数组
        HAL_Delay(500);//每500ms采集一次
	    }   
    }
```

通过调用Rs485_Send()将数据发送到OLED屏显示。
    
```c
    /*转换数据，通过485总线发送数据到显示器显示*/ 
    PM_Data[0] = PM25_ugm3>>8;PM_Data[1] = PM25_ugm3;
    Rs485_Send(Addr_PM2_5,Addr_LCD,PM2_5_Conc,2,PM_Data);   
    memset(PM_Data,0,10);//清空数组
    HAL_Delay(500);//每500ms采集一次
```

### OLED模块

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
![程序目录结构](/assets/BASE_STM32/119.jpg)

② main.c中对串口、OLED屏、RS485协议进行初始化。接收485发送来的数据并显示在OLED屏上。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库
            Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口,485总线使用这个串口
        OLED_Init();//OLED屏初始化，初始化IIC总线，设置显示驱动芯片

        /*显示一个默认的数据0*/
        sprintf((char *)PMData,"PM2.5:%03d ug/m3",0);
        OLED_P8x16Str(0,3,PMData);	
    
            while(1)
            {
            if(!DataHandling_485(Addr_LCD))//接收是发给LCD显示的数据
            {
                pmdata = Rx_Stack.Data[0]*256 + Rx_Stack.Data[1];//获取485总线上的数据 
                sprintf((char *)PMData,"PM2.5:%03d ug/m3",pmdata);//将数据转换成字符串
                OLED_P8x16Str(0,3,PMData);//OLED屏显示数据	
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


<!-- ------------------------ -->
## 实验思考


1. 修改OLED屏的代码，实现OLED屏显示最近三次的PM2.5的值。
