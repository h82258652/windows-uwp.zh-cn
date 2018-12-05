---
Description: Distribute a packaged desktop application (Desktop Bridge)
Search.Product: eADQiWindows 10XVcnh
title: 发布到 Microsoft Store 或旁加载打包桌面应用程序它到一个或多个设备上。
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.localizationpriority: medium
ms.openlocfilehash: 9fe216b70b7c10f254c0e3b7b18275a72d46215c
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8704954"
---
# <a name="distribute-a-packaged-desktop-application"></a>分发打包的桌面应用程序

发布到 Microsoft Store 或旁加载打包桌面应用程序它到一个或多个设备上。  

> [!NOTE]
> 你是否拥有如何将用户切换到打包应用程序的计划？ 分发应用前，请参阅本指南的[将用户切换到打包的应用](#transition-users)部分，获得一些参考。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>通过将其发布到 Microsoft Store 分发你的应用程序

[Microsoft Store](https://www.microsoft.com/store/apps) 是客户获取应用最便利的方法。

发布到 Microsoft Store 的应用程序，以获得最广泛的受众。 此外，组织客户还可以获取你的应用程序内部分发到通过[适用于企业的 Microsoft 应用商店](https://www.microsoft.com/business-store)。

如果你打算发布到 Microsoft Store，在提交过程中，系统将要求你额外回答几个问题。 这是因为程序包清单声明名为 **runFullTrust** 的受限功能，我们需要批准该功能对你的应用程序的使用。 可以在此处了解有关此要求的详细信息：[受限功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

你无需对你的应用程序，然后将其提交到应用商店才能。

>[!IMPORTANT]
> 如果你打算发布到 Microsoft 应用商店应用程序，请确保你的应用程序在运行 Windows 10 s。 设备上正常运行这是应用商店要求。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>而不将其置于 Microsoft Store 分发你的应用程序

如果您不会无需使用应用商店分发你的应用程序，你可以手动将应用分配到一个或多个设备。

如果要更好地控制分发体验，或者不想涉及 Microsoft Store 证书过程，这很有意义。

若要将分配到其他设备的应用程序，而将其放置在应用商店中，你必须获取一个证书，使用该证书，然后旁加载到这些设备上的应用程序对你的应用程序。

你可以[创建证书](../packaging/create-certificate-package-signing.md)或从 [Verisign](https://www.verisign.com/) 等热门供应商处获取。

如果你打算分发到运行 Windows 10 S 的设备上的应用程序，你的应用程序必须由 Microsoft Store 签名，因此你必须先完成应用商店提交过程，然后可以分发到这些设备上的应用程序。

如果创建证书，则必须将其安装到运行应用的每台设备上的**受信任根**或**受信任人**证书存储区中。 如果从热门供应商处获取证书，则不必在其他系统上安装除应用外的任何内容。  

> [!IMPORTANT]
> 确保证书上的发布者名称与应用的发布者名称匹配。

若要对你的应用程序使用证书进行签名，请参阅[登录应用程序包使用 SignTool](../packaging/sign-app-package-using-signtool.md)。

旁加载到其他设备上的应用程序，请参阅[Windows 10 中的旁加载 LOB 应用](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)。

**视频**

|发布到 Microsoft 应用商店应用程序 |分发企业应用程序  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>让用户切换到打包后的应用

分发应用前，请考虑向程序包清单添加几个扩展，以便帮助用户养成使用打包的应用的习惯。 下面是一些可以执行的操作。

* 将现有“开始”磁贴和任务栏按钮指向打包后的应用。
* 将打包的应用程序与一组文件类型相关联。
* 使打包应用程序默认打开特定类型的文件。

有关扩展的完整列表以及如何使用它们的指南，请参阅[将用户切换到应用](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，请考虑向完成这些任务的打包应用程序添加代码：

* 迁移至打包的应用的相应文件夹位置的桌面应用程序与相关联的用户数据。
* 为用户提供卸载桌面版应用的选项。

下面逐个谈谈这些任务。 首先是迁移用户数据。

### <a name="migrate-user-data"></a>迁移用户数据

如果你要添加迁移用户数据的代码，最好是只在首次启动应用程序时运行该代码。 迁移用户数据前，向用户显示一个对话框，解释将发生的情况、建议执行此操作的原因以及他们的现有数据会出现的情况。

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

它是最好不要卸载用户桌面应用程序，而无需第一个询问其权限。 显示一个对话框，向用户征得该权限。 用户可能会决定不卸载桌面版应用。 如果发生这种情况，你需要决定是否要阻止桌面应用程序的使用情况或支持的并行使用两个应用。

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

### <a name="video"></a>视频

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

如果在将应用程序发布到 Microsoft Store 时遇到问题，此[博客文章](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)包含一些有用的提示。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
