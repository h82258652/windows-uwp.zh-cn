---
author: laurenhughes
title: 平面捆绑应用包
description: 介绍如何创建平面捆绑包来将你的应用的 .appx 包文件与对应用包的引用进行绑定。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, 打包, 包配置, 平面捆绑包
ms.localizationpriority: medium
ms.openlocfilehash: b877996dd5fa32ac764fb587092f501320931527
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656705"
---
# <a name="flat-bundle-app-packages"></a>平面捆绑应用包 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用平面捆绑包。

平面捆绑包是一种捆绑应用包文件的改进的方式。 典型 Windows 应用包文件使用的应用包文件需要包含在捆绑包的多层包结构，平面捆绑包中删除这一需要通过只引用应用包文件，从而允许他们能够在应用程序包之外。 由于捆绑包中不再包含应用包文件，他们可以对它们进行并行处理，从而缩短了上传时间、 更快发布速度 （因为可以同时处理每个应用包文件），并最终加快开发迭代。

![平面捆绑包示意图](images/bundle-combined.png)

平面捆绑包的另一个好处是减少了包的数量。 由于只引用应用包文件，如果包未更改两个版本两个版本的应用可以引用相同的程序包文件。 这样，在为应用的下一个版本构建包时，你只需要创建发生了更改的应用包。
默认情况下，平面捆绑包将引用自身所在文件夹中的应用包文件。 但是，该引用可以更改为其他路径（相对路径、网络共享和 http 位置）。 要执行该操作，你必须在创建平面捆绑包期间直接提供 **BundleManifest**。 

## <a name="how-to-create-a-flat-bundle"></a>如何创建平面捆绑包

可以使用 MakeAppx.exe 工具或使用包布局定义捆绑包结构来创建平面捆绑包。

### <a name="using-makeappxexe"></a>使用 MakeAppx.exe
若要创建使用 MakeAppx.exe 平面捆绑包，请使用"MakeAppx.exe bundle"命令像往常一样，但使用 /fb 开关生成平面应用捆绑包文件 （这将是非常小，由于只引用应用包文件，不包含任何实际负载). 

以下是命令语法示例：

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

有关使用 MakeAppx.exe 的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

### <a name="using-packaging-layout"></a>使用包布局
或者，你也可以使用包布局创建平面捆绑包。 要执行该操作，请在应用程序包清单的 **PackageFamily** 元素中将 **FlatBundle** 属性设置为 **true**。 要了解有关包布局的详细信息，请参阅[用包布局创建程序包](packaging-layout.md)。

## <a name="how-to-deploy-a-flat-bundle"></a>如何部署平面捆绑包 
每个应用包（除应用程序包以外）都需要使用相同的证书进行签名，然后才可以部署平面捆绑包。 这是因为所有应用包文件 (.appx/.msix) 现在是独立的文件并且不包含在应用程序包 (.appxbundle/.msixbundle) 文件不再。 程序包签名后， [Add-appxpackage cmdlet](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps)在 PowerShell 中使用指向应用程序包文件并部署应用 （假设应用包位于应用程序包指定的位置）。 