---
layout: post
title: Security - Encryption - Signature or Key
date: 2020-06-10
Author: 山猪
tags: [IT]
comments: true
---
![img](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Illustration_of_digital_signature.svg/1024px-Illustration_of_digital_signature.svg.png)

<!-- more -->

1. A对信息签名的作用是确认这个信息是A发出的，不是别人发出的；
2. 加密是对内容进行机密性保护，主要是保证信息内容不会被其他人获取，只有B可以获取。

也就是保证整个过程的端到端的唯一确定性，这个信息是A发出的（不是别人），且是发给B的，只有B才被获得具体内容（别人就算截获信息也不能获得具体内容）。这只是大概说了作用，具体说来，涉及到密钥相关的东西。密钥有公钥和私钥之分。那么这里一共有两组四个密钥：A的公钥（PUB_A），A的私钥（PRI_A）；B的公钥（PUB_B），B的私钥（PRI_B）。公钥一般用来加密，私钥用来签名。通常公钥是公开出去的，但是私钥只能自己私密持有。公钥和私钥唯一对应，用某个公钥签名过得内容只能用对应的私钥才能解签验证；同样用某个私钥加密的内容只能用对应的公钥才能解密。

这时A向B发送信息的整个签名和加密的过程如下：
1. A先用自己的私钥（PRI_A）对信息（一般是信息的摘要）进行签名。
2. A接着使用B的公钥（PUB_B）对信息内容和签名信息进行加密。

这样当B接收到A的信息后，获取信息内容的步骤如下：
1. 用自己的私钥（PRI_B）解密A用B的公钥（PUB_B）加密的内容；
2. 得到解密后的明文后用A的公钥（PUB_A）解签A用A自己的私钥（PRI_A）的签名。

从而整个过程就保证了开始说的端到端的唯一确认。A的签名只有A的公钥才能解签，这样B就能确认这个信息是A发来的；A的加密只有B的私钥才能解密，这样A就能确认这份信息只能被B读取。

签名密钥和加密密钥
由于公钥所具有的两种不同用途,在实际应用中,需要分别配置用于数字签名/验证的
密钥对和用于数据加密/解密的密钥对,这里分别称为签名密钥对和加密密钥对｡这两对密
钥由于用途不同,因此,对于密钥的管理也就有着不同的要求｡

1. 签名密钥对的管理
签名密钥对由签名私钥和验证公钥组成｡
签名私钥是发送方身份的证明,具有日常生活中公章､私章的效力｡为保证其惟一性,
签名私钥绝对不能够做备份和存档,丢失后只需重新生成新的密钥对｡验证公钥需要存档,
用于验证旧的数字签名｡
用做数字签名的这一对密钥一般可以有较长的生命期｡

2. 加密密钥对的管理
加密密钥对由加密公钥和解密私钥组成｡
为防止密钥丢失时数据无法恢复,解密私钥应该进行备份,同时还可能需要进行存档,
以便能在任何时候解密历史密文数据｡加密公钥则无需备份和存档,加密公钥丢失时,只需
重新生成密钥对即可｡
这种密钥应该频繁更换,故加密密钥对的生命周期较短｡

不难看出,这两对密钥的密钥管理要求存在互相冲突的地方,因此,必须针对不同的用
途使用不同的密钥对｡尽管有的公钥体制算法如RSA,既可以用于加密,又可以用于签名,
但由于这两对密钥在管理上截然不同的要求,在使用中仍然必须为用户配置两对密钥:其一
用于数字签名,另一用于加密｡而采用一对密钥,既用于加密,又用于签名,这种做法是不安
全的｡


[参考 1](https://www.zhihu.com/question/27669212)

[参考 2](https://blog.csdn.net/gotohbu/java/article/details/4328066)