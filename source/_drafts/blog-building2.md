---
abbrlink: ''
categories:
- - 博客搭建
date: '2024-01-22T16:34:33.634564+08:00'
tags:
- Github Pages
- 博客
- 域名
- CDN
title: Github Pages自定义域名与CDN加速
updated: '2024-01-22T16:34:34.211+08:00'
---
## 前言

[上一篇文章](https://www.cappuccilo.top/2024/01/18/blog-building1/)中我们介绍了如何使用Github Pages + Hexo搭建我们的个人博客，但在搭建完成后存在访问速度慢的问题，这篇文章来分享一下如何给Github静态页面添加自定义域名并且利用CDN加速。

## 为Github Pages添加自定义域名

### 购买域名

由于我们是利用Github Pages部署，属于国外服务器，而国内域名不能用在国外服务器上，所以我们需要购买国外域名。

我自己的域名是在[NameSilo](https://www.namesilo.com/)购买的，支持支付宝并且相对比较便宜。当然，你也可以选用其他的国外域名注册商购买域名。

打开网站，输入你想要的域名，点击`SEARCH DOMAIN`按钮即可查询相关的域名：

![https://s2.loli.net/2024/01/22/GZKtCMD8X9xdpBy.png](https://s2.loli.net/2024/01/22/GZKtCMD8X9xdpBy.png)

{% notel blue 提示 %}

在选好域名结账时，可以添加优惠码获得一些优惠，使用搜索引擎直接搜索`NameSilo优惠码`可以找到不少优惠码

{% endnotel %}

### 配置CDN服务器

本文以腾讯云为例，阿里云的配置大同小异，不再赘述。

点击[内容分发网络控制台](https://console.cloud.tencent.com/cdn)进入腾讯云CDN控制台。

点击`域名管理`-`添加域名`：

![https://s2.loli.net/2024/01/22/iuN2t7KfozOgEhP.png](https://s2.loli.net/2024/01/22/iuN2t7KfozOgEhP.png)


### DNS配置

域名购买完成后，进入[Domain Manager](https://www.namesilo.com/account_domains.php)，然后点击刚刚购买的域名的Options一栏的如下图所示按钮，进入DNS配置页：

![https://s2.loli.net/2024/01/22/vEu9PAQCZtO81Xs.png](https://s2.loli.net/2024/01/22/vEu9PAQCZtO81Xs.png)

点击CNAME，添加一条CNAME记录：
