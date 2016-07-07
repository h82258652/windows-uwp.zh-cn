---
author: mcleblanc
Description: "此中心主题涉及企业数据保护 (EDP) 如何关联到文件、缓冲区、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图。"
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "企业数据保护 (EDP)"
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 97bdbce8360fabad63f9fe7e85e5172ccd83f403

---

# 企业数据保护 (EDP)

__注意__ 在版本 1511（内部版本 10586）或更早版本的 Windows 10 上无法应用企业数据保护 (EDP) 策略。

此中心主题涉及企业数据保护 (EDP) 如何关联到文件、缓冲区、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图。

有关最终用户和管理员视角的 EDP 的详细信息，请参阅[企业数据保护 (EDP) 概述](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)。

## 什么是 EDP？

EDP 是台式机、笔记本电脑、平板电脑和手机上用于实现移动设备管理 (MDM) 的一组功能。 EDP 使企业可以更好地控制企业所管理设备上的数据（企业文件和数据 blob）的处理方式。

-   通过加密操作来标记企业数据。 这即为“受企业保护的数据”，或简称为“受保护数据”。
-   仅管理企业通过 EDP 策略明确允许的应用才可以访问受企业保护的数据（例如，在文件中）。
-   仅通过 EDP 策略明确允许的应用才具有企业虚拟专用网络 (VPN) 的访问权限。
-   应用限制策略还用于确定允许的应用应该如何处理企业数据。
-   基于策略的限制甚至应用到通过 Windows 剪贴板或“共享”合约交换的企业内容。
-   如有必要，管理企业可以撤消设备的受保护内容的访问权限，本质上是擦除设备中的企业数据，同时完整保留个人数据。
-   通道应用是用于下载受保护数据的应用。 示例包括邮件和文件同步应用。

