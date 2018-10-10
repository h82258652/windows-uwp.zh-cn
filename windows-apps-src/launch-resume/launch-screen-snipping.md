---
author: QuinnRadich
title: 启动屏幕截图
description: 本主题介绍了 ms screenclip 和 ms screensketch URI 方案。 你的应用可以使用这些 URI 方案来启动代码段和 Sketch 应用或打开新的代码段。
ms.author: quradic
ms.date: 8/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，uri、 截图草图
ms.localizationpriority: medium
ms.openlocfilehash: e18662125ef72051a289b3f1d0f3dc09b452d256
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4502373"
---
# <a name="launch-screen-snipping"></a>启动屏幕截图

**Ms screenclip:** 和**ms screensketch:** URI 方案允许你启动代码段或编辑屏幕截图。

## <a name="open-a-new-snip-from-your-app"></a>从你的应用打开新的代码段

**Ms screenclip:** URI 允许应用自动打开并启动新的代码段。 生成的代码段复制到用户的剪贴板，但不是会自动传递回打开应用。

**ms screenclip:** 采用以下参数：

| 参数 | 类型 | 必需 | 描述 |
| --- | --- | --- | --- |
| 源 | 字符串 | 否 | 要指示启动 URI 的源的自由格式字符串。 |
| delayInSeconds | int | 否 | 从 1 到 30 的整数值。 指定以完全秒为单位，URI 调用和截图开始时之间的延迟。 |

## <a name="launching-the-snip--sketch-app"></a>启动代码段和 Sketch 应用

**Ms screensketch:** URI 允许你以编程方式启动代码段和 Sketch 的应用，并为批注该应用中打开特定的图像。

**ms screensketch:** 采用以下参数：

| 参数 | 类型 | 必需 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字符串 | 否 | 用于标识要在代码段和 Sketch 的应用中打开的文件访问令牌。 从[SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)检索。 如果省略此参数，则将打开的文件不启动应用。 |
| 源 | 字符串 | 否 | 要指示启动 URI 的源的自由格式字符串。 |
| isTemporary | Bool | 否 | 如果设置为 True，屏幕草图将尝试打开它后删除文件。 |

下面的示例调用[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)方法将从用户的应用发送到截图和 Sketch 的图像。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```