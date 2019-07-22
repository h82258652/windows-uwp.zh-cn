---
description: C++/WinRT 提供了函数和基类，当你希望实现并/或传递集合时，它们可以为你节省大量的时间和精力。
title: 使用 C++/WinRT 的集合
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 集合
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270153"
---
# <a name="collections-with-cwinrt"></a>使用 C++/WinRT 的集合

在内部，Windows 运行时集合具有大量复杂的移动部件。 但要将集合对象传递到 Windows 运行时函数，或要实现自己的集合属性和集合类型时，[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中有函数和基类可以提供支持。 这些功能消除复杂性，并节省大量时间和精力上的开销。

[IVector](/uwp/api/windows.foundation.collections.ivector_t_) 是由元素的任意随机访问集合实现的 Windows 运行时接口。 如果要自己实现 IVector，还需要实现 [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)、[IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_) 和 [IIterator](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使需要自定义的集合类型，也需要做大量工作。 但如果你在 std::vector（或者 std::map 或 std::unordered_map）中有数据，而你想要做的只是将其传递到 Windows 运行时 API，那么如果可能你会希望避免进行该级别的工作。 要避免是有可能的，因为 C++/WinRT 可以帮助高效地创建集合，且无需花费太多精力。

另参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](binding-collection.md)。

## <a name="helper-functions-for-collections"></a>集合的帮助程序函数

### <a name="general-purpose-collection-empty"></a>通用集合，空

本节介绍一个场景，在该场景中你希望创建初始为空的集合；然后在创建完毕后将其填充。

若要检索实现通用集合的类型的新对象，可以调用 [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector) 函数模板。 该对象作为 [IVector](/uwp/api/windows.foundation.collections.ivector_t_) 返回，并且它是一个接口，通过它可以调用所返回对象的函数和属性。

若要将以下代码示例直接复制并粘贴到 Windows 控制台应用程序 (C++/WinRT) 项目的主源代码文件中，请先在项目属性中设置“不使用预编译的标头”。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

如上述代码示例中所示，创建集合之后，你可以追加元素、对它们进行迭代，并且通常可以像处理从 API 接收到的任何 Windows 运行时集合对象那样处理对象。 如果需要集合的不可变视图，则可以调用 [IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview)，如下所示。 如上所示的创建和使用集合的模式适用于想要将数据传入 API 或从 API 获取数据的简单方案。 可以将 IVector 或 IVectorView 传递到预期 [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_) 的任意位置。

在上述代码示例中，调用 winrt::init_apartment 会初始化 Windows 运行时中（默认在多线程单元中）的线程。 该调用还会初始化 COM。

### <a name="general-purpose-collection-primed-from-data"></a>通用集合，从数据准备

本节介绍想要创建并填充集合的场景。

你可以避免上述代码示例中调用“追加”的开销。 你可能已拥有源数据，或者可能更倾向于在创建 Windows 运行时集合对象之前填充源数据。 下面是操作方法。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

可将包含数据的临时对象传递给 winrt::single_threaded_vector，就像上方传递 `coll1` 那样。 也可以将 std:: vector（假设不会再次访问）移动到该函数中。 在这两种情况下，都会将右值传递到函数。 这确保编译器能够高效运作并避免复制数据。 如果想要了解有关右值的详细信息，请参阅[值类别以及对它们的引用](cpp-value-categories.md)。

如果想要将 XAML 项目控件绑定到集合，则可以执行以下操作。 但要明白，若要正确设置 [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 属性，需要将其设置为 IInspectable 的 IVector 类型的值，或 [IBindableObservableVector](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) 等互操作性类型的值。

以下是代码示例，它生成适合绑定的类型集合，并向其追加元素。 可在 [XAML 项目控件；绑定到 C++/WinRT 集合](binding-collection.md)中查找到此代码的上下文。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

可以从数据创建 Windows 运行时集合，并在集合上准备好视图以便传递给 API，完全无需复制任何内容。

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

在上述示例中，我们创建的集合可以绑定到 XAML 项目控件；但是该集合是不可观测的。

### <a name="observable-collection"></a>可观测集合

若要检索实现可观测集合的类型的新对象，调用具有任何元素类型的 [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 函数模板。 但要使可观测集合适合绑定到 XAML 项目控件，使用 IInspectable 作为元素类型。

该对象作为 [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_) 返回，并且它是一个接口，你（或它绑定到的控件）可以通过它调用所返回对象的函数和属性。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

有关将用户界面 (UI) 控件绑定到可观测集合的详细信息和代码示例，请参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](binding-collection.md)。

### <a name="associative-collection-map"></a>关联集合（映射）

以下是我们介绍的这两个函数的关联集合版本。

- [winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map) 函数模板以 [IMap](/uwp/api/windows.foundation.collections.imap_k_v_) 形式返回不可观测的关联集合。
- [winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) 函数模板以 [IObservableMap](/uwp/api/windows.foundation.collections.iobservablemap_k_v_) 形式返回可观测的关联集合。

通过向函数传递 std::map 或 std::unordered_map 类型的右值，可以选择为这些集合准备好数据。

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>单线程

这些函数名称中的“单线程”表示它们不支持任何并发性&mdash;换言之，它们不是线程安全的。 此处提到的线程与单元无关，因为这些函数所返回的对象都是敏捷的（请参阅 [C++/WinRT 中的敏捷对象](agile-objects.md)）。 但是指对象是单线程的。 如果只想跨应用程序二进制接口 (ABI) 以某种方式传递数据，那么这完全合适。

## <a name="base-classes-for-collections"></a>集合的基类

如果想要实现自定义集合以实现完全灵活，最好避免采用这种复杂的方式。 例如，如果没有 C++/WinRT 基类，以下就是自定义矢量视图的外观。

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

从 [winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base) 结构模板派生自定义矢量视图，并实现 get_container 函数来公开保存数据的容器，这样做要简单得多。

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

get_container 返回的容器必须提供 winrt::vector_view_base 需要的 begin 和 end 接口。 如上述示例所示，std::vector 支持这一点。 但你可以返回任何满足相同协定的容器，包括自定义容器。

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

以下是 C++/WinRT 提供的基类，用于帮助实现自定义集合。

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

请参阅上述代码示例。

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>重要的 API
* [ItemsControl.ItemsSource 属性](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector 接口](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector 接口](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base 结构模板](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base 结构模板](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base 结构模板](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base 结构模板](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 结构模板](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 结构模板](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>相关主题
* [值类别，以及对它们的引用](cpp-value-categories.md)
* [XAML 项目控件; 绑定到 C++/WinRT 集合](binding-collection.md)
