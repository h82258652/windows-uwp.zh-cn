---
author: mijacobs
Description: "通用 Windows 平台 (UWP) 应用中的导航基于一个导航结构、导航元素和系统级功能的灵活模型。"
title: "导航历史记录和向后导航（Windows 应用）"
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: bfff3a4787a37156ef3232372a125db60678ebac

---

#  <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>UWP 应用的导航历史记录和向后导航

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

在 Web 上，个别网站提供其自己的导航系统，如内容表、按钮、菜单、链接的简单列表等。 不同网站上的导航体验可能截然不同。 但是，有一个一致的导航体验：后退。 无论使用何种网站，大多数浏览器都提供运作方式相同的后退按钮。

出于类似的原因，通用 Windows 平台 (UWP) 提供一个一致的后退导航系统，用于遍历用户在应用内和应用之间（具体取决于设备）的导航历史记录。

系统后退按钮的 UI 将针对每种外形规格和输入设备类型进行优化，但设备和 UWP 应用上的导航体验却具有全局性和一致性。

以下是后退按钮 UI 的主要外形规格：


<table>
    <tr>
        <td colspan="2">设备</td>
        <td style="vertical-align:top;">“后退”按钮行为</td>
     </tr>
    <tr>
        <td style="vertical-align:top;">手机</td>
        <td style="vertical-align:top;">![手机上的系统后退功能](images/back-systemback-phone.png)</td>
        <td style="vertical-align:top;">
        <ul>
<li>始终显示。</li>
<li>设备底部的软件或硬件按钮。</li>
<li>应用内和应用间的全局后退导航。</li>
</ul>
</td>
     </tr>
     <tr>
        <td style="vertical-align:top;">平板电脑</td>
        <td style="vertical-align:top;">![平板电脑上的系统后退功能（在平板电脑模式下）](images/back-systemback-tablet.png)</td>
        <td style="vertical-align:top;">
