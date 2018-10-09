---
author: stevewhims
description: 可有效地绑定到 XAML 项目控件的集合称为*可观测*集合。 本主题介绍如何实现和使用可观测集合以及如何将 XAML 项目控件绑定到该集合。
title: XAML 项目控件; 绑定到 C++/WinRT 集合
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 控件, 绑定, 集合
ms.localizationpriority: medium
ms.openlocfilehash: 22594c1cfc503b28163d9fca1f46a6861a4f59ad
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4464889"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 项目控件; 绑定到 C++/WinRT 集合

可有效地绑定到 XAML 项目控件的集合称为*可观测*集合。 这一想法基于称为*观察者模式*的软件设计模式。 本主题介绍如何实现可观测集合中的[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，以及如何将 XAML 绑定项目控件到它们。

本演练围绕在 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md)中创建的项目展开，而且它将添加到该主题中所述的概念。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-collection"></a>对于集合来说，*可观测*意味着什么？
如果表示集合的运行时类选择每当在其中添加或删除元素时引发 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 事件，则该运行时类是可观测集合。 XAML 项目控件可检索更新的集合然后将自行更新以显示当前元素，从而绑定到这些事件并处理事件。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的安装和使用的信息，请参阅[针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>将 **BookSkus** 集合添加到 **BookstoreViewModel**

在 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md)中，我们已将类型 **BookSku** 的属性添加到主视图模型。 在此步骤中，我们将使用[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)工厂函数模板来帮助我们在同一视图模型上实现**BookSku**可观测集合。

> [!NOTE]
> 如果尚未安装了 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年) 或更高版本，然后查看[是否有较早版本的 Windows SDK](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk)有关你可以使用而不是**winrt::single_ 可观测矢量模板的列表threaded_observable_vector**。

声明 `BookstoreViewModel.idl` 中的新属性。

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> 在上述 MIDL 3.0 列表中，请注意， **BookSkus**属性的类型的[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) 。 在本主题的下一步部分中，我们将在项目源的[**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox)绑定到绑定**BookSkus**。 列表框的项目控件，而要正确设置[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)属性，你需要设置其类型**IObservableVector**的值 （或**IVector**） **IInspectable**，或互操作性类型，如[**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。

保存并生成。 人 `Generated Files` 文件夹中的 `BookstoreViewModel.h` 和 `BookstoreViewModel.cpp` 复制访问器存根，然后实现它们。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>将 ListBox 绑定到 **BookSkus** 属性
打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 在与 **Button** 相同的 **StackPanel** 内添加以下标记。

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

在 `MainPage.cpp` 中，将一行代码添加到 **Click** 事件处理程序以将书籍追加到集合。

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

立即生成并运行该项目。 单击该按钮以执行 **Click** 事件处理程序。 我们看到了**追加**的实现引发了让 UI 知道该集合已发生更改的事件；而且 **ListBox** 重新查询了集合以更新其自己的**项目**值。 和以前一样，其中一本书籍的标题发生了更改；而且该标题更改反映在按钮上和列表框中。

## <a name="important-apis"></a>重要的 API
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [使用 C++/WinRT 创作 API](author-apis.md)
