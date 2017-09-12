---
author: normesta
Description: "分发打包的桌面应用（桌面桥）"
Search.Product: eADQiWindows 10XVcnh
title: "将打包的桌面应用发布到 Windows 应用商店或将其旁加载到一台或多台设备上。"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.openlocfilehash: 24a005309271a91d669322787fb8d341e1a6d6ad
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="distribute-a-packaged-desktop-app-desktop-bridge"></a>分发打包的桌面应用（桌面桥）

将打包的桌面应用发布到 Windows 应用商店或将其旁加载到一台或多台设备上。  

> [!NOTE]
> 是否有计划如何将用户切换到打包的应用？ 分发应用前，请参阅本指南的[将用户切换到桌面桥应用](#transition-users)部分，获得一些参考。

## <a name="distribute-your-app-by-publishing-it-to-the-windows-store"></a>通过将应用发布到 Windows 应用商店来进行发布

[Windows 应用商店](https://www.microsoft.com/store/apps)是客户获取应用最便利的方法。

<div style="float: left; padding: 10px">
    ![应用商店图标](images/desktop-to-uwp/store.png)
</div>
将应用发布到应用商店供最广泛的受众使用。 此外，组织客户还可通过[适用于企业的 Windows 应用商店](https://www.microsoft.com/business-store)获取应用以分发到企业内部。

如果计划发布到 Windows 应用商店中，但尚未联系我们，请填写[此表单](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)，Microsoft 将联系你开始载入过程。

将应用提交到应用商店前无需对其进行签名。

>[!IMPORTANT]
> 如果你打算将应用发布到 Windows 应用商店，请确保它可在运行 Windows 10 S 的设备上正常运行。这是应用商店要求。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。

## <a name="distribute-your-app-without-placing-it-onto-the-windows-store"></a>分发应用但不将其置于 Windows 应用商店中

如果不想使用应用商店来分发应用，可以手动将应用分发给一台或多台设备。

如果想要更好地控制分发体验，或者不想涉及 Windows 应用商店证书过程，这很有意义。

若将应用到分发到其他设备而不将其置于应用商店中，则需要获取一个证书，使用该证书对应用进行签名，然后将应用旁加载到这些设备上。

你可以[创建证书](../packaging/create-certificate-package-signing.md)或从 [Verisign](https://www.verisign.com/) 等热门供应商处获取。

如果计划将应用分发到运行 Windows 10 S 的设备上，必须通过 Windows 应用商店对应用进行签名，因此需要先完成应用商店提交过程，然后才可将应用分发到这些设备上。

如果创建证书，则必须将其安装到运行应用的每台设备上的**受信任根**或**受信任人**证书存储区中。 如果从热门供应商处获取证书，则不必在其他系统上安装除应用外的任何内容。  

> [!IMPORTANT]
> 确保证书上的发布者名称与应用的发布者名称匹配。

若要使用证书对应用进行签名，请参阅[使用 SignTool 对应用包进行签名](../packaging/sign-app-package-using-signtool.md)。

若要将应用旁加载到其他设备上，请参阅[在 Windows 10 中旁加载 LOB 应用](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)。

**视频**

|将应用发布到 Windows 应用商店中 |分发企业应用  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<span id="transition-users" />
## <a name="transition-users-to-your-desktop-bridge-app"></a>将用户切换到桌面桥应用

分发应用前，请考虑向程序包清单添加几个扩展，以便帮助用户养成使用桌面桥应用的习惯。 下面是一些可以执行的操作。

* 将现有“开始”磁贴和任务栏按钮指向桌面桥应用。
* 将打包的应用与一组文件类型相关联。
* 使桌面桥应用默认打开特定类型的文件。

有关扩展的完整列表以及如何使用它们的指南，请参阅[将用户切换到应用](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，请考虑向完成这些任务的桌面桥应用添加代码：

* 将与桌面应用关联的用户数据迁移至桌面桥应用的相应文件夹位置。
* 为用户提供卸载桌面版应用的选项。

下面逐个谈谈这些任务。 首先是迁移用户数据。

### <a name="migrate-user-data"></a>迁移用户数据

如果要添加迁移用户数据的代码，最好只在首次启动应用时运行该代码。 迁移用户数据前，向用户显示一个对话框，解释将发生的情况、建议执行此操作的原因以及他们的现有数据会出现的情况。

下面的示例介绍如何在基于 .NET 的桌面桥应用中执行此操作。

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        String migrateMessage =
            "Would you like to migrate your data from the previous version of this app?";

        DialogResult migrateResult = MessageBox.Show
            (migrateMessage, "Data Migration", MessageBoxButtons.YesNo);

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

事先征得用户同意前，最好不要卸载用户桌面应用。 显示一个对话框，向用户征得该权限。 用户可能会决定不卸载桌面版应用。 如果发生这种情况，则需要确定是想要阻止桌面应用的使用还是支持并行使用两个应用。

下面的示例介绍如何在基于 .NET 的桌面桥应用中执行此操作。

要查看此代码片段的完整上下文，请参阅此示例的 **MainWindow.cs** 文件[使用转换/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)。

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop App is installed.
    if (uninstallString != null)
    {
        String uninstallMessage = "To have the best experience, consider uninstalling the "
            +" previous version of this app. Would you like to do that now?";

        DialogResult uninstallResult = MessageBox.Show
            (uninstallMessage, "Uninstall the previous version", MessageBoxButtons.YesNo);

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
                //Uninstallation was unsuccessful - You can choose to block the app here.
            }
        }
    }

}
```

### <a name="video"></a>视频

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>后续步骤

**查找特定问题的答案**

我们的团队会监视这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供关于本文的反馈**

请使用下面的评论区。
