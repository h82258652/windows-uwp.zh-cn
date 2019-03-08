---
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: 创建和转换源内容组映射
description: 为让通用 Windows 平台 (UWP) 应用做好添加 UWP 应用流式安装的准备，需要创建内容组映射。 本文介绍有关创建和转换内容组映射的具体信息，同时提供一些相关提示和技巧。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, 内容组映射, 流式处理安装, uwp 应用流式处理安装, 源内容组映射
ms.localizationpriority: medium
ms.openlocfilehash: 882db0a6a97c5ee203a072156ca3eb82615607bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647942"
---
# <a name="create-and-convert-a-source-content-group-map"></a>创建和转换源内容组映射

为让通用 Windows 平台 (UWP) 应用做好添加 UWP 应用流式安装的准备，需要创建内容组映射。 本文介绍有关创建和转换内容组映射的具体信息，同时提供一些相关提示和技巧。

## <a name="creating-the-source-content-group-map"></a>创建源内容组映射

需要创建 `SourceAppxContentGroupMap.xml` 文件，然后使用 Visual Studio 或 **MakeAppx.exe** 工具将此文件转换为最终版本：`AppxContentGroupMap.xml`。 从头开始创建 `AppxContentGroupMap.xml`可跳过一个步骤，但由于 `AppxContentGroupMap.xml` 中不允许使用通配符（它们非常有用），因此建议（通常也更容易）创建 `SourceAppxContentGroupMap.xml` 并进行转换。 

我们将演练一个简单的方案，在此方案中利用 UWP 应用流式处理安装非常有用。 

假设创建了一个 UWP 游戏，但最终应用的大小超过 100 GB。 这会需要很长时间才能下载从 Microsoft Store，这可能很不方便。 如果你选择使用 UWP 应用流式处理安装，则可以指定下载应用文件的顺序。 告知应用商店先下载必要的文件，用户就能够尽快地使用应用，同时在后台下载其他不重要的文件。

> [!NOTE]
> 使用 UWP 应用流式处理安装很大程度上依赖于应用的文件结构。 建议尽快就 UWP 应用流式处理安装方面考虑应用的内容布局，使应用文件的分段更简单。

首先，创建 `SourceAppxContentGroupMap.xml` 文件。

在深入了解详细信息前，下面是一个简单的完整 `SourceAppxContentGroupMap.xml` 文件示例：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

内容组映射有两个主要组件：**所需**分区，其中包含所需内容组；**自动**分区，其中可包含多个自动内容组。

### <a name="required-content-group"></a>所需内容组

所需内容组是 `SourceAppxContentGroupMap.xml` 的 `<Required>`元素内的单一内容组。 所需内容组应包含以最少用户体验启动应用所需的所有重要文件。 由于 .NET Native 编译，所有代码（应用程序可执行文件）都必须包含在所需组中，资产和其他文件则包含在自动组中。

例如，如果应用是一款游戏，所需组可能包含用于主菜单或游戏主屏幕中的文件。

下面是原始 `SourceAppxContentGroupMap.xml` 示例文件的代码片段： 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

下面是几个需注意的重要事项：

- `<Required>` 元素内的 `<ContentGroup>`**必须**命名为“所需”。 此名称仅保留供所需内容组使用，不能用于最终内容组映射中的任何其他 `<ContentGroup>`。
- 只有一个 `<ContentGroup>`。 这样做是有意的，因为应只存在一组必需文件。
- 该文件在此示例中是一个 `.exe` 文件。 所需内容组并不限于一个文件，可存在多个文件。 

要开始编写此文件，简单的方法是在最喜欢的文本编辑器中打开一个新页面，快速将文件“另存为”至应用的项目文件夹，并将新创建的文件命名为：`SourceAppxContentGroupMap.xml`。

> [!IMPORTANT]
> 如果要开发 C++ UWP 应用，需要调整 `SourceAppxContentGroupMap.xml` 的文件属性。 将 `Content` 属性设置为 **true**，`File Type` 属性设置为 **XML 文件**。 

