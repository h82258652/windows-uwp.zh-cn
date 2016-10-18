---
author: seksenov
title: "托管 Web 应用 - 使用 Mac 将 Web 应用程序转换为 Windows 应用"
description: "使用 Mac 将你的网站转换为适用于 Windows 10 的通用 Windows 平台 (UWP) 应用。"
kw: Hosted Web Apps with a Mac, Porting to Windows 10 with a Mac, Convert website to Windows with Mac, Packaging web application with ManfoldJS for Windows Store, Add website to Windows Store with App Studio
translationtype: Human Translation
ms.sourcegitcommit: 0458dcd2aab862ccdecf1ebbc51e883405a929a6
ms.openlocfilehash: 3ba820e2ec8a3556874c0c7c7e328831bab783ca

---

# 使用 Mac 创建托管 Web 应用

只需从网站 URL 入手，即可快速创建适用于 Windows 10 的通用 Windows 平台应用。 

> [!NOTE]
> 以下说明适用于 Mac 开发平台。 Windows 用户，请访问[有关使用 Windows 开发平台的说明](/hwa-create-windows.md)。

## 在 Mac 上开发需要做哪些准备工作

- Web 浏览器。
- 命令提示符。

## 选项 1：ManifoldJS

[ManifoldJS](http://manifoldjs.com/) 是一款 Node.js 应用，可通过 NPM 轻松安装。 它会获取有关你的网站的元数据，并生成跨 Android、iOS 和 Windows 的本机托管应用。 如果你的站点没有 [Web 应用部件清单](https://www.w3.org/TR/appmanifest/)，将自动为你生成一个清单。

1. 安装包含 NPM（节点包管理器）的 [NodeJS](https://nodejs.org/)。 <br>

2. 打开命令提示符和 NPM，安装 ManifoldJS：
```
npm install -g manifoldjs
```

3. 运行你的网站 URL 上的 `manifoldjs` 命令：
```
manifoldjs http://codepen.io/seksenov/pen/wBbVyb/?editors=101
```

4. 遵循以下视频中的步骤完成打包并将托管 Web 应用发布到 Windows 应用商店。

[![使用 ManifoldJS 在 Mac 上发布 UWP Web 应用](images/hwa-to-uwp/mac_manifoldjs_video.png)](https://sec.ch9.ms/ch9/0a67/9b06e5c7-d7aa-478d-b30d-f99e145a0a67/ManifoldJS_high.mp4 "使用 ManifoldJS 在 Mac 上发布 UWP Web 应用")

## 选项 2：App Studio

[App Studio](http://appstudio.windows.com/) 是一款免费的联机应用创建工具，使你可以快速生成 Windows 10 应用。

1. 在 Web 浏览器中打开 [App Studio](http://appstudio.windows.com/)。

2. 单击“立即启动！”****。

3. 在“Web 应用模板”****下，单击“托管 Web 应用”****。

4. 按照屏幕上的说明，生成准备用于向 Windows 应用商店发布的程序包。

## 相关主题

- [通过访问通用 Windows 平台 (UWP) 功能增强你的 Web 应用](/hwa-access-features.md)
- [通用 Windows 平台 (UWP) 应用指南](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [下载 Windows 应用商店应用的设计资源](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


