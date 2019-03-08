---
description: C + + WinRT 提供函数和基类，这些您节省大量时间和精力当你想要实现和/或传递集合类。
title: 使用 C++/WinRT 的集合
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10、 uwp、 标准、 c + +、 cpp、 winrt、 投影、 集合
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635432"
---
# <a name="collections-with-cwinrt"></a>使用 C++/WinRT 的集合

在内部，Windows 运行时集合具有大量复杂的移动部件。 当你想要将集合对象传递给 Windows 运行时函数，或实现自己的集合属性和集合类型，有的函数和基类中的，但是[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)来为您提供支持。 这些功能手，消除复杂性，并将你保存时间和精力在很大的开销。

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_)是实现的任何元素的随机访问集合的 Windows 运行时接口。 如果实现了**IVector**自己，您还需要实现[ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)， [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_)，并[ **IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使您*需要*的自定义集合类型，很多工作。 但是，如果包含的数据**std:: vector** (或**std:: map**，或**std:: unordered_map**) 并想要做的就是将其传递给 Windows 运行时 API，然后想要避免的操作该级别的工作，在可能的情况。 和避免它*是*有可能，因为 C + + WinRT 可帮助你有效地和不费吹灰之力创建集合。

另请参阅[XAML 控件的项; 绑定到 C + + WinRT 集合](binding-collection.md)。

> [!NOTE]
> 如果你尚未安装 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年) 或更高版本，然后您不会有权访问的函数和本主题中介绍的基类。 相反，请参阅[如果你有较旧版本的 Windows SDK](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk)有关你可以改为使用的可观察矢量模板的列表。

## <a name="helper-functions-for-collections"></a>集合的帮助器函数

### <a name="general-purpose-collection-empty"></a>通用集合为空

本部分介绍当您想要创建集合最初为空; 方案然后填充*后*创建。

若要检索实现的通用集合类型的新对象，可以调用[ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector)函数模板。 作为返回的对象[ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_)，这是通过该调用返回的对象的函数和属性的接口。

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
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

您可以看到在上面的代码示例中，在创建集合后可以追加元素、 循环访问它们，并通常认为该对象，就像您可能已收到 API 从任何 Windows 运行时集合对象。 如果您需要的不可变的视图的集合，则可以调用[ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)，如下所示。 如上所示的模式&mdash;创建和使用集合的&mdash;是适用于想要将数据传入，或从 API 获取数据的简单方案。 可以将传递**IVector**，或**IVectorView**、 任何位置[ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_)预期。

在代码示例中，在调用**winrt::init_apartment**初始化 COM; 默认情况下，在多线程单元中。

### <a name="general-purpose-collection-primed-from-data"></a>通用的集合，从数据中装入

本部分介绍当您想要创建集合，并在同一时间填充的方案。

你可以避免的调用的开销**追加**在前面的代码示例。 可能已存在的源数据，或您可能更倾向于以填充之前创建的 Windows 运行时集合对象的源数据。 下面介绍了如何执行此操作。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

可以将包含到你的数据的临时对象传递**winrt::single_threaded_vector**，如同`coll1`、 更高版本。 也可以移动**std:: vector** （假设您将不会访问它再次） 到函数。 在这两种情况下，将传递*右值*到函数。 这样，编译器工作效率并避免将数据复制。 如果你想要深入了解*rvalues*，请参阅[值的分类，并对其的引用](cpp-value-categories.md)。

如果你想要将 XAML 项目控件绑定到你的集合，然后就可以。 但请注意，若要正确设置[ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)属性，则需要将其设置为值类型**IVector**的**IInspectable**(或的互操作性类型，如[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector))。 下面是类型的一个代码示例，生成适用于绑定的集合以及将元素追加到其中。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

可以从数据中，创建 Windows 运行时集合并准备好将传递到 API，所有而不复制任何内容的视图。

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

在上面的示例中，集合，我们创建*可以*绑定到 XAML 项控件; 但集合不是可观察量。

### <a name="observable-collection"></a>可观察集合

若要检索的类型实现的新对象*可观察量*集合中，调用[ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)与任何函数模板元素类型。 但若要使可观察集合适用于绑定到 XAML 项控件，使用**IInspectable**与元素类型。

作为返回的对象[ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)，这是通过该你 （或绑定到的控件） 调用返回的对象的函数和属性的接口。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

有关详细信息和代码示例，有关绑定用户界面 (UI) 控件添加到可观察集合，请参阅[XAML 控件的项; 绑定到 C + + WinRT 集合](binding-collection.md)。

### <a name="associative-collection-map"></a>关联的集合 (map)

有两个函数，我们介绍了关联集合版本。

- [ **Winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map)函数模板返回一个非可观察量关联集合作为[ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)。
- [ **Winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)函数模板返回作为关联可观察集合[ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

您可以根据需要素数由传递到函数的数据与这些集合*rvalue*类型的**std:: map**或**std:: unordered_map**。

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

"单线程"中的这些函数的名称指示它们未提供任何并发&mdash;换而言之，它们不是线程安全。 提及的线程是无关的单元，因为这些函数返回的对象是所有敏捷 (请参阅[敏捷对象在 C + + WinRT](agile-objects.md))。 它只是对象是单线程。 而这只是想要跨应用程序二进制接口 (ABI) 传递数据的一种方法或其他完全适用。

## <a name="base-classes-for-collections"></a>集合的的基类

如果可以很灵活，您想要实现自己的自定义集合，然后将想要避免执行该操作的硬方式。 例如，这是自定义矢量视图将如下所示*不借助 C + + WinRT 的基类，这些类*。

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

相反，它是派生自定义矢量从视图更易于[ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base)结构模板，并只需实现**get_container**函数公开保存你的数据的容器。

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

返回的容器**get_container**必须提供**开始**并**最终**接口**winrt::vector_view_base**需要。 如上面的示例中所示**std:: vector**提供的。 但是，可返回任何实现了相同的协定，其中包括你自己的自定义容器的容器。

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

这些是基本类的 C + + WinRT 提供了可帮助您实现自定义集合。

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

请参阅上面的代码示例。

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
* [winrt::map_base struct template](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base struct template](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base struct template](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base struct template](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 函数模板](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 结构模板](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 结构模板](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>相关主题
* [值的分类，并对其的引用](cpp-value-categories.md)
* [XAML 控件; 项将绑定到 C + + WinRT 集合](binding-collection.md)