创建 `SourceAppxContentGroupMap.xml` 时，在文件名中使用通配符会有所帮助，有关详细信息，请参阅[使用通配符的提示和技巧](#wildcards)部分。

如果使用 Visual Studio 开发应用，建议将此文件包含在所需内容组中：

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

添加单个通配符文件名将包括 Visual Studio 中添加到项目目录的文件，例如应用可执行文件或 DLL。 WinMetadata 和“属性”文件夹包含 Visual Studio 生成的其他文件夹。 资产通配符用于为要安装的应用选择必需的徽标和 SplashScreen 图像。

请注意，不能在文件结构的根处使用双通配符“**”将每个文件包含在项目中，因为尝试将 `SourceAppxContentGroupMap.xml` 转换为最终 `AppxContentGroupMap.xml` 时此操作将失败。

此外，还需注意内存占用文件（AppxManifest.xml、AppxSignature.p7x、resources.pri 等）不应包含在内容组映射中。 如果内存占用文件包含指定的通配符文件名之一，它们将会忽略。

### <a name="automatic-content-groups"></a>自动内容组

自动内容组是用户与已下载的内容组进行交互时在后台下载的资产。 这些组中包含对启动应用影响不大的所有其他文件。 例如，可将自动内容组分解为不同的级别，将每一级别定义为单独的内容组。 如所需内容组部分中所述：由于 .NET Native 编译，所有代码（应用程序可执行文件）都必须包含在所需组中，资产和其他文件则包含在自动组中。

下面从 `SourceAppxContentGroupMap.xml` 示例深入了解自动内容组：
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

自动组的布局非常类似于所需组的布局，以下几项例外：

- 存在多个内容组。
- 除了为所需内容组保留的名称“必需”外，自动内容组可具有唯一的名称。
- 自动内容组不能包含所需内容组中的**任何**文件。 
- 自动内容组可包含同时位于其他自动内容组中的文件。 这些文件只会下载一次，并且将使用包含它们的首个自动内容组进行下载。

#### 使用通配符<a name="wildcards"></a>的提示和技巧

内容组映射的文件布局始终与项目根文件夹相关。

在本例中，两个 `<ContentGroup>` 元素中均使用了通配符，用于检索“Assets\Level2”或“Assets\Level3”的一个文件级别内的所有文件。 如果使用的文件夹结构较深，可以使用双通配符：

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

还可以针对文件名配合使用通配符和文本。 例如，如果想要将每个文件包含在文件名包含“Level2”的“资产”文件夹中，可以使用如下所示的内容：

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>将 SourceAppxContentGroupMap.xml 转换为 AppxContentGroupMap.xml

若要将 `SourceAppxContentGroupMap.xml` 转换为最终版本 `AppxContentGroupMap.xml`，可使用 Visual Studio 2017 或 **MakeAppx.exe** 命令行工具。

使用 Visual Studio 转换内容组映射：
1. 向项目文件夹添加 `SourceAppxContentGroupMap.xml`
2. 在“属性”窗口中将 `SourceAppxContentGroupMap.xml` 的生成操作更改为“AppxSourceContentGroupMap”
2. 在解决方案资源管理器中右键单击该项目
3. 导航到“应用商店”->“转换内容组映射文件”

如果未在 Visual Studio 中开发应用，或者只是更喜欢使用命令行，请使用 **MakeAppx.exe** 工具转换 `SourceAppxContentGroupMap.xml`。 

简单的 **MakeAppx.exe** 命令可能如下所示：
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

/S 选项指定指向 `SourceAppxContentGroupMap.xml` 的路径，/f 指定指向 `AppxContentGroupMap.xml` 的路径。 最后一个选项 /d 指定应用于扩展文件名通配符的目录，在此情况下，它是应用项目目录。

若要详细了解可与 **MakeAppx.exe** 配合使用的选项，请打开一个命令提示符，导航到 **MakeAppx.exe** 并输入：

```syntax
MakeAppx convertCGM /?
```

这就是使最终 `AppxContentGroupMap.xml` 可供应用使用需要进行的所有操作！ 没有更多，您的应用程序是完全准备好进行 Microsoft Store 前的准备工作。 若要详细了解将 UWP 应用流式处理安装添加到应用的完整过程，请查看[此博客文章](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。