<ul>
<li>始终在平板电脑模式下显示。 在桌面模式下不可用。 可以改为启用标题栏后退按钮。 请参阅 [PC、笔记本电脑、平板电脑](#PC)。
通过转到**设置 &gt; 系统 &gt; 平板电脑模式**并设置**将设备用作平板电脑时使 Windows 更兼容触摸功能**，用户可以在以平板电脑模式运行和以桌面模式运行之间切换。</li>
<li> 设备底部的导航栏中的软件按钮。</li>
<li>应用内和应用间的全局后退导航。</li></ul>        
        </td>
     </tr>
    <tr>
        <td style="vertical-align:top;">PC、笔记本电脑、平板电脑</td>
        <td style="vertical-align:top;">![PC 或笔记本电脑上的系统后退功能](images/back-systemback-pc.png)</td>
        <td style="vertical-align:top;">
<ul>
<li>在桌面模式下可选。 在平板电脑模式下不可用。 请参阅[平板电脑](#Tablet)。 默认情况下处于禁用状态。 必须选择启用它。
通过转到**设置 &gt; 系统 &gt; 平板电脑模式**并设置**将设备用作平板电脑时使 Windows 更兼容触摸功能**，用户可以在以平板电脑模式运行和以桌面模式运行之间切换。</li>
<li>应用标题栏中的软件按钮。</li>
<li>仅应用内的后退导航。 不支持应用间的导航。</li></ul>        
        </td>
     </tr>
    <tr>
        <td style="vertical-align:top;">Surface Hub</td>
        <td style="vertical-align:top;">![Surface Hub 上的系统后退功能](images/nav/nav-back-surfacehub.png)</td>
        <td style="vertical-align:top;">
<ul>
<li>可选。</li>
<li>默认情况下处于禁用状态。 必须选择启用它。</li>
<li>应用标题栏中的软件按钮。</li>
<li>仅应用内的后退导航。 不支持应用间的导航。</li></ul>        
        </td>
     </tr>     
<table>


以下是一些不依赖后退按钮 UI 的替换输入类型，但这些类型仍提供完全相同的功能。


<table>
<tr><td colspan="3">输入设备</td></tr>
<tr><td style="vertical-align:top;">键盘</td><td style="vertical-align:top;">![键盘](images/keyboard-wireframe.png)</td><td style="vertical-align:top;">Windows 键 + Backspace</td></tr>
<tr><td style="vertical-align:top;">Cortana</td><td style="vertical-align:top;">![语音](images/speech-wireframe.png)</td><td style="vertical-align:top;">说“你好小娜，请返回”</td></tr>
</table>
 

当你的应用在手机、平板电脑上或者在支持系统后退功能的 PC 或笔记本电脑上运行时，系统会在按下“后退”按钮时通知你的应用。 用户预期后退按钮导航到应用导航历史记录中的上一个位置。 由你来决定要将哪些导航操作添加到导航历史记录以及如何响应后退按钮按下操作。


## <a name="how-to-enable-system-back-navigation-support"></a>如何启用系统后退导航支持


应用必须启用所有硬件和软件系统后退按钮的后退导航。 执行此操作的方法是注册 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件的侦听器并定义相应处理程序。

在此处我们为 App.xaml 代码隐藏文件中的 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件注册全局侦听器。 如果你想要从后退导航排除特定页面，或想要在显示页面前执行页面级别代码，可以在每个页面中注册此事件。

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += 
    App_BackRequested;
```
```cpp
Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->
    BackRequested += ref new Windows::Foundation::EventHandler<
    Windows::UI::Core::BackRequestedEventArgs^>(
        this, &amp;App::App_BackRequested);
```

以下是在应用的根框架上调用 [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn893596) 的相应 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn996568) 事件处理程序。

此处理程序在全局后退事件上调用。 如果应用内后退堆栈为空，系统可能导航至 应用堆栈中的上一个应用或“开始”屏幕。 桌面模式没有应用后退堆栈，并且即使在应用内后退堆栈耗尽时，用户也将停留在该应用中。

> [!div class="tabbedCodeSnippets"]
```csharp
>private void App_BackRequested(object sender, 
>    Windows.UI.Core.BackRequestedEventArgs e)
>{
>    Frame rootFrame = Window.Current.Content as Frame;
>    if (rootFrame == null)
>        return;
>
>    // Navigate back if possible, and if the event has not 
>    // already been handled .
>    if (rootFrame.CanGoBack &amp;&amp; e.Handled == false)
>    {
>        e.Handled = true;
>        rootFrame.GoBack();
>    }
>}
```
```cpp
>void App::App_BackRequested(
>    Platform::Object^ sender, 
>    Windows::UI::Core::BackRequestedEventArgs^ e)
>{
>    Frame^ rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
>    if (rootFrame == nullptr)
>        return;
>
>    // Navigate back if possible, and if the event has not
>    // already been handled.
>    if (rootFrame->CanGoBack && e->Handled == false)
>    {
>        e->Handled = true;
>        rootFrame->GoBack();
>    }
>}
```

## <a name="how-to-enable-the-title-bar-back-button"></a>如何启用标题栏后退按钮


支持桌面模式（通常是 PC 和笔记本电脑，但也有一些平板电脑）并启用了设置（“设置”&gt;“系统”&gt;“平板电脑模式”****）的设备不提供带有系统后退按钮的全局导航栏。

在桌面模式下，每个应用都在带有标题栏的窗口中运行。 你可以为在此标题栏中显示的应用提供备用后退按钮。

标题栏后退按钮仅在桌面模式下的设备上运行的应用中可用，并且仅支持应用内导航历史记录（它不支持应用间的导航历史记录）。

**重要提示**  默认情况下不显示标题栏后退按钮。 必须选择显示该按钮。

 

|                                                             |                                                        |
|-------------------------------------------------------------|--------------------------------------------------------|
| ![桌面模式下没有系统后退功能](images/nav-noback-pc.png) | ![桌面模式下的系统后退功能](images/nav-back-pc.png) |
| 桌面模式，没有后退导航。                           | 桌面模式，启用后退导航。                 |

 

在隐藏代码文件中，为你想要启用标题栏后退按钮的每个页面重写 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 事件并将 [**AppViewBackButtonVisibility**](https://msdn.microsoft.com/library/windows/apps/dn986448) 设置为 [**Visible**](https://msdn.microsoft.com/library/windows/apps/dn986276)。

例如，如果帧的 [**CanGoBack**](https://msdn.microsoft.com/library/windows/apps/br242685) 属性值为 **true**，我们将在后退堆栈中列出每个页面并启用后退按钮。

> [!div class="tabbedCodeSnippets"]
>```csharp
>protected override void OnNavigatedTo(NavigationEventArgs e)
>{
>    Frame rootFrame = Window.Current.Content as Frame;
>
>    string myPages = "";
>    foreach (PageStackEntry page in rootFrame.BackStack)
>    {
>        myPages += page.SourcePageType.ToString() + "\n";
>    }
>    stackCount.Text = myPages;
>
>    if (rootFrame.CanGoBack)
>    {
>        // Show UI in title bar if opted-in and in-app backstack is not empty.
>        SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
>            AppViewBackButtonVisibility.Visible;
>    }
>    else
>    {
>        // Remove the UI from the title bar if in-app back stack is empty.
>        SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
>            AppViewBackButtonVisibility.Collapsed;
>    }
>}
>```
>```cpp
>void StartPage::OnNavigatedTo(NavigationEventArgs^ e)
>{
>    auto rootFrame = dynamic_cast<Windows::UI::Xaml::Controls::Frame^>(Window::Current->Content);
>
>    Platform::String^ myPages = "";
>
>    if (rootFrame == nullptr)
>        return;
>
>    for each (PageStackEntry^ page in rootFrame->BackStack)
>    {
>        myPages += page->SourcePageType.ToString() + "\n";
>    }
>    stackCount->Text = myPages;
>
>    if (rootFrame->CanGoBack)
>    {
>        // If we have pages in our in-app backstack and have opted in to showing back, do so
>        Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->AppViewBackButtonVisibility =
>            Windows::UI::Core::AppViewBackButtonVisibility::Visible;
>    }
>    else
>    {
>        // Remove the UI from the title bar if there are no pages in our in-app back stack
>        Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->AppViewBackButtonVisibility =
>            Windows::UI::Core::AppViewBackButtonVisibility::Collapsed;
>    }
>}
>```


### <a name="guidelines-for-custom-back-navigation-behavior"></a>自定义后退导航行为指南

如果你选择提供你自己的后退堆栈导航，则体验应与其他应用保持一致。 我们建议你遵循以下导航操作模式：

<table>
<thead>
<tr class="header">
<th align="left">导航操作</th>
<th align="left">添加到导航历史记录？</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>页面到页面，不同的对等组</strong></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在此图中，用户从应用的级别 1 导航到级别 2，并且跨对等组，因此该导航将添加到导航历史记录。</p>
<p><img src="images/nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>在下图中，用户在两个同级别的对等组之间导航，并且再次跨对等组，因此该导航将添加到导航历史记录。</p>
<p><img src="images/nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>页面到页面，同一对等组，无屏幕导航元素</strong>
<p>用户从一个页面导航到同一对等组内的另一个页面。 没有始终存在的导航元素（例如选项卡/透视表或停靠的导航窗格）可提供到两个页面的直接导航。</p></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在下图中，用户在同一对等组中的两个页面之间导航。 这些页面不使用表或停靠的导航窗格，因此该导航将添加到导航历史记录。</p>
<p><img src="images/nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>页面到页面，同一对等组，带有屏幕导航元素</strong>
<p>用户从一个页面导航到同一对等组内的另一个页面。 两个页面显示在相同的导航元素中。 例如，两个页面使用相同的选项卡/透视表元素，或者两个页面都显示在停靠的导航窗格中。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>当用户按下后退时，将在用户导航到当前对等组之前返回到上一个页面。</p>
<p><img src="images/nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>显示瞬态 UI</strong>
<p>应用显示弹出窗口或子窗口（例如对话框、初始屏幕或屏幕键盘），或应用进入特殊模式（例如多重选择模式）。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>当用户按下后退按钮时，取消瞬态 UI（隐藏屏幕键盘、取消对话框等）并返回到生成瞬态 UI 的页面。</p>
<p><img src="images/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>枚举项目</strong>
<p>应用显示屏幕项目的内容，例如大纲/细节列表中的选定项目的详细信息。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>枚举项目与在对等组内导航类似。 当用户按下后退时，导航到位于当前页面前面的具有项目枚举的页面。</p>
<img src="images/nav/nav-enumerate.png" alt="Iterm enumeration" /></td>
</tr>
</tbody>
</table>


### <a name="resuming"></a>恢复中

当用户切换到其他应用并返回到你的应用时，我们建议返回到导航历史记录中的最后一页。


## <a name="get-the-samples"></a>获取示例
*   [后退按钮示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackButton)<br/>
    演示如何设置后退按钮事件的事件处理程序，以及如何针对应用位于窗口化的桌面模式下的情形启用标题栏后退按钮。

## <a name="related-articles"></a>相关文章
* [导航基础知识](navigation-basics.md)

 







<!--HONumber=Dec16_HO4-->


