---
description: 可有效地绑定到 XAML 项目控件的集合称为*可观测*集合。 本主题介绍了如何实现和使用可观测集合以及如何将 XAML 项目控件绑定到该集合。
title: XAML 项目控件；绑定到 C++/WinRT 集合
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 控件, 绑定, 集合
ms.localizationpriority: medium
ms.openlocfilehash: a98056190d035910a8ed83d2f37799a98b685ce6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "70304520"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 项目控件；绑定到 C++/WinRT 集合

可有效地绑定到 XAML 项目控件的集合称为*可观测*集合。 这一想法基于称为“观察者模式”的软件设计模式  。 本主题介绍如何在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中实现可观测集合，以及如何将 XAML 项控件绑定到这些集合（如需背景信息，请参阅[数据绑定](/windows/uwp/data-binding)）。

如果想要跟随此主题进行操作，建议先创建 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)中所介绍的项目。 本主题向该项目添加更多代码，更详细地阐释该主题所介绍的概念。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-collection"></a>对于集合来说，可观测意味着什么  ？
如果表示集合的运行时类选择每当在其中添加或删除元素时引发 [IObservableVector**T&lt;::VectorChanged&gt; 事件，则该运行时类是可观测集合**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)。 XAML 项目控件可检索更新的集合然后将自行更新以显示当前元素，从而绑定到这些事件并处理事件。

> [!NOTE]
> 有关安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>将 BookSkus 集合添加到 BookstoreViewModel  

在 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)中，我们已将类型 BookSku 的属性添加到主视图模型  。 在此步骤中，我们将使用 [winrt::single_threaded_observable_vector **出厂函数模板来帮助我们在同一视图模型上实现 BookSku 的可观测集合**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)  。

声明 `BookstoreViewModel.idl` 中的新属性。

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> 在上述 MIDL 3.0 列表中，注意 **BookSkus** 属性的类型是 [BookSku**的**](/uwp/api/windows.foundation.collections.ivector_t_)IObservableVector  。 在本主题的下一节中，我们会将 [ListBox **的项目源绑定到 BookSkus**](/uwp/api/windows.ui.xaml.controls.listbox)  。 列表框是项控件，用于正确设置 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 属性，你需要将其设置为某个类型为 **IObservableVector** 或 **IVector** 的值，或者某个互操作性类型（例如 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)）的值。

> [!WARNING]
> 本主题中显示的代码适用于 C++/WinRT 2.0.190530.8 及更高版本。 如果使用更低版本，则需对所示代码进行一些细微调整。 在上述 MIDL 3.0 列表中，请将 **BookSkus** 属性更改为 [**IInspectable**](/uwp/api/windows.foundation.collections.ivector_t_) 的 [**IObservableVector**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。 然后，在实现中也使用 **IInspectable**（而不是 **BookSku**）。

保存并生成。 复制 `BookstoreViewModel.h` 文件夹中 `BookstoreViewModel.cpp` 和 `\Bookstore\Bookstore\Generated Files\sources` 的访问器存根（有关详细信息，请参阅上一主题：[XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)）。 实现这些访问器存根，如下所示。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>将 ListBox 绑定到 BookSkus 属性 
打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 在与 Button 相同的 StackPanel 内添加以下标记   。

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

在 `MainPage.cpp` 中，将一行代码添加到 Click 事件处理程序以将书籍追加到集合  。

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

立即生成并运行该项目。 单击该按钮以执行 Click 事件处理程序  。 我们看到了追加的实现引发了让 UI 知道该集合已发生更改的事件；而且 ListBox 重新查询了集合以更新其自己的“项目”值    。 和以前一样，其中一本书籍的标题发生了更改；而且该标题更改反映在按钮上和列表框中。

## <a name="important-apis"></a>重要的 API
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [使用 C++/WinRT 创作 API](author-apis.md)
