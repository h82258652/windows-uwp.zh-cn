---
Description: At build time, the Resource Management System creates an index of all the different variants of the resources that are packaged up with your app. At run-time, the system detects the user and machine settings that are in effect and loads the resources that are the best match for those settings.
title: 资源管理系统
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: bedbad9e4de22ee098863d013a1e4ad16d86543e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7855255"
---
# <a name="resource-management-system"></a>资源管理系统
资源管理系统具有生成时间和运行时两种功能。 在生成时间，系统创建与你的应用打包在一起的资源的所有不同变体的索引。 此索引为包资源索引 (PRI)，它也包括在你的应用包中。 在运行时，系统检测有效的用户和计算机设置，查询 PRI 中的信息，并自动加载最匹配这些设置的资源。

## <a name="package-resource-index-pri-file"></a>包资源索引 (PRI) 文件
每个应用包都应包含应用中资源的二进制索引。 该索引在生成时间进行创建，并包含在一个或多个包资源索引 (PRI) 文件中。

- PRI 文件包含实际字符串资源，以及引用包中各种文件的一组编入索引的文件路径。
- 包通常包含每种语言的一个 PRI 文件，名称为 resources.pri。
- 当实例化 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 时，自动加载每个包的根目录的 resources.pri 文件。
- 使用工具 [MakePRI.exe](compile-resources-manually-with-makepri.md) 可以创建和转储 PRI 文件。
- 对于典型的应用开发，你无需使用 MakePRI.exe，因为它已经集成到 Visual Studio 编译工作流。 并且 Visual Studio 支持在专用的 UI 中编辑 PRI 文件。 但是，你的本地化人员以及他们使用的工具可能依赖于 MakePRI.exe。
- 每个 PRI 文件都包含一个命名资源集合，称为资源映射。 从包加载 PRI 文件时，验证资源映射名称是否匹配包标识名称。
- PRI 文件仅包含数据，因此它们不使用可移植可执行 (PE) 格式。 它们专门作为 Windows 中的仅数据资源格式。 它们替换 Win32 应用模型中 DLL 包含的资源。
- PRI 文件的大小限制为 64 千字节。

## <a name="uwp-api-access-to-app-resources"></a>应用资源的 UWP API 访问权限

### <a name="basic-functionality-resourceloader"></a>基本功能 (ResourceLoader)
以编程方式访问应用资源的最简单的方法是使用 [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) 命名空间和 [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) 类。 **ResourceLoader** 为你提供对资源文件集、引用库或其他包的字符串资源的基本访问权限。

### <a name="advanced-functionality-resourcemanager"></a>高级功能 (ResourceManager)
[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 类（位于 [**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) 命名空间中）提供了有关资源的额外信息，如枚举和检查。 这超出了 **ResourceLoader** 类的提供范围。

[**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) 对象表示具有多种语言或其他限定符变体的单个逻辑资源。 它使用字符串资源标识符（如 `Header1`）或资源文件名（如 `logo.jpg`）描述资产或资源的逻辑视图。

[**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) 对象表示单个具体的资源值及其限定符，如适用于英语的字符串“Hello World”，或特定于**比例-100** 分辨率的作为限定符图像资源的“logo.scale-100.jpg”。

对应用提供的资源存储在分层集合中，你可以使用 [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) 对象进行访问。 **ResourceManager** 类提供应用使用的各种顶级 **ResourceMap** 实例（对应于应用的各种包）的访问权限。 [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) 值对应于当前应用包的资源映射，且不包括任何引用的框架包。 每个 **ResourceMap** 都针对在包清单中指定的包名称进行命名。 **ResourceMap** 中为子树（请参阅 [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)），其中进一步包含 **NamedResource** 对象。 子树通常对应于包含资源的资源文件。 有关详细信息，请参阅[本地化 UI 和应用包清单中的字符串](localize-strings-ui-manifest.md)和[加载为比例、主题、高对比度和其他定制的图像和资产](images-tailored-for-scale-theme-contrast.md)。

下面提供了一个示例。

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**注意**资源标识符被视为统一资源标识符 (URI) 片段，遵循 URI 语义。 例如，`GetValue("Caption%20")` 被视为 `GetValue("Caption ")`。 在资源标识符中不要使用“？”或“#”，因为它们会终止资源路径评估。 例如，“MyResource?3”被视为“MyResource”。

**ResourceManager** 不仅支持访问某个应用的字符串资源，它还维护枚举和检查各种文件资源的能力。 为了避免文件和源自该文件内部的其他资源之间发生冲突，索引的文件路径全部驻留在预留的“文件”**ResourceMap** 子树中。 例如，文件 `\Images\logo.png` 对应于资源名称 `Files/images/logo.png`。

[**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) API 透明地处理作为资源的文件引用，适合典型的使用方案。 **ResourceManager** 应仅用于高级方案，例如当你想要覆盖当前上下文时。

### <a name="resourcecontext"></a>ResourceContext
基于作为资源限定符值集合（语言、比例、对比度等）的特定的 [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live) 选择候选资源。 除非覆盖，默认上下文对每个限定符值使用应用的当前配置。 例如，可以针对比例限定图像等资源，具体因不同的监视器而异，因此不同应用程序视图之间也有差异。 出于此原因，每个应用程序视图都有不同的默认上下文。 使用 [**GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView) 可以获取给定视图的默认上下文。 每当你检索候选资源时，都应该传递 **ResourceContext** 实例，以获取最适合给定视图的值。

## <a name="important-apis"></a>重要的 API
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>相关主题
* [本地化 UI 和应用包清单中的字符串](localize-strings-ui-manifest.md)
* [加载为比例、主题、高对比度和其他定制的图像和资产](images-tailored-for-scale-theme-contrast.md)
