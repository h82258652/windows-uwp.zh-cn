---
title: 在多线程环境中使用 Windows 运行时对象 | Microsoft 文档
description: 本文讨论了 .NET Framework 从 C# 和 Visual Basic 代码到 Windows 运行时或 Windows 运行时组件提供的对象处理调用的方式。
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: windows 10, uwp, 计时器, 线程
ms.localizationpriority: medium
ms.openlocfilehash: 82e1431a6689ef9ece91cef7e2b018e24f834039
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8798724"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>在多线程环境中使用 Windows 运行时对象
本文讨论了 .NET Framework 从 C# 和 Visual Basic 代码到 Windows 运行时或 Windows 运行时组件提供的对象处理调用的方式。

在 .NET Framework 中，默认情况下你可以从多个线程访问任意对象，而无需进行特殊处理。 你需要的仅仅是对该对象的引用。 在 Windows 运行时中，此类对象称为*敏捷*。 大多数 Windows 运行时类都为敏捷类，但有几类除外，而且即使是敏捷类也可能需要进行特殊处理。

在可能的情况下，常见语言运行时 (CLR) 会将来自其他来源（如 Windows 运行时）的对象视为 .NET Framework 对象：

- 如果对象实现 [IAgileObject](http://msdn.microsoft.com/library/Hh802476.aspx) 接口，或者具有 [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) 属性（带 [MarshalingType.Agile](http://go.microsoft.com/fwlink/p/?LinkId=256023)），则 CLR 会将其视为敏捷对象。

- 如果 CLR 可以从用于对目标对象进行线程处理的线程封送调用，则将以透明的方式执行该操作。

- 如果对象具有 [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) 属性（带 [MarshalingType.None](http://go.microsoft.com/fwlink/p/?LinkId=256023)），则该类将不予提供封送信息。 CLR 无法封送调用，因此引发了附带表明该对象仅可用于其创建时所在的线程处理上下文的消息的 [InvalidCastException](/dotnet/api/system.invalidcastexception) 异常。

以下部分介绍了此行为对来自各个来源的对象的影响。

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>来自以 C# 或 Visual Basic 编写的 Windows 运行时组件的对象
该组件中可激活的所有类型在默认情况下均为敏捷型。

> [!NOTE]
>  敏捷并不意味着线程安全。 在 Windows 运行时和 .NET Framework 中，大多数类都不是线程安全类，这是因为线程安全拥有性能成本，而且永远无法通过多个线程访问大多数对象。 仅在必要时同步对单个对象的访问（或使用线程安全类）将更有效。

创作 Windows 运行时组件时，你可以覆盖默认值。 请参阅 [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) 接口和 [IAgileObject](http://msdn.microsoft.com/library/Hh802476.aspx) 接口。

## <a name="objects-from-the-windows-runtime"></a>来自 Windows 运行时的对象
Windows 运行时中的大多数类都是敏捷类，CLR 会将它们视为敏捷类。 针对这些类的文档在类属性中间列出了“MarshalingBehaviorAttribute(Agile)”。 但是，其中一些敏捷类（如 XAML 控件）如果未在 UI 线程上被调用，便会引发异常。 例如，下列代码尝试使用背景线程设置被单击的按钮的属性。 按钮的[内容](http://go.microsoft.com/fwlink/p/?LinkId=256025)属性将引发异常。

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

你可以利用按钮的[调度程序](http://go.microsoft.com/fwlink/p/?LinkId=256026)属性，或存在于 UI 线程上下文中的任意对象（如按钮所在页面）的 `Dispatcher` 属性安全访问按钮。 下列代码使用 [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) 对象的 [RunAsync](http://go.microsoft.com/fwlink/p/?LinkId=256030) 方法调度 UI 线程上的调用。

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  `Dispatcher` 属性如果从另一个线程被调用，则不会引发异常。

在 UI 线程上创建的 Windows 运行时对象的生命周期受到线程生命周期的限制。 关闭窗口后，请不要访问 UI 线程上的对象。

如果你通过继承 XAML 控件或编写一组 XAML 控件来创建你自己的控件，你的控件将为敏捷控件，因为它属于 .NET Framework 对象。 但是，如果它调用基类或构成类的成员，或者如果你调用了继承的成员，则当你从任意线程（UI 线程除外）调用这些成员时将引发异常。

### <a name="classes-that-cant-be-marshaled"></a>不能封送的类
无法提供封送信息的 Windows 运行时类具有 [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) 属性（带 [MarshalingType.None](http://go.microsoft.com/fwlink/p/?LinkId=256023)）。 这对此类的文档在其属性中间列出了“MarshalingBehaviorAttribute(None)”。

以下代码在 UI 线程上创建了 [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) 对象，并尝试设置来自线程池线程的对象的属性。 CLR 无法封送调用，并且会引发附带表明该对象仅可用于其创建时所在的线程处理上下文的消息的 [System.InvalidCastException](/dotnet/api/system.invalidcastexception) 异常。

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

针对 [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) 的文档也在类属性中间列出了“ThreadingAttribute(STA)”，因为必须在单线程上下文（如 UI 线程）中进行创建。

如果想要从另一个线程访问 [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) 对象，你可以缓存 UI 线程的 [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) 对象，并于稍后用它在该线程上调度调用。 或者，你可以如下列代码所示，从 XAML 对象（如页面）获取调度程序。

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>来自以 C++ 编写的 Windows 运行时组件的对象
默认情况下，该组件中的可激活类为敏捷类。 但是，C++ 允许对线程处理模型和封送行为执行大量操作。 按照本文前面部分所述，CLR 识别敏捷类、尝试在类非敏捷类时封送调用，并在类不含封送信息时引发 [System.InvalidCastException](/dotnet/api/system.invalidcastexception) 异常。

针对在 UI 线程上运行的对象，以及从除 UI 线程以外的其他线程调用时引发异常的对象，你可以使用 UI 线程的 [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) 对象调度调用。

## <a name="see-also"></a>另请参阅
[C# 指南](/dotnet/articles/csharp/)

[Visual Basic 指南](/dotnet/articles/visual-basic/)
