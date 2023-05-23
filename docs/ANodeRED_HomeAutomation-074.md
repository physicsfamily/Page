---
layout: post
title:  "074_NodeRED_HomeAutomation"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# 综合实验_基于NodeRED的智能家居系统实验
<!-- ------------------------ -->
## 实验内容


- 构建一个基于Node-RED的智能家居系统，实现传感器数据的显示。风扇、LED、继电器的控制。

<!-- ------------------------ -->
## 实验目的


- 加强对Node-RED平台的认识，巩固学习内容。


<!-- ------------------------ -->
## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 5个 |  · |
| 3 | 风扇模块 | 1个 | ·  |
| 4 | 温湿度模块 | 1个 | ·  |
| 5 | LED模块 | 1个 | ·  |
| 6 | WiFi模块 | 1个 | ·  |
| 7 | 继电器模块 | 1个 | ·  |
| 8 | ST-Link下载器 | 1个 | · |
| 9 | ST-Link下载器连接线 | 1根 |  · |
| 10 | Node-RED智能家居实验代码 | 2份 | ·  |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

[STM32底座](https://docs.stepiot.com/docs/aiot016)

![STM32底座](/assets/STM32/2.png)

[风扇模块](https://docs.stepiot.com/docs/aiot015)

![风扇模块](/assets/STM32_OneNET/51.png)

[温湿度模块](https://docs.stepiot.com/docs/aiot004)

![温湿度模块](/assets/BASE_STM32/2.png)

[LED模块](https://docs.stepiot.com/docs/aiot001)

![LED模块](/assets/STM32_OneNET/37.png)

[WiFi模块](https://docs.stepiot.com/docs/aiot011)

![WiFi模块](/assets/BASE_CC2530/65.png)

[继电器模块](https://docs.stepiot.com/docs/aiot014)

![继电器模块](/assets/STM32_OneNET/45.png)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

<!-- ------------------------ -->
## 实验原理
0

### WIFI技术基本概念

WIFI英语全称是Wireless Fidelity，中文译成无线保真。WIFI是建立连接和进行通讯的手段，它对应一套通讯的规则，保证让两个节点能互相连接，设备连接建立后，通过TCP/IP和UDP等协议，传输数据，连接互联网络。WIFI模块的STA模式和AP模式。

AP模式：Access Point，提供无线接入服务，允许其它无线设备接入，提供数据访问，一般的无线路由/网桥工作在该模式下。AP和AP之间允许相互连接。Sta模式（SP模式）：Station类似于无线终端，sta本身并不接受无线的接入，它可以连接到AP，一般无线网卡即工作在该模式。对照表如下。

|分类|AP模式|SP模式|
| :--: | :--: | :--: |
|接入网络|接受|不接受|
|网卡|需要|不需要|
|终端|有线|无线|

### WiFi模块原理图

![WIFI模块硬件电路](/assets/STM32_OneNET/1.png)

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

    ![添加新节点](/assets/HomeAutomation/27.png)

7. 填写表头名称，名称为“—————————————————————————————————智能家居系统—————————————————————————————————”，点击“添加”按钮完成退出，如下图：

    ![输入表头名称](/assets/HomeAutomation/28.png)

8. 设置宽度，点击“更新”按钮完成编辑，如下图：

    ![输入表头名称](/assets/HomeAutomation/29.png)

9. 编辑text控件，参数下图中的红框：

    ![输入表头名称](/assets/HomeAutomation/30.png)

10. 回到编辑界面，在控件区中搜索框中输入`button`，如下图：

    ![搜索button](/assets/STM32_NodeRED/265.png)

11. 将图标节点`button`，拖入编辑界面中。

    ![拖入button控件](/assets/HomeAutomation/31.png)

12. 双击刚拖入的`button`控件，编辑`button`，参数如下图。Group的选择一定要注意，否则部署后会看不到相应的控件。

    ![拖入button控件](/assets/HomeAutomation/32.png)

13. 拖出1个`button`，设备参数如下图

    ![图片](/assets/HomeAutomation/33.png)

14. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/34.png)

15. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/35.png)

16. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/36.png)

17. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/37.png)

18. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/38.png)

19. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/39.png)

20. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/40.png)

21. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/41.png)

22. 拖出1个`button`，设备参数如下图：

    ![图片](/assets/HomeAutomation/42.png)

