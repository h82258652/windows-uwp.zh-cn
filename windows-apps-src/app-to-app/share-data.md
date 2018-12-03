---
description: 本文将说明如何在通用 Windows 平台 (UWP) 应用中支持“共享”合约。
title: 共享数据
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5ed9be96ee44635249f01e7b919f3789d84305e1
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8472544"
---
# <a name="share-data"></a>共享数据


本文将说明如何在通用 Windows 平台 (UWP) 应用中支持“共享”合约。 “共享”合约是一种在应用之间快速共享文本、链接、照片和视频等数据的简便方法。 例如，用户可能希望使用社交网络应用与其好友共享网页，或者将链接保存在笔记应用中以供日后参考。

## <a name="set-up-an-event-handler"></a>设置事件处理程序

添加 [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) 事件处理程序以在用户每次调用共享时调用。 这种情况既会在用户点击应用中的控件（例如按钮或应用栏命令）时发生，也会在特定情况下（例如，如果用户完成某一关并获得了较高分数）自动发生。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

发生 [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) 事件时，应用会收到 [**DataRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest) 对象。 该对象包含 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)，可用来提供用户要共享的内容。 你必须提供标题和要共享的数据。 描述是可选的，但建议提供。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>选择数据

你可以共享各种类型的数据，包括：

-   纯文本
-   统一资源标识符 (URI)
-   HTML
-   格式化的文本
-   位图
-   文件
-   自定义开发人员定义的数据

[**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 对象可以包含其中一种或多种格式（可任意组合）。 下面的示例演示如何共享文本。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>设置属性

当你打包数据进行共享时，可以给出各种可提供与共享内容相关的其他信息的属性。 这些属性帮助目标应用改善用户体验。 例如，当用户通过多个应用共享内容时，可提供说明帮助。 共享图像或指向网页的链接时，添加一个缩略图可为用户提供直观的参考。 有关详细信息，请参阅 [**DataPackagePropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)。

除了 Title，所有属性都是可选的。 Title 属性是强制性的，必须进行设置。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>启动共享 UI

用于共享的 UI 由系统提供。 若要启动它，请调用 [**ShowShareUI**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.ShowShareUI) 方法。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>处理错误

在大多数情况下，共享内容是一个简单直接的过程。 然而，意外总是有可能发生。 例如，应用可能需要用户选择用于共享的内容，但用户未选择任何内容。 若要处理这些情况，请使用 [**FailWithDisplayText**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest.FailWithDisplayText(System.String)) 方法，它将在出现问题时向用户显示一条消息。

## <a name="delay-share-with-delegates"></a>通过委托延迟共享

有时，它可能不宜立即准备用户要共享的数据。 例如，如果你的应用支持以几种可能的不同格式发送一个较大的图像文件，在用户进行选择之前，创建所有这些图像的效率非常低。

若要解决此问题，[**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 可以包含委托 - 一种在接收应用请求数据时调用的函数。 我们建议你在用户想要共享的数据很消耗资源时使用委托。

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>另请参阅 

* [App-to-app communication](index.md)
* [接收数据](receive-data.md)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackagePropertySet](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx)
* [DataRequested](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
 

