---
Description: 使用通用 Windows 平台（UWP） Api 增强适用于 Windows 10 用户的桌面应用程序。
title: 在桌面应用中使用 UWP Api
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ca9e91233206f0e97d17fdbdd7b0fd09a2897cd8
ms.sourcegitcommit: 3710117f24adb8555aa94b372db814e5d30ae45a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73427085"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>在桌面应用中调用 UWP Api

你可以使用通用 Windows 平台（UWP） Api 向适用于 Windows 10 用户的桌面应用添加新式体验。

首先，将你的项目设置为所需的引用。 然后，从代码调用 UWP Api，将 Windows 10 体验添加到你的桌面应用。 你可以为 Windows 10 用户单独生成，或将相同的二进制文件分发给所有用户，而不管他们运行的是哪个版本的 Windows。

某些 UWP Api 仅在具有[包标识](modernize-packaged-apps.md)的桌面应用中受支持。 有关详细信息，请参阅[可用 UWP api](desktop-to-uwp-supported-api.md)。

## <a name="set-up-your-project"></a>设置项目

你需要对项目进行一些更改才能使用 UWP API。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>修改 .NET 项目以使用 Windows 运行时 Api

.NET 项目有两个选项：

* 如果你的应用面向 Windows 10 版本1803或更高版本，则可以安装提供所有必需引用的 NuGet 包。
* 或者，您可以手动添加引用。

#### <a name="to-use-the-nuget-option"></a>使用 NuGet 选项

