---
description: 敏捷对象是可从任何线程访问的对象。 C++/WinRT 类型默认情况下是敏捷对象，但你可以选择退出。
title: 使用 C++/WinRT 的敏捷对象
ms.date: 10/20/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 敏捷, 对象, 敏捷性, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 2481396d9348250e14ebfc2d1f940b663b405f77
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639662"
---
# <a name="agile-objects-in-cwinrt"></a>敏捷对象在 C + + WinRT

在大多数情况下，可以从任意线程 （就像大多数标准 c + + 对象那样） 访问 Windows 运行时类的实例。 Windows 运行时类是*敏捷*。 仅少量的预装 Windows 的 Windows 运行时类是敏捷，但使用它们时需要考虑到其线程模型和封送处理行为 （封送处理传递数据跨单元边界）。 它是很好的默认为每个 Windows 运行时对象为敏捷，因此您自己[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)类型是敏捷默认情况下。

但可以选择退出。可能有充足的理由需要您为驻留，例如，在给定的单线程单元的类型的对象。 对于重入要求，通常需要这样做。 不过，敏捷对象越来越多，甚至用户界面 (UI) API 也提供敏捷对象。 总之，敏捷性是最简单、最有效率的选项。 此外，当实现一个激活工厂时，即使对应的运行时类不是敏捷的，激活工厂也必须是敏捷的。

> [!NOTE]
> Windows 运行时基于 COM。 在 COM 环境中，注册的敏捷类使用 `ThreadingModel` = *Both*。 有关 COM 线程模型和单元的详细信息，请参阅[理解和使用 COM 线程模型](https://msdn.microsoft.com/library/ms809971)。

## <a name="code-examples"></a>代码示例

让我们使用的运行时类的实现示例演示了如何 C + + WinRT 支持灵活性。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

由于我们没有选择退出，因此此实现是敏捷的。 [  **winrt::implements**](/uwp/cpp-ref-for-winrt/implements) base struct 实现 [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) 和 [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal)。 **IMarshal** 实现使用 **CoCreateFreeThreadedMarshaler** 来为不了解 **IAgileObject** 的旧代码执行正确的操作。

此代码检查对象的敏捷性。 如果 `myimpl` 不是敏捷的，则对 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 的调用将引发异常。

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

你可以调用 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)，而不必处理异常。

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** 没有自己的方法，因此你无法使用它执行很多操作。 接下来的变体将更典型一些。

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** 是一个*标记接口*。 **IAgileObject** 查询的成功或失败只不过是你从中获取的信息和实用工具的多少而已。

## <a name="opting-out-of-agile-object-support"></a>选择退出敏捷对象支持

你通过以下方法选择明确退出敏捷对象支持：将 [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) 标记结构作为模板参数传递给基类。

如果你直接从 **winrt::implements** 派生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

如果你正在创作运行时类。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

标记结构在 variadic 参数包中的位置无关紧要。

指示是否选择退出灵活性时，可以实现**IMarshal**自己。 例如，可以使用**winrt::non_agile**标记，以避免默认灵活性实现中，并实现**IMarshal**自己&mdash;可能是为了支持按值封送的语义。

## <a name="agile-references-winrtagileref"></a>敏捷引用 (winrt::agile_ref)

如果你正在使用一个非敏捷对象，但你需要在某些潜在的敏捷上下文中传递该对象，那么一个选项是使用 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) 结构模板来获取对非敏捷类型的实例或非敏捷对象的接口的敏捷引用。

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

或者，你可以使用 [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) 帮助程序函数。

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

在上述任一种情况中，`agile` 现在可以自由地传递到不同的单元中的线程并在其中使用。

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

[  **Agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) 调用将返回一个可在调用 **get** 的线程上下文中安全使用的代理。

## <a name="important-apis"></a>重要的 API

* [IAgileObject 接口](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal 接口](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [winrt::agile_ref 结构模板](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 结构模板](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile 函数模板](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile 标记结构](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown:: 为函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>相关主题

* [了解和使用 COM 线程模型](https://msdn.microsoft.com/library/ms809971)
