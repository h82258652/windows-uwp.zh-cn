---
author: jwmsft
description: 配置 XAML 编译，在标记和代码隐藏之间连接分部类。 代码分部类在一个独立的代码文件中定义，标记分部类由代码生成过程在 XAML 编译期间创建。
title: xClass 属性
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6746969b1b717183894d6b941be41c9aca452960
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6279186"
---
# <a name="xclass-attribute"></a>x:Class 属性


配置 XAML 编译，在标记和代码隐藏之间连接分部类。 代码分部类在一个独立的代码文件中定义，标记分部类由代码生成过程在 XAML 编译期间创建。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 说明 |
|------|-------------|
| 命名空间 | 可选。 指定一个包含 _classname_ 所标识的分部类的命名空间。 如果 _namespace_ 已指定，点 (.) 会将 _namespace_ 和 _classname_ 分开。 如果省略 _namespace_，会假设 _classname_ 没有命名空间。 |
| classname | 必需。 指定分部类的名称，该分部类连接已加载的 XAML 和该 XAML 的代码隐藏。 | 

## <a name="remarks"></a>备注

**x:Class** 可声明为作为一个 XAML 文件/对象树的根并由生成操作编译的任何元素的属性，或者已编译应用程序的应用程序定义中 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 根的属性。 在根节点以外的任何元素上，以及在不会使用“页面”**** 生成操作编译 XAML 文件的任何环境中声明 **x:Class**，会导致编译时错误。

用作 **x:Class** 的类不能是嵌套类。

**x:Class** 属性的值必须是一个字符串，用于指定一个类的完全限定名称。 只要构造代码隐藏时省略了命名空间信息（类定义从类级别开始），你可以省略命名空间信息。 一个页面或应用程序定义的代码隐藏文件必须在一个代码文件中，并且该代码文件包含在项目中。 代码隐藏类必须是公共的。 代码隐藏类必须是部分的。

## <a name="clr-language-rules"></a>CLR 语言规则

尽管代码隐藏文件可以是 C++ 文件，但一些约定仍然遵守 CLR 语言形式，因此在 XAML 语法上没有区别。 具体来讲，命名空间与任何 **x:Class** 值的类名组件之间的分隔符始终为一个点（“.”），即使与 XAML 关联的 C++ 代码文件中的命名空间和类名称之间的分隔符是“::”也是如此。 如果在 C++ 中声明嵌套的命名空间，那么在指定 **x:Class** 值的 *namespace* 部分时，连续的嵌套命名空间字符串之间的分隔符也应是一个“.”，而不是“::”。

