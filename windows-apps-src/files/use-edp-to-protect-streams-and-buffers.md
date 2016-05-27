---
author: TylerMSFT
Description: '本主题介绍了实现一些与流和缓冲区相关的最常见企业数据保护 EDP 方案所需的编码任务示例。'
MS-HAID: 'dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: '使用企业数据保护 (EDP) 来保护流和缓冲区'
---

# 使用企业数据保护 (EDP) 来保护流和缓冲区

__注意__ 在版本 1511（版本 10586）或更早版本的 Windows 10 上无法应用企业数据保护 (EDP) 策略。

本主题介绍了实现一些与流和缓冲区相关的最常见企业数据保护 EDP 方案所需的编码任务示例。 有关 EDP 如何关联到文件、数据流、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图，请参阅[企业数据保护 (EDP)](../enterprise/edp-hub.md)。

**注意** [企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵盖本主题中展示的许多相同方案。

## 先决条件


-   **完成 EDP 设置**

    请参阅[设置用于 EDP 的计算机](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力于生成企业启发式应用**

    自发确保企业数据处于管理企业控制之下的应用称为企业启发式应用。 启发式应用比未启发式应用更强大、更智能、更灵活，也更受信赖。 你的应用可以通过声明受限的 **enterpriseDataPolicy** 功能向系统声明它为启发式应用。 但要实现启发式的应用比要设置功能的应用多。 若要了解详细信息，请参阅[企业启发式应用](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C\# 或 Visual Basic 编写异步应用，请参阅[使用 C\# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 将数据流保护为企业标识


**注意** 强烈建议你每当要保护流或缓冲区时都订阅 [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411) 事件，以便你的应用知道 EDP 已在设备上变为已禁用的事件。 发生此情况时，应取消对所有流和缓冲区的保护。 如果用户从移动设备管理 (MDM) 取消注册设备，任何保留为受保护状态的流或缓冲区都有资格进行吊销。 如果在资源创建时禁用 EDP，则不宜吊销。 在 EDP 已禁用时取消保护资源将阻止该情况。



在此方案中，你的应用有权访问包含企业数据的未受保护的流。 为了确保在设备内外传输此流时受到保护，你的应用将数据保护为其所属的企业标识。 这允许企业按需擦除数据。 为了以后确定是否对流调用“unprotect”方法，应用必须保持指示流是否受到保护的状态，这便是此示例代理中函数返回该状态的原因。 如果未托管传递的标识，或者如果应用不允许使用该标识，流将无法受到保护，并且调用 [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021) 将返回“Unprotected”状态。

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

为了演示如何使用如上述代码示例中的方法，下面提供了一个使用它将字符串转换为未受保护的流然后保护此流的帮助程序方法。

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## 检索流的状态


在此方案中，对于必须阻止对其进行未授权访问的流，你的应用之前已对其进行保护。 为了在需要时检索回流的内容，应用可以检查流的状态。

请注意，流的状态也将从 [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) 返回。 此 API 将从不返回“Unprotected”的状态，因为它要求输入资源受到保护（无法可靠地验证资源是否受到保护）。 请注意，如果你的代码将结果与“Unprotected”对比，则表明存在设计缺陷。 这表明你的代码已无法跟踪流是否受到保护。

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## 取消对数据流的保护


在此方案中，你的应用想要对之前受到保护的流取消保护。 此代码示例获取受保护的流（请注意为使此过程有效，流必须受到保护）然后通过调用 [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) 取消对它的保护。 随后从流读取出一个字符串，然后将其返回。

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

为了演示到目前为止如何使用给定的帮助程序方法，下面提供一个事件处理程序，该程序从文本框获取一个字符串、将该字符串写入流、保护流、取消对流的保护（如果已成功受保护），最终从未受保护的流读回字符串然后将其显示在 UI 中。

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## 检索静态数据缓冲区的状态


在此方案中，对于必须阻止对其进行未授权访问的缓冲区，你的应用之前已对其进行保护。 为了在需要时检索回缓冲区的内容，应用可以检查缓冲区的状态。

请注意，缓冲区的状态也将从 [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022) 返回。 此 API 将从不返回“Unprotected”的状态，因为它要求输入资源受到保护。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## 保护从企业资源检索的静态数据


此方案包括与流代码示例相同的内容，只不过它适用于数据的缓冲区而已。 同样，你需要保持指示缓冲区是否受保护的状态，如下所示。 如果未托管传递的标识，或者如果应用不允许使用该标识，缓冲区将无法受到保护，并且调用 [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020) 将返回“Unprotected”状态。

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## 根据流或缓冲区的受保护标识启用 UI 策略强制实施。


如果你的应用打算在其 UI 上显示受保护的流或缓冲区的内容，则必须根据资源所保护到的标识启用 UI 策略强制实施。 你应当查询资源的保护信息，并根据检索的标识启用系统的 UI 策略强制实施。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // 'Unprotected' is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```

**注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题


[企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 命名空间**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=May16_HO2-->


