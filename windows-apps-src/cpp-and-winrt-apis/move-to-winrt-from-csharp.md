---
description: 本主题介绍将 [C#](/visualstudio/get-started/csharp) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节。
title: 从 C# 移动到 C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, C#
ms.localizationpriority: medium
ms.openlocfilehash: f7cd35dbf211b14dfb886fc9ba4305cd7ce56e5e
ms.sourcegitcommit: f288bcc108f9850671662c7b76c55c8313e88b42
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290056"
---
# <a name="move-to-cwinrt-from-c"></a>从 C# 移动到 C++/WinRT

本主题介绍将 [C#](/visualstudio/get-started/csharp) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节。

## <a name="register-an-event-handler"></a>注册事件处理程序

可以采用 XAML 标记注册事件处理程序。

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

在 C# 中，**OpenButton_Click** 方法可以是专用的，但 XAML 仍然能够将它连接到 *OpenButton* 引发的 [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。

在 C++/WinRT 中，**OpenButton_Click** 方法的[实现类型](/windows/uwp/cpp-and-winrt-apis/author-apis)必须为公共。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

也可将注册类设置为实现类的友元，将 **OpenButton_Click** 设置为专用。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>使类可供 {Binding} 标记扩展使用

若要使用 {Binding} 标记扩展将数据绑定到数据类型，则请参阅[使用 {Binding} 绑定声明的对象](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding)。

## <a name="making-a-data-source-available-to-xaml-markup"></a>使数据源可供 XAML 标记使用

在 C++/WinRT 2.0.190530.8 及更高版本中，[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 创建一个可观测的支持 **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** 和 **IObservableVector\<IInspectable\>** 的矢量。

你可以创作 **Midl 文件 (.idl)** ，如下所示（另请参阅[将运行时类重构到 Midl 文件 (.idl) 中](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)）。

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

实现方式如下。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

有关详细信息，请参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)。

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>使数据源可供 XAML 标记使用（在 C++/WinRT 2.0.190530.8 之前）

XAML 数据绑定要求项源实现 **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** 以及下述接口组合之一。

- **IObservableVector\<IInspectable\>**
- **IBindableVector** 和 **INotifyCollectionChanged**
- **IBindableVector** 和 **IBindableObservableVector**
- **IBindableVector** 本身（不会响应更改）
- **IVector\<IInspectable\>**
- **IBindableIterable**（会通过迭代将元素保存到专用集合中）

无法在运行时检测 **IVector\<T\>** 之类的泛型接口。 每个 **IVector\<T\>** 都有不同的接口标识符 (IID)，该标识符是 **T** 的函数。任何开发人员都可以随意扩展 **T** 集，因此，很明显 XAML 绑定代码不可能知道要查询的完整集。 该限制对 C# 来说不是问题，因为每个实现 **IEnumerable\<T\>** 的 CLR 对象都会自动实现 **IEnumerable**。 在 ABI 级别，这意味着每个实现 **IObservableVector\<T\>** 的对象都会自动实现 **IObservableVector\<IInspectable\>** 。

C++/WinRT 不提供该保证。 如果 C++/WinRT 运行时类实现 **IObservableVector\<T\>** ，则我们不能假定也会通过某种方式提供 **IObservableVector\<IInspectable\>** 的实现。

因此，上一示例应如下所示。

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

实现如下。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

如需访问 *m_bookSkus* 中的对象，则需将其 QI 回 **Bookstore::BookSku**。

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

C# 类型提供 [Object.ToString](/dotnet/api/system.object.tostring) 方法。

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/ WinRT 不直接提供此工具，不过可以转为使用替代方法。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT 也支持 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)，但仅限数目有限的一些类型。 对于任何其他需要字符串化的类型，你需要添加重载。

| Language | 将整数字符串化 | 将枚举字符串化 |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

如果将枚举字符串化，则需提供 **winrt::to_hstring** 的实现。

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

这些字符串化通常通过数据绑定来隐式使用。

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

这些绑定会对被绑定属性执行 **winrt::to_hstring**。 至于第二个示例 (**StatusEnum**)，则必须提供你自己的 **winrt::to_hstring** 重载，否则会出现编译器错误。

## <a name="string-building"></a>字符串生成

C# 有一个内置的 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 类型，用于字符串生成。

| | C# | C++/WinRT |
|-|-|-|
| Builder 类 | `StringBuilder builder;` | `std::wstringstream stream;` |
| 追加字符串，保留 null | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| 提取结果 | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>装箱和取消装箱

C# 自动将标量装箱到对象中。 C++/WinRT 要求你显式调用 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函数。 两种语言都要求你以显式方式取消装箱。 请参阅[使用 C++/WinRT 装箱和取消装箱](/windows/uwp/cpp-and-winrt-apis/boxing)。

在下面的表中，我们将使用这些定义。

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 操作 | C# | C++/WinRT|
|-|-|-|
| 装箱 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| 取消装箱 | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

如果尝试取消值类型的 null 指针的装箱，C++/CX 和 C# 会引发异常。 C++/WinRT 将其视为编程错误，因此会崩溃。 在 C++/WinRT 中，请使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函数来处理对象类型不符合预期的情况。

| 方案 | C# | C++/WinRT|
|-|-|-|
| 取消已知整数的装箱 |`i = (int)o;` | `i = unbox_value<int>(o);` |
| 如果 o 为 null | `System.NullReferenceException` | 崩溃 |
| 如果 o 不是装箱的整数 | `System.InvalidCastException` | 崩溃 |
| 取消整数的装箱，在为 null 的情况下使用回退；任何其他情况则崩溃 | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 尽可能取消整数的装箱；在任何其他情况下使用回退 | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>将字符串装箱和取消装箱

字符串在某些情况下是值类型，在另一些情况下是引用类型。 C# 和 C++/WinRT 对待字符串的方式有所不同。

ABI 类型 [**HSTRING**](/windows/win32/winrt/hstring) 是一个指向引用计数字符串的指针。 但是，它并非派生自 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，因此从技术上来说它不是一个对象  。 另外，null **HSTRING** 表示空字符串。 将并非派生自 **IInspectable** 的项装箱时，需将其包装到 [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_) 中，而 Windows 运行时会以 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 对象的形式提供标准实现（自定义类型以 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype) 形式报告）。

