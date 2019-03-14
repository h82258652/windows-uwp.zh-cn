---
Description: 在单独的窗口中查看您的应用程序的多个部分。
title: 显示应用的多个视图
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7ed69dc912e916f7964c125550621c22dfcd9555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607622"
---
# <a name="show-multiple-views-for-an-app"></a>显示应用的多个视图

![使用多个窗口显示应用的线框](images/multi-view.gif)

通过让用户在单独窗口中查看应用的独立部分，可帮助他们提高效率。 如果你为应用创建多个窗口，每个窗口都独立运作。 任务栏会分别显示每个窗口。 用户可以独立地移动、调整大小、显示和隐藏应用窗口，并且可以在应用窗口间切换，就像它们是单独的应用一样。 每个窗口都在它自己的线程中运行。

> **重要的 Api**:[**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094)， [ **CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

## <a name="when-should-an-app-use-multiple-views"></a>应用应在何时使用多个视图？
有多种情况可从多个视图中获益。 以下是几个示例：
 - 电子邮件应用，可让用户在撰写新电子邮件的同时查看收到邮件的列表
 - 地址簿应用，可让用户并排比较多名人员的联系信息
 - 音乐播放器应用，可让用户在查看播放内容的同时浏览其他可收听音乐的列表
 - 记笔记应用，可让用户将信息从笔记的一页复制到另一页
 - 阅读应用，可让用户在错过细读所有高水平的头条新闻后，打开多篇文章以备以后阅读

要创建应用的单独实例，请参阅[创建多实例 UWP 应用](../../launch-resume/multi-instance-uwp.md)。

## <a name="what-is-a-view"></a>什么是视图？

应用视图是指将线程与应用用来显示内容的窗口进行 1:1 配对。 它由 [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 对象表示。

视图由 [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 对象进行管理。 调用 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 来创建 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 对象。 **CoreApplicationView** 将同时引入 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 和 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)（存储在 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 和 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264) 属性中）。 你可以将 **CoreApplicationView** 视为 Windows 运行时用来与核心 Windows 系统进行交互的对象。

通常情况下，你不会直接使用 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)。 相反，Windows 运行时会在 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/hh701658) 命名空间中提供 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/br242295) 类。 该类将提供你在应用与窗口化系统进行交互时使用的属性、方法和事件。 若要使用 **ApplicationView**，请调用静态 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 方法，这样会获取与当前 **CoreApplicationView** 的线程紧密相关的 **ApplicationView** 实例。

而且，XAML 框架会将 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象包装在 [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 对象中。 在 XAML 应用中，你通常会与 **Window** 对象交互，而不是直接使用 **CoreWindow**。

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

**若要显示的新视图**

1.  调用 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) 来为视图内容创建新窗口和线程。

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  跟踪新视图的 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。 稍后使用此选项来显示视图。

    你可能需要考虑在你的应用中生成一些基础结构以帮助跟踪你创建的视图。 有关示例，请参阅 [MultipleViews 示例](https://go.microsoft.com/fwlink/p/?LinkId=620574)中的 `ViewLifetimeControl` 类。

    ```csharp
    int newViewId = 0;
    ```

3.  在新线程上，填充窗口。

    使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法在新视图的 UI 线程上安排工作。 使用 [lambda 表达式](https://go.microsoft.com/fwlink/p/?LinkId=389615)将函数作为参数传递到 **RunAsync** 方法。 你在 lambda 函数中执行的工作将在新视图的线程上进行。

    在 XAML中，通常向 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 的 [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) 属性添加 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)，然后将 **Frame** 导航到你已为其定义应用内容的 XAML [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)。 有关详细信息，请参阅[两个页面之间的对等导航](../basics/navigate-between-two-pages.md)

    在填充新 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 后，必须调用 **Window** 的 [**Activate**](https://msdn.microsoft.com/library/windows/apps/br209046) 方法才能在稍后显示 **Window**。 这项工作在新视图的线程上进行，因此会激活新 **Window**。

    最后，获取新视图的 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)，用于稍后显示该视图。 同样，此项工作在新视图的线程上进行，因此 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 会获取新视图的 **Id**。

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

4.  通过调用 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 显示新视图。

    在创建新视图之后，你可以通过调用 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 方法，将其显示在新窗口中。 此方法的 *viewId* 参数是唯一标识你的应用中每个视图的整数。 通过使用 **ApplicationView.Id** 属性或 [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109) 方法检索视图 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>主视图


应用启动时创建的第一个视图称为*主视图*。 此视图存储于 [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) 属性中，而且其 [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) 属性为 True。 不要创建此视图；应用会创建它。 主视图的线程可充当应用的管理器，而且所有应用激活事件都会在此线程上传送。

如果打开了辅助视图，主视图的窗口可以隐藏（例如，通过单击窗口标题栏中的关闭 (x) 按钮），但它的线程仍保持活动状态。 在主视图的 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 上调用 [**Close**](https://msdn.microsoft.com/library/windows/apps/br209049) 会导致 **InvalidOperationException** 发生。 (使用[ **Application.Exit** ](https://msdn.microsoft.com/library/windows/apps/br242327)关闭您的应用程序。)如果主视图的线程将被终止，在应用关闭。

## <a name="secondary-views"></a>辅助视图


其他视图都是辅助视图，包括你通过调用应用代码中的 [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 创建的所有视图。 主视图和辅助视图均存储于 [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861) 集合中。 通常情况下，创建辅助视图以响应用户操作。 在某些情况中，系统会为你的应用创建辅助视图。

> [!NOTE]
> 你可以使用 Windows *分配的访问权限*功能在[展台模式](https://technet.microsoft.com/library/mt219050.aspx)中运行应用。 执行此操作时，系统会创建一个辅助视图以显示锁屏界面上方的应用 UI。 不允许使用应用创建的辅助视图，因此如果你尝试在展台模式下显示自己的辅助视图，将引发异常。

## <a name="switch-from-one-view-to-another"></a>从一个视图切换到另一个视图

考虑为用户提供一种途径来从辅助窗口导航回父窗口。 要执行此操作，请使用 [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 方法。 从要从中切换的窗口的线程调用此方法，并传递要切换到的窗口的视图 ID。

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

当你使用 [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 时，你可以选择是否想要关闭初始窗口，并通过指定 [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105) 的值将它从任务栏中删除。

## <a name="dos-and-donts"></a>应做事项和禁止事项

* 务必利用“打开新窗口”字形为辅助视图提供清晰的入口点。
* 务必向用户传达辅助视图的用途。
* 务必确保应用可以在单个视图中充分发挥功能，用户打开辅助视图只是为了方便。
* 不依靠辅助视图提供通知或其他暂时的显示内容。

## <a name="related-topics"></a>相关主题

* [ApplicationViewSwitcher](https://msdn.microsoft.com/library/windows/apps/dn281094)
* [CreateNewView](https://msdn.microsoft.com/library/windows/apps/dn297278)
 
