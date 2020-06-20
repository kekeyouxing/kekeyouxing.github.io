---
title: 授权服务器
tags: Spring Security Oauth2

layout: article
aside:
  toc: true
sidebar:
  nav: docs-zh
author: keyouxing
comment: true
---
#### 导语
> 上文中讲到，前端发送用户信息和客户端信息给授权服务器，本文将讲解授权服务器的实现和其中涉及到的**filters**，
至于授权服务器是如何生成access_token将在后面的文章进行讲解。
## 一、授权服务器的配置
为了避免读者对授权服务器的疑惑，我们基于spring security oauth2框架直接来看看授权服务器该如何配置，伪代码如下

```java
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
        endpoints
            .allowedTokenEndpointRequestMethods(HttpMethod.GET, HttpMethod.POST)
            .authenticationManager(authenticationManager)
            .userDetailsService(userDetailsService)
            .reuseRefreshTokens(reuseRefreshToken)
            .tokenStore(tokenStore)
            .tokenEnhancer(tokenEnhancer);
    }
    
    protected void configure(HttpSecurity http) throws Exception
    {
        http
            .authorizeRequests()
            .antMatchers("/oauth/token").permitAll()
            .anyRequest().authenticated()
            .and().csrf().disable();
    }
```
对于上面的代码中，刚接触的话会是一头雾水，譬如什么是**authenticationManager**，
**userDetailsService**，**tokenStore**...没错，后面的文章都将围绕这里讲解，这里只是让大家喵一眼，留个印象。

## 二、filters
在讲解授权服务器的实现之前，首先要知道授权服务器需要做什么？
1. 验证客户端信息
2. 验证用户信息
3. 保存用户信息
4. 校验哪些路径可以免授权访问  
5. 等等......

授权服务器要想实现上述这些相互关联的功能，最佳方案就是使用[责任链设计模式](http://keyouxing.com/2020/06/14/chain-design-pattern.html)。
以下截图就是spring security oauth2使用到的filters，
![授权服务器filters](http://keyouxing.com/img/oauth2/filter_auth.png)

这些filters帮我们实现了各种功能,譬如验证客户端信息、保存用户信息...
程序员不再需要自己去实现这些功能，只需要像上面伪代码一样，提供配置就可以了，
当然去阅读这些源码和逻辑是对自身学习有很大的帮助的，所以本系列文章将带领读者去阅读这些源码，而不仅仅关心如何配置，
弄清楚spring到底是如何实现oauth2密码模式的。

从上图可以知道，在启动授权服务器的时候，spring帮我们默认定义了12个filter，精力有限，我们仅关注几个核心的filter
* ![SecurityContextPersistenceFilter](http://keyouxing.com/2020/06/15/SecurityContextPersistenceFilter.html)
* ClientCredentialsTokenEndpointsFilter
* ExceptionTranslationFilter
* FilterSecurityInterceptor
