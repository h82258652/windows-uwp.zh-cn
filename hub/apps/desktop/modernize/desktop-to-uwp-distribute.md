---
Description: 分发打包桌面应用程序 （桌面桥）
title: 在打包桌面应用程序发布到 Microsoft Store 或旁加载到一个或多个设备。
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 45d298aca60155915900f494654dce8e89fb1ee0
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131902"
---
# <a name="distribute-your-packaged-desktop-app"></a>分发打包桌面应用程序

如果你决定[MSIX 包中的将桌面应用程序打包](/windows/msix/desktop/desktop-to-uwp-root)，可以将发布到 Microsoft Store 或旁加载打包的应用程序放到一个或多个设备。

> [!NOTE]
> 你是否对打包的应用程序，您就可能会转换用户的计划？ 分发应用前，请参阅本指南的[将用户切换到打包的应用](#transition-users)部分，获得一些参考。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>通过将其发布到 Microsoft Store 中分发您的应用程序

[Microsoft Store](https://www.microsoft.com/store/apps) 是客户获取应用最便利的方法。

发布到 Microsoft Store 应用程序，以覆盖最广泛的受众。 此外，组织的客户可以获取你的应用程序在内部分发给其组织通过[于企业的 Microsoft Store](https://www.microsoft.com/business-store)。

如果你打算发布到 Microsoft Store，在提交过程中，系统将要求你额外回答几个问题。 这是因为程序包清单声明名为 **runFullTrust** 的受限功能，我们需要批准该功能对你的应用程序的使用。 你可以阅读更多有关此要求此处：[受限制的功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

您无需登录你的应用程序，然后将其提交到存储区。

>[!IMPORTANT]
> 如果你打算发布到 Microsoft Store 应用程序，请确保你的应用程序运行正常的设备上的运行 Windows 10 s。这是存储要求。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](/windows/msix/desktop/desktop-to-uwp-test-windows-s)。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>而无需将其放到 Microsoft Store 上分发应用程序

如果您而是将而无需使用应用商店分发你的应用程序，您可以手动分发应用到一个或多个设备。

如果要更好地控制分发体验，或者不想涉及 Microsoft Store 证书过程，这很有意义。

若要分发到其他设备的应用程序而无需将其放置在存储中，你必须获取证书，请使用该证书，然后旁加载到这些设备上的应用程序登录你的应用程序。

你可以[创建证书](/windows/uwp/packaging/create-certificate-package-signing)或从 [Verisign](https://www.verisign.com/) 等热门供应商处获取。

如果你打算分发到运行 Windows 10 S 的设备上的应用程序，你的应用程序必须由 Microsoft Store 签名，因此您必须在可以分发到这些设备上的应用程序之前经过应用商店提交过程。

如果创建证书，则必须将其安装到运行应用的每台设备上的**受信任根**或**受信任人**证书存储区中。 如果从热门供应商处获取证书，则不必在其他系统上安装除应用外的任何内容。  

> [!IMPORTANT]
> 确保证书上的发布者名称与应用的发布者名称匹配。

若要使用的证书签名应用程序，请参阅[签名应用程序包使用 SignTool](/windows/uwp/packaging/sign-app-package-using-signtool)。

旁加载应用程序转移到其他设备，请参阅[Windows 10 中的旁加载 LOB 应用](/windows/application-management/sideload-apps-in-windows-10)。

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>让用户切换到打包后的应用

分发应用前，请考虑向程序包清单添加几个扩展，以便帮助用户养成使用打包的应用的习惯。 下面是一些可以执行的操作。

* 将现有“开始”磁贴和任务栏按钮指向打包后的应用。
* 将打包的应用程序与文件类型的一组相关联。
* 请打开某些类型的文件默认情况下打包的应用程序。

有关扩展的完整列表以及如何使用它们的指南，请参阅[将用户切换到应用](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，请考虑将代码添加到你的打包应用程序完成这些任务：

* 将与桌面应用程序打包应用程序的相应的文件夹位置与关联的用户数据迁移。
* 为用户提供卸载桌面版应用的选项。

下面逐个谈谈这些任务。 首先是迁移用户数据。

### <a name="migrate-user-data"></a>迁移用户数据

如果您要添加将用户数据迁移的代码，最好仅在首次启动应用程序时才运行该代码。 迁移用户数据前，向用户显示一个对话框，解释将发生的情况、建议执行此操作的原因以及他们的现有数据会出现的情况。

下面的示例介绍如何在基于 .NET 的打包的应用中执行此操作。

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>卸载桌面版应用

它是更好的做法不卸载未第一个请求这些权限的用户的桌面应用程序。 显示一个对话框，向用户征得该权限。 用户可能会决定不卸载桌面版应用。 如果发生这种情况，您必须决定是否要阻止使用桌面应用程序或支持的同时使用这两个应用。

下面的示例介绍如何在基于 .NET 的打包的应用中执行此操作。

要查看此代码片段的完整上下文，请参阅此示例的 **MainWindow.cs** 文件[使用转换/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)。

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

如果在将应用程序发布到 Microsoft Store 时遇到问题，此[博客文章](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)包含一些有用的提示。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
