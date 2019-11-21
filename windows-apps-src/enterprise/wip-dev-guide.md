---
Description: 本指南帮助你启发你的应用，以处理由 Windows 信息保护 (WIP) 策略管理的企业数据以及个人数据。
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows 信息保护 (WIP) 开发人员指南
ms.date: 06/21/2017
ms.topic: article
keywords: windows 10, uwp, wip, Windows 信息保护, 企业数据, 企业数据保护, edp, 启发式应用
ms.assetid: 913ac957-ea49-43b0-91b3-e0f6ca01ef2c
ms.localizationpriority: medium
ms.openlocfilehash: 6d026c00b1aec4fd8e80b10c0b86c8bd8145f925
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258597"
---
# <a name="windows-information-protection-wip-developer-guide"></a>Windows 信息保护 (WIP) 开发人员指南

*启发式*应用可区分公司和个人数据，并且知道应基于管理员定义的 Windows 信息保护 (WIP) 策略保护哪些数据。

在本指南中，我们将向你介绍如何生成一个启发式应用。 完成此操作后，策略管理员将能够信任你的应用，允许它们使用组织数据。 员工会爱戴你，因为即使在他们从组织的移动设备管理 (MDM) 注销或完全退出组织后，你仍然将他们的个人数据完好无损保留在他们的设备上。

