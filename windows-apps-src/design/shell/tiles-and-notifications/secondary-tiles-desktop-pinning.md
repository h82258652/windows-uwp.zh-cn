---
author: vladimp
Description: Windows desktop applications can pin secondary tiles thanks to the Desktop Bridge!
title: 从桌面应用程序固定辅助磁贴
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: mijacobs
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, 桌面桥, 辅助磁贴, 固定, 快速入门, 代码示例, 示例, secondarytile, 桌面应用程序, win32, winforms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 44e37b47e22d10f509afd5d7503fa8f7a43ab365
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5928260"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>从桌面应用程序固定辅助磁贴


借助于[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)，Windows 桌面应用程序（如 Win32、Windows 窗体和 WPF）可以固定辅助磁贴！

![辅助磁贴的屏幕截图](images/secondarytiles.png)

> [!IMPORTANT]
> **需要 Fall Creators Update**：必须面向 SDK 16299 并运行版本 16299 或更高版本，才能从桌面桥应用固定辅助磁贴。

从 WPF 或 WinForms 应用程序中添加辅助磁贴非常类似于纯净版 UWP 应用的情况。 唯一的区别是，你必须指定主窗口句柄 (HWND)。 这是因为在固定磁贴时，Windows 会显示一个模式对话框，并请求用户确认他们是否想要固定磁贴。 如果桌面应用程序没有为 SecondaryTile 对象配置所有者窗口，则 Windows 不知道在哪里绘制对话框，因此操作将失败。


## <a name="package-your-app-with-desktop-bridge"></a>用桌面桥打包你的应用

如果你未使用桌面桥打包你的应用，则在使用任何 UWP API 之前，[你必须先执行此操作](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)。


## <a name="enable-access-to-iinitializewithwindow-interface"></a>启用对 IInitializeWithWindow 接口的访问

如果应用程序使用托管语言（如 C# 或 Visual Basic）编写，则在应用代码中使用 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 和 Guid 属性声明 IInitializeWithWindow 接口，如以下 C# 示例所示。 此示例假设代码文件具有 System.Runtime.InteropServices 命名空间的 using 语句。

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

或者，如果使用的是 C++，请在代码中添加对 **shobjidl.h** 头文件的引用。 此头文件包含 *IInitializeWithWindow* 接口的声明。


## <a name="initialize-the-secondary-tile"></a>初始化辅助磁贴

初始化新辅助磁贴对象与初始化普通 UWP 应用完全一样。 若要了解有关创建和固定辅助磁贴的详细信息，请参阅[固定辅助磁贴](secondary-tiles-pinning.md)。

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>分配窗口句柄

这是桌面应用程序的关键步骤。 将对象强制转换为 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 对象。 然后，调用 [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 方法，并传递你希望成为模式对话框所有者的窗口的句柄。 以下 C# 示例显示如何将应用主窗口的句柄传递到该方法。

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>固定磁贴

最后，按普通 UWP 应用中的操作那样请求固定磁贴。

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>发送磁贴通知

> [!IMPORTANT]
> **需要 2018 年 4 月版本 17134.81 或更高版本**：必须运行版本 17134.81 或更高版本，才能从桌面桥应用向辅助磁贴发送磁贴或锁屏提醒通知。 在 17134.81 服务更新之前，从桌面桥应用向辅助磁贴发送磁贴或锁屏提醒通知时会出现 0x80070490 *未找到元素* 异常。

发送磁贴或锁屏提醒通知的方法与 UWP 应用相同。 要开始使用，请参阅[发送本地磁贴通知](sending-a-local-tile-notification.md)。


## <a name="resources"></a>资源

* [完整代码示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [辅助磁贴概述](secondary-tiles.md)
* [固定辅助磁贴 (UWP)](secondary-tiles-pinning.md)
* [桌面桥](https://developer.microsoft.com/windows/bridges/desktop)
* [桌面桥代码示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)