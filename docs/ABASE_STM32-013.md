---
layout: post
title:  "013_基于STM32的LED模块实验"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# 基于STM32的LED模块实验
<!-- ------------------------ -->
## 实验内容


- 理解按键及LED灯驱动原理。
- 在LED模块上观察流水灯现象。
- 按键S1快速流水灯，按键S2慢速流水灯，按键S3流水灯样式2，按键S4，关闭所有LED灯。

<!-- ------------------------ -->
## 实验目的


- 熟悉LED模块的使用。
- 掌握LED基本电路及驱动原理（即如何点灯、如何熄灯）。
- 掌握LED模块上按键检测原理。

<!-- ------------------------ -->

## 实验环境


### 实验所需硬件

| **序号** | **名称** | **数量** | **备注** |
| --- | --- | --- | --- |
| 1 | 电脑 | 1台 | 系统Windows7及以上 |
| 2 | STM32底座模块 | 1个 |  · |
| 3 | LED模块 | 1个 |  · |
| 4 | ST-Link下载器 | 1个 | · |
| 5 | ST-Link下载器连接线 | 1根 |  · |
| 6 | LED实验代码 | 1份 | · |

### 实验所需软件

- [MDK5](https://codelab.stepiot.com/codelabs/Keil5_078/index.html?index=..%2F..index#0) 安装步骤

- [ST-LINK](https://codelab.stepiot.com/codelabs/ST_LINK_079/index.html?index=..%2F..index#0) 驱动安装步骤

- [Git](https://git-scm.com/downloads) 软件下载(可选)

ST-Link下载器 & ST-Link下载器连接线

![STLink下载器](/assets/STM32/1.png)

[STM32底座](https://docs.stepiot.com/docs/aiot016)：HIVE PRO STM32是一种基于STM32F103C8T6芯片的蜂巢底座。

![STM32底座](/assets/STM32/2.png)

[LED模块](https://docs.stepiot.com/docs/aiot001)：LED模块共有四个按键，4个LED灯，可供完成流水灯、按键处理等相关实验。按键的触发为低电平，LED灯低电平点亮。

![LED模块](/assets/STM32/3.png)


<!-- ------------------------ -->
## 实验要求


- 理解代码如何实现流水灯效果。
- 结合硬件电路，理解代码如何驱动LED灯。
- 结合硬件电路，按键检测原理。


<!-- ------------------------ -->
## 实验原理


### LED模块基本电路图

![LED模块原理图](/assets/BASE_STM32/1.png)

按键对应的IO口分别是：PA5、PA4、PB7、PB6。  
LED全称为发光二极管，当加于正向电压时，发光二极管导通，发出光。为防止电流过大造成LED灯损坏、寿命减小，通常使用限流电阻与LED灯串联，如图 中的R5、R6、R7、R8就是限流电阻。  
以LED1为例，通过观察原理图我们知道，要点亮LED1，PA3必须输出低电平，熄灭LED1，PA3必须输出高电平。点亮或熄灭LED2、LED3、LED4同理。

### 流水灯

流水灯就是一组灯在控制系统的控制下按照设定的顺序和时间来发亮和熄灭，必须保证同一时刻只能有一个灯亮，且亮灭的间隔大于50ms，否则观察不出效果。

<!-- ------------------------ -->

## 实验步骤


① LED模块安装于STM32底座上。ST-Link连接电脑与STM32底座，如下图：

![安装模块](/assets/STM32/14.png)

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

⑤ 打开`MDK5`工程软件，点击工具栏： ` Project` -> `Open Project`，选择工程文件：`基于STM32的模块实验\1.LED模块\LED模块程序\USER\KEY.uvprojx` 并打开。
   
![打开工程](/assets/STM32/39.jpg)

![选择文件](/assets/BASE_STM32/33.png)

⑥ 工程启动后，点击 `Rebuild` 重新编译。如下图：

![重新编译工程](/assets/STM32/16.jpg)

⑦ 编译成功，如下图：

![编译成功](/assets/STM32/17.jpg)

⑧ 点击 `Download` 按钮下载程序，如下图所示：

![下载程序](/assets/STM32/18.jpg)

![下载成功](/assets/STM32/41.jpg)

⑨ 下载完成后，将USB线进行重连操作（即：将STLink的USB线从底座上取下，再重新接上）。

⑩ 按下按键S1，LED1灯亮，再次按下S1，LED1灯灭；  
    按下按键S2，LED2灯亮，再次按下S2，LED2灯灭；  
    按下按键S3，LED3灯亮，再次按下S3，LED3灯灭；  
    按下按键S4，LED4灯亮，再次按下S4，LED4灯灭。  
    如下图标出了LED模块上按键与LED灯的位置。

![按键与LED灯的位置](/assets/BASE_STM32/34.png)

    
<!-- ------------------------ -->
## 代码讲解


① 程序目录结构，如下图。CORE文件夹为STM32内核代码，HALLIB文件文件夹为底层HAL库文件。我们主要关心，main.c及HARDWARE中的代码。  
 ![程序目录结构](/assets/BASE_STM32/35.jpg)

② main.c中进行硬件的初始化及整个代码的逻辑控制。

```c
int main(void)
{
    static uint8_t key = 0;  
    HAL_Init();//初始化HAL库，为随后用到的HAL_Delay()函数提供时钟。
    LED_Init();//初始化LED灯控制的IO口	
    KEY_Init();//初始化按键输入的IO口

    while(1)
    {
        key = KEY_Scan();//获取按键      
        switch(key)
        {
            case S1_PRES:
                HAL_GPIO_TogglePin(LED_PORT,LED1_PIN);//S1 按下，翻转LED1灯状态
                break;
            case S2_PRES:
                HAL_GPIO_TogglePin(LED_PORT,LED2_PIN);//S2 按下，翻转LED2灯状态
                break;
            case S3_PRES:
                HAL_GPIO_TogglePin(LED_PORT,LED3_PIN);//S3 按下，翻转LED3灯状态
                break;
            case S4_PRES:
                HAL_GPIO_TogglePin(LED_PORT,LED4_PIN);//S4 按下，翻转LED4灯状态
                break;
            default:break;
        }
    }
}
```


③ LED.c为LED驱动代码，代码实现对LED灯控制的IO进行初始化设置IO口为推挽上拉模式。重点不要忘记使用能IO接口的时钟及设置输出的速度。

```c
void LED_Init(void)
{
    GPIO_InitTypeDef GPIO_Initure;
    
    LED_IO_RCC();

    GPIO_Initure.Pin   = LED1_PIN|LED2_PIN|LED3_PIN|LED4_PIN;
    GPIO_Initure.Mode  = GPIO_MODE_OUTPUT_PP;//推挽输出
    GPIO_Initure.Pull  = GPIO_PULLUP;//上拉
    GPIO_Initure.Speed = GPIO_SPEED_HIGH;//高速
    HAL_GPIO_Init(LED_PORT,&GPIO_Initure);
        
    LED_OFF();///初始化完成全部LED灯灭
}
```

④ key.c为LED模块上的按键驱动，Key_Init()中代码中设置IO口为上拉输入。

```c
void KEY_Init(void)
{
    GPIO_InitTypeDef GPIO_Initure;

    KEY_IO_RCC();//使用IO端口时钟
    
    GPIO_Initure.Pin   = S1_PIN|S2_PIN;//S1_PIN,S2_PIN定义在key.h中 
    GPIO_Initure.Mode  = GPIO_MODE_INPUT;//输入
    GPIO_Initure.Pull  = GPIO_PULLUP;//上拉
    GPIO_Initure.Speed = GPIO_SPEED_LOW;//低速
    HAL_GPIO_Init(GPIOA,&GPIO_Initure);       
        
    GPIO_Initure.Pin   = S3_PIN|S4_PIN;//S3_PIN、S4_PIN定义在key.h中 
    GPIO_Initure.Mode  = GPIO_MODE_INPUT;//输入
    GPIO_Initure.Pull  = GPIO_PULLUP;//上拉
    GPIO_Initure.Speed = GPIO_SPEED_LOW;//低速
    HAL_GPIO_Init(GPIOB,&GPIO_Initure);
}
```

```c
//按键扫描代码
uint8_t KEY_Scan(void)
{
    static uint8_t key_up=1;//按键松开标志
        
    /*在按键松开的情况，有按键按下*/
    if(key_up&&(S1_Read()==0||S2_Read()==0||S3_Read()==0||S4_Read()==0)){
        HAL_Delay(10);//如果有按键按下延时10个毫秒消抖。
        key_up=0;//按键己按下
        if(S1_Read()==0){
            return S1_PRES;//返回 S1 按下
        }
        else if(S2_Read()==0){
            return S2_PRES;//返回 S2 按下
        }
        else if(S3_Read()==0){
            return S3_PRES;//返回 S3 按下
        }          
        else if(S4_Read()==0){
            return S4_PRES;//返回 S4 按下
        }
    }
    else if(S1_Read()==1&&S2_Read()==1&&S3_Read()==1&&S4_Read()==1){
        /*按键全部松开了*/
        key_up=1;
    }    
    return 0;//无按键按下
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


1. 实现按键S1按下后，LED1亮1秒后熄灭。
   
2. 实现按键S1按下后，LED1常亮，按键S2按键下LED1熄灭。
