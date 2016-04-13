---
唯一标识作为资源被创建和引用的元素，这些资源存在于一个 ResourceDictionary 中。
xKey 属性
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
---

# x:Key 特性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

唯一标识作为资源被创建和引用的元素，这些资源存在于一个 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中。

## XAML 属性使用方法

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## XAML 属性使用方法（隐式 **ResourceDictionary**）

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## XAML 值

| 术语 | 说明 |
|------|-------------|
| object | 任何可共享的对象。 请参阅 [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)。 |
| stringKeyValue | 一个用作键的真实字符串，它必须遵守 _XamlName_> 语法。 请参阅下面的“XamlName 语法”。 | 

##  XamlName 语法

以下是在通用 Windows 平台 (UWP) XAML 实现中作为键使用的字符串的规范语法：

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'–'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   字符被限制在较低的 ASCII 范围，具体而言，就是大写和小写罗马字母、数字和下划线 (\_) 字符。
-   不支持 Unicode 字符范围。
-   名称不能以数字开头。

## 备注

[
            **ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的子元素一般包含一个 **x:Key** 属性，该属性在该词典中指定一个唯一的键值。 键唯一性在加载时由 XAML 处理器执行。 非唯一的 **x:Key** 值将导致 XAML 分析异常。 如果 [{StaticResource} 标记扩展](staticresource-markup-extension.md)发出请求，任何未解析的键也会导致 XAML 分析异常。

**x:Key** 和 [x:Name](x-name-attribute.md) 不是同一概念。 **x:Key** 仅用于资源词典中。 x:Name 适用于 XAML 的所有区域。 一个使用键值的 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 调用不会检索键资源。

请注意，在给出的隐式语法中，[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 对象在 XAML 处理器生成一个新对象来填充 [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 集合的方式上是隐式的。

指定 **x:Key** 的代码等效于任何结合使用一个键和基础 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的操作。 例如，当向一个 **ResourceDictionary** 添加资源时，一个应用于资源标记中的 **x:Key** 等效于 **Insert** 的 *key* 参数的值。

如果资源字典中的某个项是目标 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 或 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)，则它可以省略 **x:Key** 的值；在每一种情况下，该资源项的隐式键都是解释为字符串的 **TargetType** 值。 有关详细信息，请参阅[快速入门：设置控件的样式](https://msdn.microsoft.com/library/windows/apps/hh465498)和 [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)。



<!--HONumber=Mar16_HO1-->


