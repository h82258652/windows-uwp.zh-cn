---
description: 修改 XAML 编译行为，使指定对象引用的字段被定义有“public”访问权限而不是默认的“private”行为。
title: xFieldModifier 属性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885258"
---
# <a name="xfieldmodifier-attribute"></a>x:FieldModifier 属性


修改 XAML 编译行为，使指定对象引用的字段被定义有 **public** 访问权限而不是默认的 **private** 行为。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>依存关系

[x:Name 属性](x-name-attribute.md)还必须在同一元素上进行提供。

## <a name="remarks"></a>备注

**x:FieldModifier** 属性的值将随编程语言而变。 有效值为 **private**、**public**、**protected**、**internal** 或 **friend**。 对于 C#、 Microsoft Visual Basic 或 VisualC + + 组件扩展 (C + + CX)，你可以为提供字符串值"公共"公共";分析程序不会强制执行此属性值的情况。

**Private** 访问权限是默认设置。

**x:FieldModifier** 仅适合具有 [x:Name 属性](x-name-attribute.md)的元素，因为一旦该名称是公共的，它将用来引用字段。

**注意** **X:classmodifier**或**X:subclass**Windows 运行时 XAML 不支持。

