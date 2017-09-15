---
title: CentOS 安装 supervisor 以及设置 Laravel 队列设置 
date: 2017-06-25 13:51:43
tags: Laravel CentOS supervisor
---

## CentOS 安装 supervisor 以及设置 Laravel 队列设置 
 
### 1. 安装Supervisor
   
   先安装 Python 的 easy_install，再通过 easy_install 安装 supervisor
    
   ```
   # yum install python-setuptools
   # easy_install supervisor
   ```
   
### 2. 配置文件
    
   生成配置文件，并建立相应目录，管理 supervisor 启动进程
   
   ```
   # echo_supervisord_conf > /etc/supervisord.conf
   # mkdir -p /etc/supervisor/conf.d/
   ```
   
   编辑 `/etc/supervisord.conf`，修改最下面 [include] 区块内容：
   
   ```
   [include]
   files = /etc/supervisor/conf.d/*.conf
   ```
   这样， supervisor 会加载 /etc/supervisor/conf.d/ 下的所有 .conf 文件

### 3.自动启动
   注意路径!
   ```
   # wget -O /usr/lib/systemd/system/supervisord.service  https://github.com/Supervisor/initscripts/raw/master/centos-systemd-etcs
   ```
   将 `supervisord` 服务设为自启动
   ```
   # systemctl enable supervisord.service
   ```
   输入 `supervisorctl` 命令可以进入 supervisor 控制台

### 4.设置 Laravel 队列
   新建 `/etc/supervisor/conf.d/xxxx.conf` 文件：
   ```
   [program:laravel-worker]
   process_name=%(program_name)s_%(process_num)02d
   command=/usr/local/php/bin/php /xxxx/artisan queue:work database --sleep=3 --tries=3 --daemon
   autostart=true
   autorestart=true
   user=www (注意用户)
   numprocs=8
   redirect_stderr=true
   stdout_logfile=/xxxx/storage/logs/queue.log
   ```
   这里开启了 8 个 queue:work 进程，并监视他们，如果失败的话，自动重启；在项目的 storage/logs/queue.log 记录日志。
   启动 supervisor 服务：
   
   `# supervisord`
   
   至此， supervisor 安装及 Laravel 队列设置完毕
   
   如果以后配置文件有修改，或者新增，进入 supervisor 控制台，执行下面的命令
   
   ```
   # supervisorctl
   supervisorctl> reread
   supervisorctl> update
   supervisorctl> start laravel-worker:*
   ```
