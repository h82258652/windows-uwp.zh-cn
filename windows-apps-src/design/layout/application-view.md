---
Description: 使用 w 类在单独的窗口中查看应用程序的不同部分。
title: 使用 w 类显示应用的辅助窗口
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc01894311badd9bb6e88f05c0f8b49c5824736b
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68730525"
---
# <a name="show-multiple-views-with-applicationview"></a>显示具有 w 的多个视图

通过让用户在单独窗口中查看应用的独立部分，可帮助他们提高效率。 如果你为应用创建多个窗口，每个窗口都独立运作。 任务栏会分别显示每个窗口。 用户可以独立地移动、调整大小、显示和隐藏应用窗口，并且可以在应用窗口间切换，就像它们是单独的应用一样。 每个窗口都在它自己的线程中运行。

> **重要的 API**：[**ApplicationViewSwitcher**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)、 [ **CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="what-is-a-view"></a>什么是视图？

应用视图是指将线程与应用用来显示内容的窗口进行 1:1 配对。 它由 [**Windows.ApplicationModel.Core.CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 对象表示。

视图由 [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 对象进行管理。 调用 [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) 来创建 [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 对象。 **CoreApplicationView** 将同时引入 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 和 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)（存储在 [**CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) 和 [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher) 属性中）。 你可以将 **CoreApplicationView** 视为 Windows 运行时用来与核心 Windows 系统进行交互的对象。

通常情况下，你不会直接使用 [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)。 相反，Windows 运行时会在 [**Windows.UI.ViewManagement**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView) 命名空间中提供 [**ApplicationView**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement) 类。 该类将提供你在应用与窗口化系统进行交互时使用的属性、方法和事件。 若要使用 **ApplicationView**，请调用静态 [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 方法，这样会获取与当前 **CoreApplicationView** 的线程紧密相关的 **ApplicationView** 实例。

而且，XAML 框架会将 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 对象包装在 [**Windows.UI.XAML.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 对象中。 在 XAML 应用中，你通常会与 **Window** 对象交互，而不是直接使用 **CoreWindow**。

## <a name="show-a-new-view"></a>显示新视图

虽然每个应用布局都是唯一的，但我们建议在可预测位置加入“新窗口”按钮，如你可以在新窗口中打开的内容右上角。 此外请考虑加入“在新窗口中打开”的上下文菜单选项。

让我们了解一下创建新视图的步骤。 在此处，启动新视图以响应按钮单击。

```csharp
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

1.  调用 [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) 来为视图内容创建新窗口和线程。

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  跟踪新视图的 [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id)。 稍后使用此选项来显示视图。

    你可能需要考虑在你的应用中生成一些基础结构以帮助跟踪你创建的视图。 有关示例，请参阅 [MultipleViews 示例](https://go.microsoft.com/fwlink/p/?LinkId=620574)中的 `ViewLifetimeControl` 类。

    ```csharp
    int newViewId = 0;
    ```

3.  在新线程上，填充窗口。

    使用 [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 方法在新视图的 UI 线程上安排工作。 使用 [lambda 表达式](https://go.microsoft.com/fwlink/p/?LinkId=389615)将函数作为参数传递到 **RunAsync** 方法。 你在 lambda 函数中执行的工作将在新视图的线程上进行。

    在 XAML中，通常向 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 的 [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) 属性添加 [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)，然后将 **Frame** 导航到你已为其定义应用内容的 XAML [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)。 有关框架和页面的详细信息, 请参阅[两个页面之间的对等导航](../basics/navigate-between-two-pages.md)。

    在填充新 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 后，必须调用 **Window** 的 [**Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate) 方法才能在稍后显示 **Window**。 这项工作在新视图的线程上进行，因此会激活新 **Window**。

    最后，获取新视图的 [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id)，用于稍后显示该视图。 同样，此项工作在新视图的线程上进行，因此 [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 会获取新视图的 **Id**。

    ```csharp
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

4.  通过调用 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) 显示新视图。

    在创建新视图之后，你可以通过调用 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) 方法，将其显示在新窗口中。 此方法的 *viewId* 参数是唯一标识你的应用中每个视图的整数。 通过使用 **ApplicationView.Id** 属性或 [**ApplicationView.GetApplicationViewIdForWindow**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow) 方法检索视图 [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id)。

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>主视图


应用启动时创建的第一个视图称为*主视图*。 此视图存储于 [**CoreApplication.MainView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.mainview) 属性中，而且其 [**IsMain**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) 属性为 True。 不要创建此视图；应用会创建它。 主视图的线程可充当应用的管理器，而且所有应用激活事件都会在此线程上传送。

如果打开了辅助视图，主视图的窗口可以隐藏（例如，通过单击窗口标题栏中的关闭 (x) 按钮），但它的线程仍保持活动状态。 在主视图的 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 上调用 [**Close**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.close) 会导致 **InvalidOperationException** 发生。 (使用 "[**应用程序" 退出**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.exit)关闭应用。)如果主视图的线程终止, 应用将关闭。

## <a name="secondary-views"></a>辅助视图


其他视图都是辅助视图，包括你通过调用应用代码中的 [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) 创建的所有视图。 主视图和辅助视图均存储于 [**CoreApplication.Views**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.views) 集合中。 通常情况下，创建辅助视图以响应用户操作。 在某些情况中，系统会为你的应用创建辅助视图。

> [!NOTE]
> 你可以使用 Windows *分配的访问权限*功能在[展台模式](https://docs.microsoft.com/windows/manage/set-up-a-device-for-anyone-to-use)中运行应用。 执行此操作时，系统会创建一个辅助视图以显示锁屏界面上方的应用 UI。 不允许使用应用创建的辅助视图，因此如果你尝试在展台模式下显示自己的辅助视图，将引发异常。

## <a name="switch-from-one-view-to-another"></a>从一个视图切换到另一个视图

考虑为用户提供一种途径来从辅助窗口导航回父窗口。 要执行此操作，请使用 [**ApplicationViewSwitcher.SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) 方法。 从要从中切换的窗口的线程调用此方法，并传递要切换到的窗口的视图 ID。

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

当你使用 [**SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) 时，你可以选择是否想要关闭初始窗口，并通过指定 [**ApplicationViewSwitchingOptions**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions) 的值将它从任务栏中删除。

## <a name="related-topics"></a>相关主题

- [显示多个视图](show-multiple-views.md)
- [显示具有 AppWindow 的多个视图](app-window.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
