---
title: 适用于 Windows 上的 Android 开发的 PWA 方法
description: 开始在 Windows 上使用 PWA 方法开发 Android 应用。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows 上的 android，pwa，android，cordova，ionic，phonegap，混合 web 应用
ms.date: 04/28/2020
ms.openlocfilehash: c0ff9acf1d8e93e82f1db424d7a356c974988683
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255209"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>开始为 Android 开发 PWA 或混合 web 应用

本指南将帮助你开始在 Windows 上使用可在 web 上和跨设备平台（Android、iOS、Windows）使用的单个 HTML/CSS/JavaScript 基本代码创建混合 web 应用或渐进式 Web 应用（PWA）。

通过使用正确的框架和组件，基于 web 的应用程序可以在 Android 设备上运行，这种方式与用户非常类似于本机应用程序。

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>PWA 或混合 web 应用的功能

Android 设备上可以安装两种主要类型的 web 应用。 主要区别在于应用程序代码是嵌入在应用包（混合）中还是在 web 服务器（pwa）上承载。

- **混合 web 应用**：代码（HTML，JS，CSS）打包在 APK 中，可以通过 Google Play 商店分发。 查看引擎与用户的 internet 浏览器隔离，无会话或缓存共享。

- **渐进式 Web 应用（pwa）**：代码（HTML、JS、CSS）位于 Web 上，不需要打包为 APK。 资源根据需要使用服务辅助角色进行下载和更新。 Chrome 浏览器将呈现并显示您的应用程序，但它将以本机形式显示，而不包含正常的浏览器地址栏等。可以与浏览器共享存储、缓存和会话。 这基本上类似于在特殊模式下安装 Chrome 浏览器的快捷方式。 还可使用受信任的 Web 活动 Google Play 商店中列出 Pwa。

Pwa 和混合 web 应用非常类似于本机 Android 应用，因为它们：

- 可以通过应用商店（Google Play 商店和/或 Microsoft Store）进行安装
- 有权访问相机、GPS、蓝牙、通知和联系人列表等本机设备功能
- 脱机工作（无 internet 连接）

Pwa 还具有几个独特的功能：

