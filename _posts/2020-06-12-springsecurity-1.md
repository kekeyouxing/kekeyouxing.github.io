---
title: Oauth2 密码模式
tags: Spring Security Oauth2
---
#### 导语：
> spring security oauth2密码模式是基于用户输入账号、密码实现登录的一套安全框架，本系列博客将从源码分析用户是如何登入系统，以及在这个过程中
,spring参与了怎样的角色。

## 密码模式角色分类
![密码模式流程图](http://keyouxing.com/img/oauth2/password_mode.png)

* A: 资源拥有者提供给客户端账号、密码，类似于用户在登录界面输入账号、密码。
* B: 客户端携带账号、密码及其他信息，向授权服务器请求授权
* C: 授权服务器验证客户端的信息，验证通过后返回access_token
* D: 此后客户端携带access_token与资源服务器交互


