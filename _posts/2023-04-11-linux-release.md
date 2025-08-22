---
layout: post
title: 关于Linux的版本
date: 2023-04-11
Author: 山猪
tags: [Linux]
comments: true
---
![img](https://miro.medium.com/max/1400/0*rf6Z7l0CXPB4eQeq.jpg)

<!-- more -->

## 常见的Linux发行版本

SSO 是英文 Single Sign On 的缩写，翻译过来就是单点登录。顾名思义，它把两个及以上个产品中的用户登录逻辑抽离出来，达到只输入一次用户名密码，就能同时登录多个产品的效果。

打个比方，SSO 和我们去迪士尼玩时购买的通票很像。

![WX20200629-170628.png](https://i.loli.net/2020/06/30/hL8N5oqJHdQvOkz.png)

我们只要买一次通票，就可以玩所有游乐场内的设施，而不需要在过山车或者摩天轮那里重新买一次票。在这里，买票就相当于登录认证，游乐场就相当于使用一套 SSO 的公司，各种游乐设施就相当于公司的各个产品。

![WX20200629-170713.png](https://i.loli.net/2020/06/30/xcjFe8YkBGLdQy2.png)

使用 SSO 的优点很明显：

- 提升用户体验。
就以我厂为例。我厂有两个产品，丁香人才网和丁香园论坛，假如你是我厂用户，肯定无法忍受登录丁香园论坛的时候输入一次用户名密码，登录人才网又要输入一次用户名密码吧？
- 避免重复开发
假如你是我厂后端，每天任务都饱和的不行，肯定无法忍受到人才网开发一套登录逻辑，到论坛又开发一套登录逻辑吧？
- 提升安全系数
假如你是我厂运维，发现了一个安全隐患需要紧急修复。你肯定无法忍受给茫茫多的产品后端都发一封邮件，责令修复吧？万一漏了一个呢？

综合看来，SSO 不仅是有用的，而且是必要的。

## How to check Ubuntu version

lsb_release -a

lsb_release -ds

lsb_release -cs

s - short
d - description
c - code name

hostnamectl

cat /etc/os-release

## Check how long your system is supported

ubuntu-security-status

pro security-status (Ubuntu 22.04 jammy)

## What is LTS?

## How do I know if X11 is enabled? How do I know if XORG is working?

If you want to check for errors in X11, launch "xeyes", and a simple GUI should appear on the screen.

## What is the $DISPLAY environment variable?

The magic word in the X window system is DISPLAY. A display consists (simplified) of:

- a keyboard
- a mouse
- and a screen

A display is managed by a server program, known as an X server. The server serves displaying capabilities to other programs that connect to it.

The remote server knows where it has to redirect the X network traffic via the definition of the DISPLAY environment variable which generally points to an X Display server located on your local computer.

The value of the display environment variable is:

>hostname:D.S

where:

hostname is the name of the computer where the X server runs. An omitted hostname means the localhost.

**D** is a sequence number (usually 0). It can be varied if there are multiple displays connected to one computer.

**S** is the screen number. A display can actually have multiple screens. Usually, there's only one screen though where 0 is the default.

Example of values

>localhost:4
>google.com:0
>:0.0

`hostname:D.S` means screen S on display D of host hostname; the X server for this display is listening at TCP port 6000+D.

`host/unix:D.S` means screen S on display D of host host; the X server for this display is listening at UNIX domain socket /tmp/.X11-unix/XD (so it's only reachable from host).

`:D.S` is equivalent to host/unix:D.S, where host is the local hostname.

**:0.0 means that we are talking about the first screen attached to your first display in your local host**

Read more [here: support.objectplanet.com](http://support.objectplanet.com/esupport/index.php?_m=knowledgebase&_a=viewarticle&kbarticleid=17) and [here: superuser.com](https://superuser.com/questions/368530/understanding-x-windows-display-environment-variable-when-tunnelling?_gl=1*bxrxcx*_ga*MTgwNzczOTIxMS4xNjgxNzE0Nzg1*_ga_S812YQPLT2*MTY4MTcxNDc4NC4xLjAuMTY4MTcxNDc4Ny4wLjAuMA..) and [here: docstore.mik.ua](http://docstore.mik.ua/orelly/unix3/upt/ch35_08.htm).

From a X(7) man page:

>From the user's perspective, every X server has a display name of the form:
>
>hostname:displaynumber.screennumber
>
>This information is used by the application to determine how it should connect to the server and which screen it should use by default (on displays with multiple monitors):
>
>hostname The hostname specifies the name of the machine to which the display is physically connected. If the hostname is not given, the most efficient way of communicating to a server on the same machine will be used. displaynumber The phrase "display" is usually used to refer to a collection of monitors that share a common keyboard and pointer (mouse, tablet, etc.). Most workstations tend to only have one keyboard, and therefore, only one display. Larger, multi-user systems, however, frequently have several displays so that more than one person can be doing graphics work at once. To avoid confusion, each display on a machine is assigned a display number (beginning at 0) when the X server for that display is started. The display number must always be given in a display name. screennumber Some displays share a single keyboard and pointer among two or more monitors. Since each monitor has its own set of windows, each screen is assigned a screen number (beginning at 0) when the X server for that display is started. If the screen number is not given, screen 0 will be used.

下面详述一下各种场景下的 SSO，它们之间是逐步升级，逐步复杂化的关系。

1. 同域 SSO如图，同域 SSO 是最简单的一种情况。此时，两个产品都是在一个域名下，单点登录是很自然的选择。我们来捋一捋步骤，搞清楚这里的步骤是理解后文的基础，千万不要跳过。

    1. 用户访问产品 a，向 后台服务器发送登录请求。
    2. 登录认证成功，服务器把用户的登录信息写入 session。
    3. 服务器为该用户生成一个 cookie，并加入到 response header 中，随着请求返回而写入浏览器。该 cookie 的域设定为 dxy.cn。
    4. 下一次，当用户访问同域名的产品 b 时，由于 a 和 b 在同一域名下，也是 dxy.cn，浏览器会自动带上之前的 cookie。此时后台服务器就可以通过该 cookie 来验证登录状态了。

实际上，这种场景就是最简单最传统的登录操作。虽然我们把产品 a 和 b 人为分开了，但由于它们在同域上，就算看成是同一产品的不同类目也未尝不可。我们没有设置独立的 SSO 服务器，因为业务后台服务器本身就足以承担 SSO 的职能。

## What is HWE

## What is Ubuntu Core

Ubuntu Core is a stripped-down version of Linux intended for embedded device projects like an Internet of Things device or a smart home project. While Ubuntu Core can have server functionality, it isn't a server in the same way as a machine running Ubuntu Server is. You should avoid using Ubuntu Core on a server or desktop machine as Ubuntu Server and Ubuntu Desktop are better options.

[Ubuntu Core](https://youtu.be/6NDWqH1SrGs)

[参考原文2](https://towardsdatascience.com/a-guide-to-unicode-utf-8-and-strings-in-python-757a232db95c)

[参考原文3](https://www.iteye.com/blog/mikzhang-1101705)
