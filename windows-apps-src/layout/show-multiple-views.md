---
author: Jwmsft
Description: "通过让用户在单独窗口中查看应用的多个独立部分，可帮助他们提高效率。"
title: "显示应用的多个视图"
ms.assetid: BAF9956F-FAAF-47FB-A7DB-8557D2548D88
label: Show multiple views for an app
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 23e999f86fb0552b96cddbd3b9d11803106bf6c2

---

# 显示应用的多个视图

通过让用户在单独窗口中查看应用的多个独立部分，你可以帮助他们提高效率。 一个典型示例就是电子邮件应用，其中主 UI 显示电子邮件列表以及所选电子邮件的预览。 但是，用户还可以在单独窗口中打开消息，并排查看它们。

**重要的 API**

-   [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094)
-   [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

如果你为应用创建多个窗口，每个窗口都独立运作。 任务栏会分别显示每个窗口。 用户可以独立地移动、调整大小、显示和隐藏应用窗口，并且可以在应用窗口间切换，就像它们是单独的应用一样。 每个窗口都在它自己的线程中运行。

## <span id="What_is_a_view_"></span><span id="what_is_a_view_"></span><span id="WHAT_IS_A_VIEW_"></span>什么是视图？


应用视图是指将线程与应用用来显示内容的窗口进行 1:1 配对。 它由 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 对象表示。

视图由 [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 对象进行管理。 调用 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 来创建 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 对象。 **CoreApplicationView** 将同时引入 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 和 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)（存储在 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 和 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264) 属性中）。 你可以将 **CoreApplicationView** 视为 Windows 运行时用来与核心 Windows 系统进行交互的对象。

通常情况下，你不会直接使用 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)。 相反，Windows 运行时会在 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295) 命名空间中提供 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658) 类。 该类将提供你在应用与窗口化系统进行交互时使用的属性、方法和事件。 若要使用 **ApplicationView**，请调用静态 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 方法，这样会获取与当前 **CoreApplicationView** 的线程紧密相关的 **ApplicationView** 实例。

而且，XAML 框架会将 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象包装在 [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 对象中。 在 XAML 应用中，你通常会与 **Window** 对象交互，而不是直接使用 **CoreWindow**。

## <span id="Show_a_new_view"></span><span id="show_a_new_view"></span><span id="SHOW_A_NEW_VIEW"></span>显示新视图


在继续进行操作之前，让我们看看创建新视图的步骤。 在此处，启动新视图以响应按钮单击。

```CSharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**显示新视图**

1.  调用 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) 来为视图内容创建新窗口和线程。

```    CSharp
CoreApplicationView newView = CoreApplication.CreateNewView();</code></pre></td>
    </tr>
    </tbody>
    </table>
```

2.  跟踪新视图的 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。 稍后使用此选项来显示视图。

    你可能需要考虑在你的应用中生成一些基础结构以帮助跟踪你创建的视图。 有关示例，请参阅 [MultipleViews 示例](http://go.microsoft.com/fwlink/p/?LinkId=620574)中的 `ViewLifetimeControl` 类。

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
int newViewId = 0;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

3.  在新线程上，填充窗口。

    使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法在新视图的 UI 线程上安排工作。 使用 [lambda 表达式](http://go.microsoft.com/fwlink/p/?LinkId=389615)将函数作为参数传递到 **RunAsync** 方法。 你在 lambda 函数中执行的工作将在新视图的线程上进行。

    在 XAML中，通常向 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 的 [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) 属性添加 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)，然后将 **Frame** 导航到你已在其中定义了应用内容的 XAML [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)。 有关详细信息，请参阅[两个页面之间的对等导航](peer-to-peer-navigation-between-two-pages.md)。

    在填充新 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 后，必须调用 **Window** 的 [**Activate**](https://msdn.microsoft.com/library/windows/apps/br209046) 方法以便稍后显示 **Window**。 这项工作在新视图的线程上进行，因此会激活新 **Window**。

    最后，获取新视图的 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)，用于稍后显示该视图。 同样，此项工作在新视图的线程上进行，因此 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 会获取新视图的 **Id**。

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
```

4.  通过调用 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 显示新视图。

    在创建新视图之后，你可以通过调用 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 方法，将其显示在新窗口中。 此方法的 *viewId* 参数是唯一标识你的应用中每个视图的整数。 通过使用 **ApplicationView.Id** 属性或 [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109) 方法检索视图 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。

```    CSharp
bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);</code></pre></td>
    </tr>
    </tbody>
    </table>
```

## <span id="The_main_view"></span><span id="the_main_view"></span><span id="THE_MAIN_VIEW"></span>主视图


应用启动时创建的第一个视图称为*主视图*。 此视图存储于 [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) 属性中，而且其 [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) 属性为 True。 不要创建此视图；应用会创建它。 主视图的线程可充当应用的管理器，而且所有应用激活事件都会在此线程上传送。

如果打开了辅助视图，主视图的窗口可以隐藏（例如，通过单击窗口标题栏中的关闭 (x) 按钮），但它的线程仍保持活动状态。 在主视图的 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 上调用 [**Close**](https://msdn.microsoft.com/library/windows/apps/br209049) 会导致 **InvalidOperationException** 发生。 （使用 [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327) 关闭你的应用。）如果主视图的线程终止，则应用关闭。

## <span id="Secondary_views"></span><span id="secondary_views"></span><span id="SECONDARY_VIEWS"></span>辅助视图


其他视图都是辅助视图，包括你通过调用应用代码中的 [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 创建的所有视图。 主视图和辅助视图均存储于 [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861) 集合中。 通常情况下，创建辅助视图以响应用户操作。 在某些情况中，系统会为你的应用创建辅助视图。

**注意** 你可以使用 Windows *分配的访问权限*功能在[展台模式](https://technet.microsoft.com/library/mt219050.aspx)下运行应用。 执行此操作时，系统会创建一个辅助视图以显示锁屏界面上方的应用 UI。 不允许使用应用创建的辅助视图，因此如果你尝试在展台模式下显示自己的辅助视图，将引发异常。

 

## <span id="Switch_from_one_view_to_another"></span><span id="switch_from_one_view_to_another"></span><span id="SWITCH_FROM_ONE_VIEW_TO_ANOTHER"></span>从一个视图切换到另一个视图


必须为用户提供一种途径来从辅助窗口导航回主窗口。 要执行此操作，请使用 [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 方法。 从要从中切换的窗口的线程调用此方法，并传递要切换到的窗口的视图 ID。

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);</code></pre></td>
</tr>
</tbody>
</table>
```

当你使用 [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 时，你可以选择是否想要关闭初始窗口，并通过指定 [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105) 的值将它从任务栏中删除。

 

 







<!--HONumber=Jun16_HO4-->


