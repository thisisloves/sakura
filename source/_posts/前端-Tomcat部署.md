---
title: Tomcat部署前端项目
date: 2021-12-15
tags: 前端
author: 映雪
---

似此星辰非昨夜，为谁风露立中宵。

<!--more-->

### Tomcat

- tomcat 是一个开源而且免费的 jsp 服务器，属于轻量级应用服务器。它可以实现 JavaWeb 程序的装载，是配置 JSP（Java Server Page）和 JAVA 系统必备的一款环境。

![截屏2022-10-27 下午5.00.57.png](/images/2022/10/28/wUkfEtpKZCGPDex.png)

### Tomcat 和 Apache

- 当在一台机器上配置好 Apache 服务器，可利用它响应 HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上 Tomcat 是 Apache 服务器的扩展，但运行时它是独立运行的，所以当你运行 tomcat 时，它实际上作为一个与 Apache 独立的进程单独运行的。当配置正确时，Apache 为 HTML 页面服务，而 Tomcat 实际上运行 JSP 页面和 Servlet。

### Tomcat 部署前端项目

1. **将前端项目放入 webapps 目录下**

![截屏2022-10-28 下午4.39.19.png](/images/2022/10/28/rQeSYodUEb47yuK.png)

- 输入 http://localhost:8080/对应包名/

2. **配置Server.xml**

- 修改 conf 目录下 server.xml，然后输入 http://localhost:8080/bolg/

```xml
   <Context docBase="/Users/infatuation/Desktop/sakura/public" path="/bolg"  reloadable="false"></Context>
```

3. **添加 xml**

- 在 conf/Catalina/localhost 目录下添加 testBolg.xml ，然后输入 http://localhost:8080/testBolg/

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context docBase="/Users/infatuation/Desktop/sakura/public" reloadable="false" />
```
