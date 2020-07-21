---
title: 选择 UWP 版本
description: 在 Microsoft Visual Studio 中编写 UWP 应用时，可以选择要面向的版本。 了解不同的 UWP 版本之间的区别，以及如何在新项目和现有项目中配置你的选择。
ms.date: 5/12/2019
ms.topic: article
keywords: Windows 10, uwp, 版本, 内部版本, Windows, 选择, 更新
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: medium
ms.openlocfilehash: db336eedd2ce683196c8d4914f0ac661cb3ed088
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234773"
---
# <a name="choose-a-uwp-version"></a>选择 UWP 版本

每个版本的 Windows 10 都具有适用于 UWP 平台的全新和改进功能。 在 Microsoft Visual Studio 中创建 UWP 应用时，可以选择要面向的版本。 使用 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) 的项目的**最低版本**必须为版本 16299 或更高版本。

> [!WARNING]
> 无法在 Visual Studio 2015 中打开在当前版本的 Visual Studio 中创建的 UWP 项目。

下表描述了 Windows 10 的可用版本。 请注意，此表仅适用于构建只在 Windows 10 上受支持的 UWP 应用。 无法开发适用于 Windows 旧版本的 UWP 应用，并且必须[已安装 SDK 的正确内部版本](https://developer.microsoft.com/windows/downloads#_blank)以面向对应的 Windows 版本。

| 版本 | 说明 |
| --- | --- |
| 内部版本 19041（版本 2004） | 这是最新版本的 Windows 10，已于 2020 年 5 月发布。 此版本的突出功能包括： </br> \* **WSL2：** 适用于 Linux 的 Windows 子系统已使用新的体系结构模型进行了更新，现在可在 Windows 上运行实际的 Linux 内核。 有关详细信息，请参阅[关于 WSL2](/windows/wsl/wsl2-about)。 </br> \* **MSIX：** Windows 中的新功能为新式 MSIX 应用包格式提供了进一步支持，包括创建包含服务的包的能力、创建托管应用，以及在非封装应用中包含需要包标识的功能的能力。 有关详细信息，请参阅 [MSIX 文档](/windows/msix/overview)。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的详细信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-build-19041.md)上的详细信息页面
| 内部版本 18362（版本 1903） | 此版本的 Windows 10 已于 2019 年 4 月发布。 此版本中的一些突出功能如下： </br> \* **XAML 岛：** 现在，Windows 10 支持在非 UWP 桌面应用程序中使用 UWP 控件。 如果你正在进行 WPF、Windows 窗体或 C++ Win32 方面的开发，请[查看如何将最新的 Windows 10 UI 功能添加到现有应用](../xaml-platform/xaml-host-controls.md)。 </br> \* **适用于 Linux 的 Windows 子系统：** 现在，可以直接在 Windows 中访问 Linux 文件，并使用多个新的命令行选项。 请参阅[关于 WSL](https://docs.microsoft.com/windows/wsl/about.md) 中的最新信息。 </br> 有关此版本的 Windows 中添加的这些功能和许多其他功能的信息，请访问[内部版本 18362 中的新增功能](../whats-new/windows-10-build-18362.md)
| 内部版本 17763（版本 1809） | 此版本的 Windows 10 已于 2018 年 10 月发布。 **请注意，必须使用 Visual Studio 2017 或 Visual Studio 2019 才能以此版 Windows 为目标。**  此版本中的一些突出功能如下： </br> \* **Windows 机器学习：** Windows 机器学习现已正式推出，为尖端机器学习模型提供更快的评估和支持等功能。 若要了解有关此平台的详细信息，请参阅 [Windows 机器学习](https://docs.microsoft.com/windows/ai/)。 </br> \* **Fluent Design：** 菜单栏、命令栏浮出控件和 XAML 属性动画等新功能已添加到 Windows 10。 请参阅[流畅设计概述](../design/fluent-design-system/index.md)中的最新信息。 </br> 有关此版本的 Windows 中添加的这些功能和许多其他功能的信息，请访问[内部版本 17763 中的新增功能](../whats-new/windows-10-build-17763.md)
| 内部版本 17134（版本 1803） | 此版本的 Windows 10 已于 2018 年 4 月发布。 **请注意，必须使用 Visual Studio 2017 或 Visual Studio 2019 才能以此版 Windows 为目标。**  此版本中的一些突出功能如下： </br> \* **Fluent Design：** 树视图、下拉刷新和导航视图等新功能已添加到 Windows 10。 请参阅[流畅设计概述](../design/fluent-design-system/index.md)中的最新信息。 </br> \* **控制台 UWP 应用：** 现在可以编写在控制台窗口（如 DOS 或 PowerShell 控制台窗口）中运行的 C++ /WinRT 或 /CX UWP 控制台应用。 </br> 有关此版本的 Windows 中添加的这些功能和许多其他功能的信息，请访问[内部版本 17134 中的新增功能](../whats-new/windows-10-build-17134.md)
| 内部版本 16299（Fall Creators Update 版本 1709） | 此版本的 Windows 10 已于 2017 年 10 月发布。 **请注意，必须使用 Visual Studio 2017 或 Visual Studio 2019 才能以此版 Windows 为目标。**  此版本中的一些突出功能如下： </br> \* **.NET Standard 2.0：** 显著增加了 .NET API，并在 .NET Standard 中融入了最常用的 NuGet 程序包以及第三方库。 在[此处](https://docs.microsoft.com/dotnet/standard/net-standard)查看更多详细信息并探索文档。 请注意，必须将**最低版本**设置为版本 16299 才可访问这些新 API。 </br> \* **Fluent Design：** 使用光线、深度、透视和移动来改善应用并帮助用户关注重要的 UI 元素。 </br> \* **条件 XAML：** 根据在运行时是否存在 API 轻松设置属性并实例化对象，从而使应用能够跨不同设备和版本无缝运行。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的信息，请访问[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-build-16299.md)
| 内部版本 15063（Creators Update 版本 1703） | 此版本的 Windows 10 已于 2017 年 3 月发布。 **请注意，必须使用 Visual Studio 2017 或 Visual Studio 2019 才能以此版 Windows 为目标。**  此版本中的一些突出功能如下：  </br> \* **墨迹分析：** Windows Ink 现在可将墨迹笔划分类为编写笔划或绘图笔划，并识别文本、形状和基本布局结构。 </br> \* **Windows.Ui.Composition API：** 在整个应用中轻松组合与应用动画。 </br> \* **动态编辑：** 在应用运行的时候编辑 XAML，并可查看实时更改。 </br> 有关此版本的 Windows 中添加的这些功能和许多其他功能的信息，请访问[内部版本 15063 中的新增功能](../whats-new/windows-10-build-15063.md)  |
| 内部版本 14393（周年更新版本 1607） | 此版本的 Windows 10 已于 2016 年 7 月发布。 此版本中的一些突出功能如下： </br> \* **Windows Ink：** 新的 InkCanvas 和 InkToolbar 控件。 </br> \* **Cortana API：** 使用新的 Cortana 操作将 Cortana 支持与应用的特殊功能集成。 </br> \* **Windows Hello：** Microsoft Edge 现在支持 Windows Hello，使 Web 开发人员能够访问生物识别身份验证。 </br> 有关此版本的 Windows 中添加的这些功能和许多其他功能的信息，请访问[内部版本 14393 中的新增功能](../whats-new/windows-10-build-14393.md)  |
| 内部版本 10586（11 月更新版本 1511） | 此版本的 Windows 10 已于 2015 年 11 月发布。 突出功能包括引入了用于在 Microsoft Edge 中进行视频通信的 ORTC（对象实时通信）和用于支持应用使用 Windows Hello 人脸身份验证的提供者 API。 [有关此版本中引入的功能的详细信息。](../whats-new/windows-10-build-10586.md) |
| 内部版本 10240（Windows 10 版本 1507） | 这是 Windows 10 的初始发行版本，已于 2015 年 7 月发布。 [有关此版本中引入的功能的详细信息。](../whats-new/windows-10-build-10240.md) |

我们强烈建议新的开发人员和针对普通受众编写代码的开发人员始终使用最新版本的 Windows (19041)。 编写企业应用的开发人员应着重考虑支持较旧的**最低版本**。

## <a name="whats-different-in-each-uwp-version"></a>每个 UWP 版本中有何区别？

Windows 10 的每个连续版本中都提供了适用于 UWP 的全新和更改的 API。 有关哪个版本中添加了哪些功能的特定信息，请参阅 [Windows 10 中面向开发人员的新增功能](../whats-new/windows-10-version-latest.md)。

有关枚举所有设备系列及其版本和所有 API 合约及其版本的参考主题，请参阅[设备系列](https://docs.microsoft.com/uwp/extension-sdks/)和 [API 合约](https://docs.microsoft.com/uwp/extension-sdks/)。

## <a name="net-api-availability-in-uwp-versions"></a>UWP 版本中的 .NET API 可用性

UWP 支持有限的 .NET API 子集，无论项目的**目标版本**或**最低版本**是什么，都可以使用这些 API。 [此页提供了有关可用类型的详细信息](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)。

若要创建可重用的跨平台库，可以在 UWP 中使用 .NET Standard。 [.NET Standard 文档](https://docs.microsoft.com/dotnet/standard/net-standard)提供了有关哪些版本支持 .NET Standard 的信息。

如果你正在开发桌面应用，请参阅 [.NET Framework 版本和依赖项](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)了解有关 .NET Framework 可用性的详细信息。

## <a name="choose-which-version-to-use-for-your-app"></a>选择要用于你的应用的版本

在 Visual Studio 的“新建通用 Windows 项目”对话框中，可为“目标版本”和“最低版本”选择一个版本  。 此外，可以在应用“属性”的“应用程序”部分，更改 UWP 应用的“目标版本”和“最低版本” 。

* **目标版本**。 要在其上运行应用的 Windows 10 版本。 这将在你的项目文件中设置 *TargetPlatformVersion* 设置。 这可确定应用包清单的 *TargetDeviceFamily@MaxVersionTested* 属性的值。 所选值指定你的项目面向的 UWP 平台版本（以及适用于你的应用的 API 集），因此我们建议你选择最新版本。 有关你的应用包清单的详细信息以及手动配置 TargetDeviceFamily 的一些指南，请参阅 [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)。
* **最低版本**。 支持应用的基本功能所需的最早版本的 Windows 10。 这将在你的项目文件中设置 *TargetPlatformMinVersion* 设置。 这可确定应用包清单的 *TargetDeviceFamily@MinVersion* 属性的值。 所选值指定你的项目所适用的 UWP 平台的最低版本。

请注意，在声明应用于从“最低版本”到“目标版本”范围中适用的任何 Windows 版本 。 如果这两个版本为同一版本，则无需执行任何特殊操作。 如果版本不同，请注意以下一些事项。

* 在代码中，可随意（即无需条件检查）调用“最低版本”指定的版本中存在的任何 API。
* 确保在运行“最低版本”的设备上测试你的代码，以便它无需仅在“目标版本”中的 API 即可正常运行 。
* 使用“目标版本”的值标识用于编译项目的所有参考（合约 WinMD）。 但是这些参考可让你通过对 API 的调用编译代码，这些 API 并不一定存在于你已声明支持的设备上（通过“最低版本”）。 因此，在“最低版本”后引入的任何 API 都需要通过自适应代码调用。 有关自适应代码的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。