C# 将 Windows 运行时字符串表示为引用类型，而 C++/WinRT 则将字符串投影为值类型。 这意味着装箱的 null 字符串可能有不同的表示形式，具体取决于你所采用的方法。

| 行为 | C# | C++/WinRT|
|-|-|-|
| 声明 | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| 字符串类型类别 | 引用类型 | 值类型 |
| null **HSTRING** 投影方式 | `""` | `hstring{}` |
| null 和 `""` 是否相同？ | 否 | 是 |
| null 的有效性 | `s = null;`<br>`s.Length` 引发 NullReferenceException | `s = hstring{};`<br>`s.size() == 0`（有效） |
| 如果将 null 字符串分配给对象 | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 如果将 `""` 分配给对象 | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本装箱和取消装箱。

| 操作 | C# | C++/WinRT|
|-|-|-|
| 将字符串装箱 | `o = s;`<br>空字符串变为非 null 对象。 | `o = box_value(s);`<br>空字符串变为非 null 对象。 |
| 取消已知字符串的装箱 | `s = (string)o;`<br>Null 对象变为 null 字符串。<br>如果不是字符串，则引发 InvalidCastException。 | `s = unbox_value<hstring>(o);`<br>Null 对象崩溃。<br>如果不是字符串，则崩溃。 |
| 将可能的字符串取消装箱 | `s = o as string;`<br>Null 对象或非字符串变为 null 字符串。<br><br>或者<br><br>`s = o as string ?? fallback;`<br>Null 或非字符串变为 fallback。<br>空字符串被保留。 | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 或非字符串变为 fallback。<br>空字符串被保留。 |

## <a name="derived-classes"></a>派生类

若要从运行时类派生，基类必须是可组合类。  C# 不需要你执行任何特殊步骤即可将类变为可组合类，但 C++/WinRT 需要。 请使用 [unsealed 关键字](/uwp/midl-3/intro#base-classes)来指示你希望将类用作基类。

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

在实现标头类中，在包括为派生类自动生成的标头之前，必须包括基类标头文件。 否则会出现“将此类型用作表达式非法”之类的错误。

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>使用 XAML 标记中的对象

在 C# 项目中，可以使用 XAML 标记中的专用成员和命名元素。 但在 C++/WinRT 中，以 XAML [ **{x:Bind} 标记扩展**](/windows/uwp/xaml-platform/x-bind-markup-extension)形式使用的所有实体必须在 IDL 中以公开方式公开。

另外，绑定到布尔值时，在 C# 中会显示 `true` 或 `false`，但在 C++/WinRT 中会显示 **Windows.Foundation.IReference`1\<布尔值\>** 。

有关详细信息和代码示例，请参阅[使用标记中的对象](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup)。

## <a name="important-apis"></a>重要的 API
* [winrt::single_threaded_observable_vector 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C# 教程](/visualstudio/get-started/csharp)
* [使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [深入了解数据绑定](/windows/uwp/data-binding/data-binding-in-depth)
* [XAML 项目控件; 绑定到 C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)