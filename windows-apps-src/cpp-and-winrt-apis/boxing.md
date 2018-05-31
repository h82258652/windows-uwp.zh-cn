---
author: stevewhims
description: 标量值需要先封装到引用类对象内，然后再传递到需要 **IInspectable** 的函数。 该封装过程称为对值进行*装箱*。
title: 通过 C++/WinRT 将标量值装箱到 IInspectable 和从 IInspectable 取消标量值装箱
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 控件, 装箱, 标量, 值
ms.localizationpriority: medium
ms.openlocfilehash: 61d5c7a35fb7a6ff9952f3fe768f4faa3f6c6347
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832001"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 将标量值装箱到 IInspectable 和从 IInspectable 取消标量值装箱 
[**IInspectable 接口**](https://msdn.microsoft.com/library/windows/desktop/br205821)是 Windows 运行时 (WinRT) 中每个运行时类的根接口。 这类似于位于每个 COM 接口和类的根处的 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)；而且类似于位于每个[通用类型系统](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system)类的根处的**System.Object**。

换言之，可向任何运行时类的实例传递需要 **IInspectable** 的函数。 但是你无法将标量值（如数值或文本值）直接传递到此类函数。 相反，标量值需要封装到引用类对象内。 该封装过程称为对值进行*装箱*。

C++/WinRT 提供了 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函数，该函数采用标量值并将装箱的值返回到 **IInspectable** 中。 对于取消 **IInspectable** 装箱返回到标量值，提供 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 和 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函数。

## <a name="examples-of-boxing-a-value"></a>取消值装箱的示例
[**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) 访问器函数返回 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)，这是一个标量值。 我们可以将该 **hstring** 值进行装箱并将其传递到需要 **IInspectable** 的函数，如下所示。

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

要设置 XAML [**按钮**](/uwp/api/windows.ui.xaml.controls.button)的内容属性，请调用 [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) 转变器函数。 要将内容属性设置为字符串值，你可以使用此代码。

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

首先，**hstring** 转换构造函数将此字符串参数转换为 **hstring**。 然后，调用采用 **hstring** 的 **winrt::box_value** 的重载。

## <a name="examples-of-unboxing-an-iinspectable"></a>取消 IInspectable 装箱的示例
在自己的需要 **IInspectable** 的函数中，你可以使用 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 取消装箱，也可以使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 通过默认值取消装箱。

```cppwinrt
void Unbox(Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="important-apis"></a>重要的 API
* [IInspectable 接口](https://msdn.microsoft.com/library/windows/desktop/br205821)
* [winrt::box_value 函数模板](/uwp/cpp-ref-for-winrt/box-value)
* [winrt::unbox_value 函数模板](/uwp/cpp-ref-for-winrt/unbox-value)
* [winrt::unbox_value_or 函数模板](/uwp/cpp-ref-for-winrt/unbox-value-or)
