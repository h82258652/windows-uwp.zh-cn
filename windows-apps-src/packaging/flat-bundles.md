---
title: 平面捆绑应用包
description: 介绍如何创建平面捆绑包来将你的应用的 .appx 包文件与对应用包的引用进行绑定。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, 打包, 包配置, 平面捆绑包
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638482"
---
# <a name="flat-bundle-app-packages"></a>平面捆绑应用包 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用平面捆绑包。

平面捆绑包是改进的方式用于捆绑应用程序的包文件。 应用捆绑包文件使用的应用包文件需要包含在绑定内的多层打包结构典型 Windows，平面捆绑包不需要通过仅引用应用包文件，从而允许这些外部应用程序捆绑包。 应用包文件不再包含在绑定中，因为它们可以是并行处理，这会导致减少上载时间、 更快地发布 （因为可以在同一时间处理每个应用包文件），并最终更快地开发迭代。

![平面捆绑包示意图](images/bundle-combined.png)

平面捆绑包的另一个好处是减少了包的数量。 由于仅引用应用包文件时，两个版本的应用程序可以引用同一个包文件，如果包未更改跨两个版本。 这样，在为应用的下一个版本构建包时，你只需要创建发生了更改的应用包。
默认情况下，平面捆绑包将引用作为本身在同一文件夹中的应用包文件。 但是，该引用可以更改为其他路径（相对路径、网络共享和 http 位置）。 要执行该操作，你必须在创建平面捆绑包期间直接提供 **BundleManifest**。 

## <a name="how-to-create-a-flat-bundle"></a>如何创建平面捆绑包

可以使用 MakeAppx.exe 工具或使用包布局定义捆绑包结构来创建平面捆绑包。

### <a name="using-makeappxexe"></a>使用 MakeAppx.exe
若要创建使用 MakeAppx.exe 平面捆绑包，使用"MakeAppx.exe 捆绑包"命令像往常一样，但使用 /fb 开关生成平面应用捆绑包文件 （它将是极小，因为它仅引用应用包文件，不包含任何实际的有效负载). 

以下是命令语法示例：

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

有关使用 MakeAppx.exe 的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

### <a name="using-packaging-layout"></a>使用包布局
或者，你也可以使用包布局创建平面捆绑包。 要执行该操作，请在应用程序包清单的 **PackageFamily** 元素中将 **FlatBundle** 属性设置为 **true**。 要了解有关包布局的详细信息，请参阅[用包布局创建程序包](packaging-layout.md)。

## <a name="how-to-deploy-a-flat-bundle"></a>如何部署平面捆绑包 
每个应用包（除应用程序包以外）都需要使用相同的证书进行签名，然后才可以部署平面捆绑包。 这是因为所有应用包文件 (.appx/.msix) 现在是独立的文件并没有包含在应用捆绑包 (.appxbundle/.msixbundle) 文件不再。 一旦将包进行签名，使用[Add-appxpackage cmdlet](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps)在 PowerShell 中以指向应用程序捆绑包文件并将其部署应用程序 （假设应用程序包应用程序捆绑包而需要它们）。 