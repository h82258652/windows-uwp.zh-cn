---
title: 处理应用恢复
description: 了解如何在系统恢复你的应用时刷新显示的内容。
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f424a274d3e96b58f32875620f3165ccfac82ba6
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343513"
---
# <a name="handle-app-resume"></a>处理应用恢复

**重要的 API**

- [**恢复**](https://msdn.microsoft.com/library/windows/apps/br242339)

了解当系统恢复你的应用时刷新 UI 的情况。 本主题中的示例可向事件处理程序注册 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件。

## <a name="register-the-resuming-event-handler"></a>注册 resuming 事件处理程序

注册以处理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件，该事件指示用户从你的应用切换到桌面或其他应用，而后又切换回你的应用。

```csharp
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

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Resuming({ this, &MainPage::App_Resuming });
}
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming +=
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>刷新显示的内容并重新获取资源

在用户切换到其他应用或桌面后，系统会暂停你的应用数秒钟。 每当用户切换回你的应用时，系统会恢复你的应用。 当系统恢复你的应用时，你的变量和数据结构的内容与系统将你的应用暂停之前的内容相同。 系统将还原离开时所使用的应用。 对用户来说，就好像应用一直在后台运行一样。

当你的应用处理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件时，你的应用可能会暂停几个小时或几天。 它应该在应用处于暂停状态时刷新可能已过时的任何内容，例如新闻源或用户位置。

此时，还可以还原你在应用处于暂停状态时发布的任何独占资源，例如文件句柄、相机、I/O 设备、外部设备和网络资源。

```csharp
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.

    End Sub
>
End Class
```

```cppwinrt
void MainPage::App_Resuming(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::Foundation::IInspectable const& /* e */)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

> [!NOTE]
> 因为[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)事件未从 UI 线程中引发，调度程序必须使用你的处理程序中调度对你的 UI 的任何调用。

## <a name="remarks"></a>备注

当你的应用连接到 Visual Studio 调试器时，它将不会暂停。 但是，你可以从调试器中暂停它，然后向其发送一个 **Resume** 事件，以便调试你的代码。 确保“调试位置”**** 工具栏可见并单击“暂停”**** 图标旁边的下拉列表。 然后选择“恢复”****。

对于 Windows Phone 应用商店应用，[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件始终后跟 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)，即使你的应用当前已暂停且用户从主要磁贴或应用列表中重新启动它也是如此。 如果当前窗口上已有内容集，应用可跳过初始化。 你可以检查 [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 属性以确定该应用是从主要磁贴启动还是从辅助磁贴启动，并可根据该信息，确定是应显示新的应用体验还是应恢复应用体验。

## <a name="related-topics"></a>相关主题

* [应用生命周期](app-lifecycle.md)
* [处理应用激活](activate-an-app.md)
* [处理应用暂停](suspend-an-app.md)
