---
title: Centos 卸载php并升级到php7
date: 2017-03-16 17:20:08
tags: centos
---

## Centos 卸载php并升级到php7或重新安装php5.6
1. 如果安装了php，先卸载

 强迫症患者请先yum list installed s
 ``

 `yum remove php* php-common`
 
2. rpm安装php7的源

 `rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`
3. 安装php和相关拓展 
 
 php 7
 `yum install php70w.x86_64 php70w-cli.x86_64 php70w-common.x86_64 php70w-gd.x86_64 php70w-ldap.x86_64 php70w-mbstring.x86_64 php70w-mcrypt.x86_64 php70w-mysql.x86_64 php70w-pdo.x86_64` 
 
 php 5.6
 `yum install php56w.x86_64 php56w-cli.x86_64 php56w-common.x86_64 php56w-gd.x86_64 php56w-ldap.x86_64 php56w-mbstring.x86_64 php56w-mcrypt.x86_64 php56w-mysql.x86_64 php56w-pdo.x86_64` 
 
4. 安装php-fpm

 php 7
 `yum install php70w-fpm` 
 
 php5.6
 `yum install php56w-fpm `
 
5. 检查php版本

 `php -v` 
 
## 搞定 😆

---
最近访客

<div class="ds-recent-visitors" data-num-items="39" data-avatar-size="40" id="ds-recent-visitors"></div><br/>

