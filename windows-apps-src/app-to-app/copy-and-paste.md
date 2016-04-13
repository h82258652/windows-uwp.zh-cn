---
description: 本文介绍如何支持在通用 Windows 平台 (UWP) 应用中通过使用剪贴板进行复制和粘贴。
title: 复制和粘贴
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: awkoren
---
#复制和粘贴

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文介绍如何支持在通用 Windows 平台 (UWP) 应用中通过使用剪贴板进行复制和粘贴。 复制和粘贴是在应用之间或在应用内交换数据的传统方法，并且在一定程度上，几乎每个应用都可以支持剪贴板操作。

## 检查内置剪贴板支持


许多情况下，你无需编写用于支持剪贴板操作的代码。 可用于创建应用的许多默认 XAML 控件已经支持剪贴板操作。 有关哪些控件可用的详细信息，请参阅[控件列表][ControlsList]。

## 准备工作

首先，将 [**Windows.ApplicationModel.DataTransfer**][DataTransfer] 命名空间包含在你的应用中。 然后，添加一个 [**DataPackage**][DataPackage] 对象实例。 该对象同时包含用户希望复制的数据和你希望包含的所有属性（如描述）。

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

## 复制和剪切

复制和剪切（也称为移动）的工作原理几乎完全相同。 使用 [**DataPackage.RequestedOperation**][RequestedOperation] 属性选择所需操作。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

接下来，你可以将用户已选择的数据添加到 [**DataPackage**][DataPackage] 对象。 如果 **DataPackage** 类支持该数据，则可以在 **DataPackage** 对象中使用其中一种相应的方法。 添加文本的方法如下：

```cs
dataPackage.SetText("Hello World!");
```

最后一步是通过调用静态 [**Clipboard.SetContent**][SetContent] 方法将 [**DataPackage**][DataPackage] 添加到剪贴板。

```cs
Clipboard.SetContent(dataPackage);
```
## 粘贴

要获取剪贴板的内容，需调用静态 [**Clipboard.GetContent**[GetContent] 方法。 此方法将返回一个包含该内容的 [**DataPackageView**][DataPackageView]。 该对象几乎与 [**DataPackage**][DataPackage] 对象完全相同，只不过其内容为只读。 通过该对象，你可以使用 [**AvailableFormats**][AvailableFormats] 或 [**Contains**][Contains] 方法来确定哪些格式可用。 然后，你可以调用相应的 **DataPackageView** 方法来获取数据。

```cs
DataPackageView dataPackageView = Clipboard.GetContent();
if (dataPackageView.Contains(StandardDataFormats.Text))
{
    string text = await dataPackageView.GetTextAsync();
    // To output the text from this example, you need a TextBlock control
    TextOutput.Text = "Clipboard now contains: " + text;
}
```

## 跟踪对剪贴板的更改

除了复制和粘贴命令以外，你可能还想要跟踪剪贴板更改。 可通过处理剪贴板的 [**Clipboard.ContentChanged**][ContentChanged] 事件来执行此操作。

```cs
Clipboard.ContentChanged += (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

<!-- LINKS --> 
[DataTransfer]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.aspx 
[DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
[DataPackageView]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx
[DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
[DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
[DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
[FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
[ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
[RequestedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx 
[ControlsList]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/mt185406.aspx 
[SetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx 
[GetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx
[AvailableFormats]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx 
[Contains]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx
[ContentChanged]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx 



<!--HONumber=Mar16_HO5-->


