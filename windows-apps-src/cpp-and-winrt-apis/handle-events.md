---
description: 本主题介绍了如何使用 C++/WinRT 注册和撤销事件处理委托。
title: 在 C++/WinRT 中使用委托处理事件
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 已投影, 投影, 处理, 事件, 委托
ms.localizationpriority: medium
ms.openlocfilehash: cd67ea63fc633716cabf9a293a5faeeed6d24b70
ms.sourcegitcommit: 1e8f51d5730fe748e9fe18827895a333d94d337f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87296177"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>在 C++/WinRT 中使用委托处理事件

本主题介绍了如何使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 注册和撤销事件处理委托。 可以使用任何标准 C++ 函数类对象来处理事件。

> [!NOTE]
> 有关安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>使用 Visual Studio 2019 添加事件处理程序

一种将事件处理程序添加到项目的简便方法是使用 Visual Studio 2019 中的 XAML 设计器用户界面 (UI)。 XAML 页面在 XAML 设计器中打开后，请选择要处理其事件的控件。 在该控件的属性页中的上方，单击闪电形图标以列出所有源于该控件的事件。 然后，双击想要处理的事件，例如，*OnClicked*。

XAML 设计器会将相应的事件处理程序函数原型（和存根实现）添加到源文件，供你替换为自己的实现。

> [!NOTE]
> 通常情况下，无需在 Midl 文件 (`.idl`) 中描述事件处理程序。 因此，XAML 设计器不会向 Midl 文件添加事件处理程序函数原型。 它仅将这些原型添加到 `.h` 和 `.cpp` 文件。

## <a name="register-a-delegate-to-handle-an-event"></a>注册用于处理事件的委托

