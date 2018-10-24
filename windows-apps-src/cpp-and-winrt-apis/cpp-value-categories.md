---
author: stevewhims
description: 本主题介绍了各种类别的 c + + 中存在的值。 你将肯定所知，左值和 rvalues，但也有其他类型。
title: 值类别和对它们的引用
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp、 标准、 c + +，cpp，winrt、 投影、 移动、 转发、 值的分类、 移动语义、 完全转发、 左值、 rvalue、 glvalue，prvalue，xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475283"
---
# <a name="value-categories-and-references-to-them"></a>值类别和对它们的引用
本主题介绍了各种类别的值 （和参考值） 存在于 c + + 中。 你将肯定所知，*左值*和*rvalues*，但你可能不将其视为它们在本主题提供的条款。 还有其他类型的值，过。

在 c + + 中的每个表达式将产生一个值，属于本主题中讨论的类别之一。 有方面的 c + + 语言、 其 facilies 和规则，要求正确了解这些值类别，以及对它们的引用。 例如，获取一个值，将值复制、 移动一个值，和转发到另一个函数的值的地址。 本主题不会进入所有这些方面防御，但它提供了有关清楚地了解它们的基本信息。

本主题中的信息的标识和 movability [Stroustrup，2013年] 的两个独立属性本身方面的值的分类 Stroustrup 的分析。

## <a name="an-lvalue-has-identity"></a>左值具有标识
它为某个值具有*标识*意味着什么？ 如果你拥有 （或者你可以采取） 值的内存地址并使用它，则值具有标识。 这样一来，你可以执行多个比较值的内容： 你可以比较或标识来区分它们。

*左值*具有标识。 现在是赋值的一种仅历史感兴趣"左值"中的"l"是赋值的"左"（如，左左） 的缩写。 在 c + +，左值可以显示的左侧*或*右侧的分配。 "L"中"左值"，然后，不会实际可帮助你理解，也不会定义它们是。 你只需要了解我们称之为左值是具有标识的值。

左值表达式的示例包括： 命名的变量或常数;或返回引用的函数。 *不*左值表达式的示例包括： 一个临时;或返回值的函数。

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

现在，左值具有标识 true 语句时，因此不要 xvalues。 我们将转到哪些*xvalue*是本主题后面的更多。 现在，只需注意，如果没有 glvalue，名为"通用左值"的值类别。 Glvalues 超集包含左值 (也称为*传统左值*) 和 xvalues。 因此，而"左值具有标识"为 true 时，具有标识的事项的完整集合是一套 glvalues，此图中所示。

