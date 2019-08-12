---
title: "下载github加速"
date: 2018-06-25T10:57:48+08:00
draft: false
tags: ["github"]
categories: ["github"]
subtitle: ""
descriptions: "How to Accelerate Github Download"
bigimg:
weight: 10
---

让下载 github 更快

## Step

把下面2句加入 hosts 文件

```
DESKTOP-APB1HCJ% cat /etc/hosts
151.101.185.194 github.global.ssl.fastly.net
192.30.253.112 github.com
```

windows 参考 https://wanghualong.cn/archives/40/

## 具体原理：

https://blog.csdn.net/mist99/article/details/80602090  

但是，这里配置 hosts 文件的时候，写了 `https://` 这个是不对的，去除就可以了。

