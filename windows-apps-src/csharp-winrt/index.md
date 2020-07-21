---
description: C#/WinRT 是一个为 C# 代码提供 WinRT 投影支持的工具集。
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, 标准, c#, winrt, cswinrt, 投影
ms.localizationpriority: medium
ms.openlocfilehash: e52763d78937405b308c4c4fe06f6d231fa3abcf
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580274"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> C#/WinRT 目前为公共预览版，在最终发布之前可能会进行重大修改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

C#/WinRT 是一个进行 NuGet 打包的工具包，为 C# 语言提供 Windows 运行时 (WinRT) 投影支持。 “投影”是一个转换层（例如互操作程序集），它以自然而熟悉的方式为目标语言启用编程 WinRT API。 例如，C#/WinRT 投影隐藏了 C# 与 WinRT 接口之间的互操作的详细信息，并提供了从许多 WinRT 类型到相应的 .NET 等效项（例如字符串、URI、公用值类型和泛型集合）的映射。

C#/WinRT 当前支持使用 WinRT 类型，当前的预览版允许[创建](#create-an-interop-assembly)和[引用](#reference-an-interop-assembly) WinRT 互操作程序集。 将来的 C# /WinRT 版本将支持采用 C# 创作 WinRT 类型。

## <a name="motivation-for-cwinrt"></a>采用 C#/WinRT 的动机

