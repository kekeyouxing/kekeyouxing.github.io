---
title: 责任链设计模式
tags: design-pattern
layout: article
aside:
  toc: true
sidebar:
  nav: docs-zh
author: keyouxing
comment: true
---
#### 导语
> 怎样才能快速的学习一种设计模式？手抄或者手敲代码两三遍是最好的答案。所以笔者直接上代码和程序流程图，
建议读者如果对责任链设计模式不清楚或者模糊，可以手抄一遍代码，再根据代码的执行顺序，模拟一遍代码流程就可以了。

```javascript
public interface Filter{
  doFilter(FilterChain filterChain);
}

FilterChain implements Filter{
  List<Filter> filters = new ArrayList<>();
  
  int index = 0; 
  
  public void addFilter(Filter filter){
    filters.add(filter);
  }
  
  public void doFilter(FilterChain filterChain){
    if(filters.size() == index){return;}
    Filter filter = filters.get(index);
    index++;
    filter.doFilter(filterChain);
  }
}

HTMLFilter implements Filter{
  public void doFilter(FilterChain chain){
    System.out.println("before HTML...");
    chain.doFilter(filterChain);
    System.out.println("after HTML...");
  }
}

CSSFilter implements Filter{
  public void doFilter(FilterChain chain){
    System.out.println("before CSS...");
    chain.doFilter(filterChain);
    System.out.println("after CSS...");
  }
}

public static void main(String[] args){
  FilterChain chain = new FilterChain();
  HTMLFilter htmlFilter = new HTMLFilter();
  CSSFilter cssFilter = new CSSFilter();
  chain.addFilter(htmlFilter);
  chain.addFilter(cssFilter);
  chain.doFilter(chain);
}
```
打印信息如下
```javascript
before HTML...
before CSS...
after CSS..
after HTML...
```
流程图如下
![责任链设计模式](http://keyouxing.com/img/oauth2/chain-design-pattern.png)
