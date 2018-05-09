---
layout: post
title: "ShadowSocks 配置及BBR加速"
date: 2018-05-04 
description: "------"
tag: 工具
---


# ShadowSocks 配置及BBR加速

ShadowSocks势必先要后买VPS，本人目前使用的是 [Vultr](https://www.vultr.com/?ref=7305309), 购买流程此文不再赘述。


## SSH 登录 VPS 
登录远程vps可使用Vultr提供网页的`View Console`(体验太差)，或使用ssh客户端工具进行登录。
此处以Mac终端登录为例：

打开终端,登录远程服务器，回车，输入yes确认是否连接，然后输入密码

```
$ ssh root@108.61.219.99
```
`Username`、`IP Address`可见创建的服务器详情


## VPS安装ShadowSocks


#### 1. 输入以下命令

```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

#### 2. 选择Shadowsocks server，此处本人使用的是ShadowsocksR


```
Which Shadowsocks server you'd select:
1) Shadowsocks-Python
2) ShadowsocksR
3) Shadowsocks-Go
4) Shadowsocks-libev
Please enter a number (Default Shadowsocks-Python):2

You choose = ShadowsocksR

Please enter password for ShadowsocksR
(Default password: teddysun.com):

```

#### 3. 配置信息
根据提示配置`Password`、`Port`、`Protocol`、`obfs`、`Encryption Method`

```
Congratulations, ShadowsocksR server install completed!
Your Server IP        :  108.61.0.0
Your Server Port      :  9999
Your Password         :  .......
Your Protocol         :  origin
Your obfs             :  plain
Your Encryption Method:  aes-256-cfb

Your QR Code: (For ShadowsocksR Windows, Android clients only)
 ssr://MTA4LjYxLjIxOS45OTo5OTk5O.......
Your QR Code has been saved as a PNG file path:
 /root/shadowsocks_r_qr.png

Welcome to visit: https://teddysun.com/486.html
Enjoy it!
```

#### 4. shadowsocks 常用命令
#### 卸载方法

```
$ ./shadowsocks-all.sh uninstall
```

##### 启动脚本(ShadowsocksR 版)：

```
/etc/init.d/shadowsocks-r start | stop | restart | status
```

##### 多开端口

- 找到配置文件`/etc/shadowsocks-r/config.json`或（`/etc/shadowsocks.json`）,修改如下：



```
{
    "server_port":9998,
    "password":"123456",
}

改为
{
    "port_password":{
        "9998":"password1",
        "9999":"password2"
     },
}
```


- 检测某个端口是否开启,返回`yes`说明已开启

```
$ firewall-cmd --query-port=9998/tcp --zone=public #查询9998端口是否开启
```

- 开启某端口代码

```
firewall-cmd --zone=public --add-port=9998/tcp --permanent #添加9998端口
```

- 最后，重启VPS服务器`reboot`

#### 5. BBR 脚本

使用root用户登录，运行以下命令：

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

验证方法

```
$ uname -r
# 查看内核版本，显示为最新版就表示 OK 了
# ————————————
$ sysctl net.ipv4.tcp_available_congestion_control
# 返回值一般为：
# net.ipv4.tcp_available_congestion_control = bbr cubic reno
# ————————————
$ sysctl net.ipv4.tcp_congestion_control
# 返回值一般为：
# net.ipv4.tcp_congestion_control = bbr
# ————————————
$ sysctl net.core.default_qdisc
# 返回值一般为：
# net.core.default_qdisc = fq
# ————————————
$ lsmod | grep bbr
# 返回值有 tcp_bbr 模块即说明bbr已启动。
```




## 安装 ShadowsocksR 客户端

[MAC版ShadowsocksX-NG-R](https://github.com/qinyuhang/ShadowsocksX-NG-R/releases)

下载ShadowsocksR配置服务器设置，填写相关信息

## Chrome插件—SwitchyOmega


[SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?utm_source=chrome-ntp-icon)

.......





## 参考
轻松在 VPS 搭建 Shadowsocks 翻墙 [https://www.diycode.cc/topics/738](https://www.diycode.cc/topics/738)

Shadowsocks 一键安装脚本（四合一）[https://teddysun.com/486.html](https://teddysun.com/486.html)

一键安装最新内核并开启 BBR 脚本[https://teddysun.com/489.html](https://teddysun.com/489.html)









