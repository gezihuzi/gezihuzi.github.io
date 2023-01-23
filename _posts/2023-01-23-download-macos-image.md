---
layout: post
title:  "下载完整的macOS 系统镜像"
date:   2023-01-23 13:00:00 +0800
---
最近为我的15 款的Mac 更换新的固态硬盘。需要备份系统以便更换完直接恢复。

制作U盘启动工具执行一些恢复操作, 所以需要下载完整的`macOS` 系统镜像。但是`App Store` 的下载速度过慢且十分的不稳定, 这里我们将使用`macOS` 自带的终端、浏览器和第三方下载工具下载完整的`macOS` 系统镜像

> macOS 12.6.2
> 终端 2.12.7
> Folx 5.27.1

## 监听系统安装日志

我们通过终端执行以下命令, 监听系统下载和安装时的日志信息:

```bash
tail -f /var/log/install.log | grep .pkg
```

保持终端运行, 我们执行后续操作

## 获取并下载

这里以`macOS Monterey`为例, 在`App Store` 中搜索下载或者 直接[下载macOS Monterey](https://apps.apple.com/cn/app/macos-monterey/id1576738294)进入并下载安装你需要下载的系统镜像。

> 如果你无法通过`App Store` 的内置搜索到你想要下载的系统, 你可以直接通过搜索引擎, 并加上关键字, 如: `macOS Monterey App Store`, 则可以直接获取对应系统的`App Store` 的链接。

![macOS Monterey](/assets/img/macos-monterey.png)

点击「获取」后跳转至系统设置页面, 并弹窗提示是否需要下载当前的系统:

![macOS download alert](/assets/img/macos-download-alert.png)

点击下载后, 获取到下载信息后可以直接取消下载, 监听中的日志会显示我们当前获取到的下载信息:

```bash
2023-01-23 14:27:14+08 MacBookPro15 softwareupdated[316]: Remaining packages to install for product 032-12843 : com.apple.pkg.InstallAssistant.macOSMonterey
	        SUDistributionPackageURLString = "https://swdist.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg";
	        SUIntegrityDataURL = "https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistan.pkg.integrityDataV1";
	    Identifier = "com.apple.pkg.InstallAssistant.macOSMonterey";
	    URL = "https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg";
	} IntegrityInformation:SUPackageIntegrityInformation(0x7faee45ffdb0): URL:https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg.integrityDataV1 Size:42656 Checksum:(null)
	        PKDownloaderAdditionalReferenceOptionChunklistURL = "https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg.integrityDataV1";
	        SUDistributionPackageURLString = "https://swdist.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg";
	        SUIntegrityDataURL = "https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistan.pkg.integrityDataV1";
	    Identifier = "com.apple.pkg.InstallAssistant.macOSMonterey";
	    URL = "https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg";
	      destination path: /var/folders/zz/zyxvpxvq6csfxvn_n00000s0000068/C/com.apple.SoftwareUpdate/swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg
2023-01-23 14:27:14+08 MacBookPro15 softwareupdated[316]: 032-12843: Starting download of InstallAssistant.pkg (Size:12395078361, HasIntegrityInformation:YES) with URL https://swcdn.apple.com/content/downloads/55/35/032-12843-A_35UCTHBNLI/z1twfbx3mhypq6g7ljtz52z6nsv0n7e4o5/InstallAssistant.pkg
```

日志中包含的`URL=...` 部分就是我们需要的镜像下载链接, 直接通过`Folx` 进行下载:

![macOS downloading](/assets/img/macos-downloading.png)

## 清理

回到终端, `Ctrl + C` 结束当前的监听任务, 等待下载结束即可
