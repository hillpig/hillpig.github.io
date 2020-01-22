---
layout: post
title: Android 友盟多渠道打包
date: 2020-01-22
Author: 山猪
tags: [Android]
comments: true
---
![img](https://www.ritavpn.com/blog/wp-content/uploads/2019/12/Best-APK-Download-Sites-for-2020.png)

<!-- more -->

1. 修改AndroidManifest.xml文件，添加渠道名称变量

    ```xml
            <!--value的值填写渠道名称，例如yingyongbao。这里设置动态渠道名称变量-->
            <meta-data android:value="${UMENG_CHANNEL_VALUE}" android:name="UMENG_CHANNEL"/>
    ```

2. 修改app的build.gradle文件，添加所需要的渠道

    ```java
        /*配置渠道*/
        productFlavors {
            yingyongbao {
                manifestPlaceholders = [UMENG_CHANNEL_VALUE: "yingyongbao"]
            }
            wandoujia {
                manifestPlaceholders = [UMENG_CHANNEL_VALUE: "wandoujia"]
            }
            xiaomi {
                manifestPlaceholders = [UMENG_CHANNEL_VALUE: "xiaomi"]
            }
        }
    ```
3. 如果遇到一下错误，可以参考google文档修改解决

    ```bash
        Error:All flavors must now belong to a named flavor dimension.
        The flavor 'flavor_name' is not assigned to a flavor dimension.
    ```
    在build.gradle文件的defaultConfig段落中添加

    ```java
        flavorDimensions "versionCode"
    ```


    [参考链接1](https://developer.android.com/studio/build/build-variants?utm_source=android-studio#product-flavors "Google's document")  

    [参考链接2](https://www.cnblogs.com/WUXIAOCHANG/p/10683942.html "cnblogs's document")  
    
    [参考链接3](https://www.jianshu.com/p/e4da2f477cd8 "jianshu's document")