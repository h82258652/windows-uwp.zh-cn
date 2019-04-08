---
description: 本主题介绍了 C++ 中存在的各种值类别。 你肯定听说过左值和右值，但还有其他类型。
title: 值的分类，并对其的引用
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10、 uwp、 标准版、 c + +、 cpp、 winrt、 投影、 移动、 转发、 值的分类，移动语义，完美转发、 左值、 右值、 glvalue、 prvalue，xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593012"
---
# <a name="value-categories-and-references-to-them"></a>值的分类，并对其的引用
本主题介绍在 c + + 中存在各种类别的值 （和值对的引用）。 您将肯定听说过*左值*并*右值*，但您可能不会考虑它们在本主题介绍了术语。 还有其他类型的值，过。

C + + 中的每个表达式会生成一个值，属于本主题中讨论的类别之一。 没有 c + + 语言和其 facilies，规则，需要正确理解这些值的分类，并对其的引用的方面。 例如，采用一个值，将值复制、 移动一个值，以及转发到另一个函数的值的地址。 本主题不会转到所有这些方面深入，但它提供用于深入了解其中的基本信息。

通过标识和 movability [Stroustrup，2013年] 的两个独立属性方面的值的分类的 Stroustrup 分析包含在本主题中的信息。

## <a name="an-lvalue-has-identity"></a>左值的标识
是什么意思值能够*标识*？ 如果你有 （或可能需要） 的内存地址的一个值，并安全地，使用它，则值都具有标识。 这样一来，你可以执行多个比较值的内容： 可以比较或将它们区分开来的标识。

*左值*具有标识。 现在，这是分配的"左值"中的"l"是分配的"left"（如下所示，左侧元） 的缩写的仅历史感兴趣的问题。 在 c + +，左值可以出现在左侧*或*在赋值的右侧。 "左值"中的"l"然后，不会实际帮助你理解也不能定义是什么。 只需了解我们所说的左值是具有标识的值。

都是左值的表达式的示例包括： 已命名的变量或常量;或函数返回的引用。 使用的表达式的示例*不*左值包括： 临时; 或返回值的函数。

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

现在，尽管是左值具有标识为 true 的语句，因此为由于将 xvalues。 我们将转到内容的详细信息*xvalue*是本主题中更高版本。 现在，只需注意，存在称为 glvalue，为"通用化左值"的值类别。 Glvalues 的超集包含这两个左值 (也称为*经典的左值*) 和由于将 xvalues。 因此，尽管"左值具有标识"为 true 时，具有标识的操作的完整集合是一套 glvalues，此图中所示。

