---
layout: post
title:  "Tauri MSIX 开机启动适配"
date:   2024-04-05 21:00:00 +0800
---

最近将基于`Tauri v2` 开发的产品上线微软商店后, 用户反馈开机启动不生效, 这里记录一下复现和解决方案。

## 复现问题

我们在不同环境测试开机启动都是正常的, 唯独在转换成`MSIX` 之后, 安装需要完整签名, 无法进行完整测试。猜测应该和`MSIX` 有关。

于是我们尝试了以下步骤:

1. 创建自签名证书;
2. 签名并打包成`MSIX`;
3. 安装到系统中。

测试结果发现, 确实在`MSIX` 中, 开机启动不生效的问题复现了。

## 查找问题

在`Tauri` 项目中, 使用`tauri-plugin-startup` 插件实现开机启动, 但是在转换成`MSIX` 之后, 开机启动并不生效失效了。

根据`tauri-plugin-startup` 内部使用`auto-launch` 实现开机启动的功能, `Windows` 平台特定的实现是通过`winreg` 库修改注册表实现开机启动。

以下是`auto-launch` 的`Windows` 平台实现:

```rust
use crate::{AutoLaunch, Result};
use winreg::enums::RegType::REG_BINARY;
use winreg::enums::{
    HKEY_CURRENT_USER, HKEY_LOCAL_MACHINE, KEY_ALL_ACCESS, KEY_READ, KEY_SET_VALUE,
};
use winreg::{RegKey, RegValue};


static ADMIN_AL_REGKEY: &str = "SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\Run";
static AL_REGKEY: &str = "SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run";
static ADMIN_TASK_MANAGER_OVERRIDE_REGKEY: &str =
    "SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\StartupApproved\\Run32";
static TASK_MANAGER_OVERRIDE_REGKEY: &str =
    "SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\StartupApproved\\Run";
```

permalink: [https://github.com/zzzgydi/auto-launch/blob/2d94a103ca20652a3baf581ca2c296791c35c09b/src/windows.rs#L1-L13](https://github.com/zzzgydi/auto-launch/blob/2d94a103ca20652a3baf581ca2c296791c35c09b/src/windows.rs#L1-L13)

但是在`MSIX` 中, 由于`灵活虚拟化`, 应用的注册表内容并不会真正写入到系统注册表中, 而是写入到虚拟注册表中, 所以开机启动不生效。

## 解决方案

目前有两种解决方案:

### 1. 取消灵活虚拟化

在`MSIX` 打包时, 可以选择取消`灵活虚拟化`, 这样应用的注册表内容会真正写入到系统注册表中, 从而实现开机启动。

### 2. 适配灵活虚拟化

如果需要保持`灵活虚拟化`并适配最新特性, 可以通过`MSIX` 的`StartupTask` 来实现开机启动。

1. 修改`auto-launch` 的实现并适配`MSIX` 的`StartupTask`。这个部分我已实现, 你可以查看这个[feature/msix](https://github.com/Hypobenthos/auto-launch-rs/tree/feature/msix)

2. 修改`tauri-plugin-startup` 的`auto-launch` 的依赖。这个部分我已实现, 你可以查看这个[feature/msix](https://github.com/Hypobenthos/plugins-workspace/tree/feature/msix)

3. 打包同时修改`MSIX` 清单文件。

```xml
<Package
  ...
 xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" 
 xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
 xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10" 
 xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
 xmlns:desktop7="http://schemas.microsoft.com/appx/manifest/desktop/windows10/7" 
 xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap uap10 desktop desktop7 rescap">
  ...
  <Applications>
    <Application Id="App" Executable="app.exe" EntryPoint="Windows.FullTrustApplication">
        ...
    </Application>
  </Applications>
  <Extensions>
    <desktop:Extension Category="windows.startupTask" Executable="VFS\ProgramFilesX64\App\app.exe" EntryPoint="Windows.FullTrustApplication">
          <desktop:StartupTask TaskId="AppStartupTaskId" Enabled="true" DisplayName="App" />
    </desktop:Extension>
  </Extensions>
</Package>
```

重新签名打包并安装测试, 开机启动就可以正常工作了。

---

## 参考和引用

- [为包签名创建证书](https://learn.microsoft.com/zh-cn/windows/msix/package/create-certificate-package-signing)
- [灵活虚拟化](https://learn.microsoft.com/zh-cn/windows/msix/desktop/flexible-virtualization)
- [tauri-plugin-startup](https://github.com/tauri-apps/tauri-plugin-autostart.git)
- [auto-launch](https://github.com/zzzgydi/auto-launch)
