---
abbrlink: ''
categories:
- - 博客搭建
cover: https://s2.loli.net/2024/01/23/uOJZ3pFyzxADRo2.png
date: '2024-01-22T16:34:33.634564+08:00'
tags:
- Github Pages
- 博客
- 域名
- CDN
title: Github Pages自定义域名与CDN加速
updated: '2024-01-23T09:04:28.429+08:00'
---
## 前言

[上一篇文章](https://www.cappuccilo.top/2024/01/18/blog-building1/)中我们介绍了如何使用Github Pages + Hexo搭建我们的个人博客，但在搭建完成后存在访问速度慢的问题，这篇文章来分享一下如何给Github静态页面添加自定义域名并且利用CDN加速。

## 购买域名

由于我们是利用Github Pages部署，属于国外服务器，而国内域名不能用在国外服务器上，所以我们需要购买国外域名。

我自己的域名是在[NameSilo](https://www.namesilo.com/)购买的，支持支付宝并且相对比较便宜。当然，你也可以选用其他的国外域名注册商购买域名。

打开网站，输入你想要的域名，点击`SEARCH DOMAIN`按钮即可查询相关的域名：

![https://s2.loli.net/2024/01/22/GZKtCMD8X9xdpBy.png](https://s2.loli.net/2024/01/22/GZKtCMD8X9xdpBy.png)

{% note info %}

在选好域名结账时，可以添加优惠码获得一些优惠，使用搜索引擎直接搜索`NameSilo优惠码`可以找到不少优惠码

{% endnote %}

## 为Github Pages配置自定义域名

Github的配置很简单，只要打开你的`<用户名>.github.io`的仓库，点击`Settings`->`Pages`，在`Custom domain`一栏填入你的域名，点击`Save`保存即可。

![https://s2.loli.net/2024/01/23/bJNj5BQWi91LgsT.png](https://s2.loli.net/2024/01/23/bJNj5BQWi91LgsT.png)

{% note info %}

这里Github可能会提示需要配置DNS解析记录，如果不需要配置CDN可以按照这个配置从而省略下面的步骤，但是这样仅仅是配置好了自定义域名，不会有CDN加速功能，为了接入CDN，我们可以不管Github的提示。

{% endnote %}

## 配置CDN（内容分发网络）

本文以腾讯云为例，阿里云的配置大同小异，不再赘述。

点击[内容分发网络控制台](https://console.cloud.tencent.com/cdn)进入腾讯云CDN控制台。

点击`域名管理`-`添加域名`：

![https://s2.loli.net/2024/01/22/iuN2t7KfozOgEhP.png](https://s2.loli.net/2024/01/22/iuN2t7KfozOgEhP.png)

然后在`域名配置`一栏按照下图配置：

![https://s2.loli.net/2024/01/23/VP9Sk1iGzrD6yEe.png](https://s2.loli.net/2024/01/23/VP9Sk1iGzrD6yEe.png)

{% folding green::Tips：如何添加DNS解析记录 %}

域名购买完成后，进入[Domain Manager](https://www.namesilo.com/account_domains.php)，然后点击刚刚购买的域名的Options一栏的如下图所示按钮，进入DNS配置页：

![https://s2.loli.net/2024/01/22/vEu9PAQCZtO81Xs.png](https://s2.loli.net/2024/01/22/vEu9PAQCZtO81Xs.png)

按照腾讯云提供的内容添加一条对应记录：

![https://s2.loli.net/2024/01/23/oiNDqC4LZKgl6Xp.png](https://s2.loli.net/2024/01/23/oiNDqC4LZKgl6Xp.png)

{% endfolding %}

在`源站配置`添加如下图选择，并添加四条地址，回源HOST跟上面的加速域名一致即可：

![image.png](https://s2.loli.net/2024/01/23/5j48AmXKPHux9cN.png)

```plaintext
185.199.108.153

185.199.109.153

185.199.110.153

185.199.111.153
```

在`节点缓存过期配置`一栏点击此处修改规则：

![https://s2.loli.net/2024/01/23/fUHbCTPQ7Bi3anq.png](https://s2.loli.net/2024/01/23/fUHbCTPQ7Bi3anq.png)

![https://s2.loli.net/2024/01/23/IuRHOpFdco4BK2a.png](https://s2.loli.net/2024/01/23/IuRHOpFdco4BK2a.png)

{% note info %}

另一栏缓存天数可以按需修改，好处是时间越大CDN流量消耗越小，但坏处是如果网站内容有调整，用户看到的可能还是缓存的版本，不过好在我们也可以手动刷新缓存。我这里调整为2天。

{% endnote %}

后续点击下方按钮`跳过推荐配置`即可。

完成后，回到`域名管理`发现腾讯云为我们提供了一条CNAME记录值，将它添加到NameSilo的解析记录里（注意类型是CNAME）：

![https://s2.loli.net/2024/01/23/C2o5XLhYJI7EtZd.png](https://s2.loli.net/2024/01/23/C2o5XLhYJI7EtZd.png)

![https://s2.loli.net/2024/01/23/3yRShEwulckesjv.png](https://s2.loli.net/2024/01/23/3yRShEwulckesjv.png)

## 配置HTTPS

为了给我们的域名添加证书，进入[SSL 证书控制台](https://console.cloud.tencent.com/ssl)，点击`申请免费证书`：

![https://s2.loli.net/2024/01/23/Q5KWBNiC49pyVqI.png](https://s2.loli.net/2024/01/23/Q5KWBNiC49pyVqI.png)

![https://s2.loli.net/2024/01/23/w5haHkDrClxmygB.png](https://s2.loli.net/2024/01/23/w5haHkDrClxmygB.png)

同样地，我们需要为我们的域名添加一条CNAME解析记录，等待一会后（大概半小时），点击`验证域名`：

![https://s2.loli.net/2024/01/23/yz1T3KXfuJaNkFQ.png](https://s2.loli.net/2024/01/23/yz1T3KXfuJaNkFQ.png)

{% folding blue::Tips：添加后类似这样 %}

![https://s2.loli.net/2024/01/23/asuQMXyivxj3rzc.png](https://s2.loli.net/2024/01/23/asuQMXyivxj3rzc.png)

{% endfolding %}

回到[CDN证书配置](https://console.cloud.tencent.com/cdn/certificate)，按下图所示：

![https://s2.loli.net/2024/01/23/oprNyU4Fi1HqucZ.png](https://s2.loli.net/2024/01/23/oprNyU4Fi1HqucZ.png)

![https://s2.loli.net/2024/01/23/lLOSE1sGzoMrQiH.png](https://s2.loli.net/2024/01/23/lLOSE1sGzoMrQiH.png)

完成后，静待片刻，通过访问你的域名就可以看到你的网站了，并且速度会比以前快不少。

## 其他问题

{% folding blue::Q1：如何手动刷新CDN %}

在[内容分发网络控制台](https://console.cloud.tencent.com/cdn/domains)，按下图所示操作：

![https://s2.loli.net/2024/01/23/JYkP8VotTHERBbu.png](https://s2.loli.net/2024/01/23/JYkP8VotTHERBbu.png)

刷新一般需要数分钟时间，请耐心等待。

{% endfolding %}

{% folding blue::Q2：CDN配置和申请证书时域名验证不成功怎么办？ %}

请检查你的[DNS配置](https://www.namesilo.com/account_domain_manage_dns.php)是否添加了对应的值，添加是否准确，添加完成后，一般需要等待半小时生效，或者可能会需要更长时间，请耐心等待。

{% endfolding %}

{% folding blue::Q3：网站内容变更后，访问网站为什么还是以前的内容？ %}

这是由于CDN缓存机制导致的，在上面的配置中，我们配置了缓存时间，由于缓存还未过期，现在访问网站还是以前的内容。

解决方法1：缩短CDN配置里的缓存时间；

解决方法2：手动刷新CDN

{% endfolding %}
