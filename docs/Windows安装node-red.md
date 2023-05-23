---
layout: post
title:  "Windows安装node-red"
date:   2023-05-22 10:18:00 +0800
categories: getting started
---

# Windows安装node-red

### 下载nodejs（ 官网地址：https://nodejs.org/zh-cn/ ）

![](/assets/media/16703324957019/16602856988098.jpg)

### 安装node.js 基本直接 “NEXT” 就可以了。

### 打开power shell，搜索框里搜索 power shell，并打开
![](/assets/media/16703324957019/16602858196371.jpg)

### 在power shell里运行
```
node -v
npm  -v
```
测试结果返回版本号,说明安装nodejs完成

### 安装node-red
#### 在power shell运行命令
```
npm install -g --unsafe-perm node-red
```
如果安装失败，更换淘宝镜像
```
npm config set registry " https://registry.npm.taobao.org " 
```
清除缓存，再重新安装
```
npm cache clean --force
```
```
npm install -g --unsafe-perm node-red
```

### 运行node-red

#### 在power shell里运行
```
node-red
```
出现如下界面：
```
$ node-red
29 May 18:53:07 - [info] 

Welcome to Node-RED
===================

29 May 18:53:07 - [info] Node-RED version: v2.2.2
29 May 18:53:07 - [info] Node.js  version: v14.19.3
29 May 18:53:07 - [info] Linux 5.13.0-44-generic x64 LE
29 May 18:53:07 - [info] Loading palette nodes
29 May 18:53:07 - [info] Settings file  : /home/z/.node-red/settings.js
29 May 18:53:07 - [info] Context store  : 'default' [module=memory]
29 May 18:53:07 - [info] User directory : /home/z/.node-red
29 May 18:53:07 - [warn] Projects disabled : editorTheme.projects.enabled=false
29 May 18:53:07 - [info] Flows file     : /home/z/.node-red/flows.json
29 May 18:53:07 - [info] Creating new flow file
29 May 18:53:07 - [warn] 

---------------------------------------------------------------------
Your flow credentials file is encrypted using a system-generated key.

If the system-generated key is lost for any reason, your credentials
file will not be recoverable, you will have to delete it and re-enter
your credentials.

You should set your own key using the 'credentialSecret' option in
your settings file. Node-RED will then re-encrypt your credentials
file using your chosen key the next time you deploy a change.
---------------------------------------------------------------------

29 May 18:53:07 - [info] Server now running at http://127.0.0.1:1880/
29 May 18:53:07 - [info] Starting flows
29 May 18:53:07 - [info] Started flows

```
然后访问：http://127.0.0.1:1880
如果power shell禁止执行脚本，如下:
```
& : 无法加载文件 C:\Users\liuzidong\AppData\Local\Temp\chocolatey\chocInstall\tools\chocolateyInstall.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。所在位置 行:242 字符: 3+ & $chocInstallPS1+   ~~~~~~~~~~~~~~~    + CategoryInfo          : SecurityError: (:) []，PSSecurityException    + FullyQualifiedErrorId : UnauthorizedAccess
PowerShell因为在此系统中禁止执行脚本的解决方法
```
解决办法
```
set-ExecutionPolicy RemoteSigned
```

# 安装DashBoard插件
### 点击设置
![](/assets/media/16703324957019/16703341619053.jpg)
### 打开设置面板
![](/assets/media/16703324957019/16703342054204.jpg)
### 搜索dashboard 节点库
![](/assets/media/16703324957019/16703342369034.jpg)
完整的名字叫做node-red-dashboard 搜索出来后，点击安装即可，安装需要一点时间，稍等一会。安装完成后，刷新一下页面。就可以在节点列表的左侧看到相应的节点
![](/assets/media/16703324957019/16703342717607.jpg)
左侧出现该节点，则表示安装dashboard成功。

# 导入 node-red 程序

### 点击设置，点击导入

![](/assets/media/16703324957019/16703345544239.jpg)
出现如下窗口：
![](/assets/media/16703324957019/16703345860157.jpg)
复制如下JSON：

