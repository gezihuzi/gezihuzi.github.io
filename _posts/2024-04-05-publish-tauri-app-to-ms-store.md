---
layout: post
title:  "Publish Tauri app to Microsoft Store"
date:   2024-04-05 21:00:00 +0800
---

Recently, the app developed using the Tauri framework has been completed and needs to be published to the Microsoft Store, so I am writing this article to document the process of my publication and the issues encountered.

## Should I sign the package or not?

![Step-7](/assets/img/publish-tauri-app-to-ms-store-step7-en.png)

There are currently two options for creating products in the Microsoft Store:

1. `MSIX` or `PWA`. `MSIX` supports uploading software packages without requiring signing, as Microsoft Store will automatically sign them.
2. For `EXE` or `MSIX`, a separate certificate purchase is required for signing.

If you are only publishing to the Microsoft Store, you can choose the first option and submit an unsigned software package created using the `MSIX` packaging tool.

## Package the application

First, we need to package our application using the packaging tool provided by `Tauri`. You can find a tutorial on how to package in the [Tauri official documentation - Application Publishing](https://tauri.app/v1/guides/distribution/publishing/).

Here is the command I used:

```bash
yarn tauri build
```

After the packaging is completed, you will find the packaged application in the `/target/release/bundle` directory. We will be using `*.msi` for the next steps.

## Convert installer format

We use the official tool provided by Microsoft to convert existing `*.msi` installation programs to `*.msix` format for publishing to the Microsoft Store. This is a necessary step because the Microsoft Store only accepts applications in `*.msix` format. The official documentation can be found [here](https://learn.microsoft.com/en-us/windows/MSIX/packaging-tool/create-an-MSIX-overview).

### Download tools

Before converting the application format, you need to download and install the `MSIX Packaging Tool`. You can download the `MSIX Packaging Tool` from [Microsoft Store](https://www.microsoft.com/p/MSIX-packaging-tool/9n5lw3jbcxkf). Related documentation: [MSIX Packaging Tool](https://learn.microsoft.com/en-us/windows/MSIX/packaging-tool/tool-overview).

### Convert installer

Open the `MSIX Packaging Tool`, click on **Application package - Create your package**.

![Step 1](/assets/img/publish-tauri-app-to-ms-store-step1-en.png)

Select the environment in which you want to package, I am running on a local computer, so choose to **Create package on this computer**.

![Step 2](/assets//img/publish-tauri-app-to-ms-store-step2-en.png)

Select your application installer, click **Next**.

![Step 3](/assets/img/publish-tauri-app-to-ms-store-step3-en.png)

Confirm the need to install the 'MSIX Packaging Tool Driver', click **Next**.

![Step 4](/assets/img/publish-tauri-app-to-ms-store-step4-en.png)

Select the just packaged `*.msi` application installer, fill in the installation program parameters according to the actual situation, select **Do not sign the package** as the signing preference option, and then click **Next**.

![Step 5-1](/assets/img/publish-tauri-app-to-ms-store-step5-1-en.png)

You can obtain this information through Microsoft Partner Center - Apps and Games - Applications - Product Management - Identity.

- Package/Identity/Name
- Package/Identity/Publisher
- Package/Properties/PublisherDisplayName

![Step 5-2](/assets/img/publish-tauri-app-to-ms-store-step5-2-en.png)

After obtaining it, follow the instructions to fill in the program package information. Please fill in this information correctly, otherwise an error will be prompted when uploading to the Microsoft Store, then click **Next**.

![Step 6](/assets/img/publish-tauri-app-to-ms-store-step6-en.png)

If there is no accelerator, please click **Next** directly.

Follow the prompts for the subsequent steps, and finally click **Create**. The application will be automatically installed on the current computer. Follow the installation instructions, and after restarting, the application will open automatically. Once packaging is complete, a report and `*.msix` file will be generated. Submit this file to Microsoft Store for review.
