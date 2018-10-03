---
author: laurenhughes
title: 使用包布局创建包
description: 包布局是用于描述应用的包结构的单个文档。 它指定应用中的捆绑（主要和可选）、捆绑中的包以及包中的文件。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 打包, 资产包布局, 资产包
ms.localizationpriority: medium
ms.openlocfilehash: 3f8cbb3989b58b726336b4bd757902bd9ea3f8c0
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2018
ms.locfileid: "4262454"
---
# <a name="package-creation-with-the-packaging-layout"></a>使用包布局创建包  

随着资产包的引入，开发人员现在有了构建更多包和更多包类型的工具。 当应用变得越来越大、越来越复杂时，通常会包含更多的包，管理这些包的难度也会增加（特别是在 Visual Studio 以外构建和使用映射文件的情况下）。 为了简化应用包结构的管理，你可以使用 MakeAppx.exe 支持的包布局。 

包布局是一个描述应用打包结构的 XML 文档。 它指定应用中的捆绑（主要和可选）、捆绑中的包以及包中的文件。 可以从不同的文件夹、驱动器和网络位置选择文件。 可以使用通配符选择或排除文件。

设置好应用的包布局后，其与 MakeAppx.exe 配合使用，在单个命令行调用中为应用创建所有包。 可以编辑包布局以改变包结构，从而契合你的部署需求。 


## <a name="simple-packaging-layout-example"></a>简单包布局示例

下面是一个简单的包布局示例：

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

我们一起来分析这个示例，以了解其工作原理。

### <a name="packagefamily"></a>PackageFamily
该包布局将创建单个平面应用捆绑包文件，其中包含一个 x64 体系结构程序包和一个"Media"资产包。 

**PackageFamily** 元素用于定义应用程序包。 你必须使用 **ManifestPath** 属性为该捆绑包提供一个 **AppxManifest**，**AppxManifest** 应该与该捆绑包的体系结构包的 **AppxManifest** 相对应。 此外，还必须提供 **ID** 属性。 ID 在包创建过程中与 MakeAppx.exe 配合使用，因此你可以根据需要创建该包，并且它将用作生成的包的文件名。 **FlatBundle** 属性用于描述要创建的捆绑包的类型，对于平面捆绑包（本文详细介绍的包类型），该属性为 **true**；对于经典捆绑包，该属性为 **false**。 **ResourceManager** 属性用于指定该捆绑包中的资源包是否将使用 MRT 来访问文件。 该属性默认为 **true**，但截止 Windows 10 版本 1803，该功能还没准备好，因此必须将其设置为 **false**。


### <a name="package-and-assetpackage"></a>Package 和 AssetPackage
在 **PackageFamily** 中，定义了应用程序包包含或引用的包。 这里，体系结构程序包（也称为主要包）使用 **Package** 元素定义，资产包使用 **AssetPackage** 元素定义。 体系结构程序包必须指定包适用的体系结构：“x64”、“x86”、“arm”或“neutral”。 你还可以（可选）使用 **ManifestPath** 属性直接为该包提供专门的 **AppxManifest**。 如果没有提供 **AppxManifest**，则从为 **PackageFamily** 提供的 **AppxManifest** 自动生成一个。 

默认情况下，将为捆绑包中的每一个包生成 **AppxManifest**。 对于资产包，你还可以设置 **AllowExecution** 属性。 将该属性设置为 **false**（默认值）有助于缩短应用发布时间，因为病毒扫描程序不会阻止不需要执行的包的发布过程（有关详细信息，请参阅[资产包简介](asset-packages.md)]）。 

### <a name="files"></a>File
在每个包定义中，可以使用 **File** 元素选择要包含在该包中的文件。 **SourcePath** 属性是文件在本地的位置。 你可以从不同的文件夹（通过提供相对路径）、不同的驱动器（通过提供绝对路径）、甚至网络共享（通过提供类似于 `\\myshare\myapp\*` 的路径）选择文件。 **DestinationPath** 是文件在包中的最终位置（相对于包根目录而言）。 可以使用 **ExcludePath**（而不是其他两个属性）选择要从同一个包中其他 **File** 元素的 **SourcePath** 属性选择的文件中排除的文件。

