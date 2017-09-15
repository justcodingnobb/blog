---
title: Centos环境下搭建Shadowsocks服务端翻墙
date: 2017-03-13 14:04:26
tags: 
  - Shadowsocks
  - VPS
---
#### 1.shell登录服务器
![登录服务器](/img/login.png)

#### 2. 安装python-pip

	yum install python-pip

#### 3. 安装shadowsocks

	pip install shadowsocks
	
<!-- more -->
    
#### 4.快速启动
`nohup  sslocal -s your-server-ip -p your-server-port  -l 1080 -k your-server-passwd -t 600 -m rc4-md5 > /dev/null 2>&1 &`

###  4.1 文件启动shadowsocks
 * 在一个你找到的地方新建一个json文件，如新建/var/shadowsocks.json文件
 
---
```python
 {
    "server":"your_server_ip",      #ss服务器IP
    "server_port":your_server_port, #端口
    "local_address": "127.0.0.1",   #本地ip
    "local_port":1080,              #本地端口
    "password":"your_server_passwd",#连接ss密码
    "timeout":300,                  #等待超时
    "method":"rc4-md5",             #加密方式
    "fast_open": false,             # true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟。开启方法： echo 3 > /proc/sys/net/ipv4/tcp_fastopen 开启之后，将 fast_open 的配置设置为 true 即可
    "workers": 1                    # 工作线程数
}
```
---

保存文件之后运行 `nohup sslocal -c /etc/shadowsocks.json /dev/null 2>&1 &` 来启动shadowsocks
开机自动启动  `echo " nohup sslocal -c /etc/shadowsocks.json /dev/null 2>&1 &" /etc/rc.local`


##### 注解：
 * nohup : 后台运行并记录日志

 * you-server-ip : 替换成本机IP

 * your-server-port : 替换成你的端口

 * your-server-passwd : 设置你的密码

 * rc4-md5 : shadowsocks加密方式 默认就好


注意：如果你采用的Centos7.x系统 建议禁用FireWalld防火墙，使用iptables防火墙 并且开放你设置的端口。

#### 5. 配置客户端开始翻墙
 windows、IOS、Android、Mac都可以使用，在服务器端配置完毕之后下载Shadowsocks，启动！
 
 填上地址，端口，密码，启动代理，完成！✌️
 

---

最近访客

<div class="ds-recent-visitors" data-num-items="39" data-avatar-size="40" id="ds-recent-visitors"></div><br/>