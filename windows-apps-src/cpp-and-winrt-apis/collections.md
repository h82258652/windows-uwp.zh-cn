---
author: stevewhims
description: C + + / WinRT 提供函数和节省的时间和精力要实现和/或传递集合很多的基类。
title: 集与 C + + / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp、 标准、 c + +，cpp、 winrt、 投影集合
ms.localizationpriority: medium
ms.openlocfilehash: dacfe4135402b85bac68b63c06f99f97001fa5b9
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "2889324"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用集[C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

内部 Windows Runtime 集合具有大量的复杂移动部件。 但当要将一个集合对象传递给 Windows Runtime 函数，或以实现您自己的集合的属性和集合类型，还有函数和基类在 C + + / WinRT 以支持您。 这些功能出您提交的复杂性，并将您保存时间和精力中的大量开销。

> [!IMPORTANT]
> 本主题中描述的功能均可用，如果您已安装[Windows 10 SDK Preview 构建 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)，或更高版本。

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)是由元素的任何随机访问集合实现的 Windows Runtime 接口。 如果您要自己实现**IVector** ，还需要实现[**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)、 [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)和[**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使您*需要*自定义集合类型，这是工作的大量。 但是，如果您拥有**std::vector** （或**std::map**或**std::unordered_map**） 中的数据，并且您希望执行所有已传递到 Windows 运行时 API，然后将想要避免执行该级别的工作，如果可能。 和避免它*是*可能的因为 C + + / WinRT 可帮助您有效且高效地轻松地创建集。

## <a name="helper-functions-for-collections"></a>Helper 函数集

### <a name="general-purpose-collection-empty"></a>空的通用集合

若要检索的实现通用集合的类型的新对象，您可以调用[**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector)函数模板。 作为[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)，则返回的对象，您可以通过该调用返回的对象的函数和属性的接口。

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

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

正如您所看到上面的代码示例中，创建集合后可以追加元素，循环访问它们，并通常视为对象，就像您可能收到来自 API 任何 Windows Runtime 集合对象。 如果需要对的集合不变的视图，然后可以调用[IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview)，如下所示。 上述模式&mdash;创建和使用集合的&mdash;适合于您想要将数据传入，或获取不足，API 的数据的简单方案。

### <a name="general-purpose-collection-primed-from-data"></a>从数据 primed 的通用集合

您还可以避免为**Append** ，您可以看到上面的代码示例中的呼叫的开销。 您可能已经源数据，或您可能希望填充前创建的 Windows Runtime 集合对象。 操作方法如下。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

您可以传递包含到**winrt::single_threaded_vector**，数据的临时对象与`coll1`，上述。 也可以移动 （假定您将不会访问它再次） **std::vector**到函数。 在这两种情况下，您将*rvalue*传递给该函数。 这样编译器高效并避免将数据复制。 如果您想要了解有关*rvalues*的详细信息，请参阅[值类别，并对它们的引用](cpp-value-categories.md)。

如果您想要 XAML 项目控件绑定到您的集，然后可以。 但请注意，若要正确设置[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)属性，您需要将其设置为类型**IVector** **IInspectable** （或互操作性类型，如[**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)） 的值。 下面是类型的生成的集合适合绑定，并将元素追加到它的代码示例。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

*可以*上面的集合绑定到 XAML 项目控件;但集合不可观测对象。

### <a name="observable-collection"></a>观察到的集合

若要检索的实现*可观测对象*集合的类型的新对象，请使用任何元素类型呼叫[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)函数模板。 但要使适合绑定到 XAML 项控件可观察到的集合，请使用**IInspectable**的元素类型。

作为[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)，则返回的对象，并且的是通过其您 （或绑定到的控件） 调用返回的对象的函数和属性的接口。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

有关详细信息和代码示例，有关绑定用户界面 (UI) 控制到观察到的集合，请参阅[XAML 项目控制; 绑定到 C + + / WinRT 集合](binding-collection.md)。

### <a name="associative-collection-map"></a>关联的集合 (map)

有两个函数，我们已经看关联集版本。

- [**Winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map)函数模板返回非观测关联集合为[**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)。
- [**Winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)函数模板返回作为[**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_)明显关联集合。

（可选） 可以通过将传递给函数的类型**std::map**或**std::unordered_map** *rvalue*来优化这些数据的集合。

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

"单线程"的这些函数的名称中表示它们不提供任何并发&mdash;换句话说，他们不是线程安全。 提及的线程的无关单元，因为所有敏捷从这些函数返回的对象 (请参阅[敏捷对象在 C + + / WinRT](agile-objects.md))。 只是对象是单线程。 这也完全相应，如果您只想要在应用程序二进制接口 (ABI) 上传递数据的一种方法或其他。

## <a name="base-classes-for-collections"></a>基类集合

如果要为完整的灵活性，实现您自己的自定义集合，您需要避免执行的操作的方法。 例如，这是自定义的矢量视图将如下所示*不借助于 C + + / WinRT 的基类*。

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

相反，它是轻松地从[**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base)结构模板，派生自定义向量视图并只需实现**get_container**函数来公开该容器保存您的数据。

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

返回**get_container**容器必须提供的**开始**和**结束**界面**winrt::vector_view_base**该要求。 在上面的示例所示， **std::vector**提供的。 但是，则可以返回满足相同的合同，包括您自己的自定义容器任何容器。

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

这些是基类的 C + + / WinRT 提供可帮助您实现自定义的集合。

### [<a name="winrtvectorviewbase"></a>winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

请参阅上面的代码示例。

### [<a name="winrtvectorbase"></a>winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### [<a name="winrtobservablevectorbase"></a>winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### [<a name="winrtmapviewbase"></a>winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### [<a name="winrtmapbase"></a>winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### [<a name="winrtobservablemapbase"></a>winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>相关主题
* [值类别，并对它们的引用](cpp-value-categories.md)
* [XAML 项目控件；绑定到 C++/WinRT 集合](binding-collection.md)
