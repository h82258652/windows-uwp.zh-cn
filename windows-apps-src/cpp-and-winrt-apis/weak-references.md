---
author: stevewhims
description: C++/WinRT 弱引用支持是付费使用的，除非针对 IWeakReferenceSource 查询对象，否则不会向你收取任何费用。
title: C++/WinRT 中的弱引用
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 弱, 引用
ms.localizationpriority: medium
ms.openlocfilehash: 69294115af93ec464abfe908df948c8ff5504efc
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842451"
---
# <a name="weak-references-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的弱引用
你通常应该能够设计你自己 C++/WinRT API，以避免需要进行循环引用和弱引用。 不过，当涉及基于 XAML 的 UI 框架的本机实现时（由于框架的历史设计原因），需要使用 C++/WinRT 中的弱引用来处理循环引用。 在 XAML 之外，你不可能需要使用弱引用（尽管理论上没有任何针对 XAML 的说法）。

对于你声明的任何给定的类型，在 C++/WinRT 中，是否或者何时需要弱引用并不是显而易见的。 因此，C++/WinRT 在结构模板 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)（你自己的 C++/WinRT 类型将从中直接或间接派生）上自动提供弱引用支持。 它是付费使用的，除非针对 [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609) 实际查询对象，否则不会向你收取任何费用。 并且，你可以选择明确地[选择退出该支持](#opting-out-of-weak-reference-support)。

## <a name="code-examples"></a>代码示例
[**Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 结构模板是一个用于获取对类实例的弱引用的一个选项。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```
或者，你可以使用 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 帮助程序函数。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

创建弱引用不会影响对对象自身的引用计数；这只会导致分配一个控制块。 该控制块将负责实现弱引用语义。 你然后可以尝试将弱引用提升为强引用，并在成功之后使用它。

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

如果某些其他强引用仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) 调用将会增加引用计数并向调用方返回强引用。

## <a name="a-weak-reference-to-the-this-pointer"></a>对 *this* 指针的弱引用
C++/WinRT 对象直接或间接派生自结构模板 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)。 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) 受保护成员函数将返回对 C++/WinRT 对象的 *this* 指针的弱引用。 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) 将获得强引用。

## <a name="opting-out-of-weak-reference-support"></a>选择退出弱引用支持
将自动提供弱引用支持。 但你可以通过以下方法选择明确地选择退出该支持：将 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 标记结构作为模板参数传递给基类。

如果你直接从 **winrt::implements** 派生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

如果你正在创作运行时类。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

标记结构在 variadic 参数包中的位置无关紧要。 如果你请求对某个已选择退出类型的弱引用，那么编译器将帮助你退出并显示“*此项仅用于弱引用支持*”。

## <a name="important-apis"></a>重要的 API
* [implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::make_weak 函数模板](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 标记结构](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 结构模板](/uwp/cpp-ref-for-winrt/weak-ref)
