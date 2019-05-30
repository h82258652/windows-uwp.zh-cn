---
Description: 本部分展示了如何创作、打包和使用应用的字符串、图像和文件资源。
title: 应用资源和资源管理系统
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: f40a68b6e07a90bcb1ce1d646bdfc0dfb4553495
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359420"
---
# <a name="app-resources-and-the-resource-management-system"></a>应用资源和资源管理系统


本部分展示了如何创作、打包和使用应用的字符串、图像和文件资源。 例如，你可以打包文件以及含有游戏级别定义的休闲游戏，并在运行时加载文件。 我们还展示了使资源独立于应用的逻辑将如何有助于你根据不同的区域设置、设备显示、辅助功能设置以及其他用户和计算机上下文轻松地对你的应用进行本地化和自定义。 字符串和图像等资源通常需要存在多种语言、比例和对比度变体。 对于此类资源，你可以获得[资源管理系统](resource-management-system.md)的支持。

有两种类型的应用资源。
- 文件资源是作为文件存储在磁盘上的资源。 文件资源可能包含位图图像、XAML、XML、HTML 或任何其他类型的数据。
- 嵌入式资源是嵌入在一些包含资源文件中的资源。 最常见的示例是嵌入在资源文件中的字符串资源（.resw 或 .resjson）。

有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../design/globalizing/globalizing-portal.md)。

| 文章 | 描述 |
|---------|-------------|
| [资源管理系统](resource-management-system.md) | 在生成时，资源管理系统会创建与你的应用打包在一起的资源的所有不同变体的索引。 在运行时，系统会检测有效的用户和计算机设置，并加载最匹配这些设置的资源。 |
| [资源管理系统如何匹配和选择资源](how-rms-matches-and-chooses-resources.md) | 请求资源时，可能有多个候选项在一定程度上匹配当前的资源上下文。 资源管理系统将分析所有这些候选项，并确定要返回的最佳候选项。 本主题详细介绍了该过程，并提供了示例。 |
| [资源管理系统如何匹配语言标记](how-rms-matches-lang-tags.md) | 上一个主题（[资源管理系统如何匹配和选择资源](how-rms-matches-and-chooses-resources.md)）对限定符匹配进行了总体概括。 本主题主要对语言标记匹配进行详细介绍。 |
| [定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md) | 本主题介绍了资源限定符的常规概念、如何使用它们以及每个限定符名称的用途。 |
| [对 UI 和应用包清单中的字符串进行本地化](localize-strings-ui-manifest.md) | 如果你希望应用支持其他显示语言，并且你的代码或 XAML 标记或应用包清单中有字符串文本，则将这些字符串移到资源文件 (.resw) 中。 然后，你可以针对应用支持的每种语言制作该资源文件的翻译副本。 |
| [加载在比例、主题、高对比度和其他方面进行了定制的图像和资产](images-tailored-for-scale-theme-contrast.md) | 应用可以加载在显示比例系数、主题、高对比度和其他运行时上下文方面进行了定制的图像所在的图像资源文件。 |
| [URI 方案](uri-schemes.md) | 你可以使用几种 URI（统一资源标识符）方案引用来自应用包、应用的数据文件夹或云的文件。 还可以使用 URI 方案引用从应用的资源文件 (.resw) 加载的字符串。 |
| [指定应用使用的默认资源](specify-default-resources-installed.md) | 如果应用没有与客户设备的特定设置相匹配的资源，则会使用应用的默认资源。 本主题介绍了如何指定这些默认资源。 |
| [将资源构建到你的应用包而非资源包中](build-resources-into-app-package.md) | 某些类型的应用（多语言字典、翻译工具等）需要覆盖应用程序包的默认行为，并将资源构建到应用包中，而不是构建到单独的资源包中。 本主题介绍了如何执行该操作。 |
| [包资源索引 (PRI) API 和自定义生成系统](pri-apis-custom-build-systems.md) | 通过[包资源索引 (PRI) API](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)，你可以为 UWP 应用的资源开发自定义生成系统。 生成系统可以创建、改编和转储（作为 XML）包资源索引 (PRI) 文件，以适应 UWP 应用所需的任何级别的复杂性。 |
| [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md) | MakePri.exe 是可用于创建和转储 PRI 文件的命令行工具。 它作为 MSBuild 的一部分集成到 Microsoft Visual Studio 中，但它可用于手动或使用自定义生成系统来创建包。 |
| [在旧应用或游戏中使用 Windows 10 资源管理系统](using-mrt-for-converted-desktop-apps-and-games.md) | 通过将你的 .NET 或 Win32 应用或游戏打包为 AppX 包，你可以利用资源管理系统加载为运行时上下文定制的应用资源。 本主题对方法进行了深入描述。 |

另请参阅[磁贴和 toast 通知的语言、比例和高对比度支持](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。
