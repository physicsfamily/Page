---
layout: post
title:  "关于ZIGBEE实验中，PAN_ID的修改说明"
date:   2023-06-05 10:18:00 +0800
categories: getting started
---

# Zigbee通信实验_PAN_ID的修改


### ZigBee基本通信方式

ZigBee的通讯方式主要有三种点播、组播、广播。点播顾名思义就是点对点通信，也是2个设备之间的通讯，不允许有第三个设备收到信息；组播就是把网络中的节点分组，每一个组员发出的信息只有相同组号的组员才能收到。广播最广泛的也就是1个设备上发出的信息所有设备都能接收到。这也是ZigBee通信的基本方式。

### ZigBee网络地址类型

|序号|地址类型|说明|
| :--: | :--: | :--: |
|1|AddrNotPresent|依照绑定表|
|2|Addr16Bit|16位地址，常用于点播|
|3|Addr64Bit|Addr64Bit|
|4|AddrGroup|组播|
|5|AddrBroadcast|广播|

以上的的地址类型均保存在afAddrMode_t。在实际使用过程中考虑功耗，常用使用16位地址，原因16位地址是通过路由算法计算出来的，在传输路径上通过的节点较少最大的节省了网络功耗。16位地址可分成如下几类：


|序号|16位地址分类|说明|
| :--: | :--: | :--: |
|1|0xFFFF|对所有设备广播，包括睡眠|
|2|0xFFFE|间接传输，通过绑定表寻找网络短地址|
|3|0xFFFD|对没休眠的设备广播|
|4|0xFFFC|给协调器和路由器广播|
|5|0x0000|给协调器通信|
|6|0x0001-0xFFFB|用户自设定的目标地址|

<!-- ------------------------ -->
## 实验步骤

   
① 上文中提到的用户自定义地址，是在f8wConfig.cfg文件中设定的，如下图：

![配置文件](/assets/Zigbee/PAN_ID_1.png)

② 将CoordinatorEB和EndDeviceEB分别编译并下载到对应的蜂巢底座，可以实现ZIGBEE组网，并获取到数据:

![数据显示](/assets/Zigbee/LED_DISPLAY_1.png)
    
③ 但是这时如果再次修改f8wConfig.cfg中PAND_ID或者信道，我们会发现，终端设备并没有连接到新的CoordinatorEB的PAND_ID分组。这是因为在正常工作环境时，PAND_ID和信道是基本上不需要改变的，zstack协议库底层设计时，为了让EndDeviceEB设备尽快接入协调器，在NV FLASH中记忆了上次的连接信息，而且优先使用保存的连接信息，为了使我们修改的PAND_ID或者信道能够立即得到使用，就需要清楚这个记忆的信息，有以下两种方案可以使用。



④ 第一种方案是在烧写CoordinatorEB和EndDeviceEB时，强制擦除FLASH，如下图：

![擦除FLASH](/assets/Zigbee/PAN_ID_FLASH.jpg)
   


⑤ 第二种方案是，在项目配置文件中，删除NV FLASH保存的宏定义，如下图：

![不保存信息1](/assets/Zigbee/PAN_ID_STEP1.png)
![不保存信息2](/assets/Zigbee/PAN_ID_STEP2.png)
![不保存信息3](/assets/Zigbee/PAN_ID_STEP3.png)


⑥ 上述两种方案，再修改PAN_ID或者信道后，都需要重新编译、下载。
   


