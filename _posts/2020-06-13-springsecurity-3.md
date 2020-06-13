---
title: 获取access_token-1(前端)
tags: Spring Security Oauth2
---
#### 导语:
> 本文将介绍客户端向授权服务器发送请求获取access_token时，前端处理逻辑。

## 一、路由拦截
当用户访问后台的任意界面时，譬如'/home'、'/user'，都会被**permission.js**拦截，它是一个路由拦截器，可以判断当前界面
应用是否拥有token，有token说明已经获取access_token，如果没有则跳转到登录界面获取access_token。伪代码如下
```java
const whiteList = ['/login', '/register']
router.beforeEach((to, from, next) => {
  NProgress.start()
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