1. 确保启用[包引用](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，单击 "**工具"-> "NuGet 包管理器-> 包管理器设置**"。
    2. 请确保已为**默认包管理格式**选择**PackageReference** 。

2. 在 Visual Studio 中打开项目后，在**解决方案资源管理器**中右键单击项目，然后选择 "**管理 NuGet 包**"。

3. 在 " **NuGet 包管理器**" 窗口中，选择 "**浏览**" 选项卡并搜索 `Microsoft.Windows.SDK.Contracts`。

4. 找到 `Microsoft.Windows.SDK.Contracts` 包后，在 " **NuGet 包管理器**" 窗口的右窗格中，根据要作为目标的 Windows 10 版本选择要安装的包的**版本**：

    * **10.0.18362**：为 Windows 10 （版本1903）选择此版本。
    * **10.0.17763**：为 Windows 10 （版本1809）选择此版本。
    * **10.0.17134**：为 Windows 10 （版本1803）选择此版本。

5. 单击**安装**。

#### <a name="to-add-the-required-references-manually"></a>手动添加所需引用

1. 打开**引用管理器**对话框，选择**浏览**按钮，然后选择**所有文件**。

    ![“添加引用”对话框](images/desktop-to-uwp/browse-references.png)

2. 添加对这些文件的引用。

    |文件|位置|
    |--|--|
    |System.Runtime.WindowsRuntime|C：\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C：\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C：\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows winmd|C:\Program Files （x86） \Windows Kits\10\UnionMetadata\\<*sdk 版本*> \Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files （x86） \Windows Kits\10\References\\<*sdk 版本*> \Windows.Foundation.UniversalApiContract\<*版本*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files （x86） \Windows Kits\10\References\\<*sdk 版本*> \Windows.Foundation.FoundationContract\<*版本*>|

3. 在**属性**窗口中，将每个 **.winmd** 文件的*复制本地*字段设为 **False**。

    ![复制本地字段](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>修改C++ Win32 项目以使用 Windows 运行时 api

使用[ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)来使用 Windows 运行时 api。 C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。

若要为C++/WinRT 配置项目：

* 对于新项目，可以安装[ C++/WinRT Visual Studio Extension （VSIX）](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) ，并使用该扩展中包含C++的/WinRT 项目模板之一。
* 对于现有项目，可以在项目中安装[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 包。

有关这些选项的更多详细信息，请参阅[此文](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-windows-10-experiences"></a>添加 Windows 10 体验

现在一切已准备就绪，可以添加用户在 Windows 10 上运行你的应用程序时可享受的现代化体验。 使用此设计流。

:white_check_mark: **首先，确定你要添加哪些体验**

有许多选项。 例如，您可以通过使用[盈利 api](/windows/uwp/monetize)来简化您的采购订单流，或在您有一些值得共享的内容（例如其他用户已发布的新图片）时[直接关注您的应用程序](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)。

![Toast](images/desktop-to-uwp/toast.png)

即使用户忽略或关闭你的消息，他们仍可在操作中心中再次看到该消息，然后单击该消息打开你的应用。 这会增加应用程序的参与度，并增加了额外的优点，使应用程序看起来与操作系统深度集成。 本文稍后将向你展示该体验的代码。

请访问[UWP 文档](/windows/uwp/get-started/)了解更多建议。

:white_check_mark: **决定增强还是扩展**

你经常会听到我们使用术语 "*增强*" 和 "*扩展*"，因此我们将花点时间来准确说明每个术语的含义。

我们使用术语 "*增强*" 来说明可以直接从桌面应用程序调用 Windows 运行时 api （无论是否选择将应用程序打包到 .msix 包中）。 选择了 Windows 10 体验后，确定需要创建的 Api，然后查看[此列表](desktop-to-uwp-supported-api.md)中是否显示了该 api。 这是可以直接从桌面应用程序调用的 Api 列表。 如果你的 API 未出现在此列表中，那是因为与该 API 关联的功能只在 UWP 进程内运行。 通常情况下，这些 Api 包含用于呈现 UWP XAML 的 Api，例如 UWP 地图控件或 Windows Hello 安全提示。

> [!NOTE]
> 尽管通常不能直接从桌面调用呈现 UWP XAML 的 Api，但你可能可以使用其他方法。 如果要承载 UWP XAML 控件或其他自定义视觉体验，可以使用[XAML 孤岛](xaml-islands.md)（从 windows 10 版本1903开始）和[可视层](visual-layer-in-desktop-apps.md)（从 windows 10 版本1803开始）。 可以在打包或未打包的桌面应用中使用这些功能。

如果已选择在 .MSIX 包中打包桌面应用，另一个选项是通过向解决方案添加 UWP 项目来*扩展*应用程序。 桌面项目仍是您的应用程序的入口点，但 UWP 项目可让您访问[此列表](desktop-to-uwp-supported-api.md)中未显示的所有 api。 桌面应用程序可以通过使用应用服务与 UWP 进程进行通信，我们有很多关于如何设置此操作的指导。 如果要添加需要 UWP 项目的体验，请参阅[使用 uwp 组件扩展](desktop-to-uwp-extend.md)。

: white_check_mark: **引用 API 合同**

如果可以直接从桌面应用程序调用 API，请打开浏览器并搜索该 API 的参考主题。
在 API 的摘要下，你会找到一个描述用于该 API 的 API 合同的表。 下面是该表的一个示例：

![API 合同表](images/desktop-to-uwp/contract-table.png)

如果你有基于 .NET 的桌面应用，请添加对该 API 合同的引用，然后将该文件的**复制本地**属性设置为 **False**。 如果你有一个基于 C++ 的项目，请将包含此合同的文件夹的路径添加到你的**附加包含目录**中。

:white_check_mark: **调用 API 以添加你的体验**

以下代码用于显示我们之前看到的通知窗口。 这些 Api 出现在此[列表](desktop-to-uwp-supported-api.md)中，因此你可以将此代码添加到桌面应用并立即运行。

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

你可以将你的应用程序现代化用于 Windows 10，而不必创建新的分支并维护单独的基本代码。

如果要为 Windows 10 用户生成单独的二进制文件，请使用条件编译。 如果你希望生成要部署到所有 Windows 用户的一组二进制文件，请使用运行时检查。

我们快速查看一下每个选项。

### <a name="conditional-compilation"></a>条件编译

你可以保留一个代码库，并编译一组仅面向 Windows 10 用户的二进制文件。

首先，向你的项目中添加一个新生成配置。

![生成配置](images/desktop-to-uwp/build-config.png)

对于该生成配置，请创建一个常数，用于标识调用 Windows 运行时 Api 的代码。  

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

可以不考虑用户所运行的 Windows 版本而为所有 Windows 用户编译一组二进制文件。 仅当用户在 Windows 10 上以打包应用程序的形式运行应用程序时，应用程序才会调用 Windows 运行时 Api。

向代码中添加运行时检查的最简单方法是安装此 Nuget 包： [Desktop Bridge 帮助](https://www.nuget.org/packages/DesktopBridge.Helpers/)程序，然后使用 ``IsRunningAsUWP()`` 方法来指示所有调用 Windows 运行时 api 的代码。 请参阅此博客文章以了解详细信息：[桌面桥 - 标识应用程序的上下文](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)。

## <a name="related-samples"></a>相关示例

* [Hello World 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [辅助磁贴](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [存储 API 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [实现 UWP UpdateTask 的 WinForms 应用程序](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [桌面应用桥到 UWP 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
