---
layout: post
title: ubuntu下查看log
date: 2020-01-03
Author: 山猪
tags: [Linux, ubuntu]
comments: true
---
![img](https://www.howtogeek.com/wp-content/uploads/2012/07/ximage8.png.pagespeed.gp+jp+jw+pj+ws+js+rj+rp+rw+ri+cp+md.ic.qSWlkYkoXy.png)

<!-- more -->

1. dmesg

```console
madoor@template:~$ dmesg | grep dmesg | less
```

2. cat /var/log/syslog

```console
madoor@template:~$ cat /var/log/syslog
madoor@template:~$ cat /var/log/mail.log
madoor@template:~$ cat /var/log/auth.log
madoor@template:~$ cat /var/log/faillog
```