![左值的标识](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>左值是可移动;不是左值
但是，不 glvalues 的值。 因此，有值，这些值*不能*获取的内存地址 （或不能依赖于其才会生效）。 我们已了解上面的代码示例中的某些此类值。 这听起来像一个缺点。 但事实上值的优势等，它是你可以*移动*从它 （这是通常比较便宜），而不是从它的副本 （这是成本通常很高）。 从值将移意味着，就不能再使用它来进行就地。 因此，尝试访问在要使用的位置是要避免的内容。 当讨论和*如何*移动值不在此主题的讨论范围。 对于本主题中，我们只需知道为可移动的值被称为*rvalue* (或*经典的右值*)。

"右值"中的"r"是分配的"right"（如下所示，-右侧） 的缩写。 但可以使用右值，并对右值，外部分配的引用。 "右值"中的"r"，则不需要关注的一点。 只需了解我们所说的右值都为可移动的值。

左值，与之相反，不是可移动的此图中所示。 左值移动会迎接的定义*左值*，而且它会是非常合理预期能够继续访问左值的代码出现意外的问题。

![左值是可移动;不是左值](images/is-movable.png)

不能移动左值。 但有*是*glvalue （标识操作组），则可将一种&mdash;如果您知道自己在做什么 （包括要小心，不要在移动后访问）&mdash;即 xvalue。 我们将再度讨论这一想法一次，当我们查看在全面的值的分类。

## <a name="rvalue-references-and-reference-binding-rules"></a>右值引用和引用绑定规则
本部分介绍对 rvalue 引用的语法。 我们必须等待另一个主题，以转到移动和转发，大量处理，但这些都是通过右值引用，来解决问题。 我们看一下右值引用之前，不过，我们首先需要更为清晰有关`T&`&mdash;操作中，我们已以前已调用只是"参考"。 这就是"的左值 (非 const) 引用"，它是指所引用的用户可以写入的值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

左值引用可以绑定到左值，而不是右值。

则 const 左值引用 (`T const&`)，该引用到的对象引用的用户*不能*写 （例如，常量）。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Const 左值引用可以绑定到左值或右值。

对类型的右值引用的语法`T`编写为`T&&`。 右值引用所引用的可移动值&mdash;我们不需要保留后我们使用它 （例如，临时） 其内容的值。 因为整个点是从移动 （从而修改） 值绑定到右值引用，`const`和`volatile`限定符 （也称为 cv 限定符） 不会应用于右值引用。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

右值引用绑定到右值。 事实上，在右值的重载解析方面*首选*绑定到左值常量引用比的右值引用。 但是，因为如我们所说，右值引用所引用的一个值，假定我们不需要保留 （例如，移动构造函数的参数） 其内容的右值引用不能将绑定到左值。

此外可以传递右值的按值参数的地方，通过复制构造 （或移动构造，如果左值是 xvalue）。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Glvalue 具有标识;prvalue 却不
在此阶段，我们知道必须标识的内容。 我们知道什么是可移动，什么不是。 我们未尚未命名的值集，但是*不*具有标识。 组被称为*prvalue*，或*纯右值*。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![左值具有标识;prvalue 却不](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>完整的值的分类图
它仅将保持以便合并的信息和上述插图到单一的大图片。

![完整的值的分类图](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue （通用左值） 都有标识。

### <a name="lvalue-im"></a>左值 (我\&\!m)
左值 （一种 glvalue） 都有标识，但不是可移动。 这些是你传递通过引用或常量引用或值如果复制成本低廉的通常读 / 写值。 左值不能绑定到右值引用。

### <a name="xvalue-im"></a>xvalue (我\&m)
Xvalue （一种类型的 glvalue，但也有一种类型的右值） 标识，因此还可移动。 这可能是您已决定移动，因为复制成本很高，归拢左值，就可以小心以免以后访问。 下面是如何到 xvalue 将左值。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上面的代码示例中，我们尚未迁移任何内容。 我们刚刚创建了 xvalue 通过强制转换为命名的右值引用的左值。 仍可以通过左值名称; 标识但作为 xvalue 现*能够*的移动。 执行此操作，原因，哪些移动实际上如下所示，将需要等待另一个主题。 但您可以将"xvalue"含义"专家仅"如果，它可帮助为中的"x"。 通过将左值强制转换到 xvalue （一种的右值），值然后将成为能够绑定到右值引用。

以下是两个其他示例由于将 xvalues&mdash;调用函数返回命名的右值引用，并访问 xvalue 的成员。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!我\&m)
Prvalue （纯右值; 一种类型的右值） 中没有标识，但可移动。 这些通常是临时内存，调用函数的结果返回的值或不是 glvalue 的任何其他表达式的计算结果

### <a name="rvalue-m"></a>右值 (m)
左值是可移动。 右值*引用*始终引用右值 （值假定我们不需要保留其内容）。

但是，是右值的右值引用本身呢？ *未命名*右值引用 （如上面的 xvalue 代码示例中所示） 是 xvalue 因此，是的它是右值。 它首选绑定到一个右值引用函数参数，如移动构造函数。 与之相反 （和可能是 counter-intuitively），如果右值引用具有一个名称，则包含该名称的表达式是左值。 因此它*不能*绑定到右值引用参数。 但很容易以使其执行此操作&mdash;再次只需将其转换为未命名的右值引用 (xvalue)。

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

### <a name="im"></a>\!我\&\!m
未标识并且不是可移动的值的类型为一个组合，我们还没有介绍。 但我们可以忽略它，因为该类别不是一个有用的想法在 c + + 语言中。

## <a name="reference-collapsing-rules"></a>引用折叠规则
表达式 （左值引用为左值引用或右值引用的右值引用） 中的多个 like 引用取消一个另一个扩展。

- `A& &` 折叠到`A&`。
- `A&& &&` 折叠到`A&&`。

与在表达式中引用不同的多个折叠到左值引用。

- `A& &&` 折叠到`A&`。
- `A&& &` 折叠到`A&`。

## <a name="forwarding-references"></a>转发引用
此最后一节将进行比较的右值引用，我们已经讨论，不同的概念*转发引用*。

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 是右值引用，正如我们所见。 固定和可变不会应用于右值引用。
- `foo` 接受仅右值的类型**A**。
- 原因右值引用 (如`A&&`) 存在，以便可以创作一个临时 （或其他右值） 传递的情况下进行了优化的重载。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` 是*转发引用*。 具体取决于传递给`bar`，类型 **_Ty**可能是 const/非 const 独立于易失性/非易失性。
- `bar` 接受任何左值或右值的类型 **_Ty**。
- 传递左值会导致转发引用变得`_Ty& &&`，其中将折叠为左值引用`_Ty&`。
- 传递右值会导致转发引用变得`_Ty&& &&`，其中将折叠为右值引用`_Ty&&`。
- 转发引用的原因 (如`_Ty&&`) 存在是*不*进行优化，但以执行你传递给它们，透明且有效地将其转发上。 您可能会遇到的转发引用仅当编写 （或步仔细研究） 库代码&mdash;例如，在构造函数自变量将转发的工厂函数。

## <a name="sources"></a>源
* \[Stroustrup，2013年\]B.Stroustrup:C + + 编程语言，第四版。 Addison-Wesley。 2013。
