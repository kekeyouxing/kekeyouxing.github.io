---
title: 用户信息和客户端信息
tags: Spring Security Oauth2

layout: article
aside:
  toc: true
sidebar:
  nav: docs-zh
author: keyouxing
comment: true
---
#### 导语：
> 本文将介绍客户端向授权服务器发送请求获取access_token时，需要发送哪些参数。

## 一、用户登录信息
### username
`username = admin123`  
用户的账号
### password
`username = admin123`  
用户的密码
## 二、客户端信息
### grant_type
`grant_type = password`  
spring security oauth2有多种登录模式：授权码模式、密码模式、简化模式、客户端模式。这里采用密码模式登录。
### client_id & client_secret
`client_id = uchat`  
`client_secret = admin123`  
在授权码模式的应用实例中，
譬如我们使用微信登录某东，我们为什么可以使用微信登录某东，而不可以使用微信登录某宝呢？因为微信授予了某东**appid & secret**，并没有授予给某宝
**appid & secret**，这里的**appid & secret**就是spring oauth2的**client_id & client_secret**，简单点说：就是如果想用微信登录
第三方网站，那第三方网站就需要向微信提出申请，申请通过后，会授予**appid & secret**。
![微信授权地址](http://keyouxing.com/img/oauth2/client_id_secret.png)

在密码模式的应用实例中，同样需要**client_id & client_secret**。
如果公司中有多个应用，并且想公用一套用户信息，
譬如**招聘系统**、**后勤系统**，那么就需要为每个系统设定**client_id & client_secret**，
我这里开发的应用只有一个客户端：**UCHAT后台管理系统**，
所以可以在授权服务器中把**client_id & client_secret**
写死，这里后面再讲。
### scope
`scope = server`  
官方定义为：**scope is client authorities/roles**  
scope值决定客户端的权限，服务端校验token确认身份后再根据scope的值返回不同的信息，
比如微信公众号开发中根据url参数中scope传snsapi_base和snsapi_userinfo返回的信息内容不同，
详情可见
[微信授权登录](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/Wechat_webpage_authorization.html)
理解scope。



