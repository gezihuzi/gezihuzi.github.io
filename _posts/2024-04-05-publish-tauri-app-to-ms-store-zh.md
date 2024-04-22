---
layout: post
title:  "将 Tauri 应用发布到 Microsoft Store"
date:   2024-04-05 21:00:00 +0800
---

最近使用Tauri 框架开发的App 已经开发完成, 需要发布到Microsoft Store, 所以写下这篇文章记录我发布的过程和遇到的问题。

## 打包应用程序

首先, 我们需要打包我们的应用程序, 使用`Tauri` 提供的打包工具, 你可以在[Tauri 官方文档-应用发布](https://tauri.app/v1/guides/distribution/publishing/) 找到如何打包的教程。

以下是我使用的命令:

```bash
yarn tauri build
```

在打包完成后, 你会在`/target/release/bundle` 目录下找到打包好的应用程序。我们使用`*.msi` 进行接下来的步骤。

## 转换安装程序格式

我们使用微软提供的官方工具, 将已有的`*.msi` 安装程序转换成`*.msix` 格式, 以便发布到Microsoft Store。这是一个必须的步骤, 因为Microsoft Store 只接受`*.msix` 格式的应用程序。官方文档可以在[这里](https://learn.microsoft.com/en-us/windows/MSIX/packaging-tool/create-an-MSIX-overview) 找到。

### 下载工具

转换应用格式之前, 你需要先下载和安装`MSIX Packaging Tool`, 你可以在[Microsoft Store](https://www.microsoft.com/p/MSIX-packaging-tool/9n5lw3jbcxkf) 下载`MSIX Packaging Tool`。相关说明文档: [MSIX Packaging Tool](https://learn.microsoft.com/en-us/windows/MSIX/packaging-tool/tool-overview)。

### 转换安装程序

打开`MSIX Packaging Tool`, 点击**应用程序包-创建你的应用包**。

![Step 1](/assets/img/publish-tauri-app-to-ms-store-step1.png)

选择想要在其中进行打包的环境, 我在本地计算机中运行, 所以选择在**此计算机中创建程序包**。

![Step 2](/assets//img/publish-tauri-app-to-ms-store-step2.png)

选择你的应用程序安装程序, 点击**下一步**。

![Step 3](/assets/img/publish-tauri-app-to-ms-store-step3.png)

确认需要安装的`MSIX 打包工具驱动程序`, 点击**下一步**。

![Step 4](/assets/img/publish-tauri-app-to-ms-store-step4.png)

选择刚刚打包好的`*.msi`应用安装程序, 安装程序参数根据实际情况进行填写, 签名首选项选择**不对程序包进行签名**, 然后点击**下一步**。

![Step 5-1](/assets/img/publish-tauri-app-to-ms-store-step5-1.png)

你可以通过Microsoft 合作伙伴中心-应用和游戏-应用程序-产品管理-产品标识中获取到这些信息。

- 包/标识/名称
- 包/标识/发行商
- 包/属性/发布者显示名称

![Step 5-2](/assets/img/publish-tauri-app-to-ms-store-step5-2.png)

获取之后按照说明, 填入程序包信息, 请正确填写这些信息, 否则微软商店上传时会提示错误, 然后点击**下一步**。

![Step 6](/assets/img/publish-tauri-app-to-ms-store-step6.png)

如没有加速器, 请直接点击**下一步**。

后续的步骤按照提示进行操作, 最后点击**创建**。应用会自动安装至当前计算机, 按照操作提示进行安装, 重启后, 应用会自动打开, 打包完成会生成一个报告和`*.msix`文件。将此文件提交至Microsoft Store 进行审核即可。

## 签名应用包

![Step-7](/assets/img/publish-tauri-app-to-ms-store-step7.png)

上述的步骤中, 我们选择了不对程序包进行签名, 目前在Microsoft Store中创建产品有两种选择:

1. `MSIX` 或`PWA`, `MSIX` 支持上传无需签名的软件包, 因为Microsoft Store 会自动对其进行签名。
2. `EXE` 或`MSIX` 则需要单独购买证书以进行签名。

如果您只是发布到Microsoft Store, 您可以选择第一种选项, 并提交使用`MSIX` 打包工具创建的未签名软件包。
