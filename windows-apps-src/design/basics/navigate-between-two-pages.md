---
Description: Learn how to enable peer-to-peer navigation between two basic pages in an Universal Windows Platform (UWP) app.
title: 两个页面之间的对等导航
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: e72dc726143d17f605283fa801f8e286c2c58878
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826792"
---
# <a name="implement-navigation-between-two-pages"></a>实现两个页面之间的导航

了解如何使用框架和页面在你的应用中启用基本对等导航。 

> **重要 API**：[**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 类、[**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 类、[**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) 命名空间

![对等导航](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. 创建空白应用

1.  在 Microsoft Visual Studio 菜单上，依次选择**文件** > **新建项目**。
2.  在**新建项目**对话框的左侧窗格中，依次选择**Visual C#** > **Windows** > **通用**节点或 **Visual C++** > **Windows** > **通用**节点。
3.  在中心窗格中，选择**空白应用**。
4.  在**名称**框中，输入 **NavApp1**，然后选择**确定**按钮。
    解决方案已创建，并且项目文件显示在**解决方案资源管理器**中。
5.  若要运行程序，请从菜单中依次选择**调试** > **开始调试**，或按 F5。
    将显示一个空白页。
6.  要停止调试并返回到 Visual Studio，退出该应用，或从菜单中单击**停止调试**。

## <a name="2-add-basic-pages"></a>2. 添加基本页面

接下来，将两个页面添加到项目。

1.  在**解决方案资源管理器**中，右键单击**空白应用**项目节点打开快捷菜单。
2.  从快捷菜单中依次选择**添加** > **新建项**。
3.  在**添加新项**对话框中，选择中间窗格中的**空白页面**。
4.  在**名称**框中，输入 **Page1**（或 **Page2**），然后按**添加**按钮。
5. 重复步骤 1-4 以添加第二个页面。

现在，这些文件应列为 NavApp1 项目的一部分。

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

在 Page1.xaml 中，添加以下内容：

-   名为 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素作为根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 将 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 属性更改为 `Page 1`。
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)元素作为根[**网格**](https://msdn.microsoft.com/library/windows/apps/br242704)和后的子元素`pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)元素。
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

在 Page1.xaml 代码隐藏文件中，添加以下代码以处理添加的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件来导航到 Page2.xaml。

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

在 Page2.xaml 中，添加以下内容：

-   名为 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素作为根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 将 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 属性的值更改为 `Page 2`。
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)元素作为根[**网格**](https://msdn.microsoft.com/library/windows/apps/br242704)和后的子元素`pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)元素。
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

在 Page2.xaml 代码隐藏文件中，添加以下代码以处理添加的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件来导航到 Page1.xaml。

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> 对于 C++ 项目，必须在引用其他页面的每个页面的标头文件中添加 `#include` 指令。 对于此处展示的页面间导航示例，page1.xaml.h 文件包含 `#include "Page2.xaml.h"`，依此类推，page2.xaml.h 包含 `#include "Page1.xaml.h"`。

既然页面已经准备好，我们需要让 Page1.xaml 在应用启动时显示。

打开 App.xaml 代码隐藏文件并更改 `OnLaunched` 处理程序。

我们在此处在对 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 的调用中指定 `Page1`（而不是 `MainPage`）。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> 下面的代码使用[**导航**](https://msdn.microsoft.com/library/windows/apps/br242694)的返回值引发应用异常，如果导航至应用的初始窗口框架失败。 当 **Navigate** 返回 **true** 时，就会进行导航。

现在，生成并运行应用。 单击显示“单击以转到第 2 页”的链接。 在顶部显示“第 2 页”的第二个页面应加载并显示在框架中。

### <a name="about-the-frame-and-page-classes"></a>关于 Frame 和 Page 类

在将更多功能添加到我们的应用之前，让我们来看看我们所添加的页面如何在我们的应用内提供导航。

首先，为 App.xaml 代码隐藏文件中 `App.OnLaunched` 方法中的应用创建一个名为 `rootFrame` 的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 **Frame** 类支持 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)、[**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) 和 [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) 等各种导航方法以及 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543)、[**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) 和 [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995) 等属性。
 
[**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法用于在此 **Frame** 中显示内容。 默认情况下，此方法加载 MainPage.xaml。 在我们的示例中，`Page1` 将传递到 **Navigate** 方法，因此该方法会加载 **Frame** 中的 `Page1`。 

`Page1` 是 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 类的子类。 **Page** 类具有一个只读 **Frame** 属性，可获取包含 **Page** 的 **Frame**。 当 `Page1` 中 **HyperlinkButton** 的 **Click** 事件处理程序调用 `this.Frame.Navigate(typeof(Page2))` 时，**Frame** 将显示 Page2.xaml 的内容。

最后，每当页面加载到框架中时，该页面作为 [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) 添加到 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) 的 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) 或 [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547)，从而支持[历史记录和向后导航](navigation-history-and-backwards-navigation.md)。

## <a name="3-pass-information-between-pages"></a>3. 在页面之间传递信息

我们的应用可在两个页面之间导航，但它还没有什么真正有趣的功能。 当一个应用包含多个页面时，这些页面经常需要共享信息。 让我们将一些信息从第一页传递到第二页。

在 Page1.xaml 中，替换为你添加**HyperlinkButton**更早版本以下[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)。

我们在此处添加 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 标签和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) `name`，用于输入文本字符串。

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

在`HyperlinkButton_Click`Page1.xaml 代码隐藏文件中，事件处理程序添加参数引用`Text`属性`name` **TextBox**到`Navigate`方法。

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

在 Page2.xaml 中，将你之前添加的 **HyperlinkButton** 替换为以下 **StackPanel**。

我们在此处添加 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 来显示从 Page1 传递的文本字符串。

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

在 Page2.xaml 代码隐藏文件中，添加以下内容以重写 `OnNavigatedTo` 方法：

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

运行应用、在文本框中键入你的名字，然后单击显示**单击以转到第 2 页**的链接。 

当 `Page1` 中 **HyperlinkButton** 的 **Click** 事件调用 `this.Frame.Navigate(typeof(Page2), name.Text)` 时，`name.Text` 属性将传递给 `Page2`，并且事件数据中的值将用于在页面上显示的消息。

## <a name="4-cache-a-page"></a>4. 缓存页面

默认情况下不缓存页面内容和状态，因此如果你要缓存信息，必须在应用的每个页面中启用它。

在我们的基本对等示例中，没有后退按钮（我们在[向后导航](navigation-history-and-backwards-navigation.md)中演示了后退导航），但如果你确实单击 `Page2` 上的后退按钮，则 `Page1` 上的 **TextBox**（以及任何其他字段）将设置为其默认状态。 解决此问题的一种方法是使用 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 属性指定需要添加到框架的页面缓存中的页面。 

在 `Page1` 的构造函数中，你可以将 **NavigationCacheMode** 设置为**已启用**，以为该页面保留所有内容和状态值，直到超出框架的页面缓存。 如果你要忽略 [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 限制，将 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 设置为[**必需**](https://msdn.microsoft.com/library/windows/apps/br243284)，这将在导航历史记录中指定可为框架缓存的页数。 但是，请记住，根据设备的内存限制，缓存大小限制可能非常重要。

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
}
```

```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>相关文章
* [UWP 应用的导航设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [表和透视表指南](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [导航窗格指南](https://msdn.microsoft.com/library/windows/apps/dn997766)
