---
title: Cookie
date: 2019-4-19
tags: JS
author: 映雪
---

非关美色，也不是感情，只是上一次在他觉得走投无路的时候，通往未知之路的门在他面前打开，那个像是公主又像是魔女的姑娘穿着套裙和高跟鞋美得凌厉凶狠，走到他的面前来，把一切都摆平了。

<!--more-->

### cookie(会话跟踪技术)

- 第一次跟服务器连接后，服务器给你发的一个身份牌，上面可以录跟你有关的信息（是否登录，购物车等等信息），以后只要再跟服务器通信，必须带着这个令牌，这样一来，服务器会直接知道你身份牌上所有的信息。


### cookie 的使用

1. 设置cookie

```js
// 设置cookie
function setCookie(name,value,timer){
    var date =new Date;
    date.setDate(date.getDate()+timer);
    document.cookie = name+"="+value+";expires="+date;
}

```

2. 获取cookie

```js
 // 获取cookie
function getCookie(name){
    var arr = document.cookie.split("; ");
    for(var i=0;i<arr.length;i++){
        arr2 = arr[i].split("=");
        if(arr2[0] == name){
            return arr2[1];
        }
    }
    return "";
}
```

3. 删除cookie

```js
// 删除cookie
function removeCookie(name){
    setCookie(name,1,-1);
}
```

### cookie 存储路径

1. 设置路径为 "/",表示当前服务器任何项目都可以访问cookie对象
2. 默认不设置路径，表示当前项目下的资源可以访问cookie对象
3. 设置指定路径，表示指定路径下的资源可以访问cookie对象

```java
package cn.itcast.web.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
/*
 * cookie 路径
 */

@WebServlet("/cookie05")
public class ServletCookie05 extends HttpServlet {

  /*
   * 就绪/服务器方法（处理请求数据）
   * 系统方法，服务器自动调用
   * 当有请求到达Servlet时，就会调用该方法
   * 方法可多次调用
   */

  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    Cookie cookie1 = new Cookie("cookie01","存储在/路径");
    /* 设置路径为/,表示在当前服务器下任何项目都可以访问到Cookie对象*/
    cookie1.setPath("/");
    response.addCookie(cookie1);

    Cookie cookie2 = new Cookie("cookie02","存储在默认路径");
    /* 不设置路径或者设置当前默认路径,当前默认下的资源可以访问*/
    cookie2.setPath("/servletDemo");
    response.addCookie(cookie2);

    Cookie cookie3 = new Cookie("cookie03","存储在指定路径");
    /* 设置指定目录，指定目录下的资源才可以访问*/
    cookie3.setPath("/servletDemo/cookie05");
    response.addCookie(cookie3);
  }
}

```

![截屏2022-11-14 下午3.41.33.png](/images/2022/11/14/JDIWGSnqoCTwEFZ.png)




### cookie 的特点

1. 只能使用文本文件（如果浏览器可以随意在客户端机器生成文件，比如身份令牌，那将是个定时炸弹，安全问题会变得非常严重）
2. 文件有大小限制 4K（文件若没有大小限制，相当于身份令牌重几百斤，挂在脖子上什么感觉？）
3. 数量限制，小于 50 条（一般浏览器限制大概在 50 条左右，门禁卡里能存下一部蓝光高清么）
4. 读取有域名限制（不可跨域读取，只能由写入 cookie 的 同一域名 的网页进行读取。简单来说，谁写的 cookie，谁才有权利读取）
5. 时效限制（每个 cookie 有时效，最短的有效期是：会话级别(关闭浏览器，cookie 销毁)

