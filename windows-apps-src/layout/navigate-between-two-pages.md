---
author: Jwmsft
Description: "了解如何在基本的两页对等通用 Windows 平台 (UWP) 应用中导航。"
title: "两个页面之间的对等导航"
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 324bdcd8ae61826a2be765f6a6a93441036d6984

---

# <a name="peer-to-peer-navigation-between-two-pages"></a>两个页面之间的对等导航

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

了解如何在基本的两页对等通用 Windows 平台 (UWP) 应用中导航。

![两页对等导航示例](images/nav-peertopeer-2page.png)

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)</li>
<li>[**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)</li>
<li>[**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)</li>
</ul>
</div>



## <a name="create-the-blank-app"></a>创建空白应用


1.  在 Microsoft Visual Studio 菜单上，依次选择“文件”&gt;“新建项目”****。
2.  在“新建项目”****对话框的左侧窗格中，依次选择“Visual C#”-&gt;“Windows”-&gt;“通用”****节点，或者依次选择“Visual C++”-&gt;“Windows”-&gt;“通用”****节点。
3.  在中心窗格中，选择“空白应用”****。
4.  在“名称”****框中，输入 **NavApp1**，然后选择“确定”****按钮。

    解决方案已创建并且项目文件显示在“解决方案资源管理器”****中。

5.  若要运行程序，请从菜单中依次选择“调试”****&gt;“启动调试”****，或按 F5。

    将显示一个空白页面。

6.  按 Shift + F5 可停止调试并返回到 Visual Studio。

## <a name="add-basic-pages"></a>添加基本页面

接下来，将两个内容页面添加到项目。

执行以下步骤两次可添加两个要在其间导航的页面。

1.  在“解决方案资源管理器”****中，右键单击“空白应用”****项目节点打开快捷菜单。
2.  从快捷菜单中依次选择“添加”****&gt;“新项目”****。
3.  在“添加新项”****对话框中，选择中间窗格中的“空白页面”****。
4.  在“名称”****框中，输入 **Page1**（或 **Page2**）并按“添加”****按钮。

这些文件现在应列为 NavApp1 项目的一部分。

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

 

将以下内容添加到 Page1.xaml 的 UI。

-   将名为 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素添加为根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 将 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 属性更改为 `Page 1`。

```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   将以下 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 元素添加为根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素并位于 `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素之后。

    
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

将以下代码添加到 Page1.xaml 代码隐藏文件中的 `Page1` 类，以处理之前添加的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件。 我们在此处导航到 Page2.xaml。

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

对 Page2.xaml 的 UI 进行以下更改。

-   将名为 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素添加为根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 将 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 属性的值更改为 `Page 2`。

```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   将以下 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 元素添加为根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素并位于 `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素之后。

```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

将以下代码添加到 Page2.xaml 代码隐藏文件中的 `Page2` 类，以处理之前添加的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件。 我们在此处导航到 Page1.xaml。

> [!NOTE]
> 对于 C++ 项目，必须在引用其他页面的每个页面的标头文件中添加 `#include` 指令。 对于此处展示的页面间导航示例，page1.xaml.h 文件包含 `#include "Page2.xaml.h"`，依此类推，page2.xaml.h 包含 `#include "Page1.xaml.h"`。

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```
```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

既然内容页面已经准备好，我们需要让 Page1.xaml 在应用启动时显示。

打开 app.xaml 代码隐藏文件并更改 `OnLaunched` 处理程序。

