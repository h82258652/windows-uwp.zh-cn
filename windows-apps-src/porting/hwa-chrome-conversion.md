---
author: seksenov
title: "托管 Web 应用 - 将 Chrome 应用转换为通用 Windows 平台应用"
description: "将 Chrome 应用或 Chrome 扩展转换为适用于 Windows 应用商店的通用 Windows 平台 (UWP) 应用。"
kw: Package Chrome Extension for Windows Store tutorial, Port Chrome Extension to Windows 10, How to convert Chrome App to Windows, How to add Chrome Extension to Windows Store, hwa-cli, Hosted Web Apps Command Line Interface CLI Tool, Install Chrome Extension on Windows 10 Device, convert .crx to .AppX
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 7847f69c85708cb42b878253839b06929f837708

---

# 将现有 Chrome 应用转换为通用 Windows 平台应用

我们使将现有 Chrome 托管应用转换为在通用 Windows 平台 (UWP) 上运行的应用变得更轻松。 有两种方法可转换你的 Chrome 应用：

- 选项 1：[ManifoldJS](http://manifoldjs.com/) 现在接受 Chrome 清单作为一种输入形式。 

- 选项 2：我们开发了 [CLI 工具](https://github.com/MicrosoftEdge/hwa-cli)，该工具可从现有的 `.zip` 或 `.crx` 文件生成 `.appx` 程序包。

## 使用命令行接口转换现有 Chrome 应用

1. 安装 [NodeJS](https://nodejs.org/en/) 及其程序包管理器 [npm](https://www.npmjs.com/)。 


2. 将命令提示符窗口打开到所选的目录


3. 通过在命令行中输入以下内容安装托管 Web 应用 (HWA) 命令行接口 (CLI)： `npm i -g hwa-cli`

4. 通过在命令行中输入以下内容转换你的 Chrome 程序包（`.crx` 和 `.zip` 是受支持的程序包格式）：`hwa convert path/to/chrome/app.crx`，或者 `hwa convert path/to/chrome/app.zip`

    **将 `path/to/chrome/app` 替换为引导到你的 Chrome 应用的路径信息。*
    
5. 生成的 `.appx` 将出现在你的 Chrome 程序包所在的同一文件夹中。 现在随时可将应用上传到 Windows 应用商店。 

## 将应用上传到 Windows 应用商店

若要上传应用，请访问 [Windows 开发人员中心](https://developer.microsoft.com/windows)中的仪表板。 单击“[创建新应用](https://developer.microsoft.com/dashboard/Application/New)”，然后保留应用名称。
![Windows 开发人员中心仪表板保留名称](images/hwa-to-uwp/reserve_a_name.png)


通过导航到“提交”部分的“程序包”页面，上传你的 `AppX` 程序包。

填写 Windows 应用商店提示。

    During the conversion process, you will be prompted for an Identity Name, Publisher Identity, and Publisher Display Name. To retrieve these values, visit the Dashboard in the [Windows Dev Center](https://developer.microsoft.com/windows).
    - Click on "[Create a new app](https://developer.microsoft.com/dashboard/Application/New)" and reserve your app name.
![Windows 开发人员中心仪表板保留名称](images/hwa-to-uwp/reserve_a_name.png)
 - 接下来，在“应用管理”部分下，单击左侧菜单中的“应用标识”。
    ![Windows 开发人员中心仪表板应用标识](images/hwa-to-uwp/app_identity.png)
 - 你应该看到页面上列出的用于提示你的三个值：1. 标识名称：`Package/Identity/Name`
 2. 发布者标识：`Package/Identity/Publisher`
 3. 发布者显示名称： `Package/Properties/PublisherDisplayName`


## 迁移托管 Web 应用的指南

打包你的适用于 Windows 应用商店的 Web 应用后，自定义该应用以使它在所有基于 Windows 的设备（包括 PC、平板电脑、手机、HoloLens、Surface Hub、Xbox 和 Raspberry Pi）上都能正常工作。

### 应用程序内容 URI 规则

[应用程序内容 URI 规则 (ACUR)](/hwa-access-features.md#keep-your-app-secure-setting-application-content-uri-rules-acurs) 或内容 URI 通过你的应用包清单中的 URL 允许列表定义你的托管 Web 应用的作用域。 为了控制与远程内容之间的通信，你必须在该列表中定义要包括和/或要排除的 URL。 如果用户单击一个未明确包含的 URL，Windows 会使用默认浏览器打开目标路径。 通过使用 ACUR，你还可以授予 URL 访问[通用 Windows API](https://msdn.microsoft.com/library/windows/apps/br211377.aspx) 的权限。

你的规则至少应该包括你的应用的起始页。 转换工具将基于你的起始页及其域自动为你创建一组 ACUR。 但是，如果存在任何编程方式的重定向，无论是在服务器上还是在客户端上，都需要将这些目标添加到允许列表。

*注意：ACUR 仅适用于页面导航。 图像、JavaScript 库和其他类似资源不受这些限制的影响。*

许多应用将第三方站点用于其登录流程，例如 Facebook 和 Google。 转换工具将基于最受欢迎的站点自动为你创建一组 ACUR。 如果身份验证方法未包括在该列表中，并且它是重定向流，你需要将其路径添加为 ACUR。 还可以考虑使用 [Web 身份验证代理](/hwa-access-features.md#web-authentication-broker)。

### Flash

不允许在 Windows 10 应用中使用 Flash。 需要确保即使你的应用不存在，也不会影响应用体验。

对于广告，需要确保你的广告提供商具有 HTML5 选项。 你可以查看[必应广告](https://bingads.microsoft.com/)和[应用内广告](http://adsinapps.microsoft.com/)。

只要你在使用 [`<iframe>` 嵌入方法](https://developers.google.com/youtube/iframe_api_reference)，YouTube 视频应该仍起作用，因为它们现在[默认为 HTML5 `<video>`](http://youtube-eng.blogspot.com/2015/01/youtube-now-defaults-to-html5_27.html)。 如果你的应用仍在使用 Flash API，将需要切换到前面提及的嵌入样式。

### 图像资源

Chrome Web 存储在你的应用包中已[要求使用 128x128 应用图标图像](https://developer.chrome.com/webstore/images)。 对于 Windows 10 应用，必须至少提供 44x44、50x50、150x150 和 600x350 应用图标图像。 转换工具将基于 128x128 图像自动为你创建这些图像。 若要实现更丰富、更完善的应用体验，我们强烈建议你创建自己的图像文件。 下面是一些[磁贴和图标资源指南](https://msdn.microsoft.com/library/windows/apps/mt412102.aspx)。

### 功能

应用功能必须在你的程序包清单中[声明](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)，才可以访问特定 API 和资源。 转换工具将自动为你启用三个受欢迎的设备功能：位置、麦克风和摄像头。 与以前一样，系统在授予访问权限之前仍会提示用户确认允许。

*注意：会向用户告知应用声明的所有功能。 我们建议你删除应用不需要的任何功能。*

### 文件下载

传统文件下载（如你在浏览器中所见）目前尚不支持。

### Chrome 平台 API

Chrome 会向应用提供可以作为后台脚本运行的[专用 API](https://developer.chrome.com/apps/api_index)。 这些 API 不受支持。 你可以使用 [Windows 运行时 API](https://msdn.microsoft.com/library/windows/apps/br211377.aspx) 找到等效功能以及更多功能。

## 相关主题

- [通过访问通用 Windows 平台 (UWP) 功能增强你的 Web 应用](/hwa-access-features.md)
- [通用 Windows 平台 (UWP) 应用指南](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [下载 Windows 应用商店应用的设计资源](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


