---
description: 在 XAML 标记中，为属性指定 null 值。
title: xNull 标记扩展
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0d6fd8f194a3c9c98fb969034cab5a3e9e2f0de
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619622"
---
# <a name="xnull-markup-extension"></a>{x:Null} 标记扩展


在 XAML 标记中，为属性指定 **null** 值。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>备注

对于 C# 和 C++，表示空引用的关键字是 **null**。 Microsoft Visual Basic 的 null 引用关键字是 **Nothing**。

初始默认值在不同的依赖属性之间可能会有所不同，且不一定为 **null**。 此外，许多依赖属性不会接受 **null** 作为值（无论是通过标记还是代码），这归因于其内部实现。 在此情况下，使用 **{x:Null}** 设置 XAML 属性值可能会导致分析器异常。

某些 Windows 运行时类型可为空。 在可空类型尚未使用 **null** 作为默认值时，可以在 XAML 中使用 **{x:Null}** 设置为 **null** 值。 如果使用 Visual c + + 组件扩展 (C + + /cli CX)，可以为 null 的类型表示为[ **platform:: ibox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx)。 如果使用的是 Microsoft .NET 语言，则可空类型表示为 [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)。

## <a name="related-topics"></a>相关主题

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 

