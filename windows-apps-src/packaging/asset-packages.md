---
author: laurenhughes
title: 资产包简介
description: 资产包是一种用作应用程序公用文件集中存放位置的程序包，可有效消除整个体系结构程序包中的重复文件。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, 打包, 资产包布局, 资产包
ms.localizationpriority: medium
ms.openlocfilehash: 98980e67d24eb96aa55af7fefe10b5e4c2cdfa67
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7101291"
---
# <a name="introduction-to-asset-packages"></a>资产包简介

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用资产包。

资产包是一种用作应用程序公用文件集中存放位置的程序包，可有效消除整个体系结构程序包中的重复文件。 资产包类似于资源包，它们都旨在包含应用运行所需的静态内容，但不同之处在于，不管用户的系统体系结构、语言或显示比例如何，都需要下载所有资产包。

![资产包捆绑图](images/primary-bundle.png)

资产包中包含所有与体系结构、语言和比例无关的文件，因此利用资产包可减小打包应用的整体大小（因为消除了重复文件），从而帮助你管理本地开发大型应用时所用的磁盘空间并从整体上管理你的应用的程序包。 

### <a name="how-do-asset-packages-affect-publishing"></a>资产包对发布有何影响？
资产包最明显的好处是减小了打包应用的大小。 较小的应用包可减少 Microsoft Store 处理的文件数量，从而加快应用发布过程；但这不是资产包最重要的好处。

创建资产包后，你可以指定是否允许包执行。 由于资产包只应包含与体系结构无关的文件，通常不包含任何 .dll 或 .exe 文件，因此资产包通常不需要执行。 这种区别的重要性在于，在发布过程中，必须扫描所有可执行程序包以确保它们不包含恶意软件，而对于较大的程序包，扫描过程需要更长的时间。 但是，如果将程序包指定为不可执行，则应用的安装将确保该程序包中包含的文件无法执行。 这一保证消除了扫描整个程序包的需要，将大幅减少应用发布（以及更新）期间的恶意软件扫描时间，从而显著加快使用资产包的应用的发布速度。 请注意该[平面捆绑应用包](flat-bundles.md)必须还可用于获得该发布优势，因为这是允许的应用商店才能处理并行每个.appx 或.msix 程序包文件。 


### <a name="should-i-use-asset-packages"></a>我应该使用资产包吗？
更新应用的文件结构以充分发挥资产包的优势可带来切实的好处：减小程序包大小，同时减少开发迭代次数。 如果你的体系结构程序包包含大量公用文件，或者应用的大部分内容由非可执行文件组成，我们强烈建议你投入额外的时间转换为使用资产包。

但值得注意的是，资产包不是实现应用内容可选性的手段。 资产包文件不是可选资源，不管目标设备的体系结构、语言或比例如何，**都将**下载这些文件；如果需要应用支持任何可选内容，你应该利用[可选包](optional-packages.md)来实现。 


### <a name="how-to-create-an-asset-package"></a>如何创建资产包
创建资产包的最简单方法是使用包布局。 但是，也可以使用 MakeAppx.exe 手动创建资产包。 要指定在资产包中包含哪些文件，你需要创建一个“映射文件”。 在下面的示例中，资产包中只有一个文件“Video.mp4”，但你应在此处列出资产包的所有文件。 请注意，与资源包的映射文件相比，资产包省略了 **ResourceMetadata** 中的 **ResourceDimensions** 说明符。

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

使用下面的命令来使用 MakeAppx.exe 创建资产包： 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
这里需要注意的是，AppxManifest 中引用的所有文件（徽标文件）都无法移入资产包 - 此类文件必须复制到所有体系结构程序包。 资产包也不应该包含 resources.pri；MRT 不能用于访问资产包文件。 有关如何访问资产包文件以及为什么资产包要求将你的应用安装到 NTFS 驱动器的详细信息，请参阅[使用资产包和包折叠进行开发](Package-Folding.md)。

要控制是否允许资产包执行，你可以在 AppxManifest 中使用 **Properties** 元素中的 **[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)**。此外，你还必须将 **uap6** 添加到顶层的 **Package** 元素，如下所示： 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 如果不指定 **AllowExecution**，则其默认值为 **true**；将其设置为 **false** 可以加快不含可执行文件的资产包的发布过程。  