23. 拖出1个`button`，设备参数如下图:

    ![图片](/assets/HomeAutomation/43.png)

24. 到此，工作区内容如下图:

    ![图片](/assets/HomeAutomation/44.png)

25. 在控制区搜索框中输入“switch”，搜索switch节点：

    ![图片](/assets/HomeAutomation/45.png)

26. 将switch节点拖入编辑区，双击switch节点，设置参数如图：

    ![图片](/assets/HomeAutomation/46.png)

27. 在控制区搜索框中输入`switch`，搜索`switch`节点。

    ![图片](/assets/HomeAutomation/47.png)

28. 将`switch`节点拖入编辑区，双击`switch`节点，设置参数如图：

    ![图片](/assets/HomeAutomation/48.png)

29. 将`switch`节点拖入编辑区，双击`switch`节点，设置参数如图:

    ![图片](/assets/HomeAutomation/49.png)

30. 将`switch`节点拖入编辑区，双击`switch`节点，设置参数如图：

    ![图片](/assets/HomeAutomation/50.png)

31. 到此工作区，如图:

    ![图片](/assets/HomeAutomation/51.png)

32. 在编辑界面左边的控制区搜索框内输入`text`找到`text`控制件，如下图：

    ![图片](/assets/HomeAutomation/52.png)

33. 将`text节`点拖入编辑区，双击`text`节点，设置参数如图。Group加入“—智能家居--]设备控制”：

    ![图片](/assets/HomeAutomation/53.png)

34. 加入“请输入密码”节点后，到此工作区，如图：

    ![图片](/assets/HomeAutomation/54.png)

35. 在控制区搜索框中输入`tcp out`搜索函数节点，如下图：

    ![图片](/assets/HomeAutomation/55.png)

36. 将`tcp out`节点拖入编辑区，如下图：

    ![图片](/assets/HomeAutomation/56.png)

37. 双击 `tcp out`节点弹出`tcp out`节点编辑界面，进行参数设置，如下图：

    ![图片](/assets/HomeAutomation/57.png)

38. 增加group:鼠标放在如图红箭头所示位置，出现“+group”字样，点击“+group”，出现新组group2。

    ![图片](/assets/HomeAutomation/58.png)

39. 鼠标放在如图红箭头所示位置，出现`edit`字样，点击`edit`，编辑该组：

    ![图片](/assets/HomeAutomation/59.png)

40. 设置名称为传感器数据，Tab连接“--智能家居系统--]传感器数据”：

    ![图片](/assets/HomeAutomation/60.png)

41. 在控件区中搜索框中输入`gauge`，如下图：

    ![图片](/assets/HomeAutomation/61.png)

42. 将图标节点`gauge`拖入编辑界面中：

43. 双击刚拖入的`gauge`控件，编辑`gauge`，参数如下图，Group的选择一定要注意，否则部署后会看不到相应的控件。

    ![图片](/assets/HomeAutomation/62.png)
    ![图片](/assets/HomeAutomation/63.png)

44. 再从控件区中`gauge`控件拖入工作区中，编辑参数如下图，点击完成回到工作区。

    ![图片](/assets/HomeAutomation/64.png)
    ![图片](/assets/HomeAutomation/65.png)

45. 在控制区搜索框中输入`function`搜索函数节点，如下图：

    ![图片](/assets/HomeAutomation/66.png)

46. 将函数节点`function`拖入编辑界面中，双击函数节点，弹出编辑界面，为其添加功能代码，输出个数为2，如下图：

    ![图片](/assets/HomeAutomation/67.png)

    代码如下：
    ```c
    var temp1 = (msg.payload[0]-0x30)*10+(msg.payload[1]-0x30)*1;
    var msg1 = { payload:temp1 }; 
    var temp2 = (msg.payload[2]-0x30)*10+(msg.payload[3]-0x30)*1;
    var msg2 = { payload:temp2 };  
    return [msg1,msg2];  
    ```

47. 各节点之间进行连接，最终内容如图：

    ![图片](/assets/HomeAutomation/68.png)

48. 功能完成后需要对界面图标位置进行调整，在界面右边找到相应按钮，如下图：

    ![图片](/assets/HomeAutomation/69.png)

49. 点击layout图标进入位置调整界面，随意拖动左边模块，到合适的位置，形成自己所要的排版模式，点击“完成”按钮退出到编辑界面：

    ![图片](/assets/HomeAutomation/70.png)

