---
layout: post
title:  "080_OneNET 平台应用手册"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---


# OneNET 平台应用手册
<!-- ------------------------ -->
## OneNET 平台简介


OneNET平台是中国移动物联网有限公司响应“大众创新、万众创业”以及基于开放共赢的理念，面向公共服务自主研发的开放云平台，为各种跨平台物联网应用、行业解决方案提供简便的海量连接、云端存储、消息分发和大数据分析等优质服务，从而降低物联网企业和个人（创客）的研发、运营和运维成本，使物联网企业和个人（创客）更加专注于应用，共建以OneNET为中心的物联网生态环境。

OneNET平台提供设备全生命周期管理相关工具，帮助个人和企业快速实现大规模设备的云端管理；开放第三方API接口，推进个性化应用系统构建；提供定制化“和物”APP，加速个性化智能应用生成。

物联网实验平台提供接入物联网OneNET平台及八城物联云平台的能力，平台通过网络模块与云平台实现互联，云平台具有数据接收、数据存储、数据分析显示、数据可视化组件、控制下发和账户管理等功能。通过八城物联网实验平台和OneNET平台的配套，可以实现如下功能：

- 平台可以记录用户实验数据，具有数据可追溯的功能；
- 通过制定的协议实现数据分析和展示；
- 通过网页链接来检查学生的实验过程和结果；
通过建立班级实验群的形式管理学生实验成绩。

<!-- ------------------------ -->
## OneNET 平台注册