一个简单示例将处理按钮的单击事件。 使用 XAML 标记注册用于处理该事件的成员函数很常见，如下所示。

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.h
void ClickHandler(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
void MainPage::ClickHandler(
    IInspectable const& /* sender */,
    RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

可以强制注册用于处理事件的成员函数，而不在标记中以声明方式注册。 从下面的代码示例来看可能不明显，但 [ButtonBase::Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 调用的参数是 [RoutedEventHandler](/uwp/api/windows.ui.xaml.routedeventhandler) 委托的实例   。 在本例中，我们使用了采用对象和指向成员函数的指针的 RoutedEventHandler 构造函数重载  。

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> 注册委托时，上述代码示例传递原始的 this 指针（指向当前对象）  。 若要了解如何建立对当前对象的强引用或弱引用，请参阅[如果使用成员函数作为委托](weak-references.md#if-you-use-a-member-function-as-a-delegate)。

下面是一个使用静态成员函数的示例；请注意，语法更简单。

```cppwinrt
// MainPage.h
static void ClickHandler(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click( MainPage::ClickHandler );
}
void MainPage::ClickHandler(
    IInspectable const& /* sender */,
    RoutedEventArgs const& /* args */) { ... }
```

还有其他方法可用来构建 RoutedEventHandler  。 下面是摘自 [RoutedEventHandler](/uwp/api/windows.ui.xaml.routedeventhandler) 的文档主题的语法块（从网页右上角“语言”下拉菜单中选择 C++/WinRT）    。 请注意各种构造函数：一种采用 lambda；另一种是自由函数；还有一种（我们在上面使用的）采用对象和指向成员函数的指针。

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

了解函数调用运算符的语法也很有帮助。 它将告诉你委托的形参需要是怎样的。 如你所见，在本例中，函数调用运算符语法与我们 MainPage::ClickHandler 的形参匹配  。

> [!NOTE]
> 对于任何给定事件，若要了解其委托的详细信息以及该委托的形参，先查看事件本身的文档主题。 接下来以 [UIElement.KeyDown 事件](/uwp/api/windows.ui.xaml.uielement.keydown)为例。 访问该主题，并从“语言”下拉列表中选择 C++/WinRT   。 主题开头的语法块中将显示以下内容。
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> 该信息告诉我们 UIElement.KeyDown 事件（我们正在讨论的主题）具有 KeyEventHandler 的委托类型，因为那是向此事件类型注册委托时所传递的类型   。 因此，立即单击主题上的链接，转到该 [KeyEventHandler 委托](/uwp/api/windows.ui.xaml.input.keyeventhandler)类型。 这里，语法块包含函数调用运算符。 如上所述，它将告诉你委托的形参需要是怎样的。
> 
> ```cppwinrt
> void operator()(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  如你所见，需要声明委托将 IInspectable 作为发送方，并将 [KeyRoutedEventArgs](/uwp/api/windows.ui.xaml.input.keyroutedeventargs) 类的实例作为实参  。
>
> 另以 [Popup.Closed 事件](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed)为例。 其委托类型为 [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler)。 因此，委托会将一个 IInspectable 作为发送方，另一个 IInspectable（因为它是 EventHandler 的类型形参）作为实参    。

如果你不想在事件处理程序中执行很多工作，则可以使用 lambda 函数而不是成员函数。 重复一下，从下面的代码示例来看可能不明显，但一个 RoutedEventHandler 委托正在从 lambda 函数构造，该委托同样需要与之前讨论的函数调用运算符的语法匹配  。

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

在构建委托时可以选择指示稍微明确一些， 以便传递委托或多次使用委托等。

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

当你注册委托时，通常会向你返回一个令牌。 随后，可以使用该令牌撤销委托；这意味着将从事件取消注册委托，再次引发该事件时不会调用该委托。

为简单起见，上面的代码示例都没有介绍如何执行该操作。 但下面这个代码示例将令牌存储在结构的专用数据成员中，并在析构函数中撤销令牌的处理程序。

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

可以不进行上面示例中的强引用，而存储对按钮的弱引用（请参阅 [C++/WinRT 中的强引用和弱引用](weak-references.md)）。

> [!NOTE]
> 当事件源以同步方式引发其事件时，你就可以放心地撤销处理程序：不会收到更多事件了。 但对于异步事件，即使在撤销（尤其是在析构函数中撤销）后，你的对象在开始析构后仍可能收到正在进行的事件。 在析构之前找到取消订阅的地方也许可以缓解此问题；若要查找稳妥的解决方案，请参阅[使用事件处理委托安全访问 *this* 指针](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。

或者，当你注册代理时，也可以指定 winrt::auto_revoke（即 [winrt::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t) 类型的值）以请求一个事件撤销程序（[winrt::event_revoker](/uwp/cpp-ref-for-winrt/event-revoker) 类型）    。 事件撤销程序为你保留对事件源（引发事件的对象）的弱引用。 可以通过调用 event_revoker::revoke 成员函数手动撤销；但事件撤销程序会在该函数超出范围时自动调用函数本身  。 撤销函数检查事件源是否仍然存在，如果存在，将撤销你的代理  。 在本示例中，无需存储事件源，并且不需要析构函数。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(
            winrt::auto_revoke,
            [this](IInspectable const& /* sender */,
            RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

下面是摘自 [ButtonBase::Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件的文档主题的语法块  。 它显示了三个不同的注册和撤销函数。 可以清楚地看到从第三个重载进行声明时需要哪种类型的事件撤销程序。 你可以将相同类型的委托传递给“注册”和“使用 event_revoker 撤销”重载 。

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
> 在上述代码示例中，`Button::Click_revoker` 是 `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>` 的类型别名。 类似的模式适用于所有 C++/WinRT 事件。 每个 Windows 运行时事件都具有返回事件撤销程序的撤销函数重载，且该撤销程序的类型是事件源的成员。 另一个示例是，[CoreWindow::SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) 事件具有注册函数重载，它返回类型 CoreWindow::SizeChanged_revoker 的值   。

在页面-导航方案中，可以考虑撤销处理程序。 如果反复进入某个页面然后退出，则可以在离开该页面时撤销任何处理程序。 或者，如果你重复使用同一页面实例，请检查令牌的值，仅在该值未设置时注册 (`if (!m_token){ ... }`)。 第三个选项是将事件撤销程序作为数据成员存储在页面中。 第四个选项（将在本主题后面描述）是捕获对 lambda 函数中的 this 对象的强引用或弱引用  。

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>如果“自动撤销”委托无法注册

如果在注册委托时尝试指定 [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t)，并且结果是 [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 异常，那么这通常意味着，事件源不支持弱引用。 例如，这是 [**Windows.UI.Composition**](/uwp/api/windows.ui.composition) 命名空间中的常见情况。 在此情况下，不能使用自动撤销功能。 必须故障回复到手动撤销事件处理程序。

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>异步操作和运算的委托类型

前面的示例使用的是 RoutedEventHandler 委托类型，但当然还有很多其他委托类型  。 例如，异步操作和运算（带进度和不带进度）具有期望相应类型的委托的已完成和/或进度事件。 例如，带进度的异步运算进度事件（可以是实现 [IAsyncOperationWithProgress](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) 的任何内容）需要 [AsyncOperationProgressHandler](/uwp/api/windows.foundation.asyncoperationprogresshandler-2) 类型的委托   。 下面是使用 lambda 函数创作该类型的委托的代码示例。 该示例还演示了如何创作 [AsyncOperationWithProgressCompletedHandler](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler-2) 代理  。

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](
            IAsyncOperationWithProgress<SyndicationFeed,
            RetrievalProgress> const& /* sender */,
            RetrievalProgress const& args)
        {
            uint32_t bytes_retrieved = args.BytesRetrieved;
            // use bytes_retrieved;
        });

    async_op_with_progress.Completed(
        [](
            IAsyncOperationWithProgress<SyndicationFeed,
            RetrievalProgress> const& sender,
            AsyncStatus const /* asyncStatus */)
        {
            SyndicationFeed syndicationFeed = sender.GetResults();
            // use syndicationFeed;
        });

    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

如上面的“协同程序”注释所示，与将代理与异步操作和运算的已完成事件结合使用相比，你可能会发现使用协同程序更自然。 详细信息和代码示例，请参阅[利用 C++/WinRT 实现的并发和异步运算](concurrency.md)。

> [!NOTE]
> 对一个异步操作或运算实现多个完成处理程序是错误的做法  。 可对其已完成的事件使用单个委托，或者可对其运行 `co_await`。 如果同时采用这两种方法，则第二种方法会失败。

如果坚持使用委托而不是协同程序，则可以选择更简单的语法。

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>返回一个值的代理类型

某些委托类型本身必须返回一个值。 示例：[ListViewItemToKeyHandler](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler)，它将返回一个字符串  。 下面是创作该类型的委托的示例（请注意，lambda 函数将返回一个值）。

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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>使用事件处理委托安全访问 this 指针

如果你使用对象的成员函数处理事件，或者从对象成员函数中的某个 lambda 函数内部处理事件，则需要考虑事件接收方（处理事件的对象）和事件源（引发事件的对象）的相对生存期。 详细信息以及代码示例，请参阅 [C++/WinRT 中的强引用和弱引用](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。

## <a name="important-apis"></a>重要的 API
* [winrt::auto_revoke_t 标记结构](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::implements::get_strong 函数](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中创作事件](/windows/uwp/cpp-and-winrt-apis/author-events)
* [利用 C++/WinRT 实现的并发和异步操作](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [C++/WinRT 中的弱引用](/windows/uwp/cpp-and-winrt-apis/weak-references)
