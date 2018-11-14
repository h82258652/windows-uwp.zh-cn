---
author: andrewleader
Description: Learn how to use custom audio on your toast notifications.
title: toast 上的自定义音频
label: Custom audio on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, toast, 自定义音频, 通知, 音频, 声音
ms.localizationpriority: medium
ms.openlocfilehash: 8ef27dfed400715256d1d9cfa51f383a9b72c90d
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6673634"
---
# <a name="custom-audio-on-toasts"></a>toast 上的自定义音频

Toast 通知可使用自定义音频，从而让你的应用展现品牌的独特音效。 例如，消息应用可在其 Toast 通知上使用自己的消息传递声音，而不是使用通用的通知声音，以便用户立即知道接收的通知来自该应用。

## <a name="install-uwp-community-toolkit-nuget-package"></a>安装 UWP 社区工具包 NuGet 程序包

若要通过代码创建通知，我们强烈建议使用 UWP 社区工具包通知库，它提供通知 XML 内容的对象模型。 可手动构造通知 XML，但此操作比较杂乱且容易出错。 UWP 社区工具包内部的通知库由 Microsoft 拥有通知的团队生成和维护。

通过 NuGet 安装 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)（本文档使用版本 1.0.0）。


## <a name="add-namespace-declarations"></a>添加命名空间声明

`Windows.UI.Notifications` 包含磁贴和 toast API。 `Microsoft.Toolkit.Uwp.Notifications` 包含通知库。

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>构造通知

toast 通知内容不仅包括文本和图像，还包括按钮和输入。 若要查看完整的代码片段，请参阅[发送本地 toast](send-local-toast.md)。

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>添加自定义音频

Windows 移动版始终支持 toast 通知中的自定义音频。 但是，只有版本 1511（内部版本 10586）的桌面设备添加了对自定义音频的支持。 如果将包含自定义音频的 toast 发送到版本 1511 之前的桌面设备，该 toast 将处于静音模式。 因此，对于版本 1511 之前的桌面设备，toast 通知中不应包含自定义音频，这样通知至少会使用默认通知声音。

**已知问题**：如果使用版本 1511 的桌面设备，只有在通过 Microsoft Store 安装应用时，自定义 toast 音频才能正常工作。 这意味着在提交到 Microsoft Store 之前无法在本地测试桌面设备上的自定义音频，但只要从 Microsoft Store 安装应用，音频便可正常工作。 我们已在周年更新中修复了此问题，因此本地部署的应用中的自定义音频将正常工作。

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

支持的音频文件类型包括...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>发送通知

toast 内容已完成，现可轻松地发送通知。

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>相关主题

- [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [发送本地 toast](send-local-toast.md)
- [toast 内容文档](adaptive-interactive-toasts.md)