① 打开注册连接   
打开[oneNET](https://open.iot.10086.cn/)官网，点击“注册”，如下图：
![OneNET平台](/assets/Keil/36.png)

② 填写注册信息  
选择个人或企业用户，填写用户名、密码、所在地和手机验证等信息，点击“立即注册”，如下图：
![OneNET平台](/assets/Keil/37.png)

③ 返回页面  
返回[OneNET](http://open.iot.10086.cn/)平台，点击“登录”，如下图：
![OneNET平台](/assets/Keil/38.png)

④ 登录平台  
输入注册好的用户名、密码及验证码，点击“登录”，如下图；
![OneNET平台](/assets/Keil/39.png)

<!-- ------------------------ -->
## OneNET 创建产品


① 进入开发者中心 
登录后，点击“开发者中心”，如下图：
![OneNET平台](/assets/Keil/40.png)

② 创建产品  
进入开发者中心后，将鼠标放置到左上角该处：
![OneNET平台](/assets/Keil/41.png)
![OneNET平台](/assets/Keil/42.png)
进入多协议接入以后，选择TCP透传：
![OneNET平台](/assets/Keil/43.png)
进入TCP透传后点击“添加产品”：
![OneNET平台](/assets/Keil/44.png)

③ 录入产品信息  
在新建产品页面，输入产品名称和产品行业等信息。主要是要选择好设备接入协议，这里我们以TCP协议作为例子，填好后点击“确定”，如下图：  
![OneNET平台](/assets/Keil/45.png)

④ 创建产品成功  
确定后出现创建成功页面，选择“立即添加设备”，如下图：
![OneNET平台](/assets/Keil/46.png)

⑤ 创建完成  
至此，我们创建了一个产品“蜂巢套件”，在开发者中心显示如下图： 
![OneNET平台](/assets/Keil/47.png)

<!-- ------------------------ -->
## OneNET 创建设备


① 添加设备  
在开发者中心，有两种方式进入添加设备页面，如下图所示，我们选择第一种方式进入产品概况页面，点击设备数：
![OneNET平台](/assets/Keil/48.png)
在产品概况页面，点击“添加设备”，如下图：
![OneNET平台](/assets/Keil/49.png)

② 录入设备信息
在接入设备页面，填写“设备名称”、“鉴权信息（用户任意输入）”及选择数据保密性，然后点击“接入设备”，如下图所示：  
![OneNET平台](/assets/Keil/50.png)

③ 设备添加完成
在设备管理页面，可以查看设备的详细信息，如下图：  
![OneNET平台](/assets/Keil/51.png)

④ 添加脚本
修改脚本文件后上传。找到脚本文件，直接右键选择记事本打开:
![OneNET平台](/assets/Keil/52.png)  
然后滑到最下面，查看这几行代码，这几行代码就是后面要用到的数据流，其中Temp代表温度数据，Humi代表湿度数据，Light代表光照强度数据，RFID代表RFID数据。后面括号中的（1,2）代表数据从第一位开始，数据长度为两个字节，（3，2）代表数据从第三位开始，数据长度为两个字节。可根据自己需要传输的数据已经数据流名称对脚本进行更改，默认的脚本是和程序中匹配的无需更改。  
![OneNET平台](/assets/Keil/53.png)  
修改完成后，将脚本文件上传到云平台即可。
![OneNET平台](/assets/Keil/54.png)  
![OneNET平台](/assets/Keil/55.png)  
![OneNET平台](/assets/Keil/56.png)  

<!-- ------------------------ -->
## OneNET 设备连接


① 设备准备  
将实验模块用ST_LINK仿真器连接到电脑的USB口。

② 设备连接  
- 打开工程  
打开一个我们提供的例程如下图：
![OneNET平台](/assets/Keil/57.png) 

- 打开WIFI.h文件  
在main.c中的WIFI.h上点击右键，在弹出的列表中点击`Open WIFI.h`。 
![OneNET平台](/assets/Keil/58.png) 

③ 输入相关信息  
在WIFI.h中，将WIFI名称、密码及One.NET平台账号识别码（*产品ID#鉴权信息#脚本名称*）输入到如下图中的位置：
![OneNET平台](/assets/Keil/59.png) 

④ 产品ID  
在“产品概况”中找到图示位置的“产品ID”。
![OneNET平台](/assets/Keil/60.png)

⑤ 鉴权信息  
在“设备列表”中的“操作”栏点击“详情”，找到“鉴权信息”；在创建产品的时候如果保存下该信息，可以不用再次查看。
![OneNET平台](/assets/Keil/61.png)
![OneNET平台](/assets/Keil/62.png)
信息录入之后，再编译main.c程序，编译完成后下载程序:
![OneNET平台](/assets/Keil/63.png)

⑥ 连接OneNET平台  
下载成功后，先断掉口袋开发板的电源，然后再上电，稍等会儿，设备就接入OneNET平台了，如下图所示在设备列表中带绿点的设备。
![OneNET平台](/assets/Keil/64.png)

<!-- ------------------------ -->
## OneNET 创建应用
0

① 进入“创建应用”  
设备连接上OneNET平台后，在管	理平台上点击“生成应用”；点击“创建应用”，如下图：  
![OneNET平台](/assets/Keil/65.png)

② 完善相关信息  
在创建应用页面，填写应用名称，选择应用状态，添加logo以及应用描述，然后点击“创建”，如下图：
![OneNET平台](/assets/Keil/66.png)

③ 进入“编辑页面”  
在应用管理页面进入编辑页面：点击应用名称，进入应用编辑页面，如下图：
![OneNET平台](/assets/Keil/67.png)
![OneNET平台](/assets/Keil/68.png)

④ 内容编辑  
进入编辑页面，按顺序编辑：  
⑴编辑页面名称；  
⑵在元件库中选择控件，将控件拖拽到控制台；  
⑶点中选择的控件；  
⑷点击“选择数据流”；  
⑸选择设备；  
⑹选择数据流；  
⑺设置刷新频率；  
⑻数值设置；  
⑼点击“保存”。  
至此页面编辑完成。
![OneNET平台](/assets/Keil/69.png)  
选中按钮，需要设置数据流，刷新频率，开关值。  
![OneNET平台](/assets/Keil/70.png)   
按钮设置，数据流根据创建的设备选择，刷新频率最快为3秒，开关值中EDP里面CG为帧头，SW为帧尾，其中：  
开值11：开启LED1   关值10：关闭LED1；  
开值21：开启LED2   关值20：关闭LED2；  
开值31：开启LED3   关值30：关闭LED3；  
开值41：开启LED4   关值40：关闭LED4；  
开值51：开启蜂鸣器  关值50：关闭蜂鸣器。  
在程序usart.c中可以查看相应的协议规则，也可以自行修改。  
![OneNET平台](/assets/Keil/71.png)   
usart.c文件对应函数：  
![OneNET平台](/assets/Keil/72.png) 
仪表盘参数设置：  
![OneNET平台](/assets/Keil/73.png)   
可设置图片根据数据变化，上传图片后可以根据数据值变化图片，增强显示效果。    
![OneNET平台](/assets/Keil/74.png)   

⑤ 生成应用  
返回开发者中心，点击“生成应用”，然后点击应用名称，就进入应用展示页面，如下图，至此，应用创建完毕。若要更进一步了解OneNET平台，可以进入[oneNET平台官网](http://open.iot.10086.cn/)了解。也可参考光盘提供的实验例程。
![OneNET平台](/assets/Keil/75.png)   
