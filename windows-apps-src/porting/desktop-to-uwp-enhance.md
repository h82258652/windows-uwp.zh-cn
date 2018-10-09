---
author: normesta
Description: Enhance your desktop application for Windows 10 users by using Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: 增强用于 Windows 10 的桌面应用程序
ms.author: normesta
ms.date: 08/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 392f8166e16c028a57bc9e27039a9884f1d9714a
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4463653"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>增强用于 Windows 10 的桌面应用程序

你可以使用 UWP API 来添加为 Windows 10 用户而设的现代化体验。

首先，设置项目。 然后，添加 Windows 10 体验。 你可以为 Windows 10 用户单独生成，也可以不考虑用户所运行的 Windows 版本而向所有用户分发完全相同的二进制文件。

## <a name="first-set-up-your-project"></a>首先，设置项目

你需要对项目进行一些更改才能使用 UWP API。

### <a name="modify-a-net-project-to-use-uwp-apis"></a>修改 .NET 项目以使用 UWP API

打开**引用管理器**对话框，选择**浏览**按钮，然后选择**所有文件**。

![“添加引用”对话框](images/desktop-to-uwp/browse-references.png)

然后，添加对这些文件的引用。

|文件|位置|
|--|--|
|System.Runtime.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*版本*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*版本*>|

在**属性**窗口中，将每个 **.winmd** 文件的*复制本地*字段设为 **False**。

![复制本地字段](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-uwp-apis"></a>修改 C++ 项目以使用 UWP API

打开项目的属性页。

在 **C/C++** 设置组的**常规**设置中，将**使用 Windows 运行时扩展**字段设置为**是 (/ZW)**。

   ![使用 Windows 运行时扩展](images/desktop-to-uwp/consume-runtime-extensions.png)

打开**其他 #using 目录**对话框，并添加这些目录。

* %VSInstallDir%\Common7\IDE\VC\vcpackages
* C:\Program Files (x86)\Windows Kits\10\UnionMetadata
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\<*最新版本*>
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.FoundationContract\<*最新版本*>

打开**附加包含目录**对话框，并添加此目录：C:\Program Files (x86)\Windows Kits\10\Include\<*最新版本*>\um

![附加包含目录](images/desktop-to-uwp/additional-include.png)

在 **C/C++** 设置组的**代码生成**设置中，将**启用最小重新生成**设置设为**否 (/GM-)**。

![启用最小重新生成](images/desktop-to-uwp/disable-min-build.png)


## <a name="add-windows-10-experiences"></a>添加 Windows 10 体验

现在一切已准备就绪，可以添加用户在 Windows 10 上运行你的应用程序时可享受的现代化体验。 使用此设计流。

:white_check_mark: **首先，确定你要添加哪些体验**

有许多选项。 例如，你可以使用盈利 Api 或直接注意到你的应用程序，当你有要共享，如另一个用户已发布了新图片有趣的内容来简化你采购订单流。

![Toast](images/desktop-to-uwp/toast.png)

即使用户忽略或关闭你的消息，他们仍可在操作中心中再次看到该消息，然后单击该消息打开你的应用。 这加强用户与你的应用程序，并且具有进行显示与操作系统深度集成应用程序的互动。 稍后，我们将向你演示用于该体验的代码。

访问我们的[开发人员中心](https://developer.microsoft.com/windows)获取灵感。

:white_check_mark: **决定增强还是扩展**

你经常会听到我们使用术语“增强”和“扩展”，因此我们需要花些时间来说明一下这两个术语的确切含义。

我们使用术语“增强”来描述可以直接从桌面应用程序调用的 UWP API。 当你选择 Windows 10 体验后，请确定创建它所需的 API，然后查看该 API 是否出现在此[列表](desktop-to-uwp-supported-api.md)中。 这是你可以直接从桌面应用程序中调用的 API 的列表。 如果你的 API 未出现在此列表中，那是因为与该 API 关联的功能只在 UWP 进程内运行。 通常情况下，其中包括显示新式 UI（例如 UWP 地图控件或 Windows Hello 安全提示）的 API。

也就是说，如果要在你的应用程序中包含这些体验，只需通过向你的解决方案中添加 UWP 项目来“扩展”应用程序即可。 桌面项目仍是应用程序的入口点，但 UWP 项目使你可以访问此[列表](desktop-to-uwp-supported-api.md)中未显示的所有 API。 桌面应用程序可以使用应用服务来与 UWP 进程通信，我们可针对如何进行相关设置提供很多指导。 如果你要添加的体验需要 UWP 项目，请参阅[用 UWP 扩展](desktop-to-uwp-extend.md)。

: white_check_mark: **引用 API 合同**

如果你可以直接从桌面应用程序中调用 API，请打开浏览器并搜索该 API 的参考主题。
在 API 的摘要下，你会找到一个描述用于该 API 的 API 合同的表。 下面是该表的一个示例：

![API 合同表](images/desktop-to-uwp/contract-table.png)

如果你有基于 .NET 的桌面应用，请添加对该 API 合同的引用，然后将该文件的**复制本地**属性设置为 **False**。 如果你有一个基于 C++ 的项目，请将包含此合同的文件夹的路径添加到你的**附加包含目录**中。

:white_check_mark: **调用 API 以添加你的体验**

以下代码用于显示我们之前看到的通知窗口。 此[列表](desktop-to-uwp-supported-api.md)中显示了这些 API，因此你可以将此代码添加到桌面应用程序中并立即执行此代码。

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

你可以增加你的应用程序在 Windows 10 的现代体验无需创建新分支和维护不同代码库。

如果要为 Windows 10 用户生成单独的二进制文件，请使用条件编译。 如果你希望生成要部署到所有 Windows 用户的一组二进制文件，请使用运行时检查。

我们快速查看一下每个选项。

### <a name="conditional-compilation"></a>条件编译

你可以保留一个代码库，并编译一组仅面向 Windows 10 用户的二进制文件。

首先，向你的项目中添加一个新生成配置。

![生成配置](images/desktop-to-uwp/build-config.png)

为该生成配置创建一个常量，来标识调用 UWP API 的代码。  

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

可以不考虑用户所运行的 Windows 版本而为所有 Windows 用户编译一组二进制文件。 你的应用程序调用 UWP Api 仅当用户是作为打包的应用程序在 Windows 10 上运行你的应用程序。

向代码中添加运行时检查的最简单方法是安装此 Nuget 包：[桌面桥帮助程序](https://www.nuget.org/packages/DesktopBridge.Helpers/)，然后使用 ``IsRunningAsUWP()`` 方法访问所有 UWP 代码。 请参阅此博客文章以了解详细信息：[桌面桥 - 标识应用程序的上下文](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)。

## <a name="related-video"></a>相关视频

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>相关示例

* [Hello World 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [辅助磁贴](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [应用商店 API 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [实现 UWP UpdateTask 的 WinForms 应用程序](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP 示例的桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
