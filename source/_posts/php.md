---
title: centos 升级PHP7.1并开启配置优化opcache gzip
date: 2017-06-22 17:22:07
tags: centos php 
---

## 升级PHP7.1并配置opcache，gzip，大幅提升性能

1. 如果安装了php 先卸载，并卸载相关
    
    强迫症患者请先 `yum list installed` 查看安装了哪些php，然后用通配符卸载
 
    `yum remove xxxx`
 
    标准卸载
 
    `yum remove php* php-common`
2. rpm安装php7.1的源

    `rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`
3. 安装php7.1和相关拓展 
 
    `yum install php71w.x86_64 php71w-cli.x86_64 php71w-common.x86_64 php71w-gd.x86_64 php71w-ldap.x86_64 php71w-mbstring.x86_64 php71w-mcrypt.x86_64 php71w-mysql.x86_64 php71w-pdo.x86_64 php71w-opcache.x86_64`
 
    安装php-fpm：
 
    `yum install php71w-fpm`
4. 设置php-fpm开机自启
 
    `systemctl enable php-fpm`

    至此已经完成了安装php7.1和opcache的所有工作，下面开始介绍gzip
   
    判断是否开启了opcache
   
    ![opcache](/img/php-opcache.png)
5. 开启gzip
    
    仅适用LNMP架构 （Linux+Nginx+Mysql+PHP） 
    效果：网页开启gzip压缩以后，其体积可以减小20%~90%，可以节省下大量的带宽，从而减少页面响应时间，提高用户体验。
   
    找到并编辑nginx.conf，yum安装的
   
    `vim /etc/nginx/nginx.conf`
   
    在http尖括号内容内添加:
   
    `    
        # nginx gzip压缩
        gzip            on;
        gzip_min_length 1000;
        gzip_proxied    expired no-cache no-store private auth;
        gzip_types      text/plain application/xml;
    `
   
    判断是否开启Gzip
   
   
   ![gzip](/img/php-gzip.png)     
6. 完成！ Happy Coding!😆       

