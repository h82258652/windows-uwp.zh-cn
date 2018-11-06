---
author: drewbatgit
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: 本文介绍如何将使用 Microsoft PlayReady 内容保护的多媒体内容自适应流式处理添加到通用 Windows 平台 (UWP) 应用。
title: 使用 PlayReady 的自适应流式处理
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7132de481f22f53269cd9bd69a38819c5b71cb55
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6048325"
---
# <a name="adaptive-streaming-with-playready"></a>使用 PlayReady 的自适应流式处理


本文介绍如何将使用 Microsoft PlayReady 内容保护的多媒体内容自适应流式处理添加到通用 Windows 平台 (UWP) 应用。 

此功能当前支持 HTTP 动态流 (DASH) 内容的播放。

HLS（Apple 的 HTTP 实时流）不受 PlayReady 支持。

平滑流式处理当前也不受本地支持；但是，PlayReady 可扩展，并且通过使用其他代码或库，PlayReady 保护的平滑流式处理可以受到支持，从而利用软件甚至是硬件 DRM（数字版权管理）。

本文仅介绍特定于 PlayReady 的自适应流式处理方面的内容。 有关一般实现自适应流式处理的信息，请参阅[自适应流式处理](adaptive-streaming.md)。

本文使用的代码来自 GitHub 上 Microsoft 的 **Windows-universal-samples** 存储库中的[自适应流式处理示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming)。 方案 4 介绍自适应流式处理与 PlayReady 的结合使用。 你可以下载 ZIP 文件形式的存储库，方法是导航到存储库的根级别并选择“下载 ZIP”**** 按钮。

你将需要以下 **using** 语句：

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

**LicenseRequest** 命名空间来自于 **CommonLicenseRequest.cs**，这是 Microsoft 提供给被许可方的 PlayReady 文件。

你将需要声明几个全局变量：

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

你还需要声明以下常量：

```csharp
private const uint MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = 0x8004B895;
```

## <a name="setting-up-the-mediaprotectionmanager"></a>设置 MediaProtectionManager

若要向你的 UWP 应用添加 PlayReady 内容保护，需要设置 [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040) 对象。 在初始化 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 对象时执行此操作。

以下代码可设置 [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040)：

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

只能将此代码复制到你的应用，因为它对设置内容保护是强制性的。

在加载二进制数据失败时，引发 [ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041) 事件。 我们需要添加事件处理程序来处理这一暗示加载未完成的情况：

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

同样，我们需要为 [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045) 事件添加事件处理程序，该事件将在请求服务时引发。 此代码会检查请求的类型，然后相应地做出响应：

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## <a name="individualization-service-requests"></a>个性化服务请求

下面的代码将被动发出 PlayReady 个性化服务请求。 我们将以参数形式将请求传递给函数。 我们将调用包含在 try/catch 块中，如果未出现任何异常，则说明请求已成功完成：

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null && comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

或者，我们可能需要主动发出个性化服务请求，在此情况下，我们将通过调用以下函数来代替在 `ProtectionManager_ServiceRequested` 中调用 `ReactiveIndivRequest` 的代码：

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## <a name="license-acquisition-service-requests"></a>许可证获取服务请求

如果请求改为 [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285)，我们将调用以下函数以进行请求并获取 PlayReady 许可证。 我们告知我们传入的 **MediaProtectionServiceCompletion** 对象请求是否已成功，并完成以下请求：

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## <a name="initializing-the-adaptivemediasource"></a>初始化 AdaptiveMediaSource

最后，你将需要一个函数来初始化从给定 [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 和 [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) 创建的 [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912)。 **Uri** 应为指向媒体文件（HLS 或 DASH）的链接；**MediaElement** 应在 XAML 中进行定义。

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

你可以在处理自适应流式处理开始的任何事件中调用此函数；例如在按钮单击事件中。

## <a name="see-also"></a>另请参阅
- [PlayReady DRM](playready-client-sdk.md)




