---
description: 本文介绍如何支持在通用 Windows 平台 (UWP) 应用中通过使用剪贴板进行复制和粘贴。
title: 复制和粘贴
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ba498a9bc6541f45f01e74b1ea90f4f9a1bfde90
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7701233"
---
# <a name="copy-and-paste"></a>复制和粘贴

本文介绍如何支持在通用 Windows 平台 (UWP) 应用中通过使用剪贴板进行复制和粘贴。 复制和粘贴是在应用之间或在应用内交换数据的传统方法，并且在一定程度上，几乎每个应用都可以支持剪贴板操作。

## <a name="check-for-built-in-clipboard-support"></a>检查内置剪贴板支持

许多情况下，你无需编写用于支持剪贴板操作的代码。 可用于创建应用的许多默认 XAML 控件已经支持剪贴板操作。 

## <a name="get-set-up"></a>准备工作

首先，将 [**Windows.ApplicationModel.DataTransfer**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer) 命名空间包含在你的应用中。 然后，添加一个 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 对象实例。 该对象同时包含用户希望复制的数据和你希望包含的所有属性（如描述）。

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>复制和剪切

复制和剪切（也称为*移动*）的工作原理几乎完全相同。 使用 [**RequestedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage.RequestedOperation) 属性选择所需操作。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```
## <a name="drag-and-drop"></a>拖放

接下来，你可以将用户已选择的数据添加到 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 对象。 如果 **DataPackage** 类支持该数据，则可以在 **DataPackage** 对象中使用其中一种相应的方法。 添加文本的方法如下：

```cs
dataPackage.SetText("Hello World!");
```

最后一步是通过调用静态 [**SetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.SetContent(Windows.ApplicationModel.DataTransfer.DataPackage)) 方法将 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 添加到剪贴板。

```cs
Clipboard.SetContent(dataPackage);
```
## <a name="paste"></a>粘贴

若要获取剪贴板的内容，请调用静态 [**GetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.GetContent) 方法。 此方法将返回一个包含该内容的 [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView)。 该对象几乎与 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 对象完全相同，只不过其内容为只读。 通过该对象，你可以使用 [**AvailableFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.AvailableFormats) 或 [**Contains**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.Contains(System.String)) 方法来确定哪些格式可用。 然后，你可以调用相应的 [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) 方法来获取数据。

```cs
async void OutputClipboardText()
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

## <a name="track-changes-to-the-clipboard"></a>跟踪对剪贴板的更改

除了复制和粘贴命令以外，你可能还想要跟踪剪贴板更改。 可通过处理剪贴板的 [**ContentChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.ContentChanged) 事件来执行此操作。

```cs
Clipboard.ContentChanged += async (s, e) => 
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

## <a name="see-also"></a>另请参阅

* [App-to-app communication](index.md)
* [DataTransfer](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.aspx)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackageView](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
* [RequestedOperation](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx) 
* [ControlsList](https://msdn.microsoft.com/library/windows/apps/xaml/mt185406.aspx)
* [SetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx)
* [GetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx)
* [AvailableFormats](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx)
* [Contains](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx)
* [ContentChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx)

