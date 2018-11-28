---
description: 在 XAML 标记中，指定 x:Bind 的默认模式。
title: xDefaultBindMode 属性
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8917b09f04206a5466797f48414defeb35baf5e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7970071"
---
# <a name="xdefaultbindmode-attribute"></a>x:DefaultBindMode 属性

在 XAML 标记中，指定 x:Bind 的默认模式。

**x:DefaultBindMode** 自 Windows 10 版本 1607（周年更新）SDK 版本 14393 起开始可用。

## <a name="xaml-attribute-usage"></a>XAML 属性用法

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>备注

[x:Bind](x-bind-markup-extension.md) 的默认模式为 **OneTime**。 选择它是出于性能原因，因为使用 **OneWay** 将导致生成更多代码以连接到和处理更改检测。 可以使用 **x:DefaultBindMode** 针对标记树的特定段更改 x:Bind 的默认模式。 指定的模式将应用该元素及其子元素上的任何 x:Bind 表达式，不明确指定某个模式作为绑定的一部分。

## <a name="related-topics"></a>相关主题

* [x:Bind 标记扩展](x-bind-markup-extension.md)
