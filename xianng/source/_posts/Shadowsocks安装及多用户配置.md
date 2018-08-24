---
title: Shadowsocks安装及多用户配置
date: 2018-08-24 10:38:22
tags:
- shadowsocks
- ss
- 多用户
---
### 安装Shadowsocks服务
Debian / Ubuntu:

    apt-get install python-pip
    pip install git+https://github.com/shadowsocks/shadowsocks.git@master

CentOS:

    yum install python-setuptools && easy_install pip
    pip install git+https://github.com/shadowsocks/shadowsocks.git@master

Windows:

See [Install Shadowsocks Server on Windows](https://github.com/shadowsocks/shadowsocks/wiki/Install-Shadowsocks-Server-on-Windows).

<!-- more -->
### 配置单用户
```
{
    "server":"127.0.0.1",  
    "local_address":"127.0.0.1",
    "local_port":1080,
    "server_port":8001,
    "password":"apple",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```
### 配置多用户
1. sudo vi /etc/shadowsocks.json

2. 插入以下内容 
```
{
    "server": "0.0.0.0",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password": {
        "8001": "apple1",
        "8002": "apple2",
        "8003": "apple3",
        "8004": "apple4"
    },
    "timeout": 300,
    "method": "aes-256-cfb"
}
```


Name | 说明 
----|------
server | 服务器地址，填ip或域名  
local_address | 本地地址    
server_port	|服务器对外开的端口
password	|密码，可以每个服务器端口设置不同密码
port_password	|server_port + password ，服务器端口加密码的组合
timeout	|超时重连
method|	默认: “aes-256-cfb”，见 Encryption
fast_open|	开启或关闭 TCP_FASTOPEN, 填true / false，需要服务端支持

### 启动
- 前端启动 ` ssserver -c /etc/shadowsocks.json`
- 后端启动(推荐) `ssserver -c /etc/shadowsocks.json -d start`
- 停止 `ssserver -c /etc/shadowsocks.json -d stop`
- 重启 `ssserver -c /etc/shadowsocks.json -d restart`

### 开机启动
1. `sudo vi /etc/rc.local`
2. 把 `ssserver -c /etc/shadowsocks.json -d start` 添加到最后一行
3. `wq` 保存退出
