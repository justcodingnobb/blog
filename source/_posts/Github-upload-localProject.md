---
title: 上传本地项目到Github
date: 2017-03-14 13:41:32
tags: github
---

## 1.建立GIT仓库
 cd到你本地的项目根目录，执行git命令，会在当前目录创建一个.git隐藏文件夹
 
  `git init`
 
## 2.添加文件到仓库
  `git add .`
  
  可以把.替换成你任何想要上传的文件或者文件夹，只有部分功能不需要上传的请在.gitignore文件中添加，例如
  ---
  ``` 
  .idea/  文件夹
  *.log  所有日志文件
  Thmubs.db  指定的文件
  ```
  ---
  
## 3.commit到远程仓库  

 `git commit -m "注释语句"`
   
<!-- more -->
    
## 4.获取Repository地址
 从github中获取到自己的Repository的地址。
 
##### 注意 
 ```
 是URL地址，不是Github的仓库地址.
 类似：https://github.com/justcodingnobb/blog
 https://github.com/你的github账号名/你的Repository名字
 ```
 
## 5. 关联仓库到Github
 `git remote add origin https://github.com/你的github账号名/你的Repository名字` 
## 6. 上传代码到Github
 `git push -u origin master` 
 
 操作完没有异常就是成功了，可能会要你输入账号密码，输入就ok了！😆
 
 tips:
 如果在最后上传代码的时候遇到这样的错误.
 
 ![错误](/img/error.png) 
 
 请输入
 
 `git pull --rebase origin master`
 
 原因是仓库里面的License或者Readme.md之类的文件没有下载下来，同步一下就好了。👌😄

---

最近访客

<div class="ds-recent-visitors" data-num-items="39" data-avatar-size="40" id="ds-recent-visitors"></div><br/>