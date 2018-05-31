---
author: laurenhughes
title: 平面捆绑应用包
description: 介绍如何创建平面捆绑包来将你的应用的 .appx 包文件与对应用包的引用进行绑定。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 打包, 包配置, 平面捆绑包
ms.localizationpriority: medium
ms.openlocfilehash: 757f95a5f46bad6dbe650b4b552f3de486d84e1b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818240"
---
# <a name="flat-bundle-app-packages"></a>平面捆绑应用包 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用平面捆绑包。

平面捆绑包是一种捆绑应用的 .appx 包文件的改进方式。 典型的 .appxbundle 文件使用多层包结构，其中 .appx 包文件需要包含在捆绑包中，而平面捆绑包只引用 .appx 包文件，因而允许它们位于应用程序包外部，从而消除了这一限制。 由于捆绑包中不再包含 .appx 包文件，因此可以对它们进行并行处理，从而缩短了上传时间，加快了发布速度（因为可以同时处理每个 .appx 包文件），并最终实现了更快的开发迭代。

![平面捆绑包示意图](images/bundle-combined.png)

平面捆绑包的另一个好处是减少了包的数量。 由于只引用了 .appx 包文件，因此如果两个版本的程序包没有差异，则两个版本的应用可以引用相同的程序包文件。 这样，在为应用的下一个版本构建包时，你只需要创建发生了更改的应用包。
默认情况下，平面捆绑包将引用自身所在文件夹中的 .appx 包文件。 但是，该引用可以更改为其他路径（相对路径、网络共享和 http 位置）。 要执行该操作，你必须在创建平面捆绑包期间直接提供 **BundleManifest**。 

## <a name="how-to-create-a-flat-bundle"></a>如何创建平面捆绑包

可以使用 MakeAppx.exe 工具或使用包布局定义捆绑包结构来创建平面捆绑包。

### <a name="using-makeappxexe"></a>使用 MakeAppx.exe
要使用 MakeAppx.exe 创建平面捆绑包，请像往常一样使用“MakeAppx.exe bundle”命令，但使用 /fb 开关生成 .appxbundle 平面文件（该文件只引用 .appx 包，不包含任何实际负载，因而非常小）。 

以下是命令语法示例：

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

有关使用 MakeAppx.exe 的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

### <a name="using-packaging-layout"></a>使用包布局
或者，你也可以使用包布局创建平面捆绑包。 要执行该操作，请在应用程序包清单的 **PackageFamily** 元素中将 **FlatBundle** 属性设置为 **true**。 要了解有关包布局的详细信息，请参阅[用包布局创建程序包](packaging-layout.md)。

## <a name="how-to-deploy-a-flat-bundle"></a>如何部署平面捆绑包 
每个应用包（除应用程序包以外）都需要使用相同的证书进行签名，然后才可以部署平面捆绑包。 这是因为所有应用包文件 (.appx) 现在都是独立文件，并且不再包含在应用程序包 (.appxbundle) 文件中。 给包签名后，在 PowerShell 中使用 [Add-AppxPackage cmdlet](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 指向 .appxbundle 文件并部署应用（假设应用包位于应用程序包指定的位置）。 