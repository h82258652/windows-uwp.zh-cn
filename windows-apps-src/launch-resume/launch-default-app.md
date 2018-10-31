---
author: TylerMSFT
title: 启动 URI 的默认应用
description: 了解如何启动统一资源标识符 (URI) 的默认应用。 URI 允许你启动其他应用以执行特定任务。 本主题还提供许多内置于 Windows 的 URI 方案的概述。
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.author: twhitney
ms.date: 06/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a5a0af7b7227f149722cf34a368fc588198880ef
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5812259"
---
# <a name="launch-the-default-app-for-a-uri"></a>启动 URI 的默认应用


**重要的 API**

- [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
- [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
- [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何启动统一资源标识符 (URI) 的默认应用。 URI 允许你启动其他应用以执行特定任务。 本主题还提供许多内置于 Windows 的 URI 方案的概述。 你也可以启动自定义 URI。 有关注册自定义 URI 方案和处理 URI 激活的详细信息，请参阅[处理 URI 激活](handle-uri-activation.md)。

URI 方案允许你通过单击超链接来打开应用。 正如可以使用 **mailto:** 开始打开电子邮件，你可以使用 **http:** 打开默认 Web 浏览器

本主题介绍了以下内置于 Windows 的 URI 方案：

| URI 方案 | 启动 |
| ----------:|----------|
|[bingmaps:、ms-drive-to: 和 ms-walk-to: ](#maps-app-uri-schemes) | “地图”应用 |
|[http:](#http-uri-scheme) | 默认 Web 浏览器 |
|[mailto:](#email-uri-scheme) | 默认电子邮件应用 |
|[ms-call:](#call-app-uri-scheme) |  调用应用 |
|[ms-chat:](#messaging-app-uri-scheme) | “消息”应用 |
|[ms-people:](#people-app-uri-scheme) | “人脉”应用 |
|[ms-photos:](#photos-app-uri-scheme) | “照片”应用 |
|[ms-settings:](#settings-app-uri-scheme) | “设置”应用 |
|[ms-store:](#store-app-uri-scheme)  | “应用商店”应用 |
|[ms-tonepicker:](#tone-picker-uri-scheme) | 音调选取器 |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | “114 查号”应用 |
|[msnweather:](#weather-app-uri-scheme) | 天气应用 |

<br>
例如，以下 URI 打开默认浏览器并显示必应网站。

`http://bing.com`

你还可以启动自定义 URI 方案。 如果未安装处理该 URI 的应用，你可以建议用户安装应用。 有关详细信息，请参阅[在没有可以处理该 URI 的应用时推荐一个应用](#recommend-an-app-if-one-is-not-available-to-handle-the-uri)。

通常，你的应用不能选择要启动的应用。 用户确定启动哪个应用。 可以注册多个应用来处理同一个 URI 方案。 上述操作不适用于保留 URI 方案。 忽略注册保留 URI 方案。 有关保留 URI 方案的完整列表，请参阅[处理 URI 激活](handle-uri-activation.md)。 在多个应用可能已注册同一个 URI 方案的情况下，你的应用可以推荐一个要启动的特定应用。 有关详细信息，请参阅[在没有可以处理该 URI 的应用时推荐一个应用](#recommend-an-app-if-one-is-not-available-to-handle-the-uri)。

### <a name="call-launchuriasync-to-launch-a-uri"></a>调用 LaunchUriAsync 以启动 URI

使用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法启动 URI。 调用此方法时，你的应用必须是前台应用，即对于用户必须是可见的。 此要求有助于确保用户保持控制。 为满足此要求，请确保将 URI 的所有启动都直接绑定到到应用的 UI 中。 用户必须总是采取某种操作来发起 URI 启动。 如果尝试启动 URI 并且你的应用不在前台运行，则启动将失败，且会调用错误回调。

首先创建 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/system.uri.aspx) 对象来表示 URI，然后将其传递给 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法。 使用返回结果以查看调用是否成功，如以下示例所示。

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

在某些情况下，操作系统将提示用户查看是否确实要切换应用。

![警告对话框覆盖了应用的灰显背景。 该对话框询问用户是否需要切换应用，而且在右下方有“是”和“否”按钮。 “否”按钮会突出显示。](images/warningdialog.png)

如果始终希望出现此提示，请使用 [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) 属性告诉操作系统显示一个警告。

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>在没有可以处理该 URI 的应用时推荐一个应用

在某些情况下，用户可能未安装用以处理所启动 URI 的应用。 默认情况下，为处理此类情况，操作系统会向用户提供一个链接，帮助其在应用商店中搜索相应的应用。 如果你希望为用户提供具体的建议，告知他们在此情况下应获取何种应用，则可以随所启用的 URI 传递该建议。

当多个应用注册为处理某个 URI 方案时，推荐还是有用的。 通过推荐一个特定应用，Windows 将打开该应用（如果已安装该应用）。

若要进行推荐，则调用 [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) 方法，并将 [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) 设置为应用商店中要推荐的应用的程序包系列名称。 操作系统会使用此信息将在应用商店中搜索应用这一常规选项替换为从应用商店中获取推荐的应用这一具体选项。

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>设置其余视图首选项

调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 的源应用可请求在 URI 启动后停留于屏幕上。 默认情况下，Windows 会尝试在处理该 URI 的源应用和目标应用之间平等地共享所有可用空间。 源应用可使用 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 属性向操作系统指示希望其应用占用较多或较少的可用空间。 此外，还可使用 **DesiredRemainingView** 指示源应用在 URI 启动后无需停留于屏幕上，并可由目标应用完全替代。 此属性仅指定调用应用的首选窗口大小。 不指定可能会同时显示在屏幕上的其他应用的行为。

**注意**Windows 时会考虑多个不同因素，确定源应用的最终窗口尺寸，例如源应用的首选项、 屏幕、 屏幕方向等上的应用数量。 设置 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 并不能保证为源应用设定具体的窗口化行为。

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>URI 方案 ##

下面介绍了各种 URI 方案。

### <a name="call-app-uri-scheme"></a>呼叫应用 URI 方案

使用 **ms-call:** URI 方案来启动“呼叫”应用。

| URI 方案       | 结果                   |
|------------------|--------------------------|
| ms-call:settings | “呼叫”应用设置页面。 |

### <a name="email-uri-scheme"></a>电子邮件 URI 方案

使用 **mailto:** URI 方案来启动默认邮件应用。

| URI 方案 |结果                          |
|------------|---------------------------------|
| mailto:    | 启动默认电子邮件应用。 |
| mailto:\[email address\] | 启动电子邮件应用并使用“收件人”一行上特定的电子邮件地址创建新邮件。 请注意，在用户点击“发送”之前，不会发送电子邮件。 |

### <a name="http-uri-scheme"></a>HTTP URI 方案

使用 **http:** URI 方案来启动默认 Web 浏览器。

| URI 方案 | 结果                           |
|------------|-----------------------------------|
| http:      | 启动默认 Web 浏览器。 |

### <a name="maps-app-uri-schemes"></a>“地图”应用 URI 方案

使用 **bingmaps:**、**ms-drive-to:** 和 **ms-walk-to:** URI 方案[启动 Windows 地图应用](launch-maps-app.md)，使其启动后显示特定的地图、路线和搜索结果。 例如，以下 URI 将打开 Windows 地图应用，并显示以纽约市为中心的地图。

`bingmaps:?cp=40.726966~-74.006076`

![Windows 地图应用的示例。](images/mapnyc.png)

有关详细信息，请参阅[启动 Windows 地图应用](launch-maps-app.md)。 若要在你自己的应用中使用地图控件，请参阅[以 2D、3D 和街景视图方式显示地图](https://msdn.microsoft.com/library/windows/apps/mt219695)。

### <a name="messaging-app-uri-scheme"></a>“消息”应用 URI 方案

使用 **ms-chat:** URI 方案启动 Windows 消息应用。

| URI 方案 |结果 |
|------------|--------|
| ms-chat:   | 启动“Messaging”应用。 |
| ms-chat:?ContactID={contacted}  |  允许使用特定联系人的信息启动消息应用程序。   |
| ms-chat:?Body={body} | 允许使用要用作消息内容的字符串启动消息应用程序。|
| ms-chat:?Addresses={address}&Body={body} | 允许使用特定地址的信息以及要用作消息内容的字符串启动消息应用程序。 注意：可以串联地址。 |
| ms-chat:?TransportId={transportId}  | 允许使用特定传输 ID 启动消息应用程序。 |

### <a name="tone-picker-uri-scheme"></a>音调选取器 URI 方案

使用 **ms-tonepicker:** URI 方案选择铃声、闹钟和系统音。 还可以保存新铃声和获取铃声的显示名称。

| URI 方案 | 结果 |
|------------|---------|
| ms-tonepicker: | 选取铃声、闹钟和系统音。 |

参数通过 LaunchURI API 的 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) 传递。 有关详细信息，请参阅[使用 ms-tonepicker URI 方案选择并保存音调](launch-ringtone-picker.md)。

### <a name="nearby-numbers-app-uri-scheme"></a>“114 查号”应用 URI 方案

使用 **ms-yellowpage:** URI 方案来启动“114 查号”应用。

| URI 方案 | 结果 |
|------------|---------|
| ms-yellowpage:?input=\[keyword\]&amp;method=\[String or T9\] | 启动“114 查号”应用。<br>`input` 指的是想要搜索的关键字。<br>`method` 指的是搜索类型（字符串或 T9 搜索）。<br>如果 `method` 是 `T9`（一种键盘），则 `keyword` 应该是映射到 T9 键盘字母搜索的数字字符串。<br>如果 `method` 是 `String`，则 `keyword` 是要搜索的关键字。 |

### <a name="people-app-uri-scheme"></a>“人脉”应用 URI 方案

使用 **ms-people:** URI 方案来启动“人脉”应用。
有关详细信息，请参阅[启动“人脉”应用](launch-people-apps.md)。

### <a name="photos-app-uri-scheme"></a>“照片”应用 URI 方案

使用 **ms-photos:** URI 方案来启动“照片”应用，以查看图像或编辑视频。 例如：  
查看图像： `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
或编辑视频： `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> 用于编辑视频或显示图像的 URI 仅适用于桌面。

| URI 方案 |结果 |
|------------|--------|
| ms-photos:viewer?fileName={filename} | 启动“照片”应用查看指定的图像，其 {filename} 为完全限定的路径名称。 例如： `c:\users\userName\Pictures\ImageToView.jpg` |
| ms-photos:videoedit?InputToken={input token} | 以视频编辑模式为由文件标记表示的文件启动“照片”应用。 **InputToken** 是必需的。 使用 [SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) 来获取文件的标记。 |
| ms-photos:videoedit?Action={action} | 一个可选参数，用于在指定视频编辑模式中打开“照片”应用，该模式的 {action} 为以下内容之一：**SlowMotion**、**FrameExtraction**、**Trim**、**View**、**Ink**。 如果未指定，则默认为 **View** |
| ms-photos:videoedit?StartTime={timespan} | 一个可选参数，用于指定视频开始播放的位置。 `{timespan}` 必须为 `"hh:mm:ss.ffff"` 格式。 如果未指定，则默认为 `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>“设置”应用 URI 方案

使用 **ms-settings:** URI 方案[启动 Windows“设置”应用](launch-settings-app.md)。 启动为设置应用是编写隐私感知应用的重要组成部分。 如果你的应用无法访问敏感资源，我们建议为用户提供到该资源的隐私设置的方便链接。 例如，以下 URI 将打开设置应用，并显示相机隐私设置。

`ms-settings:privacy-webcam`

![相机隐私设置。](images/privacyawarenesssettingsapp.png)

有关详细信息，请参阅[启动 Windows“设置”应用](launch-settings-app.md)和[隐私感知应用指南](https://msdn.microsoft.com/library/windows/apps/hh768223)。

### <a name="store-app-uri-scheme"></a>Microsoft Store 应用 URI 方案

使用 **ms-windows-store:** URI 方案来[启动 UWP 应用](launch-store-app.md)。 打开产品详细信息页面、产品查看页面和搜索页面等。例如，以下 URI 将打开 UWP 应用并启动 Microsoft Store 的主页。

`ms-windows-store://home/`

有关详细信息，请参阅[启动 UWP 应用](launch-store-app.md)。

### <a name="weather-app-uri-scheme"></a>天气应用 URI 方案

使用**msnweather:** URI 方案来启动天气应用。

| URI 方案 | 结果 |
|------------|---------|
| msnweather://forecast?la= \[latitude\] 和 lo = \ [longitude\] | 启动天气应用中基于位置的地理坐标的预测页面。<br>`latitude` 指位置的纬度。<br> `longitude` 指的经度的位置。<br> |
