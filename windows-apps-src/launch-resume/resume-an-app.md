---
author: mcleblanc
title: 处理应用恢复
description: 了解当系统恢复你的应用时如何刷新显示的内容。
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
---

# 处理应用恢复


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**恢复**](https://msdn.microsoft.com/library/windows/apps/br242339)

了解当系统恢复你的应用时如何刷新显示的内容。 本主题中的示例可向事件处理程序注册 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件。

**路线图：**本主题与其他主题有何关联？ 请参阅：

-   [使用 C# 或 Visual Basic 的 Windows 运行时应用的路线图](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [使用 C++ 的 Windows 运行时应用的路线图](https://msdn.microsoft.com/library/windows/apps/hh700360)

## 注册 resuming 事件处理程序

注册以处理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件，该事件指示用户从你的应用切换到桌面或其他应用，而后又切换回你的应用。

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```
```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent() 
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```
```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming += 
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## 暂停之后刷新显示的内容

当你的应用处理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件时，它将有机会刷新其显示的内容。

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data
    }
}
```
```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data

    End Sub

End Class
```
```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data
}
```

> **注意** 因为未从 UI 线程引发 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件，所以必须使用调度程序访问该 UI 线程并向 UI 插入更新（如果这是你想要在处理程序中执行的操作）。

## 备注


每当用户切换到桌面或其他应用时，系统都会挂起你的应用。 每当用户切回到你的应用时，系统就会恢复你的应用。 当系统恢复你的应用时，你的变量和数据结构的内容与系统将你的应用暂停之前的内容相同。 系统会将你的应用完全恢复到你离开时的状态，使用户感觉你的应用好像一直在后台运行一样。 但是，应用可能已暂停很长一段时间，因此，它应当刷新在应用暂停之后可能已发生更改的任何显示内容（如新闻源或用户位置）。

如果你的应用没有任何要刷新的显示内容，则它无需处理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件。

**使用 Visual Studio 进行调试时要注意：**当你的应用连接到 Visual Studio 调试程序时，你可以向你的应用发送一个 **Resume** 事件。 确保“调试位置”****工具栏显示出来，然后单击“暂停”****图标旁边的下拉列表。 然后选择“恢复”****。

> **注意** 对于 Windows Phone 应用商店应用，[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件之后始终跟着 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 事件，即使你的应用当前已暂停且用户从主要磁贴或应用列表中重新启动它也是如此。 如果当前窗口上已有内容集，则应用可跳过初始化。 你可以检查 [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 属性以确定该应用是从主要磁贴启动还是从辅助磁贴启动，并可根据该信息，确定是应显示新的应用体验还是应恢复应用体验。

## 相关主题

* [处理应用激活](activate-an-app.md)
* [处理应用暂停](suspend-an-app.md)
* [应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [应用生命周期](app-lifecycle.md)




<!--HONumber=May16_HO2-->


