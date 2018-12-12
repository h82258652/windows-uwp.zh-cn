---
description: 通过计算对来源于自定义资源查找实现的资源的引用，为任何 XAML 属性提供值。 资源查找是通过 CustomXamlResourceLoader 类实现执行的。
title: CustomResource 标记扩展
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7eabcb188aa1687d36d4b4e6f432783aa68969de
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8934044"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 标记扩展


通过计算对来源于自定义资源查找实现的资源的引用，为任何 XAML 属性提供值。 资源查找是通过 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 类实现执行的。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 说明 |
|------|-------------|
| 键 | 所请求资源的键。 键的初始分配方式特定于当前注册使用的 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 类的实现。 |

## <a name="remarks"></a>备注

**CustomResource** 这种技术可获取在自定义资源存储库中的其他地方定义的值。 此技术相对比较高级，大多数 Windows 运行时应用方案都没有使用此技术。

本主题中不介绍 **CustomResource** 解析资源词典的方法，因为解析方法会随 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 实现方法的不同而有很大的不同。

只要在标记中遇到使用 `{CustomResource}` 的情况，就由 Windows 运行时 XAML 分析程序调用 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 实现的 [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 方法。 传递给 **GetResource** 的 *resourceId* 来自 *key* 参数，其他输入参数来自上下文（例如使用情况所应用的属性）。

默认情况下，`{CustomResource}` 使用情况不起作用。（[**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 的基本实现不完整）。 要进行有效的 `{CustomResource}` 引用，必须执行以下每一个步骤：

1.  从 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 派生自定义类，并替代 [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 方法。 不要在实现中调用基类。
2.  设置 [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328) 以在初始化逻辑中引用你的类。 必须在加载包括 `{CustomResource}` 扩展在内的任何页面级 XAML 之前执行此操作。 设置 **CustomXamlResourceLoader.Current** 的一个位置是，App.xaml 代码隐藏模板中为你生成的 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 子类构造函数。
3.  现在你可以在你的应用加载为页面的 XAML 中，或从 XAML 资源词典内使用 `{CustomResource}` 扩展。

**CustomResource** 是标记扩展。 当需要将属性值转义为除文字值或处理程序名称之外的值时，以及当需求更具全局性而不是仅仅将类型转换器放在某些类型或属性上时，通常需要实现标记扩展。 XAML 中的所有标记扩展在其属性语法中都使用“\{”和“\}”字符，通过此约定，XAML 处理器可以知道标记扩展必须处理属性。

## <a name="related-topics"></a>相关主题

* [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)

