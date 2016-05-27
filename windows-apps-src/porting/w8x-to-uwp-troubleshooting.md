---
author: mcleblanc
description: 我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。
title: 将 Windows 运行时 8.x 移植到 UWP 疑难解答&#39;
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
---

# 将 Windows 运行时 8.x 移植到 UWP 疑难解答

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

上一主题是[移植项目](w8x-to-uwp-porting-to-a-uwp-project.md)。

我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。 阅读到该末尾后，你可以注释或排除非必要的代码，然后稍后返回支付该债务，从而临时加快进度。 本主题中的疑难解答症状和补救办法的表格可能在此阶段对你有用，尽管它无法替代阅读接下来的一些主题。 在你执行到以后的主题时，你可以一直重新参考该表。

## 跟踪问题

XAML 分析异常可能难以诊断出来，特别是在此类异常中没有含义明确的错误消息时。 请确保已将调试程序配置为捕获第一轮异常（以便试图捕获早期的分析异常）。 你可以检查调试程序中的异常变量，以确定 HRESULT 或消息中是否具有任何有用的信息。 也可以检查 Visual Studio 的输出窗口，以获取由 XAML 分析器输出的错误消息。

如果你的应用终止，并且你只知道在 XAML 标记分析期间引发了未处理的异常，这可能是因为引用了缺失资源（即，通用 8.1 应用中存在其键但在 Windows 10 应用中不存在的资源，例如某些系统 **TextBlock** 样式键）。 或者，它可以是在 **UserControl**、自定义控件或自定义布局面板内部引发的异常。

最后一项措施是进行二进制拆分。 从页面中删除大约一半标记并重新运行应用。 然后，你将知道错误是在已删除的那一半中（你现在应该在任何情况下恢复）还是在*未*删除的那一半中。 通过拆分包含错误的那一半来重复此过程，依此类推，直到完全解决了问题。

## TargetPlatformVersion

本部分介绍了在 Visual Studio 中打开 Windows 10 项目后看到如下消息时该执行何种操作：“需要 Visual Studio 更新。 一个或多个项目需要平台 SDK <version> (未安装该 SDK，也未将其作为 Visual Studio 后续更新的一部分进行提供)。”

-   首先，确定适用于你已安装的 Windows 10 的 SDK 版本号。 导航到 **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\<versionfoldername>**，并记下 *<versionfoldername>*，格式为四部分表示法“Major.Minor.Build.Revision”。
-   打开项目文件以进行编辑，并找到 `TargetPlatformVersion` 和 `TargetPlatformMinVersion` 元素。 将它们编辑为如下形式，使用你在磁盘上找到的四部分表示法版本号替换 *<versionfoldername>*：

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## 症状和补救方法疑难解答

表格中的补救方法信息旨在为你提供足够自行取消阻止的充足信息。 阅读以后的主题时，你将发现有关其中每个问题的进一步详细信息。

| 症状 | 补救方法 |
|---------|--------|
| 在 Visual Studio 中打开 Windows 10 项目时，你会看到如下消息：“需要 Visual Studio 更新。 一个或多个项目需要平台 SDK &lt;version&gt;（未安装该 SDK 版本，也未将其作为 Visual Studio 后续更新的一部分进行提供）。” | 请参阅本主题中的 [TargetPlatformVersion](#targetplatformversion) 部分。 |
| 当在 xaml.cs 文件中调用 InitializeComponent 时，将引发 System.InvalidCastException。| 当你有多个 xaml 文件（至少其中一个受 MRT 限定）共享同一个 xaml.cs 文件并且元素具有在两个 xaml 文件之间不一致的 x:Name 属性时，可能会发生这种情况。 尝试将相同名称添加到两个 xaml 文件中的相同元素，或全部省略名称。 |
| 当在设备上运行的应用终止，或从 Visual Studio 中启动时，你将看到错误“无法激活 Windows 应用商店应用 \[…\]。 激活请求失败并显示错误‘Windows 无法与目标应用程序进行通信。’ 这通常指示目标应用的过程已中止。 \[…\]”。 | 问题可能是在初始化过程中，在你自己的页面或绑定属性（或其他类型）中运行的强制性代码。 或者，它可能在分析将要在应用终止时显示的 XAML 文件时发生（如果从 Visual Studio 中启动，这将是启动页）。 查找无效的资源键和/或尝试使用本主题的“跟踪问题”部分中的一些指南。|
| XAML 分析程序或编译器或者运行时异常会提供错误“*无法解析资源‘<resourcekey>’。*”。 | 该资源键不适用于通用 Windows 平台 (UWP) 应用（例如，对于某些 Windows Phone 资源，存在此情况）。 找到对应的等效资源并更新你的标记。 例如，你可能立即遇到诸如 `PhoneAccentBrush` 的系统键。 |
| C# 编译器提供错误“*找不到类型或命名空间名称‘<name>’\[...\]*”或“*类型或命名空间名称‘<name>’在命名空间中不存在 \[...\]*”或“*类型或命名空间名称‘<name>’在当前上下文中不存在*”。 | 这可能意味着，该类型在扩展 SDK 中实现（尽管可能存在这样的情况，即补救措施并非简单易用）。 使用 [Windows API](https://msdn.microsoft.com/library/windows/apps/bg124285) 引用内容确定用于实现 API 的扩展 SDK，然后通过使用 Visual Studio 的“Add”**** > “Reference”****命令，将对该 SDK 的引用添加到项目。 如果应用面向称为通用设备系列的 API 集，则须在调用扩展 SDK 之前，先使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 类在运行时测试这些 API 是否存在（这称为自适应代码），这一点非常重要。 如果存在通用 API，则它始终优于扩展 SDK 中的 API。 有关详细信息，请参阅[扩展 SDK](w8x-to-uwp-porting-to-a-uwp-project.md#extension-sdks)。 |

下一主题是[移植 XAML 和 UI](w8x-to-uwp-porting-xaml-and-ui.md)。



<!--HONumber=May16_HO2-->