我们在此处在对 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 的调用中指定 `Page1`（而不是 `MainPage`）。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> protected override void OnLaunched(LaunchActivatedEventArgs e)
> {
>     Frame rootFrame = Window.Current.Content as Frame;
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == null)
>     {
>         // Create a Frame to act as the navigation context and navigate to the first page
>         rootFrame = new Frame();
> 
>         rootFrame.NavigationFailed += OnNavigationFailed;
> 
>         if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
>         {
>             //TODO: Load state from previously suspended application
>         }
> 
>         // Place the frame in the current Window
>         Window.Current.Content = rootFrame;
>     }
> 
>     if (rootFrame.Content == null)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame.Navigate(typeof(Page1), e.Arguments);
>     }
>     // Ensure the current window is active
>     Window.Current.Activate();
> }
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
> {
>     auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == nullptr)
>     {
>         // Create a Frame to act as the navigation context and associate it with
>         // a SuspensionManager key
>         rootFrame = ref new Frame();
> 
>         rootFrame->NavigationFailed += 
>             ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
>                 this, &amp;App::OnNavigationFailed);
> 
>         if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
>         {
>             // TODO: Load state from previously suspended application
>         }
>         
>         // Place the frame in the current Window
>         Window::Current->Content = rootFrame;
>     }
> 
>     if (rootFrame->Content == nullptr)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
>     }
> 
>     // Ensure the current window is active
>     Window::Current->Activate();
> }
> ```

**注意**  如果导航至应用的初始窗口框架失败，则此处的代码会使用 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 的返回值引发应用异常。 当 **Navigate** 返回 **true** 时，就会进行导航。

现在，生成并运行应用。 单击显示“单击以转到第 2 页”的链接。 在顶部显示“第 2 页”的第二个页面应加载并显示在框架中。

## <a name="frame-and-page-classes"></a>Frame 和 Page 类

在将更多功能添加到我们的应用之前，让我们来看看我们所添加的页面如何为应用提供导航支持。

首先，为 App.xaml 代码隐藏文件的 `App.OnLaunched` 方法中的应用创建一个 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`)。 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法用于在此 **Frame** 中显示内容。

**注意**  
[**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 类支持 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)、[**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) 和 [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) 等各种导航方法以及 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543)、[**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) 和 [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995) 等属性。

 
在我们的示例中，`Page1` 传递给 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法。 此方法将应用的当前窗口的内容设置为 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)，并将你指定的页面内容加载到 **Frame** 中（在我们的示例中默认为 Page1.xaml 或 MainPage.xaml）。

`Page1` 是 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 类的子类。 **Page** 类具有一个只读 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) 属性，可获取包含 **Page** 的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 当 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br227737) 的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br242739) 事件处理程序调用 ` Frame.Navigate(typeof(Page2))` 时，应用窗口中的 **Frame** 将显示 Page2.xaml 的内容。

当页面加载到框架中时，该页面作为 [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) 添加到 [**Frame**](https://msdn.microsoft.com/library/windows/apps/dn279543) 的 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) 或 [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/br227504)。

## <a name="pass-information-between-pages"></a>在页面之间传递信息

我们的应用可在两个页面之间导航，但它还没有什么真正有趣的功能。 当一个应用包含多个页面时，这些页面经常需要共享信息。 让我们将一些信息从第一页传递到第二页。

在 Page1.xaml 中，将你之前添加的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 替换为以下 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)。

我们在此处添加 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 标签和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (`name`)，用于输入文本字符串。

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

在 Page1.xaml 代码隐藏文件的 `HyperlinkButton_Click` 事件处理程序中，向 `Navigate` 方法添加引用 `name` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 的 `Text` 属性的参数。

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

在 Page2.xaml 代码隐藏文件中，将 `OnNavigatedTo` 方法重写为以下内容：

> [!div class="tabbedCodeSnippets"]
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```
```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

运行应用、在文本框中键入你的名字，然后单击显示“单击以转到第 2 页”****的链接。 如果你已在 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br227737) 的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br242739) 事件中调用 `this.Frame.Navigate(typeof(Page2), tb1.Text)`，`name.Text` 属性将传递给 `Page2`，并且事件数据中的值将用于在页面上显示的消息。

## <a name="cache-a-page"></a>缓存页面

默认情况下不缓存页面内容和状态，因此你必须在应用的每个页面中启用它。

在我们的基本对等示例中，没有后退按钮（我们在[后退按钮导航](navigation-history-and-backwards-navigation.md)中演示了后退导航），但如果你确实单击了 `Page2` 上的后退按钮，则 `Page1` 上的 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)（以及任何其他字段）将设置为其默认状态。 解决此问题的一种方法是使用 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 属性指定需要添加到框架的页面缓存中的页面。

在 `Page1` 的构造函数中，将 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 设置为 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/br243284)。 这将为该页面保留所有内容和状态值，直到超出框架的页面缓存。

如果你希望忽略该框架的缓存大小限制，请将 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 设置为 [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284)。 但是，根据设备的内存限制，缓存大小限制可能非常重要。

> [!NOTE]
> [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 属性指定导航历史记录中可为该框架缓存的页数。

> [!div class="tabbedCodeSnippets"]
```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
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
 

 







<!--HONumber=Dec16_HO2-->


