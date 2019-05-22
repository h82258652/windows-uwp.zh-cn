---
Description: 通过使用通用 Windows 平台 (UWP) Api 来提高您的 Windows 10 用户的桌面应用程序。
title: 在桌面应用程序中使用 UWP Api
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: eb0cee26ee65c029af3f0385f44630afa057b29f
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984657"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>在桌面应用程序中调用 UWP Api

可以使用通用 Windows 平台 (UWP) Api 向您的 Windows 10 用户打造更出色的桌面应用程序添加现代体验。

首先，设置你的项目所需的引用。 然后，从你的代码添加到桌面应用程序体验 Windows 10 时会调用 UWP Api。 可以生成单独的 Windows 10 用户，也可以将分发到所有用户的 Windows 版本无论它们运行相同的二进制文件。

UWP Api 仅支持在桌面应用程序中打包[MSIX 包](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。 有关详细信息，请参阅[可用的 UWP Api](desktop-to-uwp-supported-api.md)。

## <a name="set-up-your-project"></a>将项目设置

你需要对项目进行一些更改才能使用 UWP API。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>将.NET 项目修改为使用 Windows 运行时 Api

1. 打开**引用管理器**对话框，选择**浏览**按钮，然后选择**所有文件**。

    ![“添加引用”对话框](images/desktop-to-uwp/browse-references.png)

2. 添加对这些文件的引用。

  |文件|Location|
  |--|--|
  |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
  |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
  |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

3. 在**属性**窗口中，将每个 **.winmd** 文件的*复制本地*字段设为 **False**。

    ![复制本地字段](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>修改C++项目以使用 Windows 运行时 Api

使用[ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)若要使用 Windows 运行时 Api。 C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。

若要配置的项目C++/WinRT，请参阅[修改 Windows 桌面应用程序项目，以添加C++/WinRT 支持](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

## <a name="add-windows-10-experiences"></a>添加 Windows 10 体验

现在一切已准备就绪，可以添加用户在 Windows 10 上运行你的应用程序时可享受的现代化体验。 使用此设计流。

:white_check_mark:**首先，确定你想要添加哪些的体验**

有许多选项。 例如，可以通过使用简化你的采购订单流[货币化 Api](/windows/uwp/monetize)，或[指向你的应用程序](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)时有一些共享有趣的事情，如新图片另一个用户已发布。

![toast](images/desktop-to-uwp/toast.png)

即使用户忽略或关闭你的消息，他们仍可在操作中心中再次看到该消息，然后单击该消息打开你的应用。 这会增加与你的应用程序的合作，使你的应用程序会显示与操作系统深度集成的好处。 我们将稍后在本文中展示你的代码这种体验。

请访问[UWP 文档](/windows/uwp/get-started/)的更多想法。

:white_check_mark:**决定是否以增强或扩展**

您经常会听到我们使用术语*增强*并*扩展*，因此我们来看一段时间来介绍完全每个这些术语平均值。

我们使用术语*增强*来描述您可以直接从桌面应用程序 （无论您打包应用程序，MSIX 包中的） 调用的 Windows 运行时 Api。 已选择 Windows 10 体验，标识来创建，然后确定该 API 是否显示在所需的 Api[此列表](desktop-to-uwp-supported-api.md)。 这是您可以直接从桌面应用程序调用的 Api 的列表。 如果你的 API 未出现在此列表中，那是因为与该 API 关联的功能只在 UWP 进程内运行。 通常情况下，其中包括如 UWP 地图控件或 Windows Hello 安全提示呈现 UWP XAML 的 Api。

> [!NOTE]
> 虽然不能直接从您的桌面调用通常呈现 UWP XAML 的 Api，你可以使用替代方法。 如果你想要托管 UWP XAML 控件或其他自定义的视觉体验，则可以使用[XAML 群岛](xaml-islands.md)（从 Windows 10，版本 1903年） 和[可视化层](visual-layer-in-desktop-apps.md)（从 Windows 10，版本 1803年）。 可以在打包或未打包桌面应用中使用这些功能。

如果您已选择 MSIX 包中的将桌面应用程序打包，另一个选项是*扩展*通过将 UWP 项目添加到你的解决方案的应用程序。 桌面项目仍为应用程序的入口点但 UWP 项目的所有不显示在 Api 允许你访问[此列表](desktop-to-uwp-supported-api.md)。 与 UWP 进程进行通信的桌面应用程序可以通过使用应用服务，我们有许多指导如何完成该设置。 如果你想要添加一种体验的要求的 UWP 项目，请参阅[与 UWP 组件扩展](desktop-to-uwp-extend.md)。

:white_check_mark:**引用 API 协定**

如果您可以直接从桌面应用程序调用 API，打开浏览器和搜索该 API 的参考主题。
在 API 的摘要下，你会找到一个描述用于该 API 的 API 合同的表。 下面是该表的一个示例：

![API 合同表](images/desktop-to-uwp/contract-table.png)

如果你有基于 .NET 的桌面应用，请添加对该 API 合同的引用，然后将该文件的**复制本地**属性设置为 **False**。 如果你有一个基于 C++ 的项目，请将包含此合同的文件夹的路径添加到你的**附加包含目录**中。

:white_check_mark:**调用 Api 来添加你的体验**

以下代码用于显示我们之前看到的通知窗口。 这些 Api 会出现在此[列表](desktop-to-uwp-supported-api.md)这样你可以将此代码添加到桌面应用程序并稍后再试运行。

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```C++
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

要了解有关通知的更多信息，请参阅[自适应和交互式 Toast 通知](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)。

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>支持 Windows XP、Windows Vista 和 Windows 7/8 安装库

您可以运行适用于 Windows 10 应用程序的现代化，而无需创建一个新的分支和维护独立的代码库。

如果要为 Windows 10 用户生成单独的二进制文件，请使用条件编译。 如果你希望生成要部署到所有 Windows 用户的一组二进制文件，请使用运行时检查。

我们快速查看一下每个选项。

### <a name="conditional-compilation"></a>条件编译

你可以保留一个代码库，并编译一组仅面向 Windows 10 用户的二进制文件。

首先，向你的项目中添加一个新生成配置。

![生成配置](images/desktop-to-uwp/build-config.png)

对于该生成配置，创建一个常量，若要标识调用 Windows 运行时 Api 的代码。  

对于基于 .NET 的项目，该常量称为**条件编译常量**。

![预处理器](images/desktop-to-uwp/compilation-constants.png)

对于基于 C++ 的项目，该常量称为**预处理器定义**。

![预处理器](images/desktop-to-uwp/pre-processor.png)

在任何 UWP 代码块之前添加该常量。

```csharp

[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}

```

```C++

#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif

```

仅当该常量是在活动生成配置中定义时，编译器才会生成该代码。

### <a name="runtime-checks"></a>运行时检查

可以不考虑用户所运行的 Windows 版本而为所有 Windows 用户编译一组二进制文件。 你的应用程序调用 Windows 运行时 Api 仅当用户为运行您的应用程序打包的应用程序在 Windows 10 上。

若要添加到代码中的运行时检查的最简单方法是安装此 Nuget 包：[桌面桥帮助程序](https://www.nuget.org/packages/DesktopBridge.Helpers/)，然后使用``IsRunningAsUWP()``方法调用的 Windows 运行时 Api 的所有代码的入口。 请参阅此博客文章的更多详细信息：[桌面桥-标识应用程序的上下文](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)。

## <a name="related-video"></a>相关视频

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>相关示例

* [Hello World 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [辅助磁贴](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [应用商店 API 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [实现 UWP UpdateTask 的 WinForms 应用程序](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP 示例桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
