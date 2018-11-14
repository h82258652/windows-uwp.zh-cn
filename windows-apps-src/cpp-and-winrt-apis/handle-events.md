---
author: stevewhims
description: 本主题介绍如何使用 C++/WinRT 注册和撤销事件处理委托。
title: 在 C++/WinRT 中使用委托处理事件
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 已投影, 投影, 处理, 事件, 委托
ms.localizationpriority: medium
ms.openlocfilehash: 9ca257de29be8e732e05c343a4b782b1676627bf
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6447172"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>在 C++/WinRT 中使用代理处理事件

本主题介绍了如何注册和撤销事件处理委托使用[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。 你可以使用任何标准 C++ 函数类对象来处理事件。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的安装和使用的信息，请参阅[针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="register-a-delegate-to-handle-an-event"></a>注册用于处理事件的委托

一个简单示例将处理按钮的单击事件。 使用 XAML 标记注册用于处理该事件的成员函数很常见，如下所示。

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

你可以强制注册用于处理事件的成员函数，而不是在标记中以声明方式注册。 从下面的代码示例来看可能不明显，但 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 调用的参数是 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 委托的实例。 在本例中，我们使用了采用对象和指向成员函数的指针的 **RoutedEventHandler** 构造函数重载。

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> 当注册委托，上面的代码示例会传递原始*此*指针 （指向当前对象）。 若要了解如何建立的强引用或与当前的弱引用，请参阅[安全地访问*此*指针事件处理委托](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)部分的**如果你使用的成员函数用作代理**子部分。

还有其他方法可用来构建 **RoutedEventHandler**。 下面是摘自 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 的文档主题的语法块（从页面上的**语言**下拉菜单选择 *C++/WinRT*）。 请注意各种构造函数：一种采用 lambda；另一种是自由函数；还有一种（我们在上面使用的）采用对象和指向成员函数的指针。

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

了解函数调用运算符的语法也很有帮助。 它将告诉你委托的参数需要是怎样的。 如你所见，在本例中，函数调用运算符语法与我们 **MainPage::ClickHandler** 的参数匹配。

如果你不想在事件处理程序中执行很多工作，则可以使用 lambda 函数而不是成员函数。 重复一下，从下面的代码示例来看可能不明显，但一个 **RoutedEventHandler** 委托正在从 lambda 函数构造，该委托同样需要与函数调用运算符的语法匹配。

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

在构建委托时，例如希望传递委托或多次使用委托时， 你可以选择指示稍微明确一些。。

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>撤销已注册的委托

当你注册委托时，通常会向你返回一个令牌。 随后，你可以使用该令牌撤销委托；这意味着委托将从事件取消注册，当再次引发事件时，不会调用委托。 为简单起见，上面的代码示例介绍了如何执行该操作。 但后面这个代码示例将令牌存储在结构的专用数据成员中，并在析构函数中撤销令牌的处理程序。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            // ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

而不是一个强引用，如上述示例中所示，你可以存储对按钮的弱引用 (请参阅[强引用和弱引用在 C + + WinRT](weak-references.md))。

或者，当你注册委托时，你可以指定**winrt:: auto_revoke** （这是一个值的类型[**winrt:: auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)） 以请求一个事件撤销程序 （的类型[**winrt:: event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker))。 事件撤销程序为你保留对事件源 （引发事件的对象） 的弱引用。 你可以通过调用 **event_revoker::revoke** 成员函数手动撤销；但事件撤销程序会在该函数超出范围时自动调用函数本身。 **撤销**函数检查事件源是否仍然存在，如果存在，将撤销你的代理。 在本示例中，无需存储事件源，并且不需要析构函数。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

下面是摘自 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件的文档主题的语法块。 它显示了三个不同的注册和撤销函数。 你可以清楚地看到从第三个重载进行声明时需要哪种类型的事件撤销程序。

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> 在上面的代码示例`Button::Click_revoker`是类型别名`winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`。 类似的模式适用于所有 C++/WinRT 事件。 每个 Windows 运行时事件已返回一个事件撤销程序，然后撤销程序的类型是事件源的成员的撤消函数重载。 因此，若要参加另一个示例中， [**corewindow:: Sizechanged**](/uwp/api/windows.ui.core.corewindow.sizechanged)事件都有返回值的类型**CoreWindow::SizeChanged_revoker**注册函数重载。


你可以考虑撤销页面-导航方案中的处理程序。 如果你反复进入某个页面然后退出，则可以在离开该页面时撤销任何处理程序。 或者，如果你重复使用同一页面实例，则还可以检查令牌的值并且仅在它尚未被设置时注册它 (`if (!m_token){ ... }`)。 第三个选项是将事件撤销程序作为数据成员存储在页面中。 第四个选项（将在本主题后面描述）是捕获对 lambda 函数中的 *this* 对象的强引用或弱引用。

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>异步操作和运算的委托类型

前面的示例使用的是 **RoutedEventHandler** 委托类型，但当然还有很多其他委托类型。 例如，异步操作和运算（带进度和不带进度）具有期望相应类型的委托的已完成和/或进度事件。 例如，带进度的异步运算进度事件（可以是实现 [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的任何内容）需要 [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler) 类型的委托。 下面是使用 lambda 函数创作该类型的委托的代码示例。 该示例还演示了如何创作 [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler) 代理。

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

如上面的“协同程序”注释所示，与将代理与异步操作和运算的已完成事件结合使用相比，你可能会发现使用协同程序更自然。 有关详细信息和代码示例，请参阅[利用 C++/WinRT 实现的并发和异步运算](concurrency.md)。

> [!NOTE]
> 不正确实现为异步操作或操作的多个*完成处理程序*。 你可以让任一单个为其已完成的事件，委托，或者你可以`co_await`它。 如果你有两者，则第二个将失败。

如果你继续使用而不是协同程序的委托，然后你可以选择简单的语法。

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>返回一个值的代理类型

某些委托类型本身必须返回一个值。 示例：[**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler)，它将返回一个字符串。 下面是创作该类型的委托的示例（请注意，lambda 函数将返回一个值）。

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>安全地访问*此*指针事件处理委托

如果你处理事件的对象的成员函数，或从内的 lambda 函数对象的成员函数，则需要考虑事件接收者 （处理事件的对象） 和事件源 （的对象的相对生存期引发事件）。 有关详细信息和代码示例，请参阅[强引用和弱引用在 C + + WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。

## <a name="important-apis"></a>重要的 API
* [winrt:: auto_revoke_t 标记结构](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::implements::get_strong 函数](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中创作事件](author-events.md)
* [利用 C++/WinRT 实现的并发和异步操作](concurrency.md)
* [强引用和弱引用在 C + + WinRT](weak-references.md)