__注意__ 本指南有助于启发你创建 UWP 应用。 如果你想要启发 C++ Windows 桌面应用，请参阅 [Windows 信息保护 (WIP) 开发人员指南 (C++)](https://docs.microsoft.com/previous-versions/windows/desktop/EDP/wip-developer-guide?redirectedfrom=MSDN)。

可以在此处阅读有关 WIP 和启发式应用的详细信息：[Windows 信息保护 (WIP)](wip-hub.md)。

可以在[此处](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)找到完整示例。

如果你已准备好完成每个任务，那我们开始吧。

## <a name="first-gather-what-you-need"></a>首先，收集所需内容

你将需要以下内容：

* 运行 Windows 10 版本 1607 或更高版本的测试虚拟机 (VM)。 你将针对此测试 VM 调试你的应用。

* 运行 Windows 10 版本 1607 或更高版本的开发计算机。 这可能是你的测试 VM，前提是你已在其上安装了 Visual Studio。

## <a name="setup-your-development-environment"></a>设置你的开发环境

你将执行以下操作：

* [Install the WIP Setup Developer Assistant onto your test VM](#install-assistant)

* [Create a protection policy by using the WIP Setup Developer Assistant](#create-protection-policy)

* [Setup a Visual Studio project](#setup-vs-project)

* [Setup remote debugging](#setup-remote-debugging)

* [Add namespaces to your code files](#add-namespaces)

<a id="install-assistant" />

### <a name="install-the-wip-setup-developer-assistant-onto-your-test-vm"></a>在测试 VM 上安装 WIP 设置开发人员助手

 使用此工具在测试 VM 上设置 Windows 信息保护策略。

 在此处下载工具：[WIP 设置开发人员助手](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf)。

<a id="create-protection-policy" />

### <a name="create-a-protection-policy"></a>创建一个保护策略

通过将信息添加到 WIP 设置开发人员助手中的每个部分定义你的策略。 选择任何设置旁边的帮助图标，了解使用方法的详细信息。

有关如何使用此工具的更一般指南，请参阅应用下载页面上的版本说明部分。

<a id="setup-vs-project" />

### <a name="setup-a-visual-studio-project"></a>设置 Visual Studio 项目

1. 在开发计算机上，打开你的项目。

2. 添加对适用于通用 Windows 平台 (UWP) 的桌面和移动扩展的引用。

    ![添加 UWP 扩展](images/extensions.png)

3. 将此功能添加到程序包清单文件：

    ```xml
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*可选阅读*：“rescap”前缀表示*受限功能*。 请参阅[特殊和受限功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

4. 将此命名空间添加到程序包清单文件：

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. 将该命名空间前缀添加到程序包清单文件的 ``<ignorableNamespaces>`` 元素。

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    这样，如果你的应用在不支持受限功能的 Windows 操作系统版本上运行，Windows 将忽略 ``enterpriseDataPolicy`` 功能。

<a id="setup-remote-debugging" />

### <a name="setup-remote-debugging"></a>设置远程调试

如果你要在 VM 之外的计算机上开发你的应用，将仅在测试 VM 上安装 Visual Studio 远程工具。 然后，在你的开发计算机上，启动远程调试程序，并查看你的应用是否在测试 VM 上运行。

请参阅[远程电脑说明](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)。

<a id="add-namespaces" />

### <a name="add-these-namespaces-to-your-code-files"></a>将这些命名空间添加到代码文件

将这些 using 语句添加到代码文件的顶部（本指南中的代码段，使用它们）：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml;
using Windows.ApplicationModel.Activation;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Data.Xml.Dom;
using Windows.Foundation.Metadata;
using Windows.Web.Http.Headers;
```

## <a name="determine-whether-to-use-wip-apis-in-your-app"></a>确定是否要在你的应用中使用 WIP API

确保运行应用的操作系统支持 WIP，并且在设备上启用了 WIP。

```csharp
bool use_WIP_APIs = false;

if ((ApiInformation.IsApiContractPresent
    ("Windows.Security.EnterpriseData.EnterpriseDataContract", 3)
    && ProtectionPolicyManager.IsProtectionEnabled))
{
    use_WIP_APIs = true;
}
else
{
    use_WIP_APIs = false;
}
```
如果操作系统不支持 WIP，或者在设备上未启用 WIP，请不要调用 WIP API。

## <a name="read-enterprise-data"></a>读取企业数据

若要读取受保护的文件、网络终结点、剪贴板数据和你从“共享”合约中接受的数据，你的应用将必须请求访问权限。

如果你的应用在保护策略的允许列表上，则 Windows 信息保护会为你的应用提供权限。

**本部分内容：**

* [Read data from a file](#read-file)
* [Read data from a network endpoint](#read-network)
* [Read data from the clipboard](#read-clipboard)
* [Read data from a Share contract](#read-share)

<a id="read-file" />

### <a name="read-data-from-a-file"></a>从文件中读取数据

**Step 1: Get the file handle**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**Step 2: Determine whether your app can open the file**

调用 [FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync) 以确定你的应用是否可以打开该文件。

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if ((protectionInfo.Status != FileProtectionStatus.Protected &&
    protectionInfo.Status != FileProtectionStatus.Unprotected))
{
    return false;
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```

[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus) 值 **Protected** 表示文件受保护，并且你的应用可以打开它，因为你的应用在策略的允许列表中。

[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus) 值 **UnProtected** 表示文件不受保护，即使你的应用不在策略的允许列表中，你的应用仍然可以打开该文件。

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[FileProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**Step 3: Read the file into a stream or buffer**

*Read the file into a stream*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*Read the file into a buffer*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```
<a id="read-network" />

### <a name="read-data-from-a-network-endpoint"></a>从网络终结点读取数据

创建受保护的线程上下文，以从企业终结点读取。

**Step 1: Get the identity of the network endpoint**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

如果终结点不由策略管理，你将得到一个空字符串。

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)


**Step 2: Create a protected thread context**

如果终结点由策略管理，请创建一个受保护的线程上下文。 这将标记你在同一个线程上与该标识之间建立的任何连接。

它还为你提供由该策略管理的企业网络资源的访问权限。

```csharp
if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
    }
}
else
{
    return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
}
```
此示例将套接字调用包含在 ``using`` 块中。 如果你不执行此操作，请确保你在检索资源后关闭线程上下文。 请参阅 [ThreadNetworkContext.Close](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.threadnetworkcontext.close)。

不要在受保护的线程上创建任何个人文件，因为这些文件将自动加密。

无论终结点是否由策略管理，[**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext) 方法都会返回一个 [**ThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.threadnetworkcontext) 对象。 如果应用处理个人和企业资源，请为所有标识调用 [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)。  获取资源后，处置 ThreadNetworkContext 以从当前线程中清除所有标识标记。

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

**Step 3: Read the resource into a buffer**

```csharp
private static async Task<IBuffer> GetDataFromNetworkHelperMethod(Uri resourceURI)
{
    HttpClient client;

    client = new HttpClient();

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**(Optional) Use a header token instead of creating a protected thread context**

```csharp
public static async Task<IBuffer> GetDataFromNetworkbyUsingHeader(Uri resourceURI)
{
    HttpClient client;

    Windows.Networking.HostName hostName =
        new Windows.Networking.HostName(resourceURI.Host);

    string identity = await ProtectionPolicyManager.
        GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

    if (!string.IsNullOrEmpty(identity))
    {
        client = new HttpClient();

        HttpRequestHeaderCollection headerCollection = client.DefaultRequestHeaders;

        headerCollection.Add("X-MS-Windows-HttpClient-EnterpriseId", identity);

        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }
    else
    {
        client = new HttpClient();
        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }

}

private static async Task<IBuffer> GetDataFromNetworkbyUsingHeaderHelperMethod(HttpClient client, Uri resourceURI)
{

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**Handle page redirects**

有时，Web 服务器将通信重定向到资源的更新版本。

若要处理此情况，请发出请求，直到请求的响应状态的值为 **OK**。

然后使用该响应的 URI 获取终结点的标识。 以下是执行此操作的一种方法：

```csharp
private static async Task<IBuffer> GetDataFromNetworkRedirectHelperMethod(Uri resourceURI)
{
    HttpClient client = null;

    HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
    filter.AllowAutoRedirect = false;

    client = new HttpClient(filter);

    HttpResponseMessage response = null;

        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

    if (response.StatusCode == HttpStatusCode.MultipleChoices ||
        response.StatusCode == HttpStatusCode.MovedPermanently ||
        response.StatusCode == HttpStatusCode.Found ||
        response.StatusCode == HttpStatusCode.SeeOther ||
        response.StatusCode == HttpStatusCode.NotModified ||
        response.StatusCode == HttpStatusCode.UseProxy ||
        response.StatusCode == HttpStatusCode.TemporaryRedirect ||
        response.StatusCode == HttpStatusCode.PermanentRedirect)
    {
        message = new HttpRequestMessage(HttpMethod.Get, message.RequestUri);
        response = await client.SendRequestAsync(message);

        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
    else
    {
        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
}

```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="read-clipboard" />

### <a name="read-data-from-the-clipboard"></a>从剪贴板中读取数据

**Get permission to use data from the clipboard**

若要从剪贴板中获取数据，请要求 Windows 提供权限。 使用 [**DataPackageView.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync) 执行该操作。

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **API** <br>
[DataPackageView.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)

**Hide or disable features that use clipboard data**

确定当前视图是否有权获取剪贴板上的数据。

如果没有，你可以禁用或隐藏使用户从剪贴板粘贴信息或预览其内容的控件。

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **API** <br>
[ProtectionPolicyEvaluationResult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**Prevent users from being prompted with a consent dialog box**

新文档不属于*个人*，也不属于*企业*。 它只是新文档。 如果用户将企业数据复制到该文档中，Windows 会强制执行策略，并且用户会收到同意对话框提示。 此代码可防止此情况发生。 此任务不是为了帮助保护数据。 它更多是为了在你的应用创建全新的项目时防止用户收到同意对话框。

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **API** <br>
[DataPackageView.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)<br>
[ProtectionPolicyEvaluationResult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="read-share" />

### <a name="read-data-from-a-share-contract"></a>从“共享”合约读取数据

当员工选择你的应用来共享他们的信息时，你的应用将打开包含该内容的新项目。

正如我们之前提到的，新项目不属于*个人*，也不属于*企业*。 它只是新文档。 如果你的代码将企业内容添加到该项目，Windows 会强制执行策略，并且用户会收到同意对话框提示。 此代码可防止此情况发生。

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **API** <br>
[ProtectionPolicyManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.requestaccessasync)<br>
[ProtectionPolicyEvaluationResult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

## <a name="protect-enterprise-data"></a>保护企业数据

保护离开应用的企业数据。 当你在页面中显示数据、将数据保存到文件或网络终结点或者通过“共享”合约时，数据会离开你的应用。

**本部分内容：**

* [Protect data that appears in pages](#protect-pages)
* [Protect data to a file as a background process](#protect-background)
* [Protect part of a file](#protect-part-file)
* [Read the protected part of a file](#read-protected)
* [Protect data to a folder](#protect-folder)
* [Protect data to a network end point](#protect-network)
* [Protect data that your app shares through a share contract](#protect-share)
* [Protect files that you copy to another location](#protect-other-location)
* [Protect enterprise data when the screen of the device is locked](#protect-locked)

<a id="protect-pages" />

### <a name="protect-data-that-appears-in-pages"></a>保护显示在页面中的数据

当你在页面中显示数据时，请通知 Windows 它所属的数据类型（个人还是企业）。 若要执行此操作，请*标记*当前的应用视图或标记整个应用进程。

当你标记视图或进程时，Windows 将对它强制执行策略。 这有助于防止不受应用控制的操作所导致的数据泄露。 例如，在计算机上，用户可能使用 CTRL-V 从视图复制企业信息，然后将该信息粘贴到另一个应用中。 Windows 可以防范此风险。 Windows 还有助于强制执行“共享”合约。

**Tag the current app view**

如果你的应用具有多个视图，其中一些视图使用企业数据，而另一些视图使用个人数据，请执行此操作。

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty;
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**Tag the process**

如果你的应用中的所有视图都仅使用一种类型的数据（个人或企业），请执行此操作。

这可以使你无需管理独立标记的视图。

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
ProtectionPolicyManager.ClearProcessUIPolicy();
```

> **API** <br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="protect-file" />

### <a name="protect-data-to-a-file"></a>将数据保护到文件

创建受保护的文件，然后对该文件进行写入。

**Step 1: Determine if your app can create an enterprise file**

如果标识字符串由策略管理，并且你的应用在该策略的允许列表上，则你的应用可以创建企业文件。

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)


**Step 2: Create the file and protect it to the identity**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **API** <br>
[FileProtectionManager.ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)

**Step 3: Write that stream or buffer to the file**

*Write a stream*

```csharp
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*Write a buffer*

```csharp
     if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **API** <br>
[FileProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

<a id="protect-background" />

### <a name="protect-data-to-a-file-as-a-background-process"></a>将数据作为后台进程保护到文件

当设备屏幕处于锁定状态时，此代码可以运行。 如果管理员配置了安全的“锁屏下的数据保护”(DPL) 策略，则 Windows 会从设备内存中删除访问受保护的资源所需的加密密钥。 这可以防止数据在设备丢失时泄露。 此相同功能还会在受保护的文件句柄关闭时删除与之关联的密钥。

在创建文件时，你将需要使用一种使文件句柄保持打开的方法。  

**Step 1: Determine if you can create an enterprise file**

如果你正在使用的标识由策略管理，并且你的应用在该策略的允许列表上，则你可以创建企业文件。

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**Step 2: Create a file and protect it to the identity**

[  **FileProtectionManager.CreateProtectedAndOpenAsync**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync) 创建受保护的文件，并在你对其进行写入时使文件句柄保持打开状态。

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **API** <br>
[FileProtectionManager.CreateProtectedAndOpenAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync)

**Step 3: Write a stream or buffer to the file**

此示例将流写入文件。

```csharp
if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **API** <br>
[ProtectedFileCreateResult.ProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[ProtectedFileCreateResult.Stream](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.stream)<br>

<a id="protect-part-file" />

### <a name="protect-part-of-a-file"></a>保护文件的一部分

在大多数情况下，单独存储企业和个人数据更简洁，但你也可以将它们存储到同一个文件（如果需要）。 例如，Microsoft Outlook 可以将企业邮件和个人邮件一起存储在单个存档文件中。

加密企业数据，但不加密整个文件。 这样，即使用户从 MDM 注销或者他们的企业数据访问权限被吊销，他们仍然可以继续使用该文件。 此外，你的应用应跟踪它加密哪些数据，以便应用知道在它将文件读取回内存中时应保护哪些数据。

**Step 1: Add enterprise data to an encrypted stream or buffer**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **API** <br>
[DataProtectionManager.ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[BufferProtectUnprotectResult.buffer](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)


**Step 2: Add personal data to an unencrypted stream or buffer**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**Step 3: Write both streams or buffers to a file**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**Step 4: Keep track of the location of your enterprise data in the file**

跟踪该文件中企业所有的数据是你的应用的责任。

你可以将该信息存储在与该文件关联的属性中、数据库中或该文件的某些标题文本中。

此示例将该信息保存到单独的 XML 文件中。

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```
<a id="read-protected" />

### <a name="read-the-protected-part-of-a-file"></a>读取文件的受保护部分

下面介绍如何从该文件中读取企业数据。

**Step 1: Get the position of your enterprise data in the file**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**Step 2: Open the data file and make sure that it's not protected**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[FileProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

**Step 3: Read the enterprise data from the file**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**Step 4: Decrypt the buffer that contains enterprise data**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **API** <br>
[DataProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectioninfo)<br>
[DataProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync)<br>

<a id="protect-folder" />

### <a name="protect-data-to-a-folder"></a>将数据保护到文件夹

你可以创建一个文件夹，并保护它。 这样，你添加到该文件夹的任何项目都会自动受到保护。

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

在保护该文件夹前，确保它是空的。 你无法保护已经包含项目的文件夹。

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)<br>
[FileProtectionManager.ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)<br>
[FileProtectionInfo.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo.identity)<br>
[FileProtectionInfo.Status](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo.status)

<a id="protect-network" />

### <a name="protect-data-to-a-network-end-point"></a>将数据保护到网络终结点

创建受保护的线程上下文以将该数据发送到企业终结点。  

**Step 1: Get the identity of the network endpoint**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)

**Step 2: Create a protected thread context and send data to the network endpoint**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

<a id="protect-share" />

### <a name="protect-data-that-your-app-shares-through-a-share-contract"></a>保护你的应用通过“共享”合约共享的数据

如果你希望用户从应用共享内容，你将必须实现一个“共享”合约，并处理 [**DataTransferManager.DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) 事件。

在事件处理程序中，在数据包中设置企业标识上下文。

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="protect-other-location" />

### <a name="protect-files-that-you-copy-to-another-location"></a>将你复制的文件保护到另一个位置

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **API** <br>
[FileProtectionManager.CopyProtectionAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync)<br>

<a id="protect-locked" />

### <a name="protect-enterprise-data-when-the-screen-of-the-device-is-locked"></a>在设备屏幕处于锁定状态时保护企业数据

当设备锁定时，删除内存中的所有敏感数据。 当用户解锁设备时，你的应用可以安全地添加回该数据。

处理 [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending) 事件，以便你的应用知道屏幕何时锁定。 仅当管理员配置锁屏下的安全数据保护策略时，才引发此事件。 Windows 将临时删除设备上预配的数据保护密钥。 Windows 将删除这些密钥以确保在设备处于锁定状态并且可能不在其所有者的掌控下时，无法对加密数据进行未经授权的访问。  

处理 [**ProtectionPolicyManager.ProtectedAccessResumed**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) 事件，以便你的应用知道屏幕何时解锁。 无论管理员是否配置锁屏下的安全数据保护策略，都将引发此事件。

#### <a name="remove-sensitive-data-in-memory-when-the-screen-is-locked"></a>当屏幕锁定时，删除内存中的敏感数据

保护敏感数据，并关闭任何你的应用在受保护的文件上打开的文件流，以帮助确保系统不会在内存中缓存任何敏感数据。

此示例将内容从文本块保存到加密缓冲区，并从该文本块中删除该内容。

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessSuspending](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[DataProtectionManager.ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[BufferProtectUnprotectResult.buffer](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)<br>
[ProtectedAccessSuspendingEventArgs.GetDeferral](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral)<br>
[Deferral.Complete](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete)<br>

#### <a name="add-back-sensitive-data-when-the-device-is-unlocked"></a>当设备解锁时，添加回敏感数据

[**ProtectionPolicyManager.ProtectedAccessResumed**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) is raised when the device is unlocked and the keys are available on the device again.

[**ProtectedAccessResumedEventArgs.Identities**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedaccessresumedeventargs.identities) is an empty collection if the administrator hasn't configured a secure data protection under lock policy.

此示例执行上一示例的反向操作。 它解密缓冲区、将信息从缓冲区添加回文本块，然后释放该缓冲区。

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessResumed](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager.Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[DataProtectionManager.UnprotectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.unprotectasync)<br>
[BufferProtectUnprotectResult.Status](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult)<br>

## <a name="handle-enterprise-data-when-protected-content-is-revoked"></a>当吊销受保护的内容时处理企业数据

如果你希望你的应用在设备从 MDM 注销或策略管理员显式吊销对企业数据的访问权限时收到通知，请处理 [**ProtectionPolicyManager_ProtectedContentRevoked**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked) 事件。

此示例确定电子邮件应用的企业邮箱中的数据是否已吊销。

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **API** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked)<br>

## <a name="related-topics"></a>相关主题

[Windows Information Protection (WIP) sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)
