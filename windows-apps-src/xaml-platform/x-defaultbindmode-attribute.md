---
author: tbd
description: "在 XAML 标记中，指定 x:Bind 的默认模式"
title: "xDefaultBindMode 标记扩展"
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 0fa037b4c59566cb1b9bacd4d2e36520a86c508d
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2017
---
# <a name="xdefaultbindmode-markup-extension"></a>{x:DefaultBindMode} 标记扩展

\[ 已针对 Windows10 上的 UWP 应用更新。 有关 Windows8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 XAML 标记中，指定 x:Bind 的默认模式。

## <a name="xaml-attribute-usage"></a>XAML 特性用法

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>备注

x:Bind 具有默认的 OneTime 模式，选择它是出于性能原因，因为使用 OneTime 将导致生成更多代码以连接到和处理更改检测。 **x:DefaultBindMode** 可用于针对标记树的特定段更改 x:Bind 的默认模式。 所选的模式将应用该元素及其子元素上的任何 x:Bind 表达，不明确指定某个模式作为绑定的一部分。

## <a name="related-topics"></a>相关主题

* [**x:Bind**](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)
