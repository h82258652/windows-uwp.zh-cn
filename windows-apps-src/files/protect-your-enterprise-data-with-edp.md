---
author: TylerMSFT
Description: "本主题演示了实现一些与文件相关的最常见企业数据保护 (EDP) 方案所需的编码任务示例。"
MS-HAID: dev\_files.protect\_your\_enterprise\_data\_with\_edp
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "使用企业数据保护 (EDP) 保护文件"
translationtype: Human Translation
ms.sourcegitcommit: 9b9e9ecb70f3a0bb92038ae94f45ddcee3357dbd
ms.openlocfilehash: a31fc65599f43be5b302b568774a51ab77065300

---

# 使用企业数据保护 (EDP) 来保护文件

> [!NOTE]
> 在版本 1511（内部版本 10586）或更早版本的 Windows 10 上无法应用企业数据保护 (EDP) 策略。

本主题演示了实现一些与文件相关的最常见企业数据保护 (EDP) 方案所需的编码任务示例。 有关 EDP 如何关联到文件、数据流、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图，请参阅[企业数据保护 (EDP)](../enterprise/edp-hub.md)。

> [!NOTE]
> [企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵盖许多在本主题中展示的相同方案。

## 先决条件

-   **完成 EDP 设置**

    请参阅[设置用于 EDP 的计算机](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力于生成企业启发式应用**

    自发确保企业数据处于管理企业控制之下的应用称为企业启发式应用。 启发式应用比未启发式应用更强大、更智能、更灵活，也更受信赖。 你的应用可以通过声明受限的 **enterpriseDataPolicy** 功能向系统声明它为启发式应用。 但要实现启发式的应用比要设置功能的应用多。 若要了解详细信息，请参阅[企业启发式应用](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C\# 或 Visual Basic 编写异步应用，请参阅[使用 C\# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 你的本地文件夹路径，以及在“文件资源管理器”中查看受保护的文件


你可以创建承载本主题代码示例的应用以试用这些功能。 代码示例将文件编写到应用的本地文件夹，并且该文件夹在磁盘上的确切位置专用于你的应用。 若要确定应用的本地文件夹位置，你可以添加以下代码。

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

拥有路径后，你可以使用“文件资源管理器”轻松找到应用所创建的文件。 这样你将能够确认它们是否受到保护以及是否按正确标识受到保护。

在“文件资源管理器”的“更改文件夹和搜索选项”****中的“视图”****选项卡上，选择“显示用颜色加密的文件”****。 另外，使用“文件资源管理器”的“视图”****&gt;“添加列”****命令以添加“加密目标”****列，以便查看保护文件的企业标识。

## 在新文件中保护企业数据（针对某个交互式应用）


企业数据进入你的应用的方法有很多种，包括从某些网络终结点、文件、剪贴板或共享合约中进入。 你的应用也可能会创建新的企业数据。 无论启发式应用提供企业数据的方式是什么，应用在将数据保留到新文件时应谨慎将数据保护在托管企业标识下。

基本步骤是使用常规存储 API 创建文件、使用 EDP API 将文件保护在企业标识下，然后（再次使用常规存储 API）写入文件。 在写入文件之前请谨慎保护文件（如以下示例所示）。 使用 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) 方法保护文件。 并且和往常一样，仅在某个标识受托管时保护到该标识下才有意义。 有关如此操作的原因以及应用如何确定在其中运行的企业标识的详细信息，请参阅[确认标识已托管](../enterprise/edp-hub.md#confirming-an-identity-is-managed)。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## 在新文件中保护企业数据（针对某个后台任务）


我们在上一节中所使用的 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) API 仅适用于交互式应用。 对于后台任务，你的代码可在锁屏界面下执行。 而组织可能管理的是“锁屏下的数据保护”(DPL) 安全策略，其中在设备锁定时，会临时删除访问受保护的资源所需的加密密钥。 这可以防止数据在设备丢失时泄露。 此功能还会在受保护的文件句柄关闭时删除与之关联的密钥。 但是，可以在锁定窗口期间（设备锁定和解锁之间的时间）创建新的受保护文件，并在文件句柄保持打开时访问它们。 **StorageFolder.CreateFileAsync** 会在创建文件时关闭句柄，所以不可使用此算法。

1.  使用 **StorageFolder.CreateFileAsync** 创建新文件。
2.  使用 **FileProtectionManager.ProtectAsync** 进行加密。
3.  打开该文件，并对其进行写入。

由于步骤 1 中包含关闭文件句柄（即使步骤 1 不关闭句柄，步骤 2 也会关闭），步骤 3 无法执行，因为访问该文件的加密密钥不再可用。

若要解决此情况，你可以使用 [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) 创建一个受保护的文件并将 **IRandomAccessStream** 返回给它。 这允许应用在文件句柄保持打开时对文件进行多次写入。

此示例代码演示了一个在收到新邮件时写出新文件的简化邮件应用。 邮件应用必须在设备锁定时同步电子邮件。 当此应用收到新邮件时，它将使用 [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) 创建一个新文件、检索输出流并将邮件正文写入该文件。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It's important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## 保护用于包含企业数据的文件夹


如果你希望文件夹内的每一项都受到保护，你依然可以实现此目的。 你可以使用 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) 保护空文件夹。 然后，在该文件夹中后续创建的任何文件或文件夹都将受到保护。 你可以保护现有文件夹，或创建要保护的新文件夹（以下示例可创建新文件夹）。 但对于任意一种情况，若要成功保护，文件夹当时必须为空。 如果不为空，则 [**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150) 将包含 [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147) 的值。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## 将保护从一个文件复制到另一个文件


在本方案中，一个已存在的已知文件保护在相应的企业标识下。 你可以非常方便地将该保护复制到其他文件。 你甚至无需知道该标识是什么，只需知道这是正确的标识即可。

若要制作受保护文件的简单副本，你可以调用 [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190)。 生成的文件副本将具有与源文件相同的保护。

若要先保护未受保护的现有文件，然后再将企业数据写入其中，另一种调用 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157)（我们在早期的方案中见过此方法，并且你需要为此传递托管标识）的方法是调用 [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152)，如代码示例中所示。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

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

## 处理访问受保护文件遭拒的情况


在本方案中，你的应用尝试访问应用以前保护过的文件，但该访问遭到拒绝。 你将需要检查文件状态以查看出了什么问题。 在此代码示例中，应用会调用 [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154) API 来查询状态，并确定原因是否是因为现在远程管理导致吊销对文件的访问权限。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## 根据文件的受保护标识启用 UI 策略强制


如果你的应用打算在其 UI 上显示受保护的文件（例如 PDF）的内容，则必须根据文件的受保护标识启用 UI 策略强制。 你应当查询文件的保护信息，并从检索的标识启用系统的 UI 策略强制。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

> '!注意] 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题

- [企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

- [**Windows.Security.EnterpriseData 命名空间**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jul16_HO1-->


