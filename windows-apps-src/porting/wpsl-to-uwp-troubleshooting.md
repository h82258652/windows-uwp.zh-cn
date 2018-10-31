---
author: stevewhims
description: 我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。
title: 将 WindowsPhone Silverlight 移植到 UWP 疑难解答
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3a5d3ab7b50721b969859006831b33e9b00e300f
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5814570"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>将 WindowsPhone Silverlight 移植到 UWP 疑难解答


上一主题是[移植项目](wpsl-to-uwp-porting-to-a-uwp-project.md)。

我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。 阅读到该末尾后，你可以注释或排除非必要的代码，然后稍后返回支付该债务，从而临时加快进度。 本主题中的疑难解答症状和补救办法的表格可能在此阶段对你有用，尽管它无法替代阅读接下来的一些主题。 在你执行到以后的主题时，你可以一直重新参考该表。

## <a name="tracking-down-issues"></a>跟踪问题

XAML 分析异常可能难以诊断出来，特别是在此类异常中没有含义明确的错误消息时。 请确保已将调试程序配置为捕获第一轮异常（以便试图捕获早期的分析异常）。 你可以检查调试程序中的异常变量，以确定 HRESULT 或消息中是否具有任何有用的信息。 也可以检查 Visual Studio 的输出窗口，以获取由 XAML 分析器输出的错误消息。

如果你的应用终止，并且你只知道在 XAML 标记分析期间引发了未经处理的异常，则这可能是引用缺失资源 （即资源 WindowsPhone Silverlight 应用，但不是用于 windows 10 中存在其键存在的结果应用，例如某些系统**TextBlock**样式键）。 或者，它可以是在 **UserControl**、自定义控件或自定义布局面板内部引发的异常。

最后一项措施是进行二进制拆分。 从页面中删除大约一半标记并重新运行应用。 然后，你将知道错误是在已删除的那一半中（你现在应该在任何情况下恢复）还是在*未*删除的那一半中。 通过拆分包含错误的那一半来重复此过程，依此类推，直到完全解决了问题。

## <a name="targetplatformversion"></a>TargetPlatformVersion

本部分介绍了如何操作，在 Visual Studio 中打开 windows 10 项目，你看到消息"需要 Visual Studio 更新。 一个或多个项目需要平台 SDK &lt;version&gt;（未安装该 SDK 版本，也未将其作为 Visual Studio 后续更新的一部分进行提供）。”

-   首先，确定已安装的 windows 10 SDK 的版本号。 导航到 **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\&lt;versionfoldername&gt;**，并记下 *&lt;versionfoldername&gt;*，格式为四部分表示法“Major.Minor.Build.Revision”。
-   打开项目文件以进行编辑，并找到 `TargetPlatformVersion` 和 `TargetPlatformMinVersion` 元素。 将它们编辑为如下外观，使用你在磁盘上找到的四部分表示法版本号替换 *&lt;versionfoldername&gt;*。

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>症状和补救方法疑难解答

表格中的补救方法信息旨在为你提供足够自行取消阻止的充足信息。 阅读以后的主题时，你将发现有关其中每个问题的进一步详细信息。

