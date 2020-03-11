---
layout: post
title: Apache, Tomcat, Nginx 应用场景和对比
date: 2020-03-10
Author: 山猪
tags: [DevOp, IT]
comments: true
---
![img](https://www.nginx.com/wp-content/uploads/2016/02/apache-tomcat-logo.png)

<!-- more -->

## 关于HTTP服务

HTTP服务器本质上也是一种应用程序——它通常运行在服务器之上，绑定服务器的IP地址并监听某一个tcp端口来接收并处理HTTP请求，这样客户端（一般来说是IE, Firefox，Chrome这样的浏览器）就能够通过HTTP协议来获取服务器上的网页（HTML格式）、文档（PDF格式）、音频（MP4格式）、视频（MOV格式）等等资源。下图描述的就是这一过程：

![avatar](https://pic1.zhimg.com/80/904696074e077934e601f175913f42fd_720w.jpg "HTTP service")

- Apache，指的应该是Apache软件基金会下的一个项目——Apache HTTP Server Project
- Apache Tomcat则是Apache基金会下的另外一个项目，与Apache HTTP Server相比，Tomcat能够动态的生成资源并返回到客户端。
- Nginx同样也是一款开源的HTTP服务器软件（当然它也可以作为邮件代理服务器、通用的TCP代理服务器，包括负载均衡服务器）。

## 定义

Apache HTTP Server和Nginx都能够将某一个文本文件的内容通过HTTP协议返回到客户端，但是这个文本文件的内容是固定的——也就是说无论何时、任何人访问它得到的内容都是完全相同的，这样的资源我们称之为**静态资源**。

Apache HTTP Server和Nginx本身不支持生成动态页面，但它们可以通过其他模块来支持（例如通过Shell、PHP、Python脚本程序来动态生成内容）。如果想要使用Java程序来动态生成资源内容，使用这一类HTTP服务器很难做到。Java Servlet技术以及衍生的Java Server Pages技术可以让Java程序也具有处理HTTP请求并且返回内容（由程序动态控制）的能力，Tomcat正是支持运行Servlet/JSP应用程序的容器（Container，准确的说，Tomcat是一个 Application Server:

![avatar](https://pic1.zhimg.com/80/2651b72ce2170336d10ad17fd020ae7a_720w.jpg "Servlet")

Tomcat运行在JVM之上，它和HTTP服务器一样，绑定IP地址并监听TCP端口，同时还包含以下指责：管理Servlet程序的生命周期将URL映射到指定的Servlet进行处理与Servlet程序合作处理HTTP请求——根据HTTP请求生成HttpServletResponse对象并传递给Servlet进行处理，将Servlet中的HttpServletResponse对象生成的内容返回给浏览器。

Apache 和 Tomcat 都是web网络服务器，两者既有联系又有区别，在进行HTML、PHP、JSP、Perl等开发过程中，需要准确掌握其各自特点，选择最佳的服务器配置。　　Apache是web服务器（静态解析，如HTML），tomcat是java应用服务器（动态解析，如JSP）　　Tomcat只是一个servlet(jsp也翻译成servlet)容器，可以认为是apache的扩展，但是可以独立于apache运行

## 从项目的结构来说

nginx是前端服务器，甚至提供更加高级的网络功能，，用来处理请求的转发、反向代理等。Apache是前端内容服务器；是静态页面服务器.绝大部分时候他们本身并不会运行项目。

Tomcat 以及 Jetty 是后端服务器，属于Java Servlet容器.用来生成动态页面.是直接用来运行项目的容器。

简单来说就是你发出一个请求,先经过nginx,它们会合理地把请求分配到后台比较不忙的Tomcat.Tomcat会把请求处理好返回给Nginx,然后Nginx会把最终的结果传送给浏览器.当然,如果是一些静态的数据,Nginx就可以直接处理了.

## 从内容来说

Tomcat/Jetty 等等这一类叫Web Container,也就是Web容器，所谓容器，是和他负责的东西管理整个的生命周期的。所以Web Container会管理整个Servlet的生命周期。类似的Spring 的Ioc容器则会管理整个Bean的生命周期。而GlassFish/Weblogic这一类的 application Server，则会管理更多，包含命名服务器，EJB等资源。

Nginx/apache 可以说是web server. 也就是他们可以处理静态资源，比如html,图片等，但如果把Servlet交给它则处理不了。所以，一般把Nginx放在前端处理静态资源，如果有对应的Servlet请求，则通过AJP转给后面的Tomcat、Jetty进行处理。
“tomcat用在java后台程序上，java后台程序难道不能用apache和nginx吗？”不能。apache和nginx不是servlet容器。什么是servlet容器呢？即实现HttpServletRequest、HttpServletResponse、HttpSession等等接口，解析http请求，通过类加载器加载对应的servlet实现类并调用。也就是说servlet容器必须由java或者基于jvm的语言实现。



[参考链接](https://www.zhihu.com/question/32212996/answer/87524617)

[参考链接](https://blog.csdn.net/MPFLY/article/details/76578006)