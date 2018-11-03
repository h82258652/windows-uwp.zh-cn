---
author: stevewhims
Description: This section shows you how to author, package, and consume your app's string, image, and file resources.
title: 应用资源和资源管理系统
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 199f9def3150373fed2b3c7d8e711c1eeda6e721
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5997205"
---
# <a name="app-resources-and-the-resource-management-system"></a>应用资源和资源管理系统


本部分向你演示如何创作、打包和使用应用的字符串、图像和文件资源。 例如，你可以打包文件以及含有游戏级别定义的休闲游戏，并在运行时加载文件。 我们还向你演示让你的资源与应用的逻辑保持独立将方便你轻松地根据不同区域设置、设备显示、辅助功能设置和其他用户和计算机上下文对你的应用进行本地化和自定义。 字符串和图像等资源通常需要存在多种语言、比例和对比度变体。 对于此等资源，你可以获得[资源管理系统](resource-management-system.md)的支持。

应用资源包括两种类型。
- 文件资源是作为文件存储在磁盘上的资源。 文件资源可能包含位图图像、XAML、XML、HTML 或任何其他类型的数据。
- 嵌入式资源是嵌入在一些包含资源文件中的资源。 最常见的示例是嵌入在资源文件中的字符串资源（.resw 或 .resjson）。

有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../design/globalizing/globalizing-portal.md)。

| 文章 | 描述 |
|---------|-------------|
| [资源管理系统](resource-management-system.md) | 在生成时间，资源管理系统创建与你的应用打包在一起的资源的所有不同变体的索引。 在运行时，系统检测有效的用户和计算机设置，并加载最匹配这些设置的资源。 |
| [资源管理系统如何匹配和选择资源](how-rms-matches-and-chooses-resources.md) | 请求资源时，可能有多个候选项在一定程度上匹配当前的资源上下文。 资源管理系统将分析所有这些候选项，并确定要返回的最佳候选项。 本主题详细介绍该过程，并提供了示例。 |
| [资源管理系统匹配语言标记的方式](how-rms-matches-lang-tags.md) | 上一个主题（[资源管理系统如何匹配和选择资源](how-rms-matches-and-chooses-resources.md)）对限定符匹配进行总体概括。 本主题主要对语言标记匹配进行详细介绍。 |
| [定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md) | 本主题说明资源限定符的常规概念、如何使用它们以及每个限定符名称的用途。 |
| [本地化 UI 和应用包清单中的字符串](localize-strings-ui-manifest.md) | 如果你希望应用支持其他显示语言，并且你的代码或 XAML 标记或应用包清单中有字符串文本，则将这些字符串移到资源文件 (.resw) 中。 然后，你可以针对应用支持的每种语言制作该资源文件的翻译副本。 |
| [加载为比例、主题、高对比度和其他定制的图像和资产](images-tailored-for-scale-theme-contrast.md) | 应用可以加载含有为显示比例系数、主题、高对比度和其他运行时上下文定制的图像的图像资源文件。 |
| [URI 方案](uri-schemes.md) | 你可以使用几种 URI（统一资源标识符）方案引用来自应用包、应用的数据文件夹或云的文件。 还可以使用 URI 方案引用从应用的资源文件 (.resw) 加载的字符串。 |
| [指定应用使用的默认资源](specify-default-resources-installed.md) | 如果应用不具有与客户设备的特定设置相匹配的资源，则使用应用的默认资源。 本主题介绍如何指定这些默认资源是什么。 |
| [将资源构建到你的应用包而非资源包](build-resources-into-app-package.md) | 某些类型的应用（多语言字典、翻译工具等）需要覆盖应用程序包的默认行为，并将资源构建到应用包，而不是单独的资源程序包。 本主题介绍如何实现该操作。 |
| [包资源索引 (PRI) API 和自定义生成系统](pri-apis-custom-build-systems.md) | 通过[包资源索引 (PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)，你可以为 UWP 应用的资源开发自定义生成系统。 生成系统可以创建、改编和转储（作为 XML）包资源索引 (PRI) 文件，以适应 UWP 应用所需的任何级别的复杂性。 |
| [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md) | MakePri.exe 是可用于创建和转储 PRI 文件的命令行工具。 它作为 MSBuild 的一部分集成到 Microsoft Visual Studio 中，但它可用来手动或使用自定义生成系统创建包。 |
| [在旧应用或游戏中使用 Windows 10 资源管理系统](using-mrt-for-converted-desktop-apps-and-games.md) | 通过将你的 .NET 或 Win32 应用或游戏打包为 AppX 包，你可以利用资源管理系统加载为运行时上下文定制的应用资源。 本主题对方法进行了深入描述。 |

另请参阅[磁贴和 toast 通知的语言、比例和高对比度支持](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。
