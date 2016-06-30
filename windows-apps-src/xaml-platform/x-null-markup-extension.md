---
author: jwmsft
description: "在 XAML 标记中，为属性指定 null 值。"
title: "xNull 标记扩展"
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 96ec27fa36d5a30d6bcf3b3c4ad4a330bf799a09

---

# {x&#58;Null} 标记扩展

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 XAML 标记中，为属性指定 **null** 值。

## XAML 属性使用方法

``` syntax
<object property="{x:Null}" .../>
```

## 备注

对于 C# 和 C++，表示空引用的关键字是 **null**。 Microsoft Visual Basic 的 null 引用关键字是 **Nothing**。

初始默认值在不同的依赖属性之间可能会有所不同，且不一定为 **null**。 此外，许多依赖属性不会接受 **null** 作为值（无论是通过标记还是代码），这归因于其内部实现。 在此情况下，使用 **{x:Null}** 设置 XAML 属性值可能会导致分析器异常。

某些 Windows 运行时类型可为空。 在可空类型尚未使用 **null** 作为默认值时，可以在 XAML 中使用 **{x:Null}** 设置为 **null** 值。 如果使用的是 Visual C++ 组件扩展 (C++/CX)，则可空类型表示为 [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx)。 如果使用的是 Microsoft .NET 语言，则可空类型表示为 [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)。

## 相关主题

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 




<!--HONumber=Jun16_HO4-->


