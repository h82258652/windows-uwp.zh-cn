---
author: stevewhims
description: 本主题介绍各种类别的 c + + 中存在的值。 您肯定会听到左值和 rvalues，但有其他类型，太。
title: 值类别，并对它们的引用
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 标准、 c + +、 cpp、 winrt、 投影、 移动、 转接、 值类别、 移动语义，完全转发、 左值、 rvalue、 glvalue、 prvalue，xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867846"
---
# <a name="value-categories-and-references-to-them"></a>值类别，并对它们的引用
本主题介绍 c + + 中存在的各种类别的值 （和引用值）。 您将肯定听说过*左值*和*rvalues*，但您可能不认为它们在本主题提供的条款。 和太有其他类型的值。

C + + 中的每个表达式的结果值属于某个本主题中讨论的类别。 有 c + + 语言，其 facilies 和规则，这些值类别，并对它们的引用的正确理解的需求的方面。 例如，获取一个值，复制一个值，移动一个值，并转接到另一个函数的值的地址。 本主题不转到所有这些方面详解情况下，但它提供了清楚地了解它们的基本信息。

本主题中的信息由两个独立的标识和 movability [Stroustrup，2013年] 属性本身方面的值的分类 Stroustrup 的分析。

## <a name="an-lvalue-has-identity"></a>左值具有标识
对于具有*标识*的值，它意味着什么？ 如果您有 （或者您可以采取） 值的内存地址和安全地，使用它，然后值具有标识。 这样，您可以执行多个比较值的内容： 可比较或 identity 区分它们。

*左值*具有标识。 现在是"l"左"值"中的为缩写形式 （如所，左左工作分配）"left"的仅历史利息空格。 在 c + +，左值会显示上的左侧*或*右侧的工作分配。 在"左值"，"l"然后不实际帮助您理解也不定义它们是什么。 您只需了解，我们所说左值是具有标识的值。

表达式的左值的示例包括： 命名的变量或常量;或函数返回的引用。 都*不*左值的表达式的示例包括： 临时;或函数的返回值。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

现在，左值具有标识的则返回 true 语句时，因此不要 xvalues。 我们将转到哪些*xvalue*是本主题后面的更多。 现在，只需注意存在名为"常用左值"为 glvalue，值类别。 Glvalues 超集包含左值 (也称为*古典左值*) 和 xvalues。 因此，时"左值具有标识"为 true 时，具有标识的操作的完整集是一套 glvalues，在此图中所示。

