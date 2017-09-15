---
title: 让你的网站免费开启Https访问，绿色健康小清新
date: 2017-07-05 15:55:44
tags: https 
---

## centos下lnmp开启https的正确姿势
   
   尾随大佬 [安正超](http://overtrue.me/) 大佬的gayhub发现一个好东西 [直达gayhub](https://github.com/Neilpang/acme.sh)

   软件自带中文教程，闲麻烦的可以看这里。
   
### 1. 安装acme.sh
  这个软件是纯净的，只需要一个文件夹放东西即可，不会产生其他垃圾文件，官方推荐放当前用户目录，照做！
  ```  bash
  # cd ~ 
  ```
  然后curl或者wget下载
  ```  bash
    # curl  https://get.acme.sh | sh
   ```
  下载完为了方便使用添加一个alias,自行选择当前用户添加或者所有用户添加。
  添加命令：
   ```
   # vim ~/.bashrc (当前用户)
   # vim /etc/bashrc (所有用户) 
   ```
   在alias下面插入（注意路径）：
   
   ` alias acme.sh='~/.acme.sh/acme.sh' `
   
   保存之后，source 让文件生效:
   ```
    #  source ~/.bashrc (当前用户)
    #  source /etc/bashrc (所有用户) 
   ```
### 2. 使用acme.sh，生成证书文件
   
   支持多种方式，本文只选取一种（http），其他的请自行看文档。
   ```
   # acme.sh  --issue  -d mydomain.com -d www.mydomain.com  --webroot  /home/wwwroot/mydomain.com/
   ```
   把mydomain.com 换成现有域名 --webroot之后的路径换成你网站项目路径（最后一级是public）!!
   
   不出意外（没有红色错误），文件生成成功.
   
### 3.使用证书
   ```
   # acme.sh  --installcert  -d  mydomain.com   \
            --key-file   /etc/nginx/ssl/mydomain.key \
           --fullchain-file /etc/nginx/ssl/mydonain.cer \
           --reloadcmd  "service nginx force-reload"
   ```
   一行一行输，第一行输入之后会进入可输入模式，接着输入就行，没有ssl文件夹的先创建文件夹
   
### 4.配置nginx 启用https
   编辑网站的nginx文件，改动如下.
   1.顶部添加一个server block区域，保证访问全部跳转https
   ```
   server {
   
       listen 80 default_server;(default_server 表示默认端口)
   
       listen [::]:80 default_server;
   
       server_name rekkles.xyz www.rekkles.xyz; (换成你的域名)
   
       return 301 https://$server_name$request_uri;
   
   }
   ```
   修改原先的server，启用https。（关闭监听的端口）
   ```
   listen 443 ssl default_server;
   listen [::]:443 ssl default_server;
   ssl on;
   ssl_certificate /etc/nginx/ssl/rekkles.cer;（你之前生成的证书文件名）
   ssl_certificate_key /etc/nginx/ssl/rekkles.key;(你之前生成的证书文件名）
   ```
   保存之后,测试配置.
   
   ` nginx -t`
   
   没有问题就重启nginx
   
   `service nginx restart`
   
   tip:
   一定要开放443端口，ssl使用的是443端口，不开启会导致网站无法访问。
   
   
    
          