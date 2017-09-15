---
title: 如何开发一个Chrome拓展。
date: 2017-05-07 15:30:09
tags: Chrome-extension
---

### 小记，以后可能还会用到，先记录一部分。

### 翻译文档
  [360翻译文档](http://open.chrome.360.cn/extension_dev/overview.html)

  需要开发者资质，到 [谷歌开发者平台](https://chrome.google.com/webstore/developer/dashboard?utm_source=chrome-ntp-icon) 5刀认证成为开发者!(visa卡~)

 1. 建立一个manifest.json文件
 
 ``` 
 {
    "name": "xxxx",
    "manifest_version": 2,
    "version": "1.0.x",
    "description": "x",
    "browser_action": {
      "default_icon" : "images/01.png", //默认icon
      "default_popup": "popup.html" //插件页面
    },
  
    "content_scripts":[
      {
        "matches": ["*://*.acfun.cn/*","*://acfun.cn/*"], //作用域名
        "js": ["js/jquery.js","js/back.js"], //触发的JS
        "run_at":"document_end"  //文档结束触发  还有其他配置
      }
    ]
  }
  ```
  
  2. 摸清作用域和权限 就可以开始开发了~
   