EDP 增强了[加密文件系统 (EFS)](http://technet.microsoft.com/library/cc700811.aspx) 和 [Windows 选择性擦除](https://technet.microsoft.com/library/dn486874.aspx)，以便提供更多的安全性和灵活性选项。 可以使用新的 EDP API 创建具有以下功能的应用：保护和撤消企业内容的访问权限、使用受保护文件的属性和以其原始形式访问加密数据。 除了引入用于保护文件和文件夹的新 API 外，还引入了用于保护缓冲区和流的 API。 还引入了一组允许应用标识和指示企业必须强制执行数据保护策略的 API。

这样一来，管理企业便可以控制其受保护数据的访问权限、定义应用列表的应用限制策略以及这些应用的限制。 默认情况下，应用无法自发访问受保护的数据。 若要获取访问权限，必须将应用添加到名为允许列表的列表中，而允许列表中的应用称为允许应用。 在托管设备上，Windows 可以限制和/或审核剪贴板上或通过“共享”合约的受保护数据的访问权限，以便对不在允许列表上的某个应用的访问进行审核和/或要求用户同意，否则完全阻止。

管理企业会将 EDP 策略提供给设备（这使得该设备成为“托管设备”）。 可以通过用户在移动设备管理 (MDM) 中进行注册、通过 IT 手动配置或通过其他管理和策略传递机制（例如系统中心配置管理器 (SCCM)）来预配策略。

EDP 文件保护利用权限管理服务 (RMS) 密钥（如果已预配），因为这些密钥可以跨设备漫游，从而允许受保护数据漫游。 如果未预配 RMS 密钥，这些 API 将回退到本地选择性擦除密钥，并限制漫游功能。 加密的漫游数据可以通过 Microsoft 所提供的特定于平台的 RMS 应用在下层 Windows 上进行访问，也可以使用已启用 RMS 的第三方应用在第三方设备上进行访问。

总之，EDP API 提供的数据保护由企业管理，以便你可以采用有助于企业保护的方式生成你的应用并管理其数据。 也就是说，你可以生成面向企业的应用。 而且，本指南的其余相关部分会帮助你达成此目标。

## 设置用于 EDP 的计算机


为正确开发你的应用，并在企业中测试它的行为方式，需要为你的计算机或设备设置合适条件。 这通常在 IT 管理员域中涉及多个任务。

-   将你的开发计算机注册到移动设备管理 (MDM) 中。
-   通过 [AppLocker 配置服务提供程序](https://msdn.microsoft.com/library/windows/hardware/dn920019)将你的应用添加到允许列表中。

下一个任务是生成具有以下功能的应用：识别和可以动态响应企业内部运行的托管标识及生效的保护策略。 这意味着“启发”你的应用。 启发以遵循策略的应用很可能位于允许访问企业数据的列表上。

## 企业启发式应用


在你的应用位于允许列表后，该应用可以读取受保护数据。 默认情况下，你的应用输出的任何数据自动受系统保护。 不管怎样，此类自动保护是出于管理企业必须确保企业数据处于自己的控制之下。 但是，对你的应用执行此类严格控制是达成上述目的唯一默认方法。 更好的方法是要求系统对你足够信任，以提供给你更多的权利和灵活性。 该方法的入场门槛是生成更智能的应用。 这意味着要比添加到允许列表更进一步；这意味着使你的应用成为（并将该应用声明为）企业启发式应用。

如果你的应用使用我们描述为自主保护企业数据（无论该数据处于闲置、使用中还是未送达状态都是如此）的技术，则表明该应用已受启发。 你的启发式应用会识别企业数据源和企业数据，并在数据到达你的应用时对它进行保护。 成为启发式应用还意味着识别和遵循 EDP 策略，无论企业数据是否离开你的应用都是如此。 这包括禁止内容转到非企业网络终结点、在允许数据漫游前采用可移植加密形式打包数据，以及在将企业数据粘贴到不在允许列表上的应用前提示用户（具体取决于策略设置）。 在生成你的启发式应用后，你的应用通过声明受限的 **enterpriseDataPolicy** 功能向系统宣布它为启发式应用。 有关使用受限功能的详细信息，请参阅[特殊和受限功能](https://msdn.microsoft.com/library/windows/apps/mt270968#special_and_restricted_capabilities)。

理想情况下，处于闲置和未送达状态下的所有企业数据都是受保护数据。 但不可避免的是，最初生成企业数据和其受到保护之间必然存在一段空白期。 有时企业数据可能存在于未进行加密的企业网络终结点上。 启发式应用能够自主保护此类数据；受允许但并非启发式的应用需要将保护交由系统来实施。

这是因为非启发式应用始终在企业模式下运行。 系统可确保上述行为。 但对于启发式应用可在任何给定时间进行处理的数据类型，该应用可以随意和根据相应情况在企业模式和个人模式之间自由切换。 启发式应用应尊重个人数据，而不是将个人数据标记为企业数据，这一点也很重要。 只要遵守这些约定，启发式应用就可以同时处理企业数据和个人数据。 下一部分介绍如何使用代码切换模式。

## 确认托管的标识，并确定保护策略强制级别

应用通常从外部资源（例如，邮箱电子邮件地址、托管的域或 URI 主机名）获取企业标识。 你可以调用 [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) 以获取网络终结点主机名的托管标识（如果有）。

此代码示例举例说明启发式行为的常规结构，包括如何确定企业标识实际上是否受托管以及当前哪些策略已生效。

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

如上所示，首先要确定是否已为企业的标识设置了 EDP 策略。 术语“托管”是“由 EDP 策略托管”的简称。 当为一个特定标识设置了 EDP 策略时，[**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171) 将针对该标识返回 true。 这提示你将 EDP API 与该标识结合使用。 尽管文件和缓冲区 API 在它们按预期发挥作用方面存在一些例外，但该方案也毫无意义，即使对于非托管标识也是如此。 如果设备受托管，则会将它托管为企业标识。 如果标识不受托管，将数据保护为该标识将毫无意义。

下一步是确定并实现策略的强制级别。 若要确定策略的强制级别，请调用 [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406) 方法。 当企业策略在标识上强制执行时，启发式应用必须在 UI 活动或网络访问期间通过调用 [**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/dn705170) API 来帮助系统执行策略强制，以确保系统在必要时使用此标识标记数据传输。 本指南中包含有关如何解释强制级别和实施该强制级别的详细信息。 该代码示例还显示了如何进入企业模式和返回到个人模式，方法是相应地将 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 的值设置为企业标识或空字符串。 此外，请注意进入和退出企业模式仅对托管标识有意义。

## EDP 功能一览


**文件和缓冲区保护。**

-   应用可以保护、打包传输和擦除与企业标识关联的数据。
-   密钥管理由 Windows 处理。 当企业的 RMS 密钥可供设备使用时，Windows 会使用这些密钥；否则 Windows 将退回到本地选择性擦除保护。

**设备策略管理。**

-   应用可以查询正管理设备的标识（企业或组织）。
-   应用可以通过将标识与相关数据关联来防止用户无意泄漏数据。

-   应用可以通过检查企业所拥有的网络终结点连接（服务器、IP 范围）和将数据关联到托管（即，MDM 注册的）标识，来保护网络上的企业资源。
-   EDP API 仅适用于在设备上已定义 EDP 策略的托管标识。 如果标识不受托管，则 API 在必要时会向应用程序指示该标识。

下面是深入 EDP API 的主题和特定于这些特定功能区域的方案的链接列表。

## 文件

请参阅[使用 EDP 保护文件](../files/protect-your-enterprise-data-with-edp.md)。

## 流和缓冲区

请参阅[使用 EDP 保护流和缓冲区](../files/use-edp-to-protect-streams-and-buffers.md)。

## 剪贴板、共享和在应用之间交换数据

请参阅[使用 EDP 保护在应用之间传输的企业数据](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md)。

## 网络

请参阅[使用 EDP 标识标记网络连接](../networking/tagging_network_connections_with_edp_identity.md)。

## 锁屏下的数据保护 (DPL) 和后台任务

组织可以选择管理“锁屏下的数据保护”(DPL) 安全策略，其中在设备锁定时，会临时从设备内存中删除访问受保护资源所需的加密密钥。 若要使应用针对此不测事件做好准备，请参阅本主题中的[处理设备锁定事件和避免在内存中保留不受保护内容](#handle_lock_events)部分。 此外，如果应用具有需要保护文件的后台任务，请参阅[保护新文件中的企业数据（针对某个后台任务）](../files/protect-your-enterprise-data-with-edp.md#protect_data_new_file_bg)。

## UI 策略强制执行


在本部分中，我们将以启发式邮件应用为例进行说明，该应用当前正显示的企业邮箱位于同时包含企业邮箱和属于用户的个人邮箱的一组邮箱之中。 为确保企业邮箱中的企业数据不会泄露，该应用会调用 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) 以确保操作系统知道应用的当前上下文是企业还是个人。 如果标识不受企业策略的管理，该 API 将返回 false。

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```

## 处理设备锁定事件和避免在内存中保留不受保护内容


在此方案中，我们将以启发式邮件应用为例进行说明，该应用设计用于处理企业邮件和个人邮件。 当应用在已选择管理“锁屏下的数据保护”(DPL) 安全策略的组织中运行时，该应用必须确保在设备处于锁定的情况下从内存中删除所有敏感数据。 为此，它将注册 [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) 和 [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) 事件，以便在锁定和解锁设备时收到通知（如果 DPL 已生效）。

在临时删除设备上预配的数据保护密钥之前，会引发 [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787)。 在设备处于锁定状态时删除这些密钥的原因是，确保当设备已锁定且可能还未拥有其所有权时不存在对加密数据的未经授权的访问。 一旦密钥在设备解锁后再次可用，将引发 [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786)。

通过处理这两个事件，该应用可以确保使用 [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017) 保护内存中的任何敏感内容。 它还应关闭对其受保护文件打开的任何文件流，以确保系统不会在内存中缓存任何敏感数据。 可以通过多种方式之一完成此操作。 若要关闭从 **StorageFile** 的 Open 方法返回的文件流，可以调用该流上的 **Dispose** 方法。 可以使用 using 语句（C\# 或 VB）设置流的使用范围。 或者，可以将 **DataReader** 或 **DataWriter** 对象环绕在该流周围，并将 **Dispose** 方法或 using 语句与该对象结合使用。

**注意**  
在未配置 DPL 策略的环境中，会引发 [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786)事件，但不会引发 [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) 事件。 在代码中应注意到这一点，并且注意不要假定每个系统中事件总是会成对出现，也不要假定你总是可以使用事件确定设备的锁定/解锁状态。 你可以在此处的代码示例中看到：我们尽量不假定有关当前所显示的电子邮件的任何内容，也不假定数据库文件流的打开状态的任何内容。

此外，请注意，在未配置 DPL 策略的设备上从锁定恢复时，[**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772) 为空集合。

为简便起见，此代码示例稍微做了简化，侧重于介绍企业邮件的处理。 在实际应用中，个人邮件会被写入未受保护的不同邮件数据库文件，并且你无法保护锁定状态下的个人电子邮件。

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it's closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it's open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## 注册为在撤消受保护内容时收到通知


假定邮件应用已在用户设备上设置了企业邮箱这一情形。 在一些情况下（出于多个可能的原因之一），企业希望撤消对该设备上受企业保护的电子邮件和其他资源的访问权限。 撤消的可能原因有很多个。 很可能是特定企业策略要触发对注销的撤消，而一种情形是用户已从企业中注销了其设备（或许他们要赠送或出售设备、只想使用其他设备、移动到另一个作业或要淘汰设备）。 另一种情况是，IT 管理员通过移动设备管理 (MDM) 远程发送了注销通知（如果设备报告为丢失）。

无论此类情况的具体细节如何，企业都会发送擦除用户设备中所有邮件的请求，因为该设备上已不再需要。 设备上的远程管理客户端从企业的远程管理服务器接收请求，然后调用 [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) 以撤消访问该设备上保护为企业标识的内容所需的密钥。

如果需要在发生撤消时通知应用，你可以注册 [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788) 事件。 当应用收到该事件时，它可以删除任何与企业邮箱关联但不再需要的元数据。

```CSharp
using Windows.Security.EnterpriseData;

...

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

## 远程管理客户端发起受企业保护数据的擦除


用户在其个人设备上具有多个保护为企业标识的企业文件。 用户丢失了其设备。 当企业收到用户已丢失其设备的通知时，该企业会发送从用户设备擦除所有敏感数据的请求，以防止泄漏该数据。 设备上的远程管理客户端从企业的远程管理服务器接收此请求，然后调用 [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) 以撤消访问保护为企业标识的内容所需的密钥。

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 






<!--HONumber=Jun16_HO4-->


