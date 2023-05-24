---
layout: post
title:  "073_STM32_NodeRED 基于可视化编程平台传感器基础实验_蜂鸣器控制实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# 基于可视化编程平台传感器基础实验_蜂鸣器控制实验
<!-- ------------------------ -->
## 实验内容


- 在NodeRED创建图表用于控制硬件的按钮；
- 使用WiFi模块连接NodeRED平台；
- 使用485总线进行数据通信；
- 通过WiFi模块将硬件控制命令传送到控制对象实现控制。

<!-- ------------------------ -->
## 实验目的


- 从NodeRED平台下发命令;
- 在NodeRED平台构建用户输入界面;
- 熟悉NodeRED平台的基本使用。

<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 2个 |  · |
| 3 | Wifi模块 | 1个 | ·  |
| 4 | 蜂鸣器模块 | 1个 | ·  |
| 5 | ST-Link下载器 | 1个 | · |
| 6 | ST-Link下载器连接线 | 1根 |  · |
| 7 | 蜂鸣器控制实验代码 | 2份 | ·  |

### 实验所需软件

- [MDK5](https://codelabs.stepiot.com/docs/AMDK5-078.html) 安装步骤

- [ST-LINK](https://codelabs.stepiot.com/docs/AST_LINK-079.html) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)

![STM32底座](/assets/STM32/2.png)

[WiFi模块](https://docs.stepiot.com/docs/aiot011)

![WiFi模块](/assets/BASE_CC2530/65.png)

[蜂鸣器模块](https://docs.stepiot.com/docs/aiot018)



<!-- ------------------------ -->
## 实验原理
0

### WIFI技术基本概念

WIFI英语全称是Wireless Fidelity，中文译成无线保真。WIFI是建立连接和进行通讯的手段，它对应一套通讯的规则，保证让两个节点能互相连接，设备连接建立后，通过TCP/IP和UDP等协议，传输数据，连接互联网络。WIFI模块的STA模式和AP模式。

AP模式：Access Point，提供无线接入服务，允许其它无线设备接入，提供数据访问，一般的无线路由/网桥工作在该模式下。AP和AP之间允许相互连接。Sta模式（SP模式）：Station类似于无线终端，sta本身并不接受无线的接入，它可以连接到AP，一般无线网卡即工作在该模式。对照表如下:

|分类|AP模式|SP模式|
|----|----|----|
|接入网络|接受|不接受|
|网卡|需要|不需要|
|终端|有线|无线|

### WiFi模块原理图

![WIFI模块硬件电路](/assets/STM32_NodeRED/219.png)

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


1. 智能综合实训台开机，等待开机完成。

2. 打开系统的DOS界面，在界面上输入`node-red`。

    ![启动node-red](/assets/STM32_NodeRED/221.png)

3. 打开google浏览器，在输入网址处输入(http://127.0.0.1:1880)，如下图：

    ![输入Node_RED地址](/assets/STM32_NodeRED/222.png)

    ![NodeRED编辑界面](/assets/STM32_NodeRED/223.png)

4. 在编辑界面左边的控制区搜索框内输入“text”找到text控制件，如下图：

    ![NodeRED编辑界面](/assets/STM32_NodeRED/224.png)

5. 采用`text`控件实现表头(显示界面背景信息)，新Group，如下图：

    ![新建Group](/assets/STM32_NodeRED/225.png)

6. 编辑、添加节点加入的Group节点。

    ![添加新节点](/assets/STM32_NodeRED/310.png)

7. 填写表头名称，名称为“重庆八城科技有限公司———————————蜂鸣器控制———————————物联网综合实验平台”，点击“添加”按钮完成退出，如下图：

    ![输入表头名称](/assets/STM32_NodeRED/290.png)

8. 设置宽度，点击“更新”按钮完成编辑，如下图：

    ![输入表头名称](/assets/STM32_NodeRED/311.png)

9. 编辑text控件，参数下图中的红框：

    ![输入表头名称](/assets/STM32_NodeRED/312.png)

10. 完成以上步骤就可以预先看看应用界面部署后的情况，在界面的右上角看到如下界面。点击“部署”。如下图：

    ![进行部署](/assets/STM32_NodeRED/230.png)

    部署完成后，“部署”图标变灰。如下图：

    ![部署成功](/assets/STM32_NodeRED/231.png)

11. 依次点击下图所示的按钮(位于编辑界面右上角)，查看部署后的的图面：

    ![部署成功](/assets/STM32_NodeRED/232.png)

12. 执行上一步骤后，看到如下界面：

    ![查看应用界面效果](/assets/STM32_NodeRED/313.png)

13. 回到编辑界面，在控件区中搜索框中输入`button`，如下图：

    ![搜索button](/assets/STM32_NodeRED/265.png)

14. 将图标节点`button`，拖入编辑界面中。

    拖入button控件

15. 双击刚拖入的`button`控件，编辑`button`，参数如下图。Group的选择一定要注意，否则部署后会看不到相应的控件。

    ![拖入button控件](/assets/STM32_NodeRED/314.png)

16. 到此，工作区内容如下图：

    ![图片](/assets/STM32_NodeRED/315.png)

17. 在控制区搜索框中输入`tcp out`搜索函数节点，如下图：

    ![搜索tcp out节点](/assets/STM32_NodeRED/269.png)

18. 将函数节点`tcp out`拖入编辑界面中，如下图：

    ![拖入函数节点](/assets/STM32_NodeRED/316.png)

19. 双击 `tcp out`节点弹出`tcp out`节点编辑界面，进行参数设置，如下图：

    ![编辑tcp out节点](/assets/STM32_NodeRED/271.png)

20. 到此工作区内容如下图:

    ![图片](/assets/STM32_NodeRED/317.png)

21. 各个节点之间进行连线，设置如下图所示，如下图：

    ![节点连线设置](/assets/STM32_NodeRED/318.png)

22. 功能完成后需要对界面图标位置进行调整，在界面右边找到相应按钮，如下图：

    ![启动布局](/assets/STM32_NodeRED/274.png)

23. 点击layout图标进入位置调整界面。随意拖动左边模块，到合适的位置。形成自己所要的排版模式点击“完成”按钮退出到编辑界面。

    ![调整界面布局](/assets/STM32_NodeRED/319.png)

24. 点击右上角的“部署”按钮，部署成功后图标变成灰色，如下图：

    ![进行部署](/assets/STM32_NodeRED/276.png)

25. 依次点击如下图所示按钮，进入应用界面，如下图：

    ![查看部署后的界面](/assets/STM32_NodeRED/277.png)

26. 部署后的界面如下，等硬件实验连接上，就可以使用硬件控制风扇。

    ![部署后的界面](/assets/STM32_NodeRED/320.png)

27. 到此，NodeRED平台编辑完成，接下来，下载硬件程序，实现在NodeRED上控制风扇。

28. 将WiFi模块、蜂鸣器模块分别安装在STM32底座上。确认各个节点，如下图所示，ST_LINK连接WIFI节点连接。

    ![搭建实验硬件平台]

29. 打开 Keil 5 工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`NodeRED平台实验\9.蜂鸣器控制实验\WiFi模块程序\USER\WiFi.uvprojx` 并打开。

    ![启动工程](/assets/STM32_NodeRED/321.jpg)

30. 打开WiFi.h文件，修改WIFI热点名称和密码。实验台也必须连接到该WIFI热点。IP地址为运行NodeRED的计算机的IP地址，端口为此前步骤NodeRED tcp out节点监听的端口。

    ![修改WiFi数据](/assets/STM32_NodeRED/281.png)

31. 编译工程，然后将程序下载到WIFI节点的底座。

    ![编译并下载程序](/assets/STM32_NodeRED/255.png)

32. 将STLink连接到风扇节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`NodeRED平台实验\9.蜂鸣器控制实验\蜂鸣器模块程序\USER\beep.uvprojx` 并打开。

    ![启动工程](/assets/STM32_NodeRED/322.jpg)

33. 编译工程，然后将程序下载到风扇节点中。

    ![编译并下载程序](/assets/STM32_NodeRED/255.png)


34. 蜂鸣器节点与WIFI节点拼接，从STM32底座上取下STLink的USB线再重新接上，给设备重新上电。

35. 部署应用，点击右上角“部署”图标，变成灰色部署成功，如下图：

    ![进行部署](/assets/STM32_NodeRED/258.png)

36. NodeRED平台的界面上看到如下图所示，表明实验设备连接上NodeRED平台。

    ![设备连接成功](/assets/STM32_NodeRED/323.png)

37. NodeRED编辑界面右上角，依次点击如下图图标，进行部署，并查看应用界面。

    ![部署应用](/assets/STM32_NodeRED/284.png)

38. 显示界面如下图，点击按钮控制风扇的状态。

    ![操作应用](/assets/STM32_NodeRED/324.png)


<!-- ------------------------ -->
## 代码讲解


### 蜂鸣器节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/STM32_NodeRED/325.jpg)

② main.c中对串口、蜂鸣器模块、RS485协议进行初始化。初始化完成后。调用函数DataHandling_485()获取控制指令，依控制指令控制蜂鸣器打开或关闭。蜂鸣器是无源蜂鸣器，代码采用1ms切换一次蜂鸣器电平的方式，输出一个500Hz的方波控制蜂鸣器响起。当BeepState=0,时无方波输出，蜂鸣器不响，BeepState = 1,有500Hz方波输出蜂鸣器响起。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        Beep_Init();//初始化蜂鸣
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1,用于485通信
        USART3_Init(115200);
        printf("this usart3 print\r\n");
        while(1)
        {
            if(!DataHandling_485(Addr_BEEP)){//是发给本机的指令
                printf("get data\r\n"); 
                BeepCmd = Rx_Stack.Data[0];//控制指令
                if(BeepCmd){
                    BeepState = 1;//打开蜂鸣器
                }
                else{
                    BeepState = 0;//关闭蜂鸣器                
                }
            }

            if(BeepState){
                HAL_Delay(1);//延时1ms，蜂鸣器的声音频率为500HZ
                BEEP_IO_TOGGLE();//蜂鸣器IO电平反转
            }
            else{
                BEEP_IO_LOW();//停止蜂鸣器
            }
        }
    }
```

### WIFI节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/STM32_NodeRED/287.jpg)

② main.c中对串口、RS485协议进行初始化，并WIFI初始化并连接NODE-Red平台。初始化完成后，接收平台的指令,根据平台指令控制风扇节点的风扇状态。控制命令通过调用Rs485_Send()进行发送。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1 485总线使用
        UART2_Init(115200);//初始化串口2
        USART3_Init(115200);//调试串口   
        printf("this usart3 print\r\n");
        WiFi_Init();//初始化WiFi，并连接NODE-Red
        while(1)
        {
            if(USART2_RX_STA){
                HAL_Delay(50);//延时50ms等待接收完成
                printf("get cmd=%s\r\n",USART2_RX_BUF);//调试打印
                if(strstr((void*)&USART2_RX_BUF[0],(const char*)"$BEEP")){
                    Beep_Ctrl = 1 - Beep_Ctrl;//蜂鸣控制命令
                    Rs485_Send(Addr_WiFi,Addr_BEEP,BEEP_CTRL,1,(void*)&Beep_Ctrl);//发送命令控制蜂鸣器            
                }          
                USART2_RX_STA = 0;//清空串口接收计数器
                memset((void*)USART2_RX_BUF,0,USART2_REC_LEN);//清空接收缓冲
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

3. Node-RED平台设备没有上线。

    - WIFI名字、WIFI密码是否正确。
    - 实验设备计算机是否在同一个WIFI热点或者路由下。

     
<!-- ------------------------ -->
## 实验思考


1. 在NODE-Red平台增加一个按键控制蜂鸣器以另一种频率响起。