![左值具有标识](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue 是可移动;左值不是
但有不 glvalues 的值。 因此，没有您*无法*获取的内存地址 （或不能依赖有效） 的值。 我们看到上面的代码示例中的某些此类值。 此听起来像一个缺点。 但实际上值利用 like，您可以从其 （这是通常便宜），*移动*，而不是从其 （这是通常昂贵） 的副本。 从值意味着不再是它用于就地。 因此，尝试访问它用于是就地不是以避免。 从讨论了何时和*如何*移动超出此主题的范围，则值。 本主题中，我们只需了解一个值，是可移动称为*rvalue* （或*古典 rvalue*）。

"Rvalue"中的"r"是"右"（如所，右键左工作分配） 的缩写。 但是，您可以使用 rvalues 和 rvalues，外部工作分配的引用。 "Rvalues"中的"r"，则不需要重点关注的一点。 您只需了解，我们所说 rvalue 是可移动的值。

左值，相反，不是可移动的在此图中所示。 移动左值将 defy 的*左值*，定义，就像代码非常合理预期能够继续访问左值的出现意外的问题。

![Rvalue 是可移动;左值不是](images/is-movable.png)

不能移动左值。 但存在** 一种 glvalue （标识为操作的一组），可以移动&mdash;如果您知道您执行的操作 （包括小心不要在迁移之后访问它）&mdash;，xvalue。 我们将随着此主意一个更多时间下，当我们查看完成值类别的图片。

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 参考和引用绑定规则
本部分将介绍对 rvalue 的语法。 我们需要等待另一个主题，以转到移动和转发，大量处理但那些通过 rvalue 参考解决的问题。 我们看一下 rvalue 引用之前，但是我们首先需要更清晰有关`T&`&mdash;事物我们已以前被调用刚刚"参考"。 实际上，它是"左值 (非 const) 引用"，它是指参考的用户可以写入的值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

左值引用可绑定到左值，但不适用于 rvalue。

然后有左值常量引用 (`T const&`)，该引用的对象的用户的引用，*无法*写入的 （例如，常量）。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

左值 const 引用可绑定到左值或 rvalue。

对 rvalue 类型的引用的语法`T`编写为`T&&`。 Rvalue 引用引用可移动值&mdash;我们不需要保留后，我们使用它 （例如，临时） 其内容的值。 整个点后将从移动 （从而修改） 值绑定到 rvalue 引用，`const`和`volatile`限定符 （也称为 cv 限定符） 不能应用于 rvalue 引用。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Rvalue 引用将绑定到 rvalue。 实际上，方面重载决策，rvalue*喜欢*绑定到向左值 const 引用比 rvalue 引用。 但 rvalue 引用无法绑定到左值，因为我们所说过，rvalue 引用是指一个值，假定我们不需要保留 （例如，移动构造函数的参数） 其内容。

您还可以传递 rvalue 按值参数的地方，通过复制构造 （或通过移动构造，如果该右值为 xvalue）。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Glvalue 具有标识;未显示 prvalue
在此阶段，我们知道什么具有标识。 和我们知道什么是可移动，什么不是。 但我们尚未尚未命名集的值** 具有标识。 该集被称为*prvalue*或*纯 rvalue*。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![左值具有标识;未显示 prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值类别的完整图片
它仅待合并信息和到单一的大图片上面的插图。

![值类别的完整图片](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue （通用左值） 具有标识。

### <a name="lvalue-im"></a>左值 (i\ & \!m)
左值 （一种 glvalue） 具有标识，但不可移动。 这些是通常的读写值传递给周围通过引用 const 参考，或按值便宜复制是否。 左值无法绑定到 rvalue 引用。

### <a name="xvalue-im"></a>xvalue (i\ & m)
Xvalue （一种 glvalue，但还一种 rvalue） 具有标识，并且还可移动。 这可能已决定移动复制很高，因为 erstwhile 左值，您会注意不要此后访问。 下面是如何变为 xvalue 中的左值。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上面的代码示例中，我们没有移任何尚未。 只需，我们已创建 xvalue 通过强制转换为未命名的 rvalue 引用左值。 仍可以通过左值名称; 标识但是，xvalue，它是现在*支持*移。 这样的原因和哪些移动实际如下所示，都需要等待另一个主题。 但您可以认为"xvalue"作为含义"专家-仅限"如果，可帮助中的"x"。 通过强制左值转换到 xvalue （一种 rvalue），则随后将成为能够绑定到 rvalue 引用。

下面是 xvalues 的两个其他示例&mdash;调用的函数返回未命名的 rvalue 引用，并访问 xvalue 的成员。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
（纯 rvalue; 一种 rvalue） prvalue 没有 identity，但是可移动。 这些方案通常是临时变量，结果的调用的函数的返回值，或不是 glvalue 的任何其他表达式的计算结果

### <a name="rvalue-m"></a>rvalue (m)
可移动 rvalue。 Rvalue*引用*始终指 rvalue （假定我们不需要保留其内容的值）。

但是，是 rvalue 引用本身 rvalue？ *未命名*rvalue 引用 （如上述 xvalue 代码示例所示的那些） 是 xvalue 因此，它是的 rvalue。 其首选绑定到 rvalue 引用函数参数，例如的移动构造函数。 相反 （和回电 counter-intuitively），如果 rvalue 引用包含一个名称，则该名称组成的表达式是左值。 因此，它*不能*绑定到 rvalue 引用参数。 但很容易地使这样&mdash;再次只需将其转换为未命名的 rvalue 引用 (xvalue)。

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\ = \!m
值，它不具有标识，不可移动类型是我们尚未您尚未讨论的一个组合。 但是，我们可以忽略它，因为该类别中的 c + + 语言有用想法并不。

## <a name="reference-collapsing-rules"></a>引用折叠规则
表达式 （左值引用左值引用或 rvalue 引用的 rvalue 引用） 中的多个 like 引用取消一个另一个出。

- `A& &` 折叠到`A&`。
- `A&& &&` 折叠到`A&&`。

与表达式中引用不同的多折叠到左值引用。

- `A& &&` 折叠到`A&`。
- `A&& &` 折叠到`A&`。

## <a name="forwarding-references"></a>转接引用
最终本节对比 rvalue 参考，我们已讨论，与*转接引用*的不同的概念。

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 是 rvalue 引用，如我们看到。 Const 和可变不应用于 rvalue 引用。
- `foo` 接受仅 rvalues 的类型**A**。
- 该原因右值引用 (如`A&&`) 存在，以便您可以编写专为临时 （或其他 rvalue） 传递的大小写的重载。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` *转接引用*。 根据传递给`bar`，类型 **_Ty**可能 const/非-const 独立于可变/非可变。
- `bar` 接受任何左值或的类型 **_Ty**rvalue。
- 传递左值将导致转接引用成为`_Ty& &&`，其折叠左值引用`_Ty&`。
- 传递 rvalue 导致成为转接引用`_Ty&& &&`，其折叠 rvalue 引用`_Ty&&`。
- 转接引用的原因 (如`_Ty&&`) 存在是*不*进行优化，但以执行您传递为其，透明地实施、 高效地将其转接上。 您可能遇到的转发引用仅当写 （或紧密研究） 库代码&mdash;，如构造函数参数转发工厂函数。

## <a name="sources"></a>源
* \[Stroustrup，2013\] B.Stroustrup: c + + 编程语言、 第四版。 艾迪逊 Wesley。 2013。