[.NET Core](https://docs.microsoft.com/dotnet/core/) 是 .NET 平台的重点，.NET 5 是下一个主要版本。 它是一个开源的跨平台运行时，可用于构建设备、云和 IoT 应用程序。

以前版本的 .NET Framework 和 .NET Core 都内置了 WinRT（一种特定于 Windows 的技术）知识。 为了支持 .NET 5 的可移植性和高效性目标，我们从 .NET 编译器和运行时中撤销了 WinRT 投影支持，将其移到了 C#/WinRT 工具包中。 C#/WinRT 的目标是通过旧版 C# 编译器和 .NET 运行时提供的内置 WinRT 支持来提供奇偶校验。 有关详细信息，请参阅 [Windows 运行时类型的 .NET 映射](https://docs.microsoft.com/windows/uwp/winrt-components/net-framework-mappings-of-windows-runtime-types)。

C#/WinRT 还支持 WinUI 3.0。 此版本的 WinUI 从操作系统中移除了原生的 Microsoft UI 控件和功能。 这使得应用开发人员能够在 Windows 10 版本 1803 和更高版本上使用最新控件和视觉对象。

最后，C#/WinRT 是一个通用工具包，用于支持在 C# 编译器或 .NET 运行时中无法使用内置 WinRT 支持的其他方案。 C#/WinRT 支持向下兼容 .NET Standard 2.0（例如 Mono 5.4）的 .NET 运行时版本。

有关 C#/WinRT 的更多信息，请参阅 [C#/WinRT GitHub 存储库](https://aka.ms/cswinrt/repo)

## <a name="create-an-interop-assembly"></a>创建一个互操作程序集

WinRT API 在 Windows 元数据 (*.winmd) 文件中定义。 C#/WinRT NuGet 程序包中包括 C#/WinRT 编译器 **cswinrt**，可以使用它来处理 Windows 元数据文件并生成 .NET Standard 2.0 C# 代码。 可以将这些源文件编译为互操作程序集，这与 [C++/WinRT](../cpp-and-winrt-apis/index.md) 为 C++ 语言投影生成头文件的方式类似。 然后，可以将 C#/WinRT 互操作程序集与 C#/WinRT 运行时程序集一起分发，供应用程序引用。

### <a name="invoke-cswinrtexe"></a>调用 cswinrt.exe

若要显示命令行选项，请运行 `cswinrt -?`。 若要从项目中调用 cswinrt.exe，建议使用一个 Directory.Build.Targets 文件。 以下项目片段演示了对 **cswinrt** 的简单调用，该调用为 Contoso 命名空间中的类型生成投影源。 然后，这些源会包含在项目生成中。

```xml
  <Target Name="GenerateProjection" BeforeTargets="Build">
    <PropertyGroup>
      <CsWinRTParams>
# This sample demonstrates using a response file for cswinrt execution.
# Run "cswinrt -h" to see all command line options.
-verbose
# Include Windows SDK metadata to satisfy references to 
# Windows types from project-specific metadata.
-in 10.0.18362.0
# Don't project referenced Windows types, as these are 
# provided by the Windows interop assembly.
-exclude Windows 
# Reference project-specific winmd files, defined elsewhere,
# such as from a NuGet package.
-in @(ContosoWinMDs->'"%(FullPath)"', ' ')
# Include project-specific namespaces/types in the projection
-include Contoso 
# Write projection sources to the "Generated Files" folder,
# which should be excluded from checkin (e.g., .gitignored).
-out "$(ProjectDir)Generated Files"
      </CsWinRTParams>
    </PropertyGroup>
    <WriteLinesToFile
        File="$(CsWinRTResponseFile)" Lines="$(CsWinRTParams)"
        Overwrite="true" WriteOnlyWhenDifferent="true" />
    <Message Text="$(CsWinRTCommand)" Importance="$(CsWinRTVerbosity)" />
    <Exec Command="$(CsWinRTCommand)" />
  </Target>

  <Target Name="IncludeProjection" BeforeTargets="CoreCompile" AfterTargets="GenerateProjection">
    <ItemGroup>
      <Compile Include="$(ProjectDir)Generated Files/*.cs" Exclude="@(Compile)" />
    </ItemGroup>
  </Target>
```

### <a name="distribute-the-interop-assembly"></a>分发互操作程序集

互操作程序集通常以 NuGet 程序包的形式分发，并且依赖于必需的 C#/WinRT 运行时程序集 **winrt.runtime.dll** 的 C#/WinRT NuGet 程序包。 C#/WinRT 运行时程序集有两个版本，一个面向 .NET Standard 2.0，一个面向 .NET 5.0。 仅会部署其中的一个，具体取决于应用程序的目标框架。 

* 面向 .NET Standard 2.0 的版本适用于想要在 Windows 上提供启动功能的下层跨平台应用程序。
* 面向 .NET 5.0 的版本建议用于需要跨原生对象引用进行正确的垃圾收集的新式 Windows 应用，例如 XAML 应用。

互操作程序集可以在 nuspec 文件中包含 `targetFramework` 条件，确保为应用部署正确版本的 C#/WinRT 运行时。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
      <group targetFramework=".NET5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> .NET 5.0 的目标框架名字对象将从“.NETCoreApp5.0”移到“.NET5.0”。 C#/WinRT 预发行版将使用二者之一。

## <a name="reference-an-interop-assembly"></a>引用互操作程序集

通常，C#/WinRT 互操作程序集由应用程序项目引用。 但是，中间互操作程序集也可能引用它们。 例如，WinUI 互操作程序集将引用 Windows SDK 互操作程序集。

若要使用投影的 C#/WinRT 类型，请向项目中添加对相应 C#/WinRT 互操作 NuGet 程序包的引用。 这会导致将互操作程序集和 C#/WinRT 运行时程序集都添加到项目中。

如果你分发没有正式互操作程序集的第三方 WinRT 组件，则应用程序项目可能会按照[创建互操作程序集](#create-an-interop-assembly)的过程来生成其自己的专用投影源。 不建议使用此方法，因为它可能会在一个进程内生成同一类型的冲突投影。 设计了遵循[语义版本控制](https://semver.org)方案的 NuGet 打包来防止出现此情况。 最好使用正式的第三方互操作程序集。

### <a name="winrt-type-activation"></a>WinRT 类型激活

C#/WinRT 支持激活由操作系统承载的 WinRT 类型以及第三方组件（例如 [Win2D](https://www.nuget.org/packages/Win2D.uwp/)）。 桌面应用程序中对第三方组件的激活支持是通过[注册免费 WinRT 激活](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/)（在 Windows 10 版本 1903 及更高版本中提供）启用的。 如果组件是面向 UWP 应用构建的，则可能还需要使用 [VCRT 转发器](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/)程序包。

如果 Windows 未能如上所述激活该类型，则可使用 C#/WinRT 提供的激活回退路径。 在这种情况下，C#/WinRT 会尝试根据完全限定的类型名称查找一个原生实现 DLL，并逐步删除各个元素。 例如，回退逻辑会尝试按顺序从以下模块激活 Contoso.Controls.Widget 类型：

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT 使用 [LoadLibrary 备用搜索顺序](https://docs.microsoft.com/windows/win32/dlls/dynamic-link-library-search-order?#alternate-search-order-for-desktop-applications)查找一个实现 DLL。 依赖于此回退行为的应用应将该实现 DLL 与应用模块一起打包。

## <a name="known-issues"></a>已知问题

当前的 C#/WinRT 预览版中存在一些已知的与互操作相关的性能问题。 这些问题会在 2020 年底的最终发布版出来之前解决。

如果在 C#/WinRT NuGet 程序包、cswinrt.exe 编译器或生成的投影源中遇到任何功能问题，请通过 [C#/WinRT 问题页](https://github.com/microsoft/CsWinRT/issues)将问题提交给我们。

## <a name="additional-resources"></a>其他资源

* [C#/WinRT GitHub 存储库](https://aka.ms/cswinrt/repo)
