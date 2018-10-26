---
author: TylerMSFT
title: 用服务、扩展和包扩展应用
description: 了解如何创建在通用 Windows 平台 (UWP) 应用商店应用更新时运行的后台任务。
ms.author: twhitney
ms.date: 05/7/2018
ms.topic: article
keywords: windows 10, uwp, 扩展, 组件化, 应用服务, 包, 扩展
ms.localizationpriority: medium
ms.openlocfilehash: 624d52ff96fb2537afa3affb2d842aa29e1e6667
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597659"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>用服务、扩展和包扩展应用

Windows 10 中有不同的技术可以帮助你扩展应用和实现应用组件化。 此表可帮助你确定对你的方案使用哪种技术。 后面是方案和技术的简要说明。

| 方案                           | 资源包   | 资产包      | 可选包   | 平面捆绑包        | 应用扩展      | 应用服务        | 流式安装  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| 第三方代码插件            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| 进程内代码插件              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX 资产（字符串/映像）         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 按需内容 <br/> （例如其他级别） |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 单独许可和购买 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| 应用内购买                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| 优化安装时间              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 减少磁盘占用空间              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| 优化打包                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| 缩短发布时间             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>方案说明（上表中的行）

**第三方插件**  

你可以从 Microsoft Store 下载并从应用内运行的代码。 例如，Microsoft Edge 浏览器的扩展。

**进程内代码插件**  

在进程内与应用一起运行的代码。 仅支持 C++。 还可能包括内容。 由于代码在进程内运行，所以假定更高级别的信任。 你可以选择不向第三方开放此类扩展。

**UX 资产（字符串/映像）**  

你要基于区域设置或任何其他原因计入因素的用户界面资产，如本地化的字符串、图像和任何其他 UI 内容。

**按需内容**  

你要在以后下载的内容。 例如，允许你下载新级别、外观或功能的应用内购买。

**单独许可和购买**  

独立于应用许可和获取内容的能力。

**应用内购买**  

指示是否提供编程支持，以用于从应用中获取内容。

**优化安装时间**

提供功能用于减少从 Microsoft Store 获取应用并开始运行所需的时间。

**减少磁盘占用空间** 通过只包含必要的应用或资源来减少应用大小。

**优化打包** 优化大型或复杂应用的应用打包过程。

**缩短发布时间** 最大限度减少在 Microsoft Store、本地共享或 Web 服务器中发布应用所需的时间。

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>技术说明（上表中的列）

**资源包**

资源包是仅资产包，使你的应用能够适应多个屏幕大小和系统语言。 资源包面向用户语言、系统规模和 DirectX 功能，使应用可根据多种用户方案进行定制。 尽管应用包可以包含若干资源，但操作系统只根据用户设备下载相关资源，从而节省带宽和磁盘空间。

**资产包** 资产包是应用使用的可执行文件或非可执行文件的公用集中源。 这些通常是非处理器或语言特定的文件。 例如，可以在一个资产包中包含一系列图片，在另一个资产包中包含视频，两种资产都由同一个应用使用。 例如，可以在一个资产包中包含一系列图片，在另一个资产包中包含视频。 如果应用支持多种体系结构和多种语言，则可以将这些资产包含在体系结构包或资源包中，但这也意味着资产将在不同的体系结构包中重复包含多次，占用更多的磁盘空间。 如果使用资产包，则只需将其包含在整个应用包中一次。 有关详细信息，请参阅[资产包简介](../packaging/asset-packages.md)。

**可选包**

可选包用于补充或扩展应用包的原始功能。 可先发布应用，晚些时候再发布可选包，或同时发布应用和可选包。 通过可选包来扩展应用，你将拥有将内容作为单独的应用包来分发和盈利的优势。 可选包通常由原始应用开发人员来开发，因为它们用主应用的标识来运行（与应用扩展不同）。 根据定义可选包的方式，你可以从可选包向主应用中加载代码、资产或代码和资产。 如果你要通过可以单独盈利、许可和分发的内容来增强应用，那么可选包是你的正确选择。 有关实现详细信息，请参阅[可选包和相关集创作](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)。

**平面捆绑包**
[平面捆绑应用包](../packaging/flat-bundles.md)与常规应用程序包相似，不同之处在于平面捆绑包不在文件夹中包含所有应用包，而是只包含这些应用包的*引用*。 由于平面捆绑包包含应用包的引用而不是文件本身，因而可减少打包和下载应用所需的时间。

**应用扩展**

[应用扩展](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)允许 UWP 应用托管由其他 UWP 应用提供的内容。 从这些应用发现、枚举和访问只读内容。

如果应用支持扩展，任何开发人员均可为该应用提交扩展。 因此，在加载未经过预测试的扩展时，主机应用需要非常可靠。 应将扩展视为不受信任。

应用程序不能从扩展加载代码。 如果你需要执行代码，请考虑应用服务。

**应用服务**

Windows 应用服务通过允许 UWP 应用向其他通用 Windows 应用提供服务来实现应用到应用的通信。 应用服务允许你创建应用可在同一设备上调用的无 UI 服务，从 Windows 10 版本 1607 开始，应用可在远程设备上调用这些服务。 有关详细信息，请参阅[创建和使用应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

应用服务是可向其他 UWP 应用提供服务的 UWP 应用。 它们与设备上的 Web 服务类似。 应用服务作为后台任务在主机应用中运行，并可向其他应用提供其服务。 例如，应用服务可能会提供其他应用可能使用的条形码扫描仪服务。 应用的企业套件中可能有一个通用的拼写检查应用服务，该服务可供套件中的其他应用使用。

**UWP 应用流式安装**

流式安装是可对向用户交付应用的途径进行优化的方式。 无需等待整个应用下载完成再使用，只要下载完所需部分，用户便可开始使用应用。 完全由作为开发人员的你来决定，将应用分段为用于基本激活的必需部分，并为应用的其余部分启动其他内容。 有关详细信息和实现详细信息，请参阅 [UWP 应用流式安装](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)。

## <a name="see-also"></a>另请参阅

[创建和使用应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[资产包简介](../packaging/asset-packages.md)  
[使用包布局创建包](../packaging/packaging-layout.md)  
[可选包和相关集创作](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[用资产包和包折叠进行开发](../packaging/package-folding.md)  
[UWP 应用流式安装](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[平面捆绑应用包](../packaging/flat-bundles.md)  
[Windows.ApplicationModel.AppService 命名空间](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Windows.ApplicationModel.Extensions 命名空间](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  