每个 **File** 元素都可以使用通配符来选择多个文件。 通常，可以在路径中的任意位置使用单通配符 (`*`) 任意次。 但是，单通配符将只匹配文件夹中的文件，不匹配任何子文件夹。 例如，可以在 **SourcePath** 中使用 `C:\MyGame\*\*` 选择文件 `C:\MyGame\Audios\UI.mp3` 和 `C:\MyGame\Videos\intro.mp4`，但它无法选择 `C:\MyGame\Audios\Level1\warp.mp3`。 也可以使用双通配符 (`**`) 代替文件夹或文件名来递归匹配任何内容（但它不能与部分名称连用）。 例如，`C:\MyGame\**\Level1\**` 可以选择 `C:\MyGame\Audios\Level1\warp.mp3` 和 `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`。 在打包过程中，也可以使用通配符直接更改文件名 - 即在源和目标之间的不同位置使用通配符。 例如，为 **SourcePath** 指定 `C:\MyGame\Audios\*` 和为 **DestinationPath** 指定 `Sound\copy_*` 可以选择 `C:\MyGame\Audios\UI.mp3` 并使其在包中显示为 `Sound\copy_UI.mp3`。 一般情况下，对于单一 **File** 元素的 **SourcePath** 和 **DestinationPath**，单通配符和双通配符的数量必须相同。


## <a name="advanced-packaging-layout-example"></a>高级包布局示例

下面是一个更复杂的包布局示例：

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

该示例与前面的简单示例不同，它增加了 **ResourcePackage** 和 **Optional** 元素。

可以使用 **ResourcePackage** 元素指定资源包。 在 **ResourcePackage** 中，必须使用 **Resources** 元素指定资源包的资源限定符。 资源限定符是资源包支持的资源，在本例中，我们可以看到定义了两个资源包，每个资源包都包含英语和法语特定文件。 资源包可以有多个限定符，这可通过在 **Resources** 中添加其他 **Resource** 元素实现。 如果存在维度（维度为语言、比例、dxfl），则还必须为资源维度指定默认资源。 在本例中，我们可以看到默认语言为英语，这意味着对于没有设置法语系统语言的用户，他们将回退下载英语资源包并以英语显示。


每个可选包都有自己独特的包系列名称，必须使用 **PackageFamily** 元素进行定义，同时将 **Optional** 属性指定为 **true**。 **RelatedSet** 属性用于指定可选包是否在相关集中（默认为 true）- 可选包是否应随主包一起更新。

**PrebuiltPackage**元素用于添加包布局以包含或引用要构建的应用包文件中未定义的包。 在此情况下，另一个 DLC 可选包包含在此处，以便主应用程序包文件可以引用它并使其成为相关集的一部分。


## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>使用包布局和 MakeAppx.exe 构建应用包
为应用准备好包布局后，就可以开始使用 MakeAppx.exe 构建应用的包了。 要构建包布局中定义的所有包，请使用以下命令：

``` example 
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

但是，如果你正在更新应用，而某些包的文件没有任何更改，则你可以只构建发生更改的包。 以本页面上的简单包布局示例为例，要构建 x64 体系结构程序包，我们可以输入以下命令：

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

`/id` 标志可用于从包布局中选择要构建的包，它与布局中的 **ID** 属性相对应。 在本例中，`/ip` 用于指示包的以前版本所在的位置。 必须提供以前的版本，因为应用捆绑包文件仍需要引用以前版本的**媒体**包。 `/iv` 标志用于自动递增要构建的包的版本（而不是在 **AppxManifest** 中更改版本）。 或者，也可以使用 `/pv` 和 `/bv` 开关分别直接提供包版本（用于要创建的所有包）和捆绑包版本（用于要创建的所有捆绑包）。
以本页面上的高级包布局示例为例，要只构建 **Themes** 可选捆绑包和它引用的 **Themes.main** 应用包，可以使用以下命令：

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

`/bc` 标志用于表示还应该构建 **Themes** 捆绑包的子项（在本例中，这将构建 **Themes.main**）。 `/nbp` 标志用于表示不应该构建 **Themes** 捆绑包的父项。 **Themes** 的父项是主应用程序包 **MyGame**，这是一个可选应用程序包。 通常，对于相关集中的可选包，还必须构建主应用程序包以便能够安装可选包，因为当可选包位于相关集中时，主应用程序包也会引用它（以确保在主包和可选包之间实施版本控制）。 下图说明了包之间的父子关系：

![包布局图](images/packaging-layout.png)