50. 点击右上角的“部署”按钮，部署成功后图标变成灰色，如下图：

    ![图片](/assets/HomeAutomation/71.png)

51. 依次点击如下图所示按钮，进入应用界面。

    ![图片](/assets/HomeAutomation/72.png)

52. 部署后的界面如下，等硬件实验连接上，就可以控制硬件及显示传感器数据。

    ![图片](/assets/HomeAutomation/73.png)

53. 到此，NodeRED平台编辑完成，接下来，下载硬件程序。

54. 将WiFi模块、风扇模、LED模块、继电器模块分别安装在STM32底座上。确认各个节点，如下图所示，ST_LINK连接WIFI节点连接。

    ![图片](/assets/HomeAutomation/74.png)

55. 打开 Keil 5 工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\NodeRED智能家居实验\WiFi模块程序\USER\WIFI.uvprojx` 并打开。

    ![图片](/assets/HomeAutomation/75.jpg)

56. 打开WiFi.h文件，修改WIFI热点名称和密码，实验台也必须连接到该WIFI热点，IP地址为运行NodeRED的计算机的IP地、端口为此前步骤NodeRED tcp out、tcp in节点监听的端口。

    ![图片](/assets/HomeAutomation/76.png)

57. 编译工程，然后将程序下载到WIFI节点的底座。

    ![图片](/assets/HomeAutomation/77.png)

58. 将STLink连接到风扇节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\NodeRED智能家居实验\风扇模块程序\USER\FAN.uvprojx` 并打开。

    ![图片](/assets/HomeAutomation/78.jpg)

59. 编译工程，然后将程序下载到风扇节点的底座。

    ![图片](/assets/HomeAutomation/77.png)

60. 将STLink连接到LED节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\NodeRED智能家居实验\LED模块程序\USER\LED.uvprojx` 并打开。

    ![图片](/assets/HomeAutomation/79.jpg)

61. 编译工程，然后将程序下载到LED节点的底座。

    ![图片](/assets/HomeAutomation/77.png)

62. 将STLink连接到继电器节点，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`综合实验\NodeRED智能家居实验\继电器模块程序\USER\Relay.uvprojx` 并打开。

    ![图片](/assets/HomeAutomation/80.jpg)

63.编译工程，然后将程序下载到继电器节点的底座。

    ![图片](/assets/HomeAutomation/77.png)

64. 各节点保持拼接，从STM32底座上取下STLink的USB线再重新接上，即给设备重新上电。

65. 部署应用，点击右上角“部署”图标，变成灰色部署成功。

    ![进行部署](/assets/STM32_NodeRED/258.png)

66. NodeRED编辑界面右上角，依次点击如下图图标，进行部署，并查看应用界面。

    ![部署应用](/assets/STM32_NodeRED/284.png)

67. 在输入键盘上输入“123456”如图，继电器打开2秒后关闭。

    ![输入密码](/assets/HomeAutomation/81.png)

68. 操作开关对LED模块上的四个LED灯及风扇进行控制。

    ![风扇及LED灯控制](/assets/HomeAutomation/82.png)

69. 查看传感器数据。

    ![查看温湿度数据](/assets/HomeAutomation/83.png)


<!-- ------------------------ -->
## 代码讲解


### 风扇节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/84.jpg)

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

### 继电器节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/85.png)

② main.c对串口、继电器模块、RS485协议进行初始化。初始化完成后。调用函数DataHandling_485()获取控制指令，依控制指令控制继电器1打开或关闭。

```c
    int main(void)
    {
        HAL_Init();//初始化HAL库  
        Relay_Init();//初始化继电器
        Rs485_Init();//初始化485
        UART1_Init(115200);//初始化串口1,用于485通信
        USART3_Init(115200);
        printf("this usart3 print\r\n");
        while(1)
        {
            HAL_Delay(10);//延时10ms每10ms检测一次指令
            if(!DataHandling_485(Addr_Relay)){//是发给本机的指令
                printf("get data\r\n"); 
                RelayState = Rx_Stack.Data[0];//控制指令
                if(RelayState){
                    RELAY1_OPEN();//继电器1开 
                    HAL_Delay(2000);//延时2000ms
                    RELAY1_CLOSE();//继电器1关               
                }
            }        
        }
    }
