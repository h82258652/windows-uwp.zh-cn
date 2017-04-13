---
author: seksenov
title: "托管 Web 应用 - 使用 Visual Studio 将 Web 应用程序转换为 Windows 应用"
description: "使用 Visual Studio 将你的网站转换为适用于 Windows 10 的通用 Windows 平台 (UWP) 应用。"
kw: Hosted Web Apps tutorial, Porting to Windows 10 with Visual Studio, How to convert website to Windows, How to add website to Windows Store, Packaging web application for Microsoft Store, Test Windows 10 native features and runtime APIs with CodePen, How to use Windows Cortana Live Tiles Built-in Camera on my Website with remote JavaScript
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "托管 Web 应用, 将 Web 应用移植到 Windows 10, 将网站转换为 Windows, 打包适用于 Windows 应用商店的 Web 应用"
ms.assetid: a58d2c67-77f8-4d01-bea3-a6ebce2d73b9
ms.openlocfilehash: e135b63e2ba5f455dbbf6524e5519a33acd822b0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-your-web-application-to-a-universal-windows-platform-uwp-app"></a>将 Web 应用程序转换为通用 Windows 平台 (UWP) 应用

了解只需从网站 URL 入手，即可快速创建适用于 Windows 10 的通用 Windows 平台应用的方法。 

> [!NOTE]
> 以下说明适用于 Windows 开发平台。 Mac 用户，请访问[有关使用 Mac 开发平台的说明](./hwa-create-mac.md)。

## <a name="what-you-need-to-develop-on-windows"></a>在 Windows 上开发需要做哪些准备工作

- [Visual Studio 2015。](https://www.visualstudio.com/) 功能齐全的免费 Visual Studio Community 2015 包含 Windows 10 开发人员工具、通用应用模板、代码编辑器、功能强大的调试器、Windows 移动版仿真器、丰富的语言支持等 - 所有这些工具均可立即用于生产。
- （可选）[适用于 Windows 10 的 Windows 独立 SDK](https://dev.windows.com/downloads/windows-10-sdk)。 如果你使用的是 Visual Studio 2015 之外的开发环境，则可以下载适用于 Windows 10 安装程序的独立 Windows SDK。 请注意，如果你使用的是 Visual Studio 2015，则无需安装该 SDK，因为该 SDK 已包含在其中。

## <a name="step-1-pick-a-website-url"></a>步骤 1：选取网站 URL
选择一个可作为单页应用良好工作的现有网站。 我们强烈建议你本人是该站点的所有者或开发者，如此你就可以进行所有必要更改。 如果你忘记了 URL，请尝试将此 [Codepen 示例](http://codepen.io/seksenov/pen/wBbVyb/?editors=101)用作网站。 复制要在整个教程中使用的你的 URL 或 Codepen URL。 

![步骤 1：选取网站 URL](images/hwa-to-uwp/windows_step1.png)

## <a name="step-2-create-a-blank-javascript-app"></a>步骤 2：创建一个空白 JavaScript 应用

启动 Visual Studio。
1. 单击**文件**。
2. 单击**新建项目**。
3. 在 **JavaScript** 下的 **Windows 通用**中，单击**空白应用(Windows 通用)**。

![步骤 2：创建一个空白 JavaScript 应用](images/hwa-to-uwp/windows_step2.png)

## <a name="step-3-delete-any-packaged-code"></a>步骤 3：删除任何封装代码

由于这是托管 Web 应用，其中的内容由远程服务器提供，因此默认情况下，不需要 JavaScript 模板提供的大部分本地应用文件。 删除任何本地 HTML、JavaScript 或 CSS 资源。 只需保留 `package.appxmanifest` 文件即可，你可以在该文件中配置应用和图像资源。

![步骤 3：删除任何封装代码](images/hwa-to-uwp/windows_step3.png)

## <a name="step-4-set-the-start-page-url"></a>步骤 4：设置起始页 URL

1. 打开 `package.appxmanifest` 文件。
2. 在**应用程序**选项卡下，找到**起始页**文本字段。
3. 将 `default.html` 替换为你的网站 URL。

![步骤 4：设置起始页 URL](images/hwa-to-uwp/windows_step4.png)

## <a name="step-5-define-the-boundaries-of-your-web-app"></a>步骤 5：定义你的 Web 应用的边界

应用程序内容 URI 规则 (ACUR) 指定允许访问你的应用和通用 Windows API 的远程 URL。 至少需要为你的起始页以及该页面所利用的任何 Web 资源添加一条 ACUR。 有关 ACUR 的详细信息，请[单击此处](./hwa-access-features.md)。
1. 打开 `package.appxmanifest` 文件。
2. 单击**内容 URI** 选项卡。
3. 为你的起始页添加任何必要的 URI。

例如：
```
1. http://codepen.io/seksenov/pen/wBbVyb/?editors=101
2. http://*.codepen.io/
```
4. 针对你添加的每个 URI，将 **WinRT 访问**设置为**全部**。

![步骤 5：定义你的 Web 应用的边界](images/hwa-to-uwp/windows_step5.png)

## <a name="step-6-run-your-app"></a>步骤 6：运行应用

此时，你有一个可以访问通用 Windows API、完全正常运行的 Windows 10 应用！

如果你遵循的是我们的 Codepen 示例，请单击 **Toast 通知**按钮以调用托管脚本中的 Windows API。

![步骤 6：运行应用](images/hwa-to-uwp/windows_step6.png)

## <a name="bonus-add-camera-capture"></a>额外功能：添加相机捕获

复制并粘贴以下 JavaScript 代码即可支持相机捕获。 如果你遵循的是你自己的网站，请创建用于调用 `cameraCapture()` 方法的按钮。 如果你遵循的是我们的 Codepen 示例，按钮已存在于 HTML 中。 单击该按钮并拍摄图片。

```JavaScript
function cameraCapture() {
  if(typeof Windows != 'undefined') {
   var captureUI = new Windows.Media.Capture.CameraCaptureUI();
   //Set the format of the picture that's going to be captured (.png, .jpg, ...)
   captureUI.photoSettings.format = Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;
   //Pop up the camera UI to take a picture
   captureUI.captureFileAsync(Windows.Media.Capture.CameraCaptureUIMode.photo).then(function (capturedItem) {
      // Do something with the picture
   });
  }
}
```

## <a name="related-topics"></a>相关主题

- [通过访问通用 Windows 平台 (UWP) 功能增强你的 Web 应用](hwa-access-features.md)
- [通用 Windows 平台 (UWP) 应用指南](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [下载 Windows 应用商店应用的设计资源](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)
