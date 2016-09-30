---
title: "托管 Web 应用 - 将 Web 应用程序转换为通用 Windows 平台 (UWP) 应用并访问本机 Windows 10 功能"
description: "从你的网站 URL 创建通用 Windows 平台 (UWP) 应用。 从 Web 应用内的代码访问 Windows 10 本地设备功能。 适用于托管 Web 应用的 Microsoft Windows 桥（以前称为 Project Westminster）可使你快速轻松地将 Web 应用包含在 Windows 应用商店中。"
author: seksenov
translationtype: Human Translation
ms.sourcegitcommit: 765789ef83f9b6a845ab79505b1b9ecbfd987126
ms.openlocfilehash: 491665558f713dcbaae7ea20739ed72c61a12cd2

---

# 托管 Web 应用 - 从 Web 应用访问 Windows 10 功能

你的 Web 应用程序可以具有对通用 Windows 平台 (UWP) 的完全访问权限，包括从托管在服务器上的脚本直接调用 Windows 运行时 API、利用 Cortana 集成，以及使用联机验证提供程序。 混合应用也受支持，因为你可以包含可从托管脚本调用的本地代码，并在远程页面和本地页面之间管理应用导航。

## 入门

无论你使用的是 Mac 还是 PC，你都可以在几分钟内创建你自己的托管 Web 应用。 入门的最佳方式是使用功能齐全的免费 [Visual Studio Community 2015](https://www.visualstudio.com/)，当你使用 Windows 设备时尤其有效。 如果你无法访问 Visual Studio，有一些选项可供你选择。 如果你熟悉命令行接口 (CLI) 实用程序，请查看 [ManifoldJS](http://manifoldjs.com/)。 你还可以使用 [App Studio](http://appstudio.windows.com/)，它是一款无需编码的免费联机创建工具，可使你快速生成 Windows 10 应用。

- [将 Web 应用程序转换为使用 Windows 的通用 Windows 平台 (UWP) 应用的分步指南](hwa-create-windows.md)

- [使用 Mac 将 Web 应用程序转换为通用 Windows 平台 (UWP) 应用的分步说明](hwa-create-mac.md)

## 增强应用

- 通过从 Windows 运行时使用 JavaScript [访问本机 Windows 功能](hwa-access-features.md)来使你的应用夺人眼目。

- 通过使用我们的内容安全策略 (CSP) 模型设置应用程序内容 URI 规则 (ACUR) 来使你的应用保持安全。
- 通过与 Cortana 语音命令集成来使用语音功能运行你的应用。

- 通过声明应用功能来授予对用户资源和设备功能的编程访问权限。

- 为你的用户创建简单流畅的登录流程，方法是通过 OpenID 和 OAuth 验证其身份。

- 不再需要在封装和托管 Web 应用之间进行抉择。 通过创建混合应用，可实现二者兼而有之。

## 转换你的现有 Chrome 应用

我们使[将现有 Chrome 托管应用](hwa-chrome-conversion.md)转换为 Windows 托管 Web 应用变得更轻松。 [ManifoldJS](http://manifoldjs.com/) 现在接受 Chrome 清单作为一种输入形式。 我们还开发了 [CLI 工具](https://github.com/MicrosoftEdge/hwa-cli)，该工具可从现有的 `.zip` 或 `.crx` 文件生成 `.appx` 程序包。

## 演示

- [Contoso 旅行应用](http://contosotravel.azurewebsites.net/)

- [Windows 运行时 API：JavaScript 代码示例](http://rjs.azurewebsites.net/)



<!--HONumber=Jul16_HO1-->


