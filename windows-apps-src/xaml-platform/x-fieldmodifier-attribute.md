---
author: jwmsft
description: 修改 XAML 编译行为，使指定对象引用的字段被定义有“public”访问权限而不是默认的“private”行为。
title: xFieldModifier 属性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: cad84be24836bc6a33a4ab08f1ca4fa2d9e97512
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.locfileid: "205581"
---
# <a name="xfieldmodifier-attribute"></a>x:FieldModifier 属性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

修改 XAML 编译行为，使指定对象引用的字段被定义有 **public** 访问权限而不是默认的 **private** 行为。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>依存关系

[x:Name 属性](x-name-attribute.md)还必须在同一元素上进行提供。

## <a name="remarks"></a>备注

**x:FieldModifier** 属性的值将随编程语言而变。 有效值为 **private**、**public**、**protected**、**internal** 或 **friend**。 对于 C#、Microsoft Visual Basic 或 Visual C++ 组件扩展 (C++/CX)，你可以将字符串赋值为“public”或“Public”；分析器没有强制规定此属性值的大小写。

**Private** 访问权限是默认设置。

**x:FieldModifier** 仅适合具有 [x:Name 属性](x-name-attribute.md)的元素，因为一旦该名称是公共的，它将用来引用字段。

**注意**  Windows 运行时 XAML 不支持 **x:ClassModifier** 或 **x:Subclass**。

