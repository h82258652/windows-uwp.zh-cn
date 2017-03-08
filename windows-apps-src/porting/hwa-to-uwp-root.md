---
author: seksenov
title: "托管 Web 应用 - 将 Web 应用程序转换为通用 Windows 平台 (UWP) 应用并访问本机 Windows 10 功能"
description: "查找将 Web 应用转换为适用于 Windows 应用商店的通用 Windows 平台 (UWP) 应用的资源。"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "托管 Web 应用, 将网站转换为 Windows 应用, Windows 应用商店上的 Web 应用, 适用于 Windows 的 Chrome 应用"
translationtype: Human Translation
ms.sourcegitcommit: 2e230e95be01f0b14fa6346be9fa836c66a812cf
ms.openlocfilehash: c9239f3a3c14bf9da99e60cfef8154eefb4305b4
ms.lasthandoff: 01/20/2017

---

# <a name="hosted-web-apps---access-windows-10-features-from-your-web-app"></a>托管 Web 应用 - 从 Web 应用访问 Windows 10 功能

你的 Web 应用程序可以具有对通用 Windows 平台 (UWP) 的完全访问权限，包括从托管在服务器上的脚本直接调用 Windows 运行时 API、利用 Cortana 集成，以及使用联机验证提供程序。 混合应用也受支持，因为你可以包含可从托管脚本调用的本地代码，并在远程页面和本地页面之间管理应用导航。

## <a name="get-started"></a>入门

无论你使用的是 Mac 还是 PC，你都可以在几分钟内创建托管 Web 应用。 入门的最佳方式是使用功能齐全的免费 [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/)，当你使用 Windows 设备时尤其有效。 如果你无法访问 Visual Studio，有一些选项可供你选择。 如果你熟悉命令行接口 (CLI) 实用程序，请查看 [ManifoldJS](http://manifoldjs.com/)。 你还可以使用 [App Studio](http://appstudio.windows.com/)，它是一款无需编码的免费联机创建工具，可使你快速生成 Windows 10 应用。

- [将 Web 应用程序转换为使用 Windows 的通用 Windows 平台 (UWP) 应用的分步说明](hwa-create-windows.md)

- [使用 Mac 将 Web 应用程序转换为通用 Windows 平台 (UWP) 应用的分步说明](hwa-create-mac.md)

## <a name="enhance-your-app"></a>增强应用

- 使用直接从 JavaScript 调用的[本机 Windows 运行时功能](hwa-access-features.md)让应用绽放光彩。

- 通过使用我们的内容安全策略 (CSP) 模型设置应用程序内容 URI 规则 (ACUR) 来使你的应用保持安全。

- 通过集成 Cortana 语音命令来使用语音功能运行你的应用。

- 通过声明应用功能来授予对用户资源和设备功能的编程访问权限。

- 为你的用户创建简单流畅的登录流程，方法是通过 OpenID 和 OAuth 验证其身份。

- 不再需要在封装和托管 Web 应用之间进行抉择。 通过创建混合应用，可实现二者兼而有之。

## <a name="convert-your-existing-chrome-app"></a>转换你的现有 Chrome 应用

我们使[将现有 Chrome 托管应用](hwa-chrome-conversion.md)转换为 Windows 托管 Web 应用变得更轻松。 [ManifoldJS](http://manifoldjs.com/) 现在接受 Chrome 清单作为一种输入形式。 我们还开发了 [CLI 工具](https://github.com/MicrosoftEdge/hwa-cli)，该工具可从现有的 `.zip` 或 `.crx` 文件生成 `.appx` 程序包。

## <a name="demos"></a>演示

- [Contoso 旅行应用](http://contosotravel.azurewebsites.net/)