```
[
    {
        "id": "d80cfe57.3ca7",
        "type": "tab",
        "label": "阶石物联",
        "disabled": false,
        "info": ""
    },
    {
        "id": "b46627ed.34e3f8",
        "type": "tcp in",
        "z": "d80cfe57.3ca7",
        "name": "TCP端口:8077",
        "server": "server",
        "host": "127.0.0.1",
        "port": "8077",
        "datamode": "stream",
        "datatype": "utf8",
        "newline": "",
        "topic": "",
        "trim": false,
        "base64": false,
        "tls": "",
        "x": 110,
        "y": 320,
        "wires": [
            [
                "76850272a23de90a",
                "7076a321cad3725c"
            ]
        ]
    },
    {
        "id": "58315427.7d1a9c",
        "type": "debug",
        "z": "d80cfe57.3ca7",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 470,
        "y": 140,
        "wires": []
    },
    {
        "id": "20e56ccf.468934",
        "type": "inject",
        "z": "d80cfe57.3ca7",
        "name": "模拟客户端",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "2232",
        "payloadType": "num",
        "x": 120,
        "y": 500,
        "wires": [
            [
                "3c19737a81c77e01"
            ]
        ]
    },
    {
        "id": "3c19737a81c77e01",
        "type": "tcp request",
        "z": "d80cfe57.3ca7",
        "name": "链接TCP客户端",
        "server": "127.0.0.1",
        "port": "8077",
        "out": "sit",
        "ret": "string",
        "splitc": " ",
        "newline": "",
        "trim": false,
        "tls": "",
        "x": 400,
        "y": 500,
        "wires": [
            [
                "55485485ac3f7420"
            ]
        ]
    },
    {
        "id": "55485485ac3f7420",
        "type": "debug",
        "z": "d80cfe57.3ca7",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 650,
        "y": 500,
        "wires": []
    },
    {
        "id": "619835f7eb170508",
        "type": "inject",
        "z": "d80cfe57.3ca7",
        "name": "发送信息",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "on",
        "payloadType": "str",
        "x": 120,
        "y": 420,
        "wires": [
            [
                "93bd92ba7f2c55f5"
            ]
        ]
    },
    {
        "id": "93bd92ba7f2c55f5",
        "type": "tcp out",
        "z": "d80cfe57.3ca7",
        "name": "发送到客户端",
        "host": "",
        "port": "",
        "beserver": "reply",
        "base64": false,
        "end": false,
        "tls": "",
        "x": 460,
        "y": 420,
        "wires": []
    },
    {
        "id": "dddb0f428c8f6616",
        "type": "ui_button",
        "z": "d80cfe57.3ca7",
        "name": "",
        "group": "fef2877f0b83bcc6",
        "order": 3,
        "width": 6,
        "height": 1,
        "passthru": false,
        "label": "开启风扇",
        "tooltip": "",
        "color": "",
        "bgcolor": "",
        "className": "",
        "icon": "play_arrow",
        "payload": "on",
        "payloadType": "str",
        "topic": "topic",
        "topicType": "msg",
        "x": 100,
        "y": 120,
        "wires": [
            [
                "58315427.7d1a9c",
                "93bd92ba7f2c55f5"
            ]
        ]
    },
    {
        "id": "967f515df4897c34",
        "type": "ui_button",
        "z": "d80cfe57.3ca7",
        "name": "",
        "group": "fef2877f0b83bcc6",
        "order": 4,
        "width": 6,
        "height": 1,
        "passthru": false,
        "label": "关闭风扇",
        "tooltip": "",
        "color": "",
        "bgcolor": "",
        "className": "",
        "icon": "stop",
        "payload": "off",
        "payloadType": "str",
        "topic": "topic",
        "topicType": "msg",
        "x": 100,
        "y": 220,
        "wires": [
            [
                "58315427.7d1a9c",
                "93bd92ba7f2c55f5"
            ]
        ]
    },
    {
        "id": "76850272a23de90a",
        "type": "function",
        "z": "d80cfe57.3ca7",
        "name": "解析温度",
        "func": "let payload=msg.payload;\nlet temp=payload.slice(0, 2);\nnode.error(\"温度:\" + temp);\nmsg.payload = temp;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 480,
        "y": 340,
        "wires": [
            [
                "65655366bb2ab9a8"
            ]
        ]
    },
    {
        "id": "7076a321cad3725c",
        "type": "function",
        "z": "d80cfe57.3ca7",
        "name": "解析湿度",
        "func": "let payload=msg.payload;\nlet humi=payload.slice(2, 4);\nnode.error(\"湿度:\" + humi);\nmsg.payload = parseInt(humi);\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 480,
        "y": 260,
        "wires": [
            [
                "651d560099e13367"
            ]
        ]
    },
    {
        "id": "65655366bb2ab9a8",
        "type": "ui_gauge",
        "z": "d80cfe57.3ca7",
        "name": "",
        "group": "fef2877f0b83bcc6",
        "order": 1,
        "width": 0,
        "height": 0,
        "gtype": "gage",
        "title": "温度显示",
        "label": "摄氏度",
        "format": "{{value}}℃",
        "min": "0",
        "max": "99",
        "colors": [
            "#00b500",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "",
        "seg2": "",
        "className": "",
        "x": 700,
        "y": 340,
        "wires": []
    },
    {
        "id": "651d560099e13367",
        "type": "ui_gauge",
        "z": "d80cfe57.3ca7",
        "name": "",
        "group": "fef2877f0b83bcc6",
        "order": 2,
        "width": 0,
        "height": 0,
        "gtype": "gage",
        "title": "湿度显示",
        "label": "湿度",
        "format": "{{value}} %rh",
        "min": "0",
        "max": "99",
        "colors": [
            "#00b500",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "",
        "seg2": "",
        "className": "",
        "x": 700,
        "y": 260,
        "wires": []
    },
    {
        "id": "fef2877f0b83bcc6",
        "type": "ui_group",
        "name": "阶石物联",
        "tab": "09b9cb70cc76f936",
        "order": 1,
        "disp": true,
        "width": "12",
        "collapse": false,
        "className": ""
    },
    {
        "id": "09b9cb70cc76f936",
        "type": "ui_tab",
        "name": "阶石物联",
        "icon": "check",
        "disabled": false,
        "hidden": false
    }
]
```
点击导入：
![](/assets/media/16703324957019/16703346936183.jpg)
导入成功后，点击右上角 部署：
![](/assets/media/16703324957019/16703347613695.jpg)
出现下图，表示部署成功。
![](/assets/media/16703324957019/16703347947930.jpg)
然后访问：http://127.0.0.1:1880/ui/ 出现如下界面，则表示程序导入成功：
![](/assets/media/16703324957019/16703349468208.jpg)

该TCP地址为：127.0.0.1:8077
本机的8077端口不能已被占用