| 症状 | 补救方法 |
|---------|--------|
| XAML 分析程序或编译器提供错误“_名称‘&lt;typename&gt;’在命名空间 […] 中不存在。_” | 如果 &lt;typename&gt; 是自定义类型，那么在 XAML 标记中的命名空间前缀声明中，将“clr-namespace”更改为“using”，并删除任何程序集令牌。 对于平台类型，这意味着该类型不适用于通用 Windows 平台 (UWP)，因此请查找等效项并更新标记。 你可能立即遇到的示例是 `phone:PhoneApplicationPage` 和 `shell:SystemTray.IsVisible`。 | 
| XAML 分析程序或编译器提供错误“_未识别或无法访问成员‘&lt;membername&gt;’。_” 或“_未在类型 [...] 中找到‘&lt;propertyname&gt;’。_”。 | 这些错误将在你移植某些类型名称后开始显示，例如根 **Page**。 成员或属性不适用于 UWP，因此请查找等效项并更新标记。 你可能立即遇到的示例是 `SupportedOrientations` 和 `Orientation`。 |
| XAML 分析程序或编译器提供错误“_未找到可附加属性 [...] [...]。_” 或“_未知的可附加成员 [...]。_”。 | 这可能是由类型（而不是附加的属性）导致的；在这种情况下，你将已经具有该类型的错误，并且此错误将在你修复该错误后消失。 你可能立即遇到的示例是 `phone:PhoneApplicationPage.Resources` 和 `phone:PhoneApplicationPage.DataContext`。 | 
|XAML 分析程序或编译器或者运行时异常会提供错误“_无法解析资源‘&lt;resourcekey&gt;’。_”。 | 该资源键不适用于通用 Windows 平台 (UWP) 应用。 找到对应的等效资源并更新你的标记。 例如，你可能立即遇到诸如 `PhoneTextNormalStyle` 的 **TextBlock** 样式键。 |
| C# 编译器提供错误“_找不到类型或命名空间名称‘&lt;name&gt;’[...]_”或“_命名空间 [...] 中不存在类型或命名空间名称‘&lt;name&gt;’_”或“_类型或命名空间名称‘&lt;name&gt;’在当前上下文中不存在_”。 | 这可能意味着编译器尚不知道某个类型的正确 UWP 命名空间。 使用 Visual Studio 的 **Resolve** 命令解决该问题。 <br/>如果该 API 不在称为通用设备系列的 API 集中（换句话说，该 API 在扩展 SDK 中实现），则使用[扩展 SDK](wpsl-to-uwp-porting-to-a-uwp-project.md)。<br/>可能存在端口较复杂的其他情况。 你可能立即遇到的示例是 `DesignerProperties` 和 `BitmapImage`。 | 
|当在设备上运行，该应用终止，或从 Visual Studio 启动时，你将看到错误"无法激活 Windows 运行时 8.x 应用 [...]。 激活请求失败并显示错误“Windows 无法与目标应用程序通信。 这通常指示目标应用的过程已中止。 […]”. | 问题可能是在初始化过程中，在你自己的页面或绑定属性（或其他类型）中运行的强制性代码。 或者，它可能在分析将要在应用终止时显示的 XAML 文件时发生（如果从 Visual Studio 中启动，则将是启动页）。 查找无效的资源键并/或尝试使用本主题的[跟踪问题](#tracking-down-issues)部分中的一些指南。|
| _XamlCompiler 错误 WMC0055：无法将文本值“&lt;your stream geometry&gt;”分配到类型‘RectangleGeometry’的属性“Clip”中_ | 在 UWP 中，[Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 的类型和 XAML C++ UWP 应用。 |
| _XamlCompiler 错误 WMC0001：XML 命名空间中的“RadialGradientBrush”类型未知 [...]_ | UWP 不具有 **RadialGradientBrush** 类型。 从标记中删除 **RadialGradientBrush** 并使用一些其他类型的 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 和 XAML C++ UWP 应用。 |
| _XamlCompiler 错误 WMC0011：元素“&lt;UIElement type&gt;”上的成员“OpacityMask”未知_ | UWP [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 和 XAML C++ UWP 应用。 |
| _SYSTEM.NI.DLL 中发生的类型“System.Runtime.InteropServices.COMException”的第一轮异常。 其他信息：应用程序调用了为其他线程封送的接口。 （来自 HRESULT 的异常：0x8001010E (RPC_E_WRONG_THREAD)）。_ | 必须在 UI 线程上完成你正在执行的工作。 调用 [**CoreWindow.GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589))。 |
| 动画正在运行，但是它对其目标属性没有影响。 | 使动画独立，或对其设置 `EnableDependentAnimation="True"`。 请参阅[动画](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| 在 Visual Studio 中打开 windows 10 项目时，你看到消息"需要 Visual Studio 更新。 一个或多个项目需要平台 SDK &lt;version&gt;（未安装该 SDK 版本，也未将其作为 Visual Studio 后续更新的一部分进行提供）。” | 请参阅本主题中的 [TargetPlatformVersion](#targetplatformversion) 部分。 |
| 当在 xaml.cs 文件中调用 InitializeComponent 时，将引发 System.InvalidCastException。 | 当你有多个 xaml 文件（至少其中一个受 MRT 限定）共享同一个 xaml.cs 文件并且元素具有在两个 xaml 文件之间不一致的 x:Name 属性时，可能会发生这种情况。 尝试将相同名称添加到两个 xaml 文件中的相同元素，或全部省略名称。 | 

下一主题是[移植 XAML 和 UI](wpsl-to-uwp-porting-xaml-and-ui.md)。

