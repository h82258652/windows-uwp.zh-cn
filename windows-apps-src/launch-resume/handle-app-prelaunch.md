---
title: 处理应用预启动
description: 了解如何通过替代 OnLaunched 方法来处理应用预启动。
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
---

# 处理应用预启动


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

了解如何通过替代 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法来处理应用预启动。

## 简介


当允许使用可用的系统资源时，可通过在后台主动启动用户的常用应用，来提升 Windows 应用商店应用的启动性能。 预启动的应用在启动后不久就会处于暂停状态。 在用户调用该应用时，它通过从暂停状态切换到运行状态而得到恢复，这比冷启动应用的速度要快得多。

在 Windows 10 之前，应用不会自动利用预启动。 从 Windows 10 开始，所有通用 Windows 平台 (UWP) 应用均自动利用预启动。

大多数应用在未进行任何更改的情况下都可以与预启动结合使用。 但是，某些类型的应用可能需要更改启动行为，以便可以与预启动很好地结合使用。 例如，在启动期间更改用户联机可见性的消息应用，或在应用启动时假设用户存在并显示复杂视觉效果的游戏。

## 预启动和应用声明周期


一旦预启动应用，它将很快进入暂停状态。 （请参阅[处理应用暂停](suspend-an-app.md)）。

## 检测和处理预启动


应用在激活期间接收 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 标志。 使用此标志确定是否要执行仅应在用户显式启动应用时执行的操作，如以下 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 摘录中所示。

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a 
            // what&#39;s new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**提示** 如果要选择退出预启动，请检查 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 标志。 如果它已设置，请在执行任何创建框架或激活窗口的操作前从 OnLaunched() 返回。

 

## 使用 VisibilityChanged 事件


用户看不到通过预启动激活的应用。 它们在用户切换到它们时可见。 你可能想要延迟某些操作，直到可以看到应用主窗口为止。 例如，如果你的应用显示来自某个源的新增项列表，你可以在 [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 事件期间更新该列表，而非依赖在预启动该应用时生成的列表，该列表可能在用户激活该应用时就已过时。 以下代码处理 **MainPage** 的 **VisibilityChanged** 事件：

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than 
        // when it is prelaunched, such as building a what&#39;s new feed 
    }
}
```

## 指南


-   应用不应在预启动期间执行运行时间较长的操作，因为该应用会在无法快速暂停的情况下终止。
-   预启动应用时，应用不应从 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 启动音频播放，因为该应用不可见并且音频播放的原因并不明显。
-   假设应用对用户可见，或假设应用由用户显式启动，则应用不应在启动期间执行任何操作。 因为应用现在可以在后台启动并且无需用户显式操作，所以开发人员应该考虑隐私、用户体验和性能含义。
    -   一个隐私注意事项的示例是，社交应用应该何时将用户状态更改为在线。 它应该等到用户切换到该应用时才更改状态，而非在预启动应用时更改状态。
    -   一个用户体验注意事项的示例是，如果你拥有某个在启动时显示初级序列的应用（例如游戏），你可能会想要将该初级序列延迟到用户切换到该应用时。
    -   一个性能含义的示例是，你可能会等到用户切换到应用时才检索当前天气信息（而非在应用预启动时加载该信息），并且需要在应用可见时重新加载该信息，以确保该信息是最新的。
-   如果你的应用在启动时清除动态磁贴，请将此操作延迟到执行可见性更改事件时。
-   应用的遥测应可以区分正常磁贴激活和预启动激活，以便可以标识发生问题的方案。
-   如果你拥有 Microsoft Visual Studio 2015 Update 1 和 Windows 10 版本 1511，可以在 Visual Studio 2015 中模拟应用预启动，方法是依次选择**“调试”**>**“其他调试目标”**>**“调试 Windows 通用应用预启动”**。

## 相关主题

* [应用生命周期](app-lifecycle.md)

 

 





<!--HONumber=Mar16_HO1-->


