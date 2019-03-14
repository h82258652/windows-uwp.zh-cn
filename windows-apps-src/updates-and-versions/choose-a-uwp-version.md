---
title: 选择 UWP 版本
description: 在 Microsoft Visual Studio 中编写 UWP 应用时，可以选择要面向的版本。 了解不同的 UWP 版本之间的区别，以及如何在新项目和现有项目中配置你的选择。
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, 版本, 内部版本, Windows, 选择, 更新
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: medium
ms.openlocfilehash: fed67ca6547ec8c8f90bc67a7be1f0d97af475df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623592"
---
# <a name="choose-a-uwp-version"></a>选择 UWP 版本

每个版本的 Windows 10 都具有适用于 UWP 平台的全新和改进功能。 在 Microsoft Visual Studio 中创建 UWP 应用时，可以选择要面向的版本。 使用 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) 的项目的**最低版本**必须为版本 16299 或更高版本。

> [!WARNING]
> 不能在 Visual Studio 2015 中打开当前版本的 Visual Studio 中创建的 UWP 项目。

下表介绍了 Windows 10 的可用版本。 请注意，此表仅适用于构建只在 Windows 10 上受支持的 UWP 应用。 无法开发适用于 Windows 旧版本的 UWP 应用，并且必须[已安装 SDK 的正确内部版本](https://go.microsoft.com/fwlink/?LinkId=821431) 以面向对应的 Windows 版本。 

| 版本 | 描述 |
| --- | --- |
| 生成 17763 （版本 1809年） | 这是 Windows 10 中，在 2018 年 10 月发布的最新版本。 **请注意，您_必须_为了针对此版本的 Windows 使用 Visual Studio 2017。** 此版本中的一些突出功能如下： </br> \* **Windows 机器学习：** Windows 机器学习具有现在正式启动了，为前沿的机器学习模型提供更快地分析和支持等功能。 若要了解有关此平台的详细信息，请参阅 [Windows 机器学习](https://docs.microsoft.com/windows/ai/)。 </br> \* **Fluent 设计：** 已添加到 Windows 10 新功能，例如菜单栏、 命令栏浮出控件和 XAML 属性动画。 请在 [Fluent Design 概述](../design/fluent-design-system/index.md)中查看最新内容。 </br> 有关这些功能和很多在此版本的 Windows 中添加其他功能的信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)上的更深入的页或[什么是 Windows 10 中面向开发人员的新增功能](../whats-new/windows-10-build-17763.md)
| 内部版本 17134（版本 1803） | 这是在 2018 年 4 月发布的 Windows 10 版本。 **请注意，您_必须_为了针对此版本的 Windows 使用 Visual Studio 2017。** 此版本中的一些突出功能如下： </br> \* **Fluent 设计：** 树视图、 下拉刷新和导航视图等新功能已添加到 Windows 10。 请在 [Fluent Design 概述](../design/fluent-design-system/index.md)中查看最新内容。 </br> \* **控制台 UWP 应用：** 你现在可以编写在控制台窗口（如 DOS 或 PowerShell 控制台窗口）中运行的 C++ /WinRT 或 /CX UWP 控制台应用。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-build-17134.md)上的详细信息页面。
| 内部版本 16299（Fall Creators Update 1709 版） | 此版本的 Windows 10 已于 2017 年 10 月发布。 **请注意，您_必须_为了针对此版本的 Windows 使用 Visual Studio 2017。** 此版本中的一些突出功能如下： </br> \* **.NET standard 2.0:** 尽情享受在多个.NET Api 中的大规模增长，将你最喜欢的 NuGet 包和第三方库合并到.NET Standard。 在[此处](https://docs.microsoft.com/dotnet/standard/net-standard)查看更多详细信息并探索文档。 请注意：必须将你的**最低版本**设置为版本 16299 才可访问这些新 API。 </br> \* **Fluent 设计：** 使用 light、 深度、 角度来看和移动来增强您的应用程序并帮助用户专注于重要的 UI 元素。 </br> \* **条件 XAML:** 轻松设置属性和实例化对象根据在运行时，使应用程序进行无缝地跨设备和版本的 API 存在。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-build-16299.md)上的详细信息页面。
| 内部版本 15063（Creators Update 1703 版） | 此版本的 Windows 10 已于 2017 年 3 月发布。 **请注意，你_必须_使用 Visual Studio 2017 以面向此版本的 Windows**。 此版本中的一些突出功能如下：  </br> \* **墨迹分析：** Windows 手写内容现在可以将墨迹笔画分类为书写或绘图笔划和识别的文本、 形状和 basid 布局结构。 </br> \* **Windows.Ui.Composition Api:** 轻松地组合并在您的应用程序之间应用动画。 </br> \* **实时编辑：** 在您的应用程序运行时，编辑 XAML，并查看实时应用的更改。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-build-15063.md)上的详细信息页面。  |
| 内部版本 14393（周年更新 1607 版） | 此版本的 Windows 10 已于 2016 年 7 月发布。 此版本中的一些突出功能如下： </br> \* **Windows 手写内容：** 新 InkCanvas 和 InkToolbar 控件。 </br> \* **Cortana Api:** 使用新 Cortana 操作与您的应用程序的特定函数集成 Cortana 的支持。 </br> \* **Windows Hello:** Microsoft Edge 现在支持 Windows Hello，授予对生物识别身份验证的 web 开发人员访问权限。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-build-14393.md)上的详细信息页面。  |
| 内部版本 10586（11 月更新 1511 版） | 此版本的 Windows 10 已于 2015 年 11 月发布。 突出功能包括引入了用于在 Microsoft Edge 中进行视频通信的 ORTC（对象实时通信）和用于支持应用使用 Windows Hello 人脸身份验证的提供者 API。 [此版本中引入的功能的详细信息。](../whats-new/windows-10-build-10586.md) |
| 内部版本 10240（Windows 10 1507 版） | 这是 Windows 10 的初始发行版本，已于 2015 年 7 月发布。 [此版本中引入的功能的详细信息。](../whats-new/windows-10-build-10240.md) |

