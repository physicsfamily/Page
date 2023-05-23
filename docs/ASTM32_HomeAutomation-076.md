---
layout: post
title:  "076_STM32_HomeAutomation"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# 综合实验_基于STM32的智能家居系统实验
<!-- ------------------------ -->
## 实验内容
Duration: 5

- 获取温湿度并显示；
- 实现对风扇及LED模块LED灯的控制。

<!-- ------------------------ -->
## 实验目的
Duration: 5

- 熟练使用485总线读数据及发送控制指令。


<!-- ------------------------ -->
## 实验环境
Duration: 6

### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 4个 |  · |
| 3 | 风扇模块 | 1个 | ·  |
| 4 | 温湿度模块 | 1个 | ·  |
| 5 | LED模块 | 1个 | ·  |
| 6 | OLED模块 | 1个 | ·  |
| 7 | ST-Link下载器 | 1个 | · |
| 8 | ST-Link下载器连接线 | 1根 |  · |
| 9 | STM32智能家居实验代码 | 4份 | ·  |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)

![STM32底座](/assets/STM32/2.png)

[风扇模块](https://docs.stepiot.com/docs/aiot015)

![风扇模块](/assets/STM32_OneNET/51.png)

[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](/assets/BASE_STM32/2.png)

[LED模块](https://docs.stepiot.com/docs/aiot001)

![LED模块](/assets/STM32_OneNET/37.png)

[OLED模块](https://docs.stepiot.com/docs/aiot003)

![OLED模块](/assets/BASE_STM32/67.png)

<!-- ------------------------ -->
## 实验原理
Duration: 20

### RS485总线标准

RS485采用平衡发送和差分接收方式实现通信：发送端将串行口的ttl电平信号转换成差分信号a,b两路输出，经过线缆传输之后在接收端将差分信号还原成ttl电平信号。由于传输线通常使用双绞线，又是差分传输，所以有极强的抗共模干扰的能力，总线收发器灵敏度很高，可以检测到低至200mv电压。故传输信号在千米之外都是可以恢复。rs-485最大的通信距离约为1219m，最大传输速率为10mb/s，传输速率与传输距离成反比，在100kb/s的传输速率下，才可以达到最大的通信距离，如果需传输更长的距离，需要加485中继器。rs-485采用半双工工作方式，支持多点数据通信。rs-485总线网络拓扑一般采用终端匹配的总线型结构。即采用一条总线将各个节点串接起来，不支持环形或星型网络。如果需要使用星型结构，就必须使用485中继器或者485集线器才可以。rs-485总线一般最大支持32个节点，如果使用特制的485芯片，可以达到128个或者256个节点，最大的可以支持到400个节点。

### UART与RS-485性能对比

**抗干扰性：**RS485 接口是采用平衡驱动器和差分接收器的组合，抗噪声干扰性好。RS232 接口使用一根信号线和一根信号返回线而构成共地的传输形式，这种共地传输容易产生共模干扰。

**传输距离：**RS485 接口的最大传输距离标准值为 1200 米（9600bps 时），实际上可达 3000 米。RS232 传输距离有限，最大传输距离标准值为 50 米，实际上也只能用在 15 米左右。

**通信能力：**RS-485 接口在总线上是允许连接多达128个收发器，用户可以利用单一的 RS-485 接口方便地建立起设备网络。RS-232只允许一对一通信。

**传输速率：**RS-232传输速率较低，在异步传输时，波特率为20Kbps。RS-485 的数据最高传输速率为 10Mbps。
信号线：RS485 接口组成的半双工网络，一般只需二根信号线。RS-232 口一般只使用 RXD、TXD、GND 三条线。

**电气电平值：**RS-485的逻辑"1"以两线间的电压差为+（2-6）V表示；逻辑"0"以两线间的电压差为-（2-6）V表示。在 RS-232-C 中任何一条信号线的电压均为负逻辑关系。即：逻辑"1"，-5- -15V；逻辑"0 " +5- +15V。

### 实验底座RS485总线原理

每个底座周边都有RS485总线接口，由底座的485信号由MCU的UART信号+MAX3485 485总线转换芯片组成。

![实验底座RS485总线原理](/assets/STM32_NodeRED/220.jpg)

<!-- ------------------------ -->
## 实验步骤
Duration: 15

1. 将风扇模块、温湿度模块、OLED模块、LED模块分别安装在STM32底座上，确认各个节点，如下图所示，ST_LINK连接OLED节点连接。

    ![搭建实验硬件平台](/assets/HomeAutomation/1.png)

2. 打开 Keil 5 工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\STM32智能家居实验\OLED模块程序\USER\OLED.uvprojx` 并打开。

    ![启动工程](/assets/HomeAutomation/2.jpg)

3. 编译工程，然后将程序下载到OLED节点底座中。

    ![编译并下载程序](/assets/STM32_NodeRED/255.png)

4. 将STLink连接到温湿度节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\STM32智能家居实验\温湿度模块程序\USER\SHT20.uvprojx` 并打开。

    ![启动工程](/assets/HomeAutomation/3.jpg)

5. 编译工程，然后将程序下载到温湿度节点底座中。

    ![编译并下载程序](/assets/STM32_NodeRED/255.png)

6. 将STLink连接到LED节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\STM32智能家居实验\LED模块程序\USER\LED.uvprojx` 并打开。

    ![启动工程](/assets/HomeAutomation/4.jpg)

7. 编译工程，然后将程序下载到LED节点底座中。

    ![编译并下载程序](/assets/STM32_NodeRED/255.png)

8. 将STLink连接到风扇节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\STM32智能家居实验\风扇模块程序\USER\FAN.uvprojx` 并打开。

    ![启动工程](/assets/HomeAutomation/5.jpg)

9. 编译工程，然后将程序下载到风扇节点底座中。

    ![编译并下载程序](/assets/STM32_NodeRED/255.png)

10. 从STM32底座上取下STLink的USB线。各节点拼接，STLink的USB线重新接上(接任意底座)，给设备重新上电。

11. 上电后OLED屏显示传感器数据。

    ![传感器数据](/assets/HomeAutomation/6.png)

12. 单击OLED上的按键，选择控制风扇、灯1、灯2、灯3、灯4(灯1、灯2、灯3、灯4指的是LED模块上的四个灯)，选择好后双击OLED的按键进行控制操作。

    ![控制设备](/assets/HomeAutomation/7.png)


<!-- ------------------------ -->
## 代码讲解
Duration: 15

### 风扇节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/8.jpg)

② main.c中对串口、风扇模块、RS485协议进行初始化。初始化完成后。调用函数DataHandling_485()获取控制指令，依控制指令控制风扇开/关。调用PWM_SetTIM4Compare2()设置风扇转运或者停止。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        /*PWM占空比100%~0%,分别对应，2000~0*/
        TIM4_PWM_Init(64-1,2000-1);//初始化定时器4输出PWM信号
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1,用于485通信
        USART3_Init(115200);
        printf("this usart3 print\r\n");
        while(1)
        {
            HAL_Delay(10);//延时10ms每10ms检测一次指令
            if(!DataHandling_485(Addr_Fan)){//是发给本机的指令
                printf("get data\r\n"); 
                RelayState = Rx_Stack.Data[0];//控制指令
                if(RelayState){
                    PWM_SetTIM4Compare2(2000);//风扇打开 
                }
                else{
                    PWM_SetTIM4Compare2(0);//风扇关闭                
                }
            }        
        }
    }
```

### 温湿度节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/9.jpg)

② main.c中对串口、定时器、温湿度传感器、RS485协议进行初始化。初始化完成后，定时读取传感器数据、显示传感器数据，处理WIFI节点的请求，并返回传感器数据到温湿度节点。

```c
    int main(void)
    {
        uint8_t state = 0;//显示温度 还是湿度
        HAL_Init();//初始化HAL库  
        Rs485_Init();//初始化485
        SHT2x_Init();//初始化SHT20
        TM1640_Init();//初始化TM1640
        USART3_Init(115200);//用于调试
        UART1_Init(115200);//初始化串口1,RS485通信
        
        /*中断频率20HZ,关联RS485_HandlerCb()回调函数*/
        TIM3_Init(10000-1,320-1,RS485_HandlerCb);    
        while(1)
        {
            if((state == 0) && (HAL_GetTick()>(Tick_Disp))){//更新温度
                state = 1;
                SHT2x_GetTempHumi();//读取一次温湿度，保存在g_sht2x_param.TEMP_HM
                send_LED_Display(0xC0,g_sht2x_param.TEMP_HM,1);//数码管显示
                SendBuffer[0] = g_sht2x_param.TEMP_HM;//保存用于发送到WIFI节点
                Tick_Disp = HAL_GetTick()+1000/*1000ms*/;//设置下一次更新数据的时间点
            }
            else if((state == 1) && (HAL_GetTick()>(Tick_Disp))){//更新湿度
                state = 0;
                SHT2x_GetTempHumi();//读取一次温湿度,保存在g_sht2x_param.HUMI_HM        
                send_LED_Display(0xC0,g_sht2x_param.HUMI_HM,2);
                SendBuffer[1] = g_sht2x_param.HUMI_HM;//保存用于发送到WIFI节点        
                Tick_Disp = HAL_GetTick()+1000/*1000ms*/;//设置下一次更新数据的时间点        
            }
        }
    }
```

### LED节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/11.jpg)

② main.c中对串口、LED模块、RS485协议进行初始化。初始化完成后。调用函数DataHandling_485()获取控制指令，依控制指令调用LED_SetState()控制LED节点LED灯亮/灭。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        LED_Init();//初始化模块
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1,用于485通信
        USART3_Init(115200);//用于调试
        printf("this usart3 print\r\n");
        while(1)
        {
            HAL_Delay(50);//延时100ms,每一100ms检测一次。
            if(!DataHandling_485(Addr_LED)){//是发给本机的命令
                printf("get cmd\r\n=%d,%d\r\n",Rx_Stack.Data[0],Rx_Stack.Data[1]);//调试打印
                LED_Index = Rx_Stack.Data[0];
                LED_State = Rx_Stack.Data[1];//LED1灯命令
                LED_SetState(LED_Index,LED_State);//LED灯控制
            }        
        }
    }
```

### OLED节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/11.jpg)

② main.c中对串口、RS485协议进行初始化，OLED初始化。初始化完成后，定时请求传感器数据，更新数据。检测按键操作根据根据不同的操作实现风扇、LED灯的控制。

③ 设备控制代码，OLED_KeyPoll()，获取当前按键是单击还是双击。根据单击为选择控制对象(风扇、灯)。双击进行控制操作(打开、关闭、亮、灭)。

```c
    switch(OLED_KeyPoll())
    {
        case KV_DOUBLE://如果是双击
        printf("KV_DOUBLE\r\n");
        if(DevItem == 1){//控制风扇
            FANCtrl = 1;//使能风扇控制
            FANCmd  = 1 - FANCmd;
        }
        else{//控制LED模块上的LED灯
            LEDCtrl   = 1; //使能LED灯控制
            LEDCmd[0] = DevItem - 1;//控制哪个灯
            LED_State[LEDCmd[0]-1] = 1 - LED_State[LEDCmd[0]-1];//保存状态                  
            LEDCmd[1] = LED_State[LEDCmd[0]-1];//控制动作(亮/灭)
        }
        break;
        case KV_SINGLE://如果是单击
        printf("KV_SINGLE\r\n"); 
        DevItem++;
        if(DevItem>5){//只有5个选项，风扇 灯1 灯2 灯3 灯4
            DevItem = 1;
        }
        OLED_SelectDev(DevItem);//更新界面的选项中反显。
        break;
    }
```

④ 传感器数据更新。

```c
    if(HAL_GetTick() > Tick_SendSensor){//更新传感器数据
        Tick_SendSensor = HAL_GetTick() + 2000;//每两秒更新一次
        OLED_Disp_Temp(0,0,SensorData[0]);//更新温度 
        OLED_Disp_Humi(0,2,SensorData[1]);//更新湿度
    }
```

⑤ RS485_HandlerCb()是定时器回调函数回调频率为4Hz，同时也是485通信的接口，485总线上的数据发送及请求传感器数据，均由该函数实现。

```c
    void RS485_HandlerCb(void)
    {
        static uint8_t state = 0;
        if(state == 0){//发送请求
            if(FANCtrl == 1){//发送风扇控制命令
                FANCtrl = 0;
                /*发送到风扇节点*/ 
                Rs485_Send(Addr_WiFi,Addr_Fan,FAN_CTRL,1,(void*)&FANCmd);             
            }
            else if(LEDCtrl == 1){//发送LED控制命令
                LEDCtrl = 0;
                /*发送到LED节点*/           
                Rs485_Send(Addr_WiFi,Addr_LED,LED_Control,2,(void*)&LEDCmd[0]);           
            }
            else{//请求数据
                Rs485_Send(Addr_WiFi,Addr_SHT20,SHT20_Get_All,0,(void*)0);
                printf("请求传感器数据\n");
                state = 1;
            }
        }
        else{//检测是否返回数据
            if(!DataHandling_485(Addr_WiFi)){	//是本机期望的485数据处理
                printf("get data\r\n");
                SensorData[0] = Rx_Stack.Data[0];
                SensorData[1] = Rx_Stack.Data[1];            
            }
            state = 0;
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

1. 将继电器模块加入其中并实现对继电器的控制。
