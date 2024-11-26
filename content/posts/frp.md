---
title: "frp内网穿透工具服务端与客户端配置"
date: 2023-04-02T12:54:56+08:00
draft: false
---
在没有公网ip的情况下，访问内网设备需要通过内网穿透，frp是一种常用的方法。

使用frp需要有一台拥有公网ip的服务器运行frp服务端。

frp github仓库地址：https://github.com/fatedier/frp/

frp下载地址：https://github.com/fatedier/frp/releases/

以linux x86_64机型为例：
---

首先，防火墙开放7000、7500端口，以及所有用于端口映射的端口

下载解压frp（客户端服务端相同）
```shell
wget https://github.com/fatedier/frp/releases/download/v0.48.0/frp_0.48.0_linux_amd64.tar.gz
tar -zxvf frp_0.48.0_linux_amd64.tar.gz
cd ./frp_0.48.0_linux_amd64
```
---
修改frps.ini文件
```
[common]
bind_port = 7000
#管理页面端口与用户名密码设置，可以没有用户名密码，也可以不开启管理页面，不影响frp服务端运行
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = password
#设置token以验证frp客户端身份，token服务端须与客户端一致，也可以不设置token
token = 12345678
```
---
运行frp服务端
```shell
./frps -c ./frps.ini
```
或在后台运行frp服务端
```shell
nohup ./frps -c ./frps.ini &
```
---
设置客户端，修改frpc.ini文件
```
[common]
#填入frp服务器的ip地址或域名
server_addr = x.x.x.x
server_port = 7000
#token客户端须与服务端一致，若服务端未设置token则此处不用设置token
token = 12345678

#端口映射示例
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

[web]
type = http
local_port = 80
custom_domains = example.com
```
---
运行frp客户端
```shell
./frpc -c ./frpc.ini
```
或在后台运行frp客户端
```shell
nohup ./frpc -c ./frpc.ini &
```
---
在 x.x.x.x:7500 可登入管理页面

不出意外，frp内网穿透就以及配置完成了。