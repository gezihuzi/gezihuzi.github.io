---
layout: post
title:  "Apple TV 设置代理"
date:   2023-02-17 21:00:00 +0800
---

最近新购入Apple TV, 需要配置代理服务使用Netflix 和Youtube 服务, 所以写下这篇文章记录我配置代理的过程和遇到的问题。

Apple TV 的网络设置中, 是无法直接设置代理服务器的, 我们需要通过Mac 版本的Apple Configurator 软件来设置。

我这边使用使用Surge 作为代理软件, iPad 作为我的代理服务器。Surge 可以替换成任意的主流代理软件, 例如ClashX, 代理软件需要开启局域网代理服务。并使用Apple Configurator 来创建和分享代理配置至Apple TV。

> macOS 12.6.2
> Surge v5.3.0
> Apple Configurator v2.16

## 开启并查看代理配置

![Surge settings](/assets/img/apple-tv-surge-settings.png)

如图所示, 当前本地服务的代理服务地址和端口信息, 记录以下信息, 配置时会使用到。

代理地址: `192.168.31.114`
代理端口: `6152`

每个代理软件的配置会有所区别, 请查看相关软件的配置, 并确保代理软件开启`局域网代理` 服务。

## 创建WiFi 和代理配置

打开Apple Configurator, 在菜单栏中打开文件-新建描述文件。

![New profile](/assets/img/apple-tv-profile-step0.png)

1. 键入配置文件名称, 建议选择简单易懂的名称，例如`WiFi-Proxy`, 这将展示在Apple TV 中。
2. 选择侧边导航`WiFi` 栏并添加配置信息。

![Step 1](/assets/img/apple-tv-profile-step1.png)

点击配置开始

![Step 2](/assets/img/apple-tv-profile-step2.png)

以下是示例信息, 请根据实机的情况进行配置。

1. Wi-Fi SSID: `HUAWEI-G5WW3D_HiLink`
2. 代理设置: `手动`
3. 代理服务器和端口: `192.168.31.114` 和`6152`
4. 安全性类型: `WPA/WPA2 个人级`
5. 密码: `password1234`

![Step 3](/assets/img/apple-tv-profile-step3.png)

打开菜单栏, 文件-存储, 来保存当前的配置文件。

![Step 4](/assets/img/apple-tv-profile-step4.png)

## 连接Apple TV

请确保当前设备在同一个网络中, 我使用的是带网口的版本,可以直接连接网线。如果你是无网口的版本, 建议使用非上面配置中的已使用的Wi-Fi进行。也可以使用手机热点或者其他的Wi-Fi。

进入Apple TV, 打开设置-遥控器与设备-"遥控器"App与设备。停留在此页面并等待配对连接。

回到Apple Configurator, 打开菜单栏-配对的设备...

![Step 5](/assets/img/apple-tv-profile-step5.png)

选择附近的Apple TV 进行配对连接

![Step 6](/assets/img/apple-tv-profile-step6.png)

输入Apple TV 上的认证码进行连接

![Step 7](/assets/img/apple-tv-profile-step7.png)

配对连接成功

![Step 8](/assets/img/apple-tv-profile-step8.png)

关闭此窗口并回到首页

![Step 9](/assets/img/apple-tv-profile-step9.png)

点击首页的设备进入配置, 选择侧边栏-描述文件, 并拖入刚才保存的配置文件

![Step 10](/assets/img/apple-tv-profile-step10.png)

传输配置文件后并在TV 端安装配置文件后重启TV 即可, TV 会自动连接刚才配置的Wi-Fi 和代理。
