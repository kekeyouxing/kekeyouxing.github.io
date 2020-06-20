---
title: 客户端
tags: Spring Security Oauth2

layout: article
aside:
  toc: true
sidebar:
  nav: docs-zh
author: keyouxing
comment: true
---
#### 导语:
> 本文将介绍客户端向授权服务器发送请求获取access_token时，前端处理逻辑，流程图如下。

![获取access_token(前端)](http://keyouxing.com/img/oauth2/access_token_1.png)
## 一、路由拦截
当用户访问后台的任意界面时，譬如'/home'、'/user'，都会被路由拦截器，它可以判断当前界面
应用是否拥有token，有token说明已经获取access_token，如果没有则跳转到登录界面获取access_token。
伪代码如下
```javascript
const whiteList = ['/login', '/register']
router.beforeEach((to, from, next) => {
  if (getToken()) {
    // Successfully obtained token
  } else {
    if (whiteList.indexOf(to.path) !== -1) {
      // In the login-free whitelist, enter directly
      next()
    } else {
      // Otherwise, all redirect to the login page
      next(`/login?redirect=${to.path}`) 
    }
  }
})
```
## 二、输入用户信息和客户端信息
在登录界面，用户输入账号、密码后。前端携带[用户信息和客户端信息](http://keyouxing.com/2020/06/12/springsecurity-2.html)向后台发送请求，
后台验证失败，返回失败，重新输入；验证成功，返回access_code，伪代码如下
```javascript
//client info, let's fix it
const client_id = 'admin'
const client_secret = 'admin123'
const grant_type = 'password'
const scope = 'server'

//login api
export function login(username, password) {
  return request({
    url: '/oauth/token',
    method: 'post',
    params: { username, password, client_id, client_secret, grant_type, scope }
  })
}

login(username, password).then(res => {
  // save access_token
  set_token(res.access_token)
  // redirect to '/index'
  next('/index')
  resolve()
  notify('login successful!')
}).catch(error => {
  reject(error)
  notify('Login failed!')
})

```
