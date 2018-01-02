---
title: shaowsocks
date: 2017-03-21 10:53:15
tags: [shadowsocks,breakwall]
---
![Shadowsocks](https://shadowsocks.be/usr/uploads/shadowsocks.png)
### 本脚本适用环境
#### 系统支持：CentOS，Debian，Ubuntu
#### 内存要求：≥128M

### 关于本脚本
#### 一键安装 ShadowsocksR 服务端。
请下载与之配套的客户端程序来连接。
（以下客户端只有 [Windows 客户端](https://github.com/breakwa11/shadowsocks-csharp/releases)和 [Python 版客户端](https://github.com/breakwa11/shadowsocks-rss/wiki/Python-client)可以使用 SSR 新特性，其他原版客户端只能以兼容的方式连接 SSR 服务器）

#### 默认配置
##### 服务器端口：自己设定（如不设定，默认为 8989）
##### 客户端端口：1080
##### 密码：自己设定（如不设定，默认为teddysun.com）

##### 客户端下载
* [windows](https://github.com/breakwa11/shadowsocks-csharp/releases) 
* [mac os](https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-Help)
* [linux](https://github.com/librehat/shadowsocks-qt5">Linux)
* [android](https://github.com/shadowsocks/shadowsocks-android)

#### 使用方法
##### 使用root用户登录，运行以下命令
> `wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2&gt;&amp;1 | tee shadowsocksR.log`

##### 安装完成后，脚本提示如下
> `Congratulations, 
    ShadowsocksR install completed!
    Server IP:your_server_ip
    Server Port:your_server_port
    Password:your_password
    Local IP:127.0.0.1
    Local Port:1080
    Protocol:origin
    obfs:plain
    Encryption Method:aes-256-cfb
    Welcome to visit:https://shadowsocks.be/9.html
    If you want to change protocol &amp; obfs, reference URL:
    https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup
    Enjoy it!`

#### 卸载方法
使用 root 用户登录，运行以下命令：
> ./shadowsocksR.sh uninstall

#### 查看运行状态
> /etc/init.d/shadowsocks status

可以查看 ShadowsocksR 进程是否已经启动。
本脚本安装完成后，已将 ShadowsocksR 自动加入开机自启动。

#### 使用命令
1. 启动：/etc/init.d/shadowsocks start
2. 停止：/etc/init.d/shadowsocks stop
3. 重启：/etc/init.d/shadowsocks restart
4. 状态：/etc/init.d/shadowsocks status

#### 配置文件路径：/etc/shadowsocks.json
#### 日志文件路径：/var/log/shadowsocks.log
#### 代码安装目录：/usr/local/shadowsocks

#### 多用户配置 sample：
> `{
"server":"0.0.0.0",
"server_ipv6": "[::]",
"local_address":"127.0.0.1",
"local_port":1080,
"port_password":{
    "8989":"password1",
    "8990":"password2"，
    "8991":"password3"
},
"timeout":300,
"method":"aes-256-cfb",
"protocol": "origin",
"protocol_param": "",
"obfs": "plain",
"obfs_param": "",
"redirect": "",
"dns_ipv6": false,
"fast_open": false,
"workers": 1
}`

#### 如果你想修改配置文件，请参考：
[https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup)


[参考链接 https://github.com/breakwa11/shadowsocks-rss](https://github.com/breakwa11/shadowsocks-rss)