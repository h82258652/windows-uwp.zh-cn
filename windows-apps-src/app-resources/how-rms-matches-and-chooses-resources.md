---
author: stevewhims
Description: When a resource is requested, there may be several candidates that match the current resource context to some degree. The Resource Management System will analyze all of the candidates and determine the best candidate to return. This topic describes that process in detail and gives examples.
title: 资源管理系统如何匹配和选择资源
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: c7576f98045bce3bcfcee093aa8d61059354d45a
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7569464"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>资源管理系统如何匹配和选择资源
请求资源时，可能有多个候选项在一定程度上匹配当前的资源上下文。 资源管理系统将分析所有候选资源，然后确定要返回的最佳候选资源。 为此，需考虑所有限定符，以便对所有候选资源评级。

在此评级过程中，会为不同限定符提供不同优先级：语言对整体评级的影响最大，其次是对比度，然后是比例等。 对于每个限定符，均需将候选限定符与上下文限定符值比较，以便确定匹配的程度。 如何进行比较取决于限定符。

有关如何完成语言标记匹配的详细信息，请参阅[资源管理系统如何匹配语言标记](how-rms-matches-lang-tags.md)。

对于某些限定符，例如缩放比例和对比度，始终有一些最低的匹配度。 例如，为"缩放比例-100"的上下文匹配"缩放比例-400"某种程度较小，但不以及候选限定为"缩放比例-200"或 （适用于完全匹配）"比例-400"限定的候选项。

但是，对于其他限定符（例如语言或住宅区域），可能有非匹配比较（以及匹配度）。 例如，语言限定为“en-US”的候选项与“en-GB”的上下文部分匹配，但限定为“fr”的候选项完全不匹配。 同样，主区域限定为“155”（西欧）的候选项在一定程度上匹配主区域设置为“FR”的用户的上下文，但限定为“US”的候选项完全不匹配。

在评估候选资源时，如果任何限定符存在非匹配比较，则该候选资源将获得整体非匹配评级，并且不能获选。 这样，在选择最佳匹配时，较高优先级的限定符具有最大权重，但低优先级的限定符会因非匹配排除候选资源。

如果某候选资源根本没有标记为某限定符，则此候选资源相对于此限定符是中性的。 对于所有限定符，中性候选资源始终与上下文限定符值匹配，但与已标记为该限定符并且具有某种匹配度（精确或部分）的所有候选资源相比，只有较低匹配度。 例如，如果我们具有限定为“en-US”、“en”、“fr”的候选项和一个语言中性候选项，那么对于语言限定符值为“en-GB”的上下文，候选项将按照以下顺序评级：“en”、“en-US”、中性和“fr”。 在此情况下，“fr”完全不匹配，而其他候选项在一定程度上匹配。

总体评级过程先从根据最高优先级限定符（即语言）对候选项进行评估开始。 排除非匹配项。 相对于语言的匹配度对其余的候选资源评级。 如果有任何相同的等级，则考虑使用下一个最高优先级限定符（对比度），通过使用对比度的匹配度来区分相同等级的候选资源。 在使用对比度后，可使用比例限定符来区分其余的相同等级，然后依此类推，通过所需的限定符来达到井然有序的评级效果。

如果因为限定符与上下文不匹配而将所有候选项从考虑范围中剔除，则资源加载器进入另一个阶段，查找要显示的默认候选项。 默认候选项在创建 PRI 文件期间进行确定，并且是确保任何运行时上下文始终有一些候选项可供选择所必需的。 如果某个候选项有任何不匹配且不默认的限定符，则将该候选资源永久性地排除出考虑范围。

对于所有仍处于考虑范围之内的候选资源，资源加载程序会查找优先级最高的上下文限定符值，选择匹配度最高的一个或具有最佳默认分数的一个。 任何实际匹配项都被视为优于默认分数。

如果存在关联，则检查次高优先级上下文限定符值且继续执行该过程，直到找到最佳匹配。

## <a name="example-of-choosing-a-resource-candidate"></a>选择候选资源的示例
考虑这些文件。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

并假设这些是当前上下文中的设置。

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

资源管理系统消除三个文件，因为高对比度和德语不匹配设置定义的上下文。 这些候选项被保留。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

对于那些剩余的候选项，资源管理系统使用最高优先级上下文限定符，即语言。 英语资源比法语资源更加匹配，因为英语在设置中列于法语的前面。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

接下来，在资源管理系统使用次高优先级上下文限定符，即缩放比例。 因此，这是返回的资源。

```console
en/images/logo.scale-400.jpg
```

你可以使用高级 [**NamedResource.ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) 方法，按照候选项与上下文设置的匹配度顺序检索所有候选项。 在我们刚刚演示的示例中，**ResolveAll** 按照该顺序返回候选项。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>生成回退选择的示例
考虑这些文件。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

并假设这些是当前上下文中的设置。

```console
User language: de-DE;
Scale: 400
Contrast: High
```

所有文件都不匹配上下文，因此全部被消除。 因此我们输入默认传递，这是在 PRI 文件创建过程中的默认值（请参阅[使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)）。

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

所有匹配当前用户或默认值的资源被保留。

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

资源管理系统使用最高优先级上下文限定符，即语言，返回分数最高的命名资源。

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>重要的 API
* [NamedResource.ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>相关主题
* [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)