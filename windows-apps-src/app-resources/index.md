---
author: stevewhims
Description: "本部分向你演示如何创作、打包和使用应用的字符串、图像和文件资源。"
title: "应用资源和资源管理系统"
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 资源, 图像, 资产, MRT, 限定符"
localizationpriority: medium
ms.openlocfilehash: 38a131704bacbffdf89636aa70b405aa30861d27
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2017
---
# <a name="app-resources-and-the-resource-management-system"></a>应用资源和资源管理系统
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

本部分向你演示如何创作、打包和使用应用的字符串、图像和文件资源。 例如，你可以打包文件以及含有游戏级别定义的休闲游戏，并在运行时加载文件。 我们还向你演示让你的资源与应用的逻辑保持独立将方便你轻松地根据不同区域设置、设备显示、辅助功能设置和其他用户和计算机上下文对你的应用进行本地化和自定义。 字符串和图像等资源通常需要存在多种语言、比例和对比度变体。 对于此等资源，你可以获得[资源管理系统](resource-management-system.md)的支持。

应用资源包括两种类型。
- 文件资源是作为文件存储在磁盘上的资源。 文件资源可能包含位图图像、XAML、XML、HTML 或任何其他类型的数据。
- 嵌入式资源是嵌入在一些包含资源文件中的资源。 最常见的示例是嵌入在资源文件中的字符串资源（.resw 或 .resjson）。

有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../globalizing/globalizing-portal.md)。

| 文章 | 描述 |
|---------|-------------|
| [资源管理系统](resource-management-system.md) | 在生成时间，资源管理系统创建与你的应用打包在一起的资源的所有不同变体的索引。 在运行时，系统检测有效的用户和计算机设置，并加载最匹配这些设置的资源。 |
| [资源管理系统如何匹配和选择资源](how-rms-matches-and-chooses-resources.md) | 请求资源时，可能有多个候选项在一定程度上匹配当前的资源上下文。 资源管理系统将分析所有这些候选项，并确定要返回的最佳候选项。 本主题详细介绍该过程，并提供了示例。 |
| [资源管理系统匹配语言标记的方式](how-rms-matches-lang-tags.md) | 上一个主题（[资源管理系统如何匹配和选择资源](how-rms-matches-and-chooses-resources.md)）对限定符匹配进行总体概括。 本主题主要对语言标记匹配进行详细介绍。 |
| [定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md) | 本主题说明资源限定符的常规概念、如何使用它们以及每个限定符名称的用途。 |
| [本地化 UI 和应用包清单中的字符串](localize-strings-ui-manifest.md) | 如果你希望应用支持其他显示语言，并且你的代码或 XAML 标记或应用包清单中有字符串文本，则将这些字符串移到资源文件 (.resw) 中。 然后，你可以针对应用支持的每种语言制作该资源文件的翻译副本。 |
| [加载为比例、主题、高对比度和其他定制的图像和资产](images-tailored-for-scale-theme-contrast.md) | 你的应用可以加载含有为显示比例系数、主题、高对比度和其他运行时上下文定制的图像的图像资源文件。 |
| [磁贴和 toast 通知的语言、比例和高对比度支持](tile-toast-language-scale-contrast.md) | 你的磁贴和 toast 可以加载为显示语言、显示比例系数、高对比度和其他运行时上下文定制的字符串和图像。 |
| [URI 方案](uri-schemes.md) | 你可以使用几种 URI（统一资源标识符）方案引用来自应用包、应用的数据文件夹或云的文件。 你还可以使用 URI 方案引用从应用的资源文件 (.resw) 加载的字符串。 |
| [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md) | MakePri.exe 是可用于创建和转储 PRI 文件的命令行工具。 它作为 MSBuild 的一部分集成到 Microsoft Visual Studio 中，但它可用来手动或使用自定义生成系统创建包。 |
| [MakePri.exe 命令行选项](makepri-exe-command-options.md) | MakePri.exe 具有命令集 `createconfig`、`dump`、`new`、`resourcepack` 和 `versioned`。 本主题对命令行选项的使用进行详细介绍。 |
| [MakePri.exe 配置文件](makepri-exe-configuration.md) | 本主题介绍 MakePri.exe XML 配置文件架构。 |
| [MakePri.exe 特定格式索引器](makepri-exe-format-specific-indexers.md) | 本主题介绍 MakePri.exe 工具生成其资源索引时所使用的特定格式索引器。 |
| [在旧应用或游戏中使用 Windows 10 资源管理系统](using-mrt-for-converted-desktop-apps-and-games.md) | 通过将你的 .NET 或 Win32 应用或游戏打包为 AppX 包，你可以利用资源管理系统加载为运行时上下文定制的应用资源。 本主题对方法进行了深入描述。 |

另请参阅最初为 Windows 8.x 创建但仍然适用于通用 Windows 平台 (UWP) 应用和 Windows 10 的文档。

-   [应用资源和本地化](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)
