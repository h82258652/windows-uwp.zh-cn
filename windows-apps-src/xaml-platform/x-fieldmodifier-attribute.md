---
author: jwmsft
description: 修改 XAML 编译行为，使指定对象引用的字段被定义有“public”访问权限而不是默认的“private”行为。
title: xFieldModifier 属性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de1d7dedbd2bd3d51bd2e1c1a9652d18f2b78ef0
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5811994"
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

**注意**Windows 运行时 XAML 不支持**X:classmodifier**或**X:subclass**。

