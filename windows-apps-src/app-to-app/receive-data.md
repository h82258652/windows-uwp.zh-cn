---
description: 本文介绍如何接收使用“共享”合约从另一个应用共享的通用 Windows 平台 (UWP) 应用中的内容。 此“共享”合约允许在用户调用“共享”时，将你的应用表示为一个选项。
title: 接收数据
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d64e4a2d4251aca6bbce39b5f24e3e5f35295b8
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7286128"
---
# <a name="receive-data"></a>接收数据



本文介绍如何接收使用“共享”合约从另一个应用共享的通用 Windows 平台 (UWP) 应用中的内容。 此“共享”合约允许在用户调用“共享”时，将你的应用表示为一个选项。

## <a name="declare-your-app-as-a-share-target"></a>将应用声明为共享目标

在用户调用“共享”时，系统将显示可能的目标应用列表。 为了显示在列表中，你的应用需要声明它支持“共享”合约。 这让系统知道你的应用可用于接收内容。

1.  打开清单文件。 该文件的名称应类似 **package.appxmanifest**。
2.  打开“声明”**** 选项卡。
3.  从“可用声明”**** 列表中，选择“共享目标”****，然后选择“添加”****。

## <a name="choose-file-types-and-formats"></a>选择文件类型和格式

接下来，确定你支持的文件类型和数据格式。 共享 API 支持多种标准格式，如文本、HTML 和位图。 你还可以指定自定义文件类型和数据格式。 指定时，请记住源应用必须明确这些类型和格式；否则它们无法使用这些格式来共享数据。

仅注册你的应用可以处理的格式。 当用户调用“共享”时，仅显示支持正在共享的数据的目标应用。

若要设置文件类型：

1.  打开清单文件。 该文件的名称应类似于 **package.appxmanifest**。
2.  在“声明”**** 页的“支持的文件类型”**** 部分，选择“新增”****。
3.  键入要支持的文件扩展名，例如“.docx”。 你需要包括句点。 如果希望支持所有文件类型，请选中**SupportsAnyFileType** 复选框。

若要设置数据格式：

1.  打开清单文件。
2.  打开“声明”**** 页的“数据格式”**** 部分，然后选择“新增”****。
3.  键入支持的数据格式的名称，例如“Text”。

## <a name="handle-share-activation"></a>处理共享激活

当用户选择你的应用时（通常从共享 UI 中可用目标应用的列表中进行选择），将引发 [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Application.OnShareTargetActivated(Windows.ApplicationModel.Activation.ShareTargetActivatedEventArgs)) 事件。 你的应用需要处理此事件才能处理用户要共享的数据。

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

用户要共享的数据包含在一个 [**ShareOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation) 对象中。 你可以使用该对象来检查所包含的数据格式。

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>报告共享状态

在某些情况下，你的应用可能需要花费一定时间来处理要共享的数据。 示例包括用户共享文件或图像的集合。 这些项目比简单文本字符串更大，因此处理时间较长。

```cs
shareOperation.ReportStarted(); 
```

在调用 [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted) 之后，将不再有任何与你的应用所进行的用户交互。 因此，你不应该调用它，除非你的应用处于可以由用户关闭的位置。

使用扩展共享，用户有可能会在你的应用获得来自 DataPackage 对象的所有数据之前关闭源应用。 因此，我们建议你让系统知道你的应用何时已获得它所需的数据。 这样，系统可以根据需要暂停或终止源应用。

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

如果发生错误，调用 [**ReportError**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportError(System.String)) 向系统发送一条错误消息。 用户在检查共享的状态时将看到该消息。 此时，你的应用将关闭并且共享将结束。 用户将需要再次启动才能将内容共享到你的应用。 根据你的方案，你可能确定某个特殊错误并不严重，不足以结束共享操作。 在这种情况下，你可以选择不调用 **ReportError** 并且继续此共享。

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

最后，当你的应用成功处理好共享内容之后，你应当调用 [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted) 来通知系统。

```cs
shareOperation.ReportCompleted();
```

在使用这些方法时，通常按照所述的顺序来进行调用，不要多次调用它们。 但是，某些时候目标应用可能会在调用 [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted) 之前调用 [**ReportDataRetrieved**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportDataRetrieved)。 例如，应用可能在激活处理程序中执行任务时检索数据，但在用户选择“共享”**** 按钮之前不调用 **ReportStarted**。

## <a name="return-a-quicklink-if-sharing-was-successful"></a>如果共享成功，则返回 QuickLink

当用户选择你的应用来接收内容时，我们建议你创建一个 [**QuickLink**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink)。 **QuickLink** 类似于快捷方式，可让用户更轻松地使用你的应用共享信息。 例如，你可以创建一个 **QuickLink**，用来打开预配置了好友电子邮件地址的新邮件。

**QuickLink** 必须有标题、图标以及 ID。用户点击“共享”超级按钮时显示标题（如“向妈妈发送电子邮件”）和图标。 你的应用使用 ID 来访问任何自定义信息，如电子邮件地址或登录凭据。 当你的应用创建 **QuickLink** 时，该应用会通过调用 [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted)，将 **QuickLink** 返回到系统。

**QuickLink** 实际上并不存储数据。 而是包含一个标识符，如果选中该标识符，则会将该标识符发送至你的应用。 你的应用负责存储 **QuickLink** 的 ID 及相应的用户数据。 当用户点击 **QuickLink** 时，你可以通过 [**QuickLinkId**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.QuickLinkId) 属性获取其 ID。

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>另请参阅 

* [App-to-app communication](index.md)
* [共享数据](share-data.md)
* [OnShareTargetActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onsharetargetactivated.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [ReportError](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror.aspx)
* [ReportCompleted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted.aspx)
* [ReportDataRetrieved](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [QuickLink](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.aspx)
* [QuickLInkId](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.id.aspx)
