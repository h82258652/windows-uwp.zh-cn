---
author: stevewhims
description: 本主题介绍如何使用 C++/WinRT 注册和撤销事件处理委托。
title: 在 C++/WinRT 中使用委托处理事件
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 已投影, 投影, 处理, 事件, 委托
ms.localizationpriority: medium
ms.openlocfilehash: a29c095e49b49baa63bd547c0bb928ad7f78aa86
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2018
ms.locfileid: "3369913"
---
# <a name="handle-events-by-using-delegates-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的代理来处理事件
本主题介绍如何使用 C++/WinRT 注册和撤销事件处理代理。 你可以使用任何标准 C++ 函数类对象来处理事件。

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
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
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

    Button().Click([this](IInspectable const&, RoutedEventArgs const&)
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

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const&)
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
            ...
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

不是如上面示例中的强引用，你可以存储对按钮的弱引用（请参阅 [C++/WinRT 中的弱引用](weak-references.md)）。

或者，当你注册委托时，你可以指定**winrt:: auto_revoke** （即类型[**winrt:: auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)一个值） 以请求一个事件撤销程序 （的类型[**winrt:: event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker))。 事件撤销程序为你保留对事件源 （引发事件的对象） 的弱引用。 你可以通过调用 **event_revoker::revoke** 成员函数手动撤销；但事件撤销程序会在该函数超出范围时自动调用函数本身。 **撤销**函数检查事件源是否仍然存在，如果存在，将撤销你的代理。 在本示例中，无需存储事件源，并且不需要析构函数。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
        });
    }

private:
    winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> m_event_revoker;
};
```

下面是摘自 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件的文档主题的语法块。 它显示了三个不同的注册和撤销函数。 你可以清楚地看到从第三个重载进行声明时需要哪种类型的事件撤销程序。

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

类似的模式适用于所有 C++/WinRT 事件。

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
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const&, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

如上面的“协同程序”注释所示，与将代理与异步操作和运算的已完成事件结合使用相比，你可能会发现使用协同程序更自然。 有关详细信息和代码示例，请参阅[利用 C++/WinRT 实现的并发和异步运算](concurrency.md)。

但是，如果你确实要继续使用代理，你可以选择更简单的语法。

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const)
{
    ....
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

## <a name="using-the-this-object-in-an-event-handler"></a>在事件处理程序中使用 *this* 对象
如果你处理了对象的成员函数内的 lambda 函数中的一个事件，则需要考虑事件接收者（处理事件的对象）和事件源（引发事件的对象）的相对生存期。

在很多情况下，接收者的生存时间超过给定的 lambda 函数中的 *this* 指针上的所有依赖项。 这些情况中有一部分很明显，例如当 UI 页面处理由页面上的控件引发的事件时。 按钮的生存时间不会超过页面，处理程序也是如此。 每当接收者拥有源（例如作为数据成员）时，或者每当接收者和源是同级且直接由其他某个对象拥有时，就会出现这种情况。 如果你不确定是否遇到处理程序的生存时间不会超过它依赖的 *this* 的情况，则通常可以捕获 *this* 而不考虑强或弱生存时间。

但仍有一些情况，*this* 的生存时间不及它在处理程序（包括用于由异步操作和运算引发的完成和进度事件的处理程序）中的使用时间。

- 如果你要创作用于实现异步方法的协同程序，那么这是可能的。
- 在存在特定 XAML UI 框架对象（例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)）的极少数情况下，如果接收者在完成时没有从事件源取消注册，那么这是可能的。

在这些情况下，处理程序中的代码或协同程序对使用无效的 *this* 对象的连续尝试中的代码将产生访问冲突。

> [!IMPORTANT]
> 如果你遇到任一这些情况，则需要考虑 *this* 对象的生存期；以及已捕获的 *this* 对象的生存时间是否超过捕获时间。 如果未超过，则根据需要使用强引用或弱引用捕获它。 请参阅 [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) 和 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)。
> 或者，如果它对你的场景有意义，而线程处理注意事项进一步增加实现的可能，则你还可以选择在接收者完成该事件后撤销处理程序，或者在接收者的析构函数中撤销处理程序。

此代码示例使用 [**SwapChainPanel.CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 事件作为演示。 它将注册一个事件处理程序，该处理程序使用捕获对接收者的弱引用的 lambda。 有关弱引用的详细信息，请参阅 [C++/WinRT 中的弱引用](weak-references.md)。 

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weakReferenceToThis{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strongReferenceToThis = weakReferenceToThis.get())
        {
            strongReferenceToThis->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

在 lamba 捕获子句中，将创建一个临时变量，用来表示对 *this* 的弱引用。 在 lambda 的正文中，如果可以获取对 *this* 的强引用，则会调用 **OnCompositionScaleChanged** 函数。 这样，在 **OnCompositionScaleChanged** 内便可以安全地使用 *this*。

## <a name="important-apis"></a>重要的 API
* [winrt::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::implements::get_strong 函数](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中创作事件](author-events.md)
* [利用 C++/WinRT 实现的并发和异步操作](concurrency.md)
* [C++/WinRT 中的弱引用](weak-references.md)
