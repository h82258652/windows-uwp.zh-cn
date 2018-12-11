---
title: 启动屏幕截取
description: 本主题介绍了 ms screenclip 和 ms screensketch URI 方案。 你的应用可以使用这些 URI 方案来启动代码段和 Sketch 应用或打开新的代码段。
ms.date: 8/1/2017
ms.topic: article
keywords: windows 10，uwp，uri、 截图草图
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a90772e01885a7361cd51b54fc6e5ea9930bfbd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8920556"
---
# <a name="launch-screen-snipping"></a>启动屏幕截取

**Ms screenclip:** 和**ms screensketch:** URI 方案允许你启动代码段或编辑屏幕截图。

## <a name="open-a-new-snip-from-your-app"></a>从你的应用打开新的代码段

**Ms screenclip:** URI 允许应用自动打开并启动新的代码段。 生成的代码段复制到用户的剪贴板，但不是会自动传递回打开应用。

**ms screenclip:** 采用以下参数：

| 参数 | 类型 | 必需 | 描述 |
| --- | --- | --- | --- |
| 源 | 字符串 | 否 | 要指示启动 URI 的源的自由格式字符串。 |
| delayInSeconds | int | 否 | 从 1 到 30 的整数值。 指定以完全秒为单位，URI 调用和截图开始时之间的延迟。 |

## <a name="launching-the-snip--sketch-app"></a>启动代码段和 Sketch 应用

**Ms screensketch:** URI 允许你以编程方式启动代码段和 Sketch 的应用，并在注释该应用中打开特定的图像。

**ms screensketch:** 采用以下参数：

| 参数 | 类型 | 必需 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字符串 | 否 | 用于标识要在代码段和 Sketch 的应用中打开的文件访问令牌。 从[SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)检索。 如果省略此参数，则将打开的文件不启动应用。 |
| secondarySharedAccessToken | 字符串 | 否 | 提供关于截图元数据中标识的 JSON 文件的字符串。 元数据可能包括具有 x、 y 坐标的数组和/或[userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) **clipPoints**字段。 |
| 源 | 字符串 | 否 | 要指示启动 URI 的源的自由格式字符串。 |
| isTemporary | Bool | 否 | 如果设置为 True，屏幕草图将尝试打开它后删除文件。 |

下面的示例调用[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)方法将从用户的应用发送到截图和 Sketch 的图像。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

以下示例说明了由**ms 屏幕截图** **secondaryFileAccessToken**参数指定的文件可能包含：

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
