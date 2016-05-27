---
author: mcleblanc
title: 处理应用暂停
description: 了解当系统挂起你的应用时如何保存重要的应用程序数据。
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
---

# 处理应用暂停


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**暂停中**](https://msdn.microsoft.com/library/windows/apps/br242341)

了解当系统挂起你的应用时如何保存重要的应用程序数据。 以下示例向事件处理程序注册 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件并将字符串保存到文件中。

## 注册 suspending 事件处理程序


注册以处理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件，该事件指示在系统暂停你的应用之前，应用应该保存其应用程序数据。

> [!div class="tabbedCodeSnippets"]
```cs
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```
```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent() 
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```
```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending += 
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## 在挂起之前保存应用程序数据


当你的应用处理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件时，它将有机会将其重要的应用程序数据保存到处理程序函数中。 应用应该使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 存储 API 来同步保存简单的应用程序数据。

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
    async void App_Suspending(
        Object sender, 
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated
    }
}
```
```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object, 
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated
    End Sub

End Class
```
```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated
}
```

## 备注


每当用户切换到其他应用、桌面或“开始”屏幕时，系统都会暂停你的应用。 每当用户切回到你的应用时，系统就会恢复你的应用。 当系统恢复你的应用时，你的变量和数据结构的内容与系统将你的应用暂停之前的内容相同。 系统会将你的应用完全恢复到你离开时的状态，使用户感觉你的应用好像一直在后台运行一样。

当你的应用被暂停后，系统会尝试将你的应用及其数据保留在内存中。 但是，如果系统没有资源将你的应用保存在内存里，则将终止你的应用。 当用户切换回已终止的暂停应用时，该应用会发送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，且应该在其 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中还原其应用程序数据。

当终止应用时系统不会通知应用，因此当暂停应用时，你的应用必须保存其应用程序数据并释放独占资源和文件句柄，并且当在终止后又激活应用时还原这些内容。

> **注意** 如果在应用暂停期间，你需要执行一些异步工作，则需要将暂停完成时间延迟到工作完成之后。 你可以使用 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 对象（可通过事件参数获取）上的 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 方法将暂停的完成延迟到你针对所返回的 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 对象调用 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法位置。

> **注意** 为了改进 Windows 8.1 中的系统响应，在应用暂停时，我们为应用提供低优先级的资源访问。 为了支持新的优先级，延长了暂停操作超时，以便应用具有与普通优先级相当的 5 秒（在 Windows 上）或者 1 到 10 秒超时（在 Windows Phone 上）。 你无法扩展或改变此超时窗口。

> **有关使用 Visual Studio 进行调试的注释：**Visual Studio 阻止 Windows 暂停连接到调试程序的应用。 这是为了允许用户在应用正在运行时查看 Visual Studio 调试 UI。 调试应用时，可以使用 Visual Studio 将一个暂停事件发送给该应用。 请确保**“调试位置”**工具栏正在显示，然后单击**“暂停”**图标。

## 相关主题


* [处理应用激活](activate-an-app.md)
* [处理应用恢复](resume-an-app.md)
* [启动、暂停和恢复的用户体验指南](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [应用生命周期](app-lifecycle.md)

 

 





<!--HONumber=May16_HO2-->