我们强烈建议新的开发人员和开发人员始终为一般用户编写的代码使用最新版本的 Windows (17763)。 编写企业应用的开发人员应着重考虑支持较旧的**最低版本**。

## <a name="whats-different-in-each-uwp-version"></a>每个 UWP 版本中有何区别？

Windows 10 的每个连续版本中都提供了适用于 UWP 的全新和更改的 API。 有关哪个版本中添加了哪些功能的特定信息，请参阅 [Windows 10 中面向开发人员的新增功能](../whats-new/windows-10-version-latest.md)。

有关枚举所有设备系列及其版本和所有 API 合约及其版本的参考主题，请参阅[设备系列](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx)和 [API 合约](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx)。

## <a name="net-api-availability-in-uwp-versions"></a>在 UWP 版本的.NET API 可用性

UWP 支持.NET Api，而不考虑可用的有限的子集**目标版本**或**最低版本**的你的项目。 [此页提供有关可用的类型的详细信息](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501(d=robot).aspx)。

如果你想要创建可重用的跨平台库，.NET Standard 支持在 UWP 上。 [.NET 标准文档](https://docs.microsoft.com/dotnet/standard/net-standard)提供的.NET Standard 版本均支持哪些 UWP 的信息。

如果要开发桌面应用程序，请参阅相反[.NET Framework 版本和依赖关系](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)有关.NET framework 可用性的详细信息。

## <a name="choose-which-version-to-use-for-your-app"></a>选择要用于你的应用的版本

在 Visual Studio 的“新的通用 Windows 项目”对话框中，可以选择一个版本作为“目标版本”和“最低版本”。 此外，可以在应用**属性**的*应用程序*部分更改 UWP 应用的**目标版本**和**最低版本**。

* **目标版本**。 这将在你的项目文件中设置 *TargetPlatformVersion* 设置。 这可确定你的应用包清单的 *TargetDeviceFamily@MaxVersionTested* 属性的值。 所选值指定你的项目面向的 UWP 平台版本（以及适用于你的应用的 API 集），因此我们建议你选择最新版本。 有关你的应用包清单的详细信息以及手动配置 TargetDeviceFamily 的一些指南，请参阅 [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903)。
* **最低版本**。 这将在你的项目文件中设置 *TargetPlatformMinVersion* 设置。 这可确定你的应用包清单的 *TargetDeviceFamily@MinVersion* 属性的值。 所选值指定你的项目所适用的 UWP 平台的最低版本。

请注意，你在声明应用于从“最低版本”到“目标版本”范围中适用的任何 Windows 版本。 如果这两个版本为同一版本，则无需执行任何特殊操作。 如果版本不同，请注意以下一些事项。

* 在代码中，可随意（即无需条件检查）调用“最低版本”指定的版本中存在的任何 API。
* 确保在运行“最低版本”的设备上测试你的代码，以便它无需仅在“目标版本”中的 API 即可正常运行。
* 使用“目标版本”的值标识用于编译项目的所有参考（合约 WinMD）。 但是这些参考使你通过对 API 的调用编译代码，这些 API 并不一定存在于你已声明支持的设备上（通过“最低版本”）。 因此，在“最低版本”后引入的任何 API 都需要通过自适应代码调用。 有关自适应代码的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。