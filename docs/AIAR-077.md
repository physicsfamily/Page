---
layout: post
title:  "077_IAR 驱动安装"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# IAR 驱动安装
<!-- ------------------------ -->
## IAR 简介
Duration: 5

IAR Embedded Workbench（简称EW）的C/C++交叉编译器和调试器是今天世界最完整的和最容易使用的专业嵌入式应用开发工具。EW对不同的微处理器提供一样直观用户界面。EW现在已经支持35种以上的8位/16位/32位ARM的微处理器结构。EW包括：嵌入式C/C++优化编译器、汇编器、连接定位器和库管处理器结构。

![IAR软件](/assets/Keil/26.png)

EWARM是IAR目前发展很快的产品，已经支持 ARM7/9/10/11 XSCALE，并且在同类产品中具有明显价格优势。其编译器可以对一些SOC芯片进行专门的优化。如Atmel、TI、ST和Philips。除了EWARM标准版外，IAR公司还提供EWARM BL（256K）的版本，方便不同层次客户的需求。IAR System是嵌入式领域唯一能够提供这种解决方案的公司，IAR Embedded Workbench集成的编译器产品特征：

- 高效PROMable代码；
- 完全标准C兼容，瓶颈性能分析；
- 内建对芯片的程序速度和大小优化器；
- 目标特性扩充；
- 版本控制和扩展工具支持良好；
- 便捷的中断处理和模拟；
- 高浮点支持和内存模式选择；
- 工程中相对路径支持。

<!-- ------------------------ -->
## IAR 下载
Duration: 5

请至[IAR](https://www.iar.com/iar-embedded-workbench/#!?architecture=8051)官网下载安装包。

<!-- ------------------------ -->
## IAR 安装
Duration: 15

1. 打开下载好的安装文件`EW8051-EV-8103-Web.exe`，单击鼠标右键，以管理员身份运行，单击左键运行：

    ![以管理员身份运行IAR安装包](/assets/Keil/27.png)

2. 选中`Install a new instance of this application`，点击`Next`，进入下一步：

    ![IAR安装界面](/assets/Keil/28.png)

3. 一直选择`Next`，直到进入下图界面，选择`I accept the terms of the license agreement`，继续选择`Next`，进入安装的下一步:

    ![License Agreement界面](/assets/Keil/35.png)

4. 到这里需要完善用户名（`Name`）、公司（`company`）和你获取到的密钥（`License#`）等信息后，才能进入安装的下一步：

    ![确认用户信息界面](/assets/Keil/29.jpg)

5. 将`IAR Kegen partA.exe`中`License Key`的内容复制填入IAR的`License Key`，点击Next进入下一步：

    ![输入生成的License Key](/assets/Keil/30.jpg)

6. 选择安装`Complete`版本（完整版），点击`Next`，进入安装下一步：

    ![选择安装完整版](/assets/Keil/31.png)

7. 选择安装路径：

    ![更改安装路径](/assets/Keil/32.png)
    
    ![选择安装路径](/assets/Keil/33.png)

8. 安装运行到最后，点击`Finish`即可完成安装：

    ![IAR安装完成](/assets/Keil/34.png)