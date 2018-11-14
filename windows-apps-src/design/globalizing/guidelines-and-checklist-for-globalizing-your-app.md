---
author: stevewhims
Description: Design and develop your app in such a way that it functions appropriately on systems with different language and culture configurations.
Search.Refinement.TopicID: 180
title: 全球化指南
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.author: stwhi
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 177332515db26eca7cef7a7be75c5752a239a8f1
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6674678"
---
# <a name="guidelines-for-globalization"></a>全球化指南

设计和开发应用，使其能够在具有不同语言和文化配置的系统上正常运行。 使用[**全球化**](/uwp/api/Windows.Globalization?branch=live) API 设置数据格式，并避免在代码中对语言、地区、字符分类、写入系统、日期/时间格式、数字、货币、重量和排序规则进行假设。

| 建议 | 说明 |
| ------------- | ----------- |
| 处理和比较字符串时要考虑文化因素。 | 例如，在比较字符串之前，不更改其大小写。 请参阅[字符串用法建议](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)。 |
| 对字符串和其他数据进行整理（排序）时，请勿假定其总是按字母顺序进行的。 | 对于不使用拉丁语脚本的语言，整理是基于诸如发音、笔划数等因素进行的。 即使是使用拉丁语脚本的语言也不是始终使用字母顺序排序的。 例如，在某些文化中，电话簿可能不是按照字母顺序排序。 Windows 可以为你处理排序问题，但如果自行创建排序算法，请确保将目标市场使用的排序方法考虑在内。 |
| 相应地设置数字、日期、时间、地址和电话号码格式。 | 这些格式因文化、地区、语言和市场而异。 如果要显示这些数据，则使用[**全球化**](/uwp/api/Windows.Globalization?branch=live) API 获取适合特定受众的格式。 请参阅[全球化日期/时间/数字格式](use-global-ready-formats.md)。 家庭和给定名字的显示顺序以及地址格式也可能有所不同。 采用标准日期、时间和数字显示。 采用标准日期和时间选取器控件。 使用标准地址信息。 |
| 支持国际度量单位和货币单位。 | 尽管最流行的度量单位为公制和英制，但不同的国家/地区会采用不同的度量单位和尺度。 如果在处理度量（如长度、温度和面积），请确保支持正确的系统度量。 使用 [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) 属性获取在区域内使用的一组货币。 |
| 使用 Unicode 进行字符编码。 | 默认情况下，Microsoft Visual Studio 针对所有文档使用 Unicode 字符编码。 如果使用的是不同的编辑器，则请确保在适当的 Unicode 字符编码中保存源文件。 所有 UWP API 均返回 UTF-16 编码字符串。 |
| 支持国际纸张尺寸。 | 最常用的纸张尺寸因国家/地区而异，因此如果包含打印等依赖纸张尺寸的功能，则请确保支持和测试常用国际尺寸。 |
| 记录键盘或输入法的语言。 | 应用要求用户输入文本时，请记录当前启用的键盘布局或输入法编辑器（输入法）的语言标记。 这将确保在稍后显示输入时，将为用户采用正确的格式进行显示。 使用 [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) 属性获取当前输入语言。 |
| 请勿通过语言假定用户所在的地区，并且请勿通过地区假定用户的语言。 | 语言和地区是独立的概念。 用户可以使用某种语言的特定区域变体（例如在英国使用的 en-GB 形式的英语），而自身位于完全不同的国家或地区。 请考虑应用是否需要有关用户语言的知识（例如 UI 文本）或有关地区的知识（例如许可）。 有关详细信息，请参阅[了解用户配置文件语言和应用清单语言](manage-language-and-region.md)。 |
| 语言标记的比较规则并不重要。 | [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)非常复杂。 在比较语言标记时会存在许多问题，包括匹配脚本信息、旧标记以及多个地区变体的问题。 Windows 中的资源管理系统负责进行匹配。 你可以指定一组资源（以任何语言），而系统将为用户和应用选择合适的资源。 请参阅[应用资源和资源管理系统](../../app-resources/index.md)及[资源管理系统匹配语言标记的方式](../../app-resources/how-rms-matches-lang-tags.md)。 |
| 设计 UI，使其适应标签和文本输入控件的不同文本长度和字号。 | 不同语言的译文长度可能存在很大差异，因此需要 UI 控件根据其内容动态调整带下。 其他语言中的常用字符包括英语常用字符上方或下方的标记（例如 Å 或 Ņ）。 使用标准字号和行高提供足够的垂直空间。 请注意，其他语言的字体可能需要较大的最小字号才能保持内容的清晰显示。 请参阅 [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live) 命名空间中的类。 |
| 支持读取顺序的镜像。 | 文本对齐和读取顺序可以采用从左到右的方式（如英语采用的方式）或者从右到左 (RTL) 的方式（如阿拉伯语或希伯来语采用的方式）。 如果要将产品本地化为使用与你本身语言读取顺序不同的语言，则请确保 UI 元素的布局支持镜像。 甚至诸如后退按钮、UI 过渡效果以及图像之类的项都可能需要镜像。 有关详细信息，请参阅[调整布局和字体，并支持 RTL](adjust-layout-and-fonts--and-support-rtl.md)。 |
| 正确地显示文本和字体。 | 理想的字体、字号和文本方向因市场而异。 有关详细信息，请参阅[**调整布局和字体，并支持 RTL**](adjust-layout-and-fonts--and-support-rtl.md)及[国际字体](loc-international-fonts.md)。 |

## <a name="important-apis"></a>重要的 API
 
* [全球化](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>相关主题

* [字符串用法建议](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [全球化日期/时间/数字格式](use-global-ready-formats.md)
* [了解用户配置文件语言和应用清单语言](manage-language-and-region.md)
* [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [应用资源和资源管理系统](../../app-resources/index.md)
* [资源管理系统匹配语言标记的方式](../../app-resources/how-rms-matches-lang-tags.md)
* [调整布局和字体并支持 RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [国际字体](loc-international-fonts.md)
* [对应用进行可本地化处理](prepare-your-app-for-localization.md)

## <a name="samples"></a>示例

* [全球化首选项示例](http://go.microsoft.com/fwlink/p/?linkid=231608)
