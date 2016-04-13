---
修改 XAML 编译行为，使指定对象引用的字段被定义有“public”访问权限而不是默认的“private”行为。
xFieldModifier 属性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
---

# x:FieldModifier 属性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

修改 XAML 编译行为，使指定对象引用的字段被定义有 **public** 访问权限而不是默认的 **private** 行为。

## XAML 属性使用方法

``` syntax
<object x:FieldModifier="public".../>
```

## 依存关系

[x:Name 属性](x-name-attribute.md)还必须在同一元素上进行提供。

## 备注

**x:FieldModifier** 属性的值将随编程语言而变。 要使用的字符串将取决于每种语言实现其 **CodeDomProvider** 的方式以及它所返回的用来定义 **TypeAttributes.Public** 和 **TypeAttributes.NotPublic** 的含义的类型转换器。 对于 C#、Microsoft Visual Basic 或 Visual C++ 组件扩展 (C++/CX)，你可以将字符串赋值为“public”或“Public”；分析器没有强制规定此属性值的大小写。

你还可以指定 **NonPublic**（在 C# 或 C++/CX 中为 **internal**，在 Visual Basic 中为 **Friend**），但这不常见。 对于 Windows 运行时 XAML 代码生成模型，没有对其应用任何内部访问权限。 Private 访问权限是默认设置。

**x:FieldModifier** 仅适合具有 [x:Name 属性](x-name-attribute.md)的元素，因为一旦该名称是公共的，它将用来引用字段。

**注意** Windows 运行时 XAML 不支持 **x:ClassModifier** 或 **x:Subclass**。



<!--HONumber=Mar16_HO1-->


