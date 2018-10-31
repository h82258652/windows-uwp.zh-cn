---
author: stevewhims
Description: Some kinds of apps (multilingual dictionaries, translation tools, etc.) need to override the default behavior of an app bundle, and build resources into the app package instead of having them in separate resource packages. This topic explains how to do that.
title: 将资源构建到你的应用包而非资源包
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 61b526cd7aa2da8733457b16dd0487ef4ead9cca
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5812891"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>将资源构建到你的应用包而非资源包

某些类型的应用（多语言字典、翻译工具等）需要覆盖应用程序包的默认行为，并将资源构建到应用包，而不是单独的资源程序包（或资源包）。 本主题介绍如何实现该操作。

默认情况下，构建[应用程序包 (.appxbundle)](../packaging/packaging-uwp-apps.md) 时，只会将语言、缩放和 DirectX 功能级别的默认资源构建到应用包。 翻译的资源以及为非默认缩放和/或 DirectX 功能级别定制的资源都构建到资源包中，仅供需要的设备下载。 如果客户使用语言首选项设置为西班牙语的设备从 Microsoft Store 购买应用，则只下载并安装应用和西班牙语资源包。 如果同一用户稍后在**设置**中将他们的语言首选项更改为法语，则将下载并安装应用的法语资源包。 符合缩放和 DirectX 功能级别的资源的情况也类似。 对于大多数应用，此行为会提高效率，而这正是你和客户*想要*实现的。

但是如果应用支持用户在应用中即时（而不是通过**设置**）更改语言，则该默认行为并不适合使用。 你实际上希望无条件下载所有语言资源并随应用一次性安装，然后将它们保留在设备上。 你希望将所有这些资源构建到应用包而不是单独的资源包。

**注意**：将资源包含在应用包中本质上增加了该应用的大小。 正因如此，只有应用的性质要求时才值得执行此操作。 如果没有要求，除了像往常一样构建常规的应用程序包之外，无需执行任何操作。

可以配置 Visual Studio 以通过以下两种方式之一将资源构建到应用包。 可以将配置文件添加到项目，也可以直接编辑项目文件。 使用这些选项中最熟悉或最适用于生成系统的选项。

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>选项 1 使用 priconfig.packaging.xml 将资源构建到应用包

1. 在 Visual Studio 中，将新项添加到项目。 选择 XML 文件，并将该文件命名为 `priconfig.packaging.xml`。
2. 在“解决方案资源管理器”中，选择 `priconfig.packaging.xml` 并检查“属性”窗口。 文件的“生成操作”应设置为 None，“复制到输出目录”应设置为“不要复制”。
3. 将文件的内容替换为此 XML。
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. 每个 `<autoResourcePackage>` 元素会告知 Visual Studio 将给定限定符名称的资源自动拆分为单独的资源包。 这称为*自动拆分*。 就目前拥有的文件内容来说，实际上尚未更改 Visual Studio 的行为。 换言之，Visual Studio *已采取*文件存在这些内容时的行为，因为这些都是默认行为。 如果不希望 Visual Studio 对限定符名称进行自动拆分，请从文件中删除 `<autoResourcePackage>` 元素。 如果希望将所有语言资源都构建到应用包，而不是自动拆分为单独的资源包，文件的内容应如下所示。
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. 保存并关闭该文件，然后重新构建项目。

若要确认是否将自动拆分选择考虑在内，请查找文件 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` 并确认其内容与选择相匹配。 如果匹配，则代表已成功通过配置 Visual Studio 将选择的资源构建到应用包。

还有一个需要执行的最终步骤。 **但仅在已删除 `Language` 限定符名称**时需要执行该步骤。 你需要将支持所有应用的语言作为应用的默认语言。 有关详细信息，请参阅[指定应用使用的默认资源](specify-default-resources-installed.md)。 如果要在应用包中包含英语、西班牙语和法语的资源，下面就是 `priconfig.default.xml` 应包含的内容。

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>这是如何实现的？

在后台，Visual Studio 启动一个名为 `MakePri.exe` 的工具来生成一个称为包资源索引的文件，用于描述所有应用的资源，包括指示要自动拆分的资源限定符名称。 有关此工具的详细信息，请参阅[使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)。 Visual Studio 将配置文件传递给 `MakePri.exe`。 `priconfig.packaging.xml` 文件的内容用作该配置文件的 `<packaging>` 元素，即确定自动拆分的部分。 因此，添加和编辑 `priconfig.packaging.xml` 最终会影响 Visual Studio 为应用生成的包资源索引文件的内容，以及应用程序包中包的内容。

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>使用不同的文件名而不是 `priconfig.packaging.xml`

如果将文件命名为 `priconfig.packaging.xml`，Visual Studio 将识别并自动使用它。 如果为其提供不同的名称，需要让 Visual Studio 知道。 将此 XML 添加到项目文件中第一个 `<PropertyGroup>` 元素的开始和结束标记之间。

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

将 `FILE-PATH-AND-NAME` 替换为文件的路径和名称。

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>选项 2 使用项目文件将资源构建到应用包

这是选项 1 的替代方法。 了解选项 1 的工作原理后，如果选项 2 更适合开发和/或构建工作流，则可以选择执行选项 2。

将此 XML 添加到项目文件中第一个 `<PropertyGroup>` 元素的开始和结束标记之间。

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

删除第一个限定符名称后，文件的内容如下所示。

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

保存并关闭，然后重新生成项目。

还有一个需要执行的最终步骤。 **但仅在已删除 `Language` 限定符名称**时需要执行该步骤。 你需要将支持所有应用的语言作为应用的默认语言。 有关详细信息，请参阅[指定应用使用的默认资源](specify-default-resources-installed.md)。 如果要在应用包中包含英语、西班牙语和法语的资源，下面就是项目文件应包含的内容。

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>相关主题

* [使用 Visual Studio 打包 UWP 应用](../packaging/packaging-uwp-apps.md)
* [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)
* [指定应用使用的默认资源](specify-default-resources-installed.md)