- 可以直接从 web 安装在 Android 主屏幕上（无需应用商店）
- 还可以通过[使用受信任的 Web 活动](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/)的 Google Play 商店进行安装
- 可以通过 web 搜索或通过 URL 链接来发现
- 依赖[服务辅助角色](https://developers.google.com/web/fundamentals/primers/service-workers)，以避免需要打包本机代码

不需要使用框架来创建混合应用或 PWA，但本指南中会涵盖几个常用框架，其中包括 PhoneGap （带有 Cordova）和 Ionic （使用带有角度或反应的 Cordova 或电容器）。

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/)是一种开源框架，它可以通过使用[插件](https://cordova.apache.org/plugins/?platforms=cordova-android)来简化在本机[Web 视图](https://developer.android.com/reference/android/webkit/WebView)中生活的 JavaScript 代码和本机 Android 平台之间的通信。 这些插件公开 JavaScript 终结点，这些终结点可从你的代码调用并用于调用本机 Android 设备 Api。 一些示例 Cordova 插件包括对设备服务（如电池状态、文件访问、振动/铃声等）的访问。这些功能通常不适用于 web 应用或浏览器。

Cordova 有两个常用的分发：

- [PhoneGap](https://phonegap.com/)

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

[PhoneGap](https://phonegap.com/)： Adobe 支持 Cordova 的一种框架，它支持具有附加工具的 Cordova，如[命令行](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/)、[桌面应用程序](https://phonegap.com/products#desktop-app-section)和[PhoneGap 生成](https://build.phonegap.com/)，这是一种服务，它使你能够将代码上传到将生成本机应用的 Adobe server，而无需在本地计算机上安装本机 sdk。 这使你可以执行一些操作，如使用 Windows 计算机生成 iOS 应用。

### <a name="install-phonegap"></a>安装 PhoneGap

若要开始使用 PhoneGap 构建 PWA 或混合 web 应用，应该首先安装以下工具：

- 与 Ionic 生态系统交互的 node.js。 [下载适用于 windows 的 NodeJS](https://nodejs.org/en/) ，或遵循使用适用于 Linux 的 Windows 子系统（WSL）的[NodeJS 安装指南](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2)。 如果要使用多个项目和 NodeJS 版本，则可能需要考虑使用[节点版本管理器（nvm）](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) 。

在命令行中输入以下命令以安装 PhoneGap：

```bash
npm install -g phonegap
```

若要创建新的 PhoneGap 项目，请按照其步骤[开始](https://phonegap.com/getstarted/)操作。 若要了解如何将应用从混合到 PWA，请访问 PhoneGap 文档的[PWA 功能](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/)部分。  

## <a name="ionic"></a>Ionic

[Ionic](https://ionicframework.com/)是一个框架，用于调整应用程序的用户界面（UI），以匹配每个平台（Android、IOS、Windows）的设计语言。 Ionic 使你能够使用[角度](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)或[反应](https://ionicframework.com/react)。

> [!NOTE]
> 有一个新版本的 Ionic 使用了 Cordova 的替代方法，称为 "[电容器](https://capacitor.ionicframework.com/)"。 这种替代方法使用容器来使应用[更易于 web](https://ionicframework.com/blog/announcing-capacitor-1-0/)使用。

### <a name="get-started-with-ionic-by-installing-required-tools"></a>通过安装必需的工具开始使用 Ionic

若要开始使用 Ionic 构建 PWA 或混合 web 应用，应该首先安装以下工具：

- 与 Ionic 生态系统交互的 node.js。 [下载适用于 windows 的 NodeJS](https://nodejs.org/en/) ，或遵循使用适用于 Linux 的 Windows 子系统（WSL）的[NodeJS 安装指南](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2)。 如果要使用多个项目和 NodeJS 版本，则可能需要考虑使用[节点版本管理器（nvm）](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) 。

- 用于编写代码的 VS Code。 [下载适用于 Windows 的 VS Code](https://code.visualstudio.com/)。 如果希望使用 Linux 命令行生成应用程序，则可能还需要安装[WSL 远程扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。

- 用于使用首选命令行接口（CLI）的 Windows 终端。 [从 Microsoft Store 安装 Windows 终端](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)。

- 用于版本控制的 Git。 [下载 Git](https://git-scm.com/downloads)。

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>使用 Ionic Cordova 和角度创建新项目

在命令行中输入以下命令，安装 Ionic 和 Cordova：

```bash
npm install -g @ionic/cli cordova
```

通过输入以下命令，使用 "Tabs" 应用模板创建 Ionic 角应用：

```bash
ionic start photo-gallery tabs
```

更改为应用程序文件夹：

```bash
cd photo-gallery
```

在 web 浏览器中运行应用程序：

```bash
ionic serve
```

有关详细信息，请参阅[Ionic Cordova 角度文档](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)。若要了解如何将应用从混合到 PWA，请访问 Ionic 文档的 "创建边缘的[PWA](https://ionicframework.com/docs/angular/pwa) " 部分。

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>使用 Ionic 电容器和角度创建新项目

在命令行中输入以下命令，安装 Ionic 和 Cordova-Res：

```bash
npm install -g @ionic/cli native-run cordova-res
```

通过输入以下命令，使用 "Tabs" 应用模板创建 Ionic 角应用并添加电容器：

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

更改为应用程序文件夹：

```bash
cd photo-gallery
```

添加组件以使应用成为 PWA：

```bash
npm install @ionic/pwa-elements
```

导@ionic/pwa-elements入，方法是将以下`src/main.ts`内容添加到文件：

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

在 web 浏览器中运行应用程序：

```bash
ionic serve
```

有关详细信息，请参阅[Ionic 电容器角文档](https://ionicframework.com/docs/angular/your-first-app)。若要了解如何将应用从混合到 PWA，请访问 Ionic 文档的 "创建边缘的[PWA](https://ionicframework.com/docs/angular/pwa) " 部分。  

## <a name="create-a-new-project-with-ionic-and-react"></a>使用 Ionic 和反应创建新项目

在命令行中输入以下命令，安装 Ionic CLI：

```bash
npm install -g @ionic/cli
```

通过输入以下命令创建具有反应的新项目：

```bash
ionic start myApp blank --type=react
```

更改为应用程序文件夹：

```bash
cd myApp
```

在 web 浏览器中运行应用程序：

```bash
ionic serve
```

有关详细信息，请参阅[Ionic 的响应文档](https://ionicframework.com/docs/react/quickstart)。若要了解如何将应用从混合式应用到 PWA，请访问 Ionic 文档的[PWA](https://ionicframework.com/docs/react/pwa)部分。

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>在设备或仿真程序上测试 Ionic 应用

若要在 Android 设备上测试 Ionic 应用，请在设备上插入设备（[确保它首先启用了开发](emulator.md#enable-your-device-for-development)），然后在命令行中输入：

```bash
ionic cordova run android
```

若要在 Android 设备模拟器上测试 Ionic 应用，必须执行以下操作：

1. [安装所需的组件--Java 开发工具包（JDK）、Gradle 和 Android SDK](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements)。

2. [创建 Android 虚拟设备（AVD）](https://developer.android.com/studio/run/managing-avds.html)。

3. 输入 Ionic 的命令，以生成应用并将其部署到模拟器： `ionic cordova emulate [<platform>] [options]`。 在这种情况下，该命令应为：

```bash
ionic cordova emulate android --list
```

有关详细信息，请参阅 Ionic 文档中的[Cordova 模拟器](https://ionicframework.com/docs/cli/commands/cordova-emulate)。

## <a name="additional-resources"></a>其他资源

- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)

- [添加 Windows Defender 排除项以提高性能](defender-settings.md)

- [启用虚拟化支持以提高模拟器性能](emulator.md#enable-virtualization-support)