```

### 温湿度节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/86.jpg)

② main.c中对串口、定时器器、温湿度传感器、RS485协议进行初始化。初始化完成后，定时读取传感器数据、显示传感器数据，处理WIFI节点的请求，并返回传感器数据到WIFI节点。

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

main.c->RS485_HandlerCb()(回调函数)，处理WIFI节点的请求，调用函数Rs485_Send()返回温湿数据。

```c
    void RS485_HandlerCb(void)
    {
        if(!DataHandling_485(Addr_SHT20)){//是本机期望的485数据处理
            printf("get requery\r\n");
            /*485发送数据*/
            Rs485_Send(Addr_SHT20,Addr_WiFi,SHT20_All,2,&SendBuffer[0]);
        }
    }
```

### LED节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/87.jpg)

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

### WIFI节点

① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件，我们主要关心，main.c及HARDWARE中的代码。

![程序目录结构](/assets/HomeAutomation/88.jpg)

② main()函数中对串口、RS485协议进行初始化，并WIFI初始化并连接NODE-Red平台。

```c
    HAL_Init();//初始化HAL库  
    Rs485_Init();//初始化485
    UART1_Init(115200);//初始化串口1 485总线使用
    UART2_Init(115200);//初始化串口2
    USART3_Init(115200);//调试串口   
    printf("this usart3 print\r\n");
    WiFi_Init();//初始化WiFi，并连接Node-RED  
    /*中断频率2HZ 关联回调函数RS485_HandlerCb*/
    TIM3_Init(10000-1,3200-1,RS485_HandlerCb);  
```

③ PasswordInputProcess()处理输入的密码，密码保存在PassWordBuf中。利用strncmp()比较输入的密码与预先设置的密码是否相同。

```c
    printf(">>>%s\r\n",RxBuffer);//打印接收到的指令
    if(isCommand(RxBuffer,"$BTN")){//密码输入
        if(PasswordInputProcess(&RxBuffer[0],&PassWordBuf[0])){//处理输入的密码
            if(strncmp((const char*)PassWordBuf,"123456",6) == 0){//密码相同
                RelayCtrl = 1;//使用发送开锁指令在RS485_HandlerCb()中调用
                memset((void*)PassWordBuf,0,12);
            }
        }
    }
```

④ 风扇控制指令解析，FANCtrl使用指令发送，FANCmd使用指令的动作开/关。

```c
    else if(isCommand(RxBuffer,"$FAN")){//风扇控制指令
        FANCtrl = 1;//使用发送风扇指令在RS485_HandlerCb()中调用
        FANCmd  = RxBuffer[5] - 0x30;
    }
```

⑤ LED控制指令解析，LEDCmdProcess()从RxBuffer中解析指令，LEDCmd[0]保存的是控制哪个灯，LEDCmd[1]保存控制的动作(亮/灭)。

```c
    else if(isCommand(RxBuffer,"$LED")){//是LED控制指令
        if(LEDCmdProcess(RxBuffer,&LEDCmd[0],&LEDCmd[1])){
            LEDCtrl = 1;//使用发送灯控制指令在RS485_HandlerCb()中调用
        }
    }        
```

⑥ 传感器数据更新。

```c
    if(HAL_GetTick() > Tick_SendSensor){//更新传感器数据
        Tick_SendSensor = HAL_GetTick() + 2000;//每两秒更新一次
        /*转成字符串*/
        sprintf((void*)&SendBuffer[0],"%02d%02d",SensorData[0],SensorData[1]);
        WiFi_SendSensor(SENSOR_LINK_ID,&SendBuffer[0]);//发送到Node-RED 
        printf("传感器数据==%d,%d\r\n",SensorData[0],SensorData[1]);//调试打印
    }
```


<!-- ------------------------ -->
## 常见问题


1. 弹出警告窗口，不能下载程序。

    - 请确认STLink驱动、STM32F103C8的DFP包是否安装。
    - STLink仿真器是否正常接入。

2. 下载代码后程序没观察到实验现象。

    - 请重新上电，或者按下底座上的复位按键。

3. NODE-Red平台设备没有上线。

    - WIFI名字、WIFI密码正确。
    - 实验设备计算机是否在同一个WIFI热点或者路由下。
     
<!-- ------------------------ -->
## 实验思考


1. 代码中预先设置的密码如何修改？
