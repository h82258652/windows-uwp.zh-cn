---
Description: 了解有关优势的全球化和本地化您的应用程序，以及完全这些术语的含义。
Search.SourceType: Video
title: 全球化和本地化
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 12/07/2018
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 91396051ec822f469e300f51217191fc4e704cac
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714051"
---
# <a name="globalization-and-localization"></a>全球化和本地化

Windows 的使用遍及世界各地，用户的语言、区域及文化背景各不相同。 用户讲各种不同的语言，位于不同的国家和地区。 某些用户会说多种语言。 因此，应用在涉及语言、区域和文化系统设置的多种排列方式的配置上运行。 通过使用*全球化*和*本地化*对应用进行设计使其更具有适应性，可以拓展其潜在市场。

此视频提供简要介绍了有关如何为世界上准备您的应用程序：[全球化和本地化简介](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization)。

**全球化**是一种应用设计和开发过程，可使应用无需进行特定于文化的更改或自定义，即可在不同的全球市场（不同语言和文化配置的系统上）正常工作。

- 处理字符串时需要考虑文化，例如在比较字符串之前不更改其大小写。
- 使用适用于当前文化的日历。
- 使用全球化 API 来显示针对国家或地区相应地格式化的数据，例如数字、日期、时间以及货币。
- 应考虑到不同的文化对文本及其他数据有不同的整理（排序）规则。

代码需要在确定应用会提供支持的任何文化中同样出色地运行。 理想情况下，代码在*任何*语言、地区或文化背景下都能同样出色地运行。 应用功能全球化的最有效方式是使用文化/区域设置的概念。 文化/区域设置是特定于给定语言和地理区域的一组规则和一组数据。 这些规则和数据包括以下相关信息。

- 字符分类
- 写入系统
- 日期和时间格式设置
- 数字、货币、重量和测量约定
- 排序规则

>[!NOTE]
> Microsoft 使用的标准的国家/地区代码的列表，请参阅[正式国家/地区列表](https://globalready.azurewebsites.net/marketreadiness/OfficialCountryregion)。


**可本地化性**是为本地化准备全球化应用和/或验证应用是否已准备好进行本地化的过程。 以正确方式对应用进行可本地化处理意味着应用在之后的本地化过程中不会出现任何功能缺陷。 可本地化应用最重要的属性是其可执行代码与应用的可本地化资源完全分隔。

- 字符串的不同译文的长度可能会有很大差异。 因此，设计 UI，使其适应标签和文本输入控件的不同文本长度和字号。
- 请尽量避免图像中出现文本和/或文化敏感的材料。
- 请勿对应用的代码和标注中的字符串和文化相关图像进行硬编码。 而是将这些字符串和图像存储为字符串和图像资源，以便它们独立于应用的生成二进制文件之外适应不同本地市场。
- 伪本地化应用，发现任何可本地化性问题。

**本地化**是调整或翻译应用的可本地化资源的过程，其目的是满足应用计划支持的特定本地市场的语言、文化和政治需求。 进入本地化应用的阶段时，如果应用可本地化，则在此过程中无需修改任何逻辑。

- 为新的市场翻译应用的字符串资源和其他资源。
- 根据需要修改任何与文化相关的图像。
- 文件也可能因用户的地区而有所不同，独立于其语言。 例如，根据用户的位置，地图可能具有不同的边框，但标签应该遵循用户的首选语言。

大多数本地化团队在专用工具的帮助下完成这一过程。 例如，循环利用重复文本的翻译。

| 文章 | 描述 |
|---------|-------------|
| [全球化](guidelines-and-checklist-for-globalizing-your-app.md) | 设计和开发应用，使其能够在具有不同语言和文化配置的系统上正常运行。 |
| [了解用户配置文件的语言和应用程序清单语言](manage-language-and-region.md) | 本主题定义了术语“用户配置文件语言列表”、“应用清单语言列表”和“应用运行时语言列表”。 我们将在此主题和此功能区域中的其他主题中使用以上术语，因此了解它们的含义非常重要。 |
| [全球化您的日期/时间/数字格式](use-global-ready-formats.md) | 通过适当设置日期、时间、数字、电话号码和货币的格式，设计全球通用的应用。 稍后即可调整应用，以适应全球市场中更多的文化、区域和语言。 |
| [使用模板和对日期和时间格式模式](use-patterns-to-format-dates-and-times.md) | 结合使用 [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 命名空间中的类及自定义模板和模式，以严格按照所需格式显示日期和时间。 |
| [调整布局和字体并支持 RTL](adjust-layout-and-fonts--and-support-rtl.md) | 设计应用，使其支持多种语言布局和字体，包括 RTL（从右到左）排列方向。 |
| [NumeralSystem 值](glob-numeralsystem-values.md) | 本主题列出了可用于 [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) 命名空间中各种类的 **NumeralSystem** 属性的值。 |
| [使您的应用程序可本地化](prepare-your-app-for-localization.md) | 本地化应用是一种可针对其他市场、语言或地区进行本地化且未发现应用中的任何功能性缺陷的应用。 可本地化应用最重要的属性是其可执行代码与其可本地化资源完全分隔。 |
| [国际字体](loc-international-fonts.md) | 本主题列出了可用于 UWP 应用的已本地化为美国以外的语言的字体（美国）。 |
| [设计应用程序的双向文本](design-for-bidi-text.md) | 设计应用，使其提供双向文本支持（双向），以便组合从左到右和从右到左写入系统的脚本。 |
| [使用多语言应用工具包 4.0](use-mat.md) | 多语言应用工具包 (MAT) 4.0 与 Microsoft Visual Studio 2017 集成，为 UWP 应用提供翻译支持、翻译文件管理和编辑工具。 |
| [多语言应用工具包 4.0 常见问题和故障排除](mat-faq-troubleshooting.md) | 本主题提供有关多语言应用工具包 (MAT) 4.0 的常见问题解答。 |
| [准备用于日语纪元更改应用程序](japanese-era-change.md) | 了解 2019 年 5 月日本纪元更改以及如何使你的应用程序准备就绪。 |