![左值具有标识](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue 是可移动;左值不是
但不是 glvalues 的值。 因此，存在你*不能*获得的内存地址 （或不能依靠它来有效） 的值。 我们之前所见上面的代码示例中的某些此类值。 这听起来像一个缺点。 但实际上值利用像也就是说，你可以从它 （这是通常成本低廉），*移动*，而不是从它 （这是通常成本高昂） 的复制。 从一个值意味着它不会再在它所使用的位置。 因此，尝试在它所使用的位置中访问它是一款用于避免。 从讨论了何时以及*如何*移动值已超出本主题的范围。 本主题中，我们只需知道该可移动的值称为*rvalue* （或*传统 rvalue*）。

在"rvalue"中的"r"是赋值的"权限"（如，右键左） 的缩写。 但你可以使用 rvalues，并对 rvalues，之外分配的引用。 在"rvalues"，"r"，则不需要关注的件事。 你只需要了解我们称之为 rvalue 是可移动的值。

左值，相反，不是可移动，此图中所示。 移动左值会 defy 的定义*左值*，并且它会非常合理预期能够继续访问左值的代码意外的问题。

![Rvalue 是可移动;左值不是](images/is-movable.png)

你无法移动左值。 但有** 一种 glvalue （与标识的内容的一组），可以移动&mdash;如果你知道你正在执行的操作 （包括小心不要访问它在移动后）&mdash;，这 xvalue。 我们将重新这一想法访问一次下方，当我们看一下值类别的全面地了解情况。

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 引用，并引用绑定规则
本部分介绍对 rvalue 引用的语法。 我们将需要等待另一个主题，以转到大量处理移动和转发，但这些 rvalue 引用解决的问题。 我们看一下 rvalue 引用之前，不过，我们首先需要有关清晰`T&`&mdash;件事我们已以前已调用只是"引用"。 它实际上是"左值 (非 const) 引用"，这是指引用的用户可写入的值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

左值引用可以绑定到左值，但不能向 rvalue。

然后有左值 const 引用 (`T const&`)，它引用的对象向其用户*不能*引用写入 （例如，常量）。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

左值 const 引用可以绑定到左值或 rvalue。

参考 rvalue 类型的语法`T`编写为`T&&`。 Rvalue 引用指的是可移动值&mdash;一个值，我们无需保留后，我们使用它 （例如，临时） 其内容。 由于整个点将从移动 （从而修改） 值绑定到 rvalue 引用，`const`和`volatile`限定符 （也称为 cv 限定符） 不适用于 rvalue 引用。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Rvalue 引用将绑定到 rvalue。 事实上，根据重载决策，rvalue*首选*绑定到左值 const 引用比 rvalue 引用。 但 rvalue 引用无法绑定到左值，因为我们已经说过，rvalue 引用引用的值将假设我们无需保留 （比如，移动构造函数的参数） 的内容。

你还可以传递 rvalue 的值通过参数的地方，通过复制构造 （或移动构造，如果该右值是 xvalue）。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Glvalue 具有标识;prvalue 没有
在这个阶段，我们知道内容有身份。 并且，我们已经知道什么是可移动及其内容不是。 但我们尚未名为一组值*不*具有标识。 这组称为*prvalue*或*纯 rvalue*。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![左值具有标识;prvalue 没有](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值类别的全面地了解情况
它仅保留组合的信息和上面到单个、 大图片的图示。

![值类别的全面地了解情况](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue （一般化左值） 具有标识。

### <a name="lvalue-im"></a>左值 （i\ 和 \!m）
左值 （一种 glvalue） 具有标识，但不是可移动。 以下是你传递通过引用 const 引用，或通过值如果复制是便宜的通常读写值。 左值无法绑定到 rvalue 引用。

### <a name="xvalue-im"></a>xvalue (i\ & m)
Xvalue （一种 glvalue，但也一种 rvalue） 具有标识，并且还可移动。 这可能是你已决定移动复制的成本很高，因为 erstwhile 左值，并且你将注意不要以后访问。 下面介绍了如何将左值转换 xvalue。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上面的代码示例中，我们尚未移动任何尚未。 我们只需创建了 xvalue 通过强制转换为未命名的 rvalue 引用左值。 它仍然可以标识通过其左值名称;但是，xvalue，它是现在*能够*移动。 执行此操作的原因，哪些移动实际上如下所示，需要等待另一个主题。 但是，你可以将其视为"xvalue"作为含义"专家仅"如果，可帮助中的"x"。 通过强制左值转换到 xvalue （一种 rvalue），则该值将成为能够被绑定到 rvalue 引用。

下面是两个其他 xvalues 示例&mdash;调用返回命名的 rvalue 引用、 函数和访问 xvalue 的成员。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Prvalue （纯 rvalue; 一种 rvalue） 不具有标识，但可移动。 这些通常是临时变量，调用的函数的结果返回的值或不是 glvalue 的任何其他表达式的计算结果

### <a name="rvalue-m"></a>rvalue (m)
可移动 rvalue。 Rvalue*引用*通常是指 rvalue （假设我们无需保留其内容的值）。

但是，是 rvalue 引用本身 rvalue 吗？ *未命名*rvalue 引用 （如上面的 xvalue 代码示例所示） 是 xvalue 因此，它是的 rvalue。 它倾向于绑定到一个 rvalue 引用函数参数，如移动构造函数。 相反，或许 counter-intuitively） 如果 rvalue 引用具有一个名称，那么该名称组成表达式是左值。 因此它*无法*绑定到 rvalue 引用参数。 很容易使其执行此操作，但是&mdash;再次只需将其转换到未命名的 rvalue 引用 (xvalue)。

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

### <a name="im"></a>\!i\ 和 \!m
不具有标识，不是值的可移动的类型是值的我们尚未尚未所述的一个组合。 但是，我们可以忽略它，因为该类别不是在 c + + 语言中非常有用的想法。

## <a name="reference-collapsing-rules"></a>引用折叠规则
表达式 （左值引用，左值引用或 rvalue 引用到 rvalue 引用） 中的多个类似引用取消一个另一个出。

- `A& &` 折叠到`A&`。
- `A&& &&` 折叠到`A&&`。

与在表达式中引用的多个折叠到左值引用。

- `A& &&` 折叠到`A&`。
- `A&& &` 折叠到`A&`。

## <a name="forwarding-references"></a>转发引用
此最后一部分对比 rvalue 引用，我们已经讨论，与*转发引用*的不同的概念。

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 是 rvalue 引用，正如我们所见。 Const 和不稳定不适用于 rvalue 引用。
- `foo` 接受仅 rvalues 的**类型**。
- 该原因右值引用 (如`A&&`) 存在，以便你可以编写一个重载，针对临时 （或其他 rvalue） 传递的情况下进行了优化。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` 是*转发引用*。 具体取决于你传递到`bar`，类型 **_Ty**可能是 const/非量独立易失性/非易失性。
- `bar` 接受任何左值或类型 **_Ty**的 rvalue。
- 传递左值将导致转发参考成为`_Ty& &&`，其中折叠左值引用`_Ty&`。
- 传递 rvalue 导致成为转发引用`_Ty&& &&`，其中折叠 rvalue 引用`_Ty&&`。
- 转发引用的原因 (如`_Ty&&`) 存在是*不*进行优化，但以执行你传递给他们，以透明方式、 高效地将其转发上。 你可能会遇到转发参考仅当你编写 （或仔细研究） 库代码&mdash;例如，在构造函数参数将转发工厂函数。

## <a name="sources"></a>源
* \[Stroustrup、 2013\] b。 Stroustrup: c + + 编程语言，第四个版本。 艾迪逊 Wesley。 2013。
