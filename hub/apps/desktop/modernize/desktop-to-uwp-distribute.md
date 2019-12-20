---
Description: 分发使用桌面桥打包的应用
title: 将打包的桌面应用程序发布到 Microsoft Store 或将其旁加载到一个或多个设备上。
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 14ad6707b7203dddd9aa7be186e76da677bbd675
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302701"
---
# <a name="distribute-your-packaged-desktop-app"></a>分发打包桌面应用

如果决定将[桌面应用打包到 .msix 包中](/windows/msix/desktop/desktop-to-uwp-root)，可以将打包的应用程序发布到 Microsoft Store 或者将其旁加载到一个或多个设备上。

> [!NOTE]
> 你是否有计划将用户转换为打包应用程序的方法？ 分发应用前，请参阅本指南的[将用户切换到打包的应用](#transition-users)部分，获得一些参考。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>通过将应用程序发布到 Microsoft Store 来分发应用程序

[Microsoft Store](https://www.microsoft.com/store/apps) 是客户获取应用最便利的方法。

将您的应用程序发布到 Microsoft Store，以吸引最广泛的受众。 此外，组织客户可以通过[Microsoft Store For Business](https://businessstore.microsoft.com/store)获取应用程序，以便在内部分发给其组织。

如果你打算发布到 Microsoft Store，在提交过程中，系统将要求你额外回答几个问题。 这是因为程序包清单声明名为 **runFullTrust** 的受限功能，我们需要批准该功能对你的应用程序的使用。 可以在此处了解有关此要求的详细信息：[受限功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

在将应用程序提交到应用商店之前，无需对应用程序进行签名。

>[!IMPORTANT]
> 如果计划将应用程序发布到 Microsoft Store，请确保应用程序在运行 Windows 10 S 的设备上正常运行。这是一个存储要求。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](/windows/msix/desktop/desktop-to-uwp-test-windows-s)。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>分发应用程序而不将其放到 Microsoft Store

如果要在不使用应用商店的情况下分发应用程序，可以手动将应用分发到一个或多个设备。

如果要更好地控制分发体验，或者不想涉及 Microsoft Store 证书过程，这很有意义。

若要将应用程序分发给其他设备而不将其放入存储区中，则必须获取证书，使用该证书对应用程序进行签名，然后将应用程序旁加载到这些设备。

你可以[创建证书](/windows/msix/package/create-certificate-package-signing)或从 [Verisign](https://www.verisign.com/) 等热门供应商处获取。

如果你计划将应用程序分发到运行 Windows 10 S 的设备，你的应用程序必须由 Microsoft Store 签名，因此，你必须完成应用商店提交过程，然后才能将应用程序分发到这些设备。

如果创建证书，则必须将其安装到运行应用的每台设备上的**受信任根**或**受信任人**证书存储区中。 如果从热门供应商处获取证书，则不必在其他系统上安装除应用外的任何内容。  

> [!IMPORTANT]
> 确保证书上的发布者名称与应用的发布者名称匹配。

若要使用证书对应用程序进行签名，请参阅[使用 SignTool 对应用程序包进行签名](/windows/msix/package/sign-app-package-using-signtool)。

若要将应用程序旁加载到其他设备，请参阅[Windows 10 中的旁加载 LOB 应用](/windows/application-management/sideload-apps-in-windows-10)。

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>让用户切换到打包后的应用

分发应用前，请考虑向程序包清单添加几个扩展，以便帮助用户养成使用打包的应用的习惯。 下面是一些可以执行的操作。

* 将现有“开始”磁贴和任务栏按钮指向打包后的应用。
* 将打包应用程序与一组文件类型相关联。
* 默认情况下，使打包应用程序打开特定类型的文件。

有关扩展的完整列表以及如何使用它们的指南，请参阅[将用户切换到应用](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，请考虑向打包应用程序添加代码来完成以下任务：

* 将与桌面应用程序关联的用户数据迁移到打包应用的相应文件夹位置。
* 为用户提供卸载桌面版应用的选项。

下面逐个谈谈这些任务。 首先是迁移用户数据。

### <a name="migrate-user-data"></a>迁移用户数据

如果要添加用于迁移用户数据的代码，最好只在首次启动应用程序时运行该代码。 迁移用户数据前，向用户显示一个对话框，解释将发生的情况、建议执行此操作的原因以及他们的现有数据会出现的情况。

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

最好不要首先向用户的桌面应用程序询问权限。 显示一个对话框，向用户征得该权限。 用户可能会决定不卸载桌面版应用。 如果发生这种情况，你必须决定是要阻止使用桌面应用程序还是支持并行使用这两个应用。

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

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

如果在将应用程序发布到 Microsoft Store 时遇到问题，此[博客文章](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)包含一些有用的提示。
