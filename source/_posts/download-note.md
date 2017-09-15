---
title: 关于文件下载的最佳实践
date: 2017-07-05 09:17:49
tags: nginx
---
### 本篇日志主要记录大神 little 关于下载的讲解

#### nginx :

  假设私有文件放这里：
  
  /data/private/xxx/yyy.zip
  
  nginx 给个专门的域名 或者 单独的 location，例如 http://private.abc.com/xxx/yyy.zip
  
  这个时候，先做好测试，路径、域名都配置正确，保证访问 http://private.abc.com/xxx/yyy.zip 是能够下载到 服务器上的 /data/private/xxx/yyy.zip 文件。
  
  然后给这个 server {} 或者 location {} 配置成 internal;
  
  配置好之后，直接用 http://private.abc.com/xxx/yyy.zip 应该就是访问不了
  
#### laravel:
  php、laravel 这边，这么做，用这个域名 http://download.abc.com/xxx/yyy.zip
  
  指向某个 controller action，里面做各种判断，判断用户无权限，直接 return 403
  
  有权限，那么 header 送出 X-Accel-Redirect: http://private.abc.com/xxx/yyy.zip
  
### 小记
  这样，你看 php-fpm，只做身份鉴权的事，丢个 header 头，不会占用php资源，也不用把文件读进来。
  
  （域名无限制，只需要在nginx设置 Internal）
  
  不仅仅是文件下载的，同样的，比如你有个 nodejs、lua 写的代码。
  
  X-Accel-Redirect 是给 nginx 的指定，nginx 收到这个指令，就知道要去别的 server 或者 location 找
  
  至于这个 location 里干什么，可以很丰富的，下载文件也行，proxy google 也行，proxy 别的nodejs都可以
  
  X-Accel-Redirect 太常见，太好用，我好想在群里也提过两三次
  
  nginx X-Accel-Redirect 就是替代 apache 的 X-Sendfile，并且是加强版。
