---
Description: 分发使用桌面桥打包的应用
title: 将打包的桌面应用程序发布到 Microsoft Store，或将其旁加载到一台或多台设备上。
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 7eb57e8cea83a4d45087be4c4685ada8d108fa7a
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334493"
---
# <a name="distribute-your-packaged-desktop-app"></a>分发打包的桌面应用

如果决定[将桌面应用打包到 MSIX 程序包](/windows/msix/desktop/desktop-to-uwp-root)，可以将打包的应用程序发布到 Microsoft Store，或将其旁加载到一台或多台设备上。

> [!NOTE]
> 你有关于如何将用户切换到打包的应用程序的计划吗？ 分发应用前，请参阅本指南的[将用户切换到打包的应用](#transition-users)部分，获得一些参考。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>通过将应用程序发布到 Microsoft Store 来进行分发

[Microsoft Store](https://www.microsoft.com/store/apps) 可以方便客户获取你的应用。

将应用程序发布到 Microsoft Store，以供最广泛的受众使用。 此外，组织客户还可通过[适用于企业的 Microsoft Store](https://businessstore.microsoft.com/store) 获取应用程序以分发到企业内部。

如果你打算发布到 Microsoft Store，在提交过程中，系统将要求你额外回答几个问题。 这是因为你的程序包清单声明了名为 runFullTrust  的受限功能，我们需要批准你的应用程序使用该功能。 可以在此处阅读有关此要求的更多信息：[受限功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

将应用程序提交到 Microsoft Store 前无需对其进行签名。

>[!IMPORTANT]
> 如果你打算将应用程序发布到 Microsoft Store，请确保应用程序能够在运行 Windows 10 S 的设备上正常工作。这是 Microsoft Store 的要求。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](/windows/msix/desktop/desktop-to-uwp-test-windows-s)。

<a id="side-load"></a>

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>分发应用程序但不将其置于 Microsoft Store 中

如果不想使用 Microsoft Store 来分发应用程序，可以手动将应用分发给一台或多台设备。

如果要更好地控制分发体验，或者不想涉及 Microsoft Store 认证过程，可以这么做。

要将应用程序分发到其他设备而不将其置于 Microsoft Store 中，需获取一个证书，使用该证书对应用程序进行签名，然后将应用程序旁加载到这些设备上。

你可以[创建证书](/windows/msix/package/create-certificate-package-signing)或从 [Verisign](https://www.verisign.com/) 等热门供应商处获取。

如果计划将应用程序分发到运行 Windows 10 S 的设备上，必须通过 Microsoft Store 对应用程序进行签名，因此需要先完成 Microsoft Store 提交过程，然后才可将应用程序分发到这些设备上。

如果创建证书，必须将其安装到运行应用的每台设备上的“受信任根”或“受信任人”证书存储区。   如果从热门供应商处获取证书，则不必在其他系统上安装除应用外的任何内容。  

> [!IMPORTANT]
> 确保证书上的发布者名称与应用的发布者名称匹配。

要使用证书为应用程序签名，请参阅[使用 SignTool 为应用程序包签名](/windows/msix/package/sign-app-package-using-signtool)。

若要将应用程序旁加载到其他设备上，请参阅[在 Windows 10 中旁加载 LOB 应用](/windows/application-management/sideload-apps-in-windows-10)。

<a id="transition-users"></a>

## <a name="transition-users-to-your-packaged-app"></a>让用户切换到打包的应用

分发应用前，请考虑向程序包清单添加几个扩展，以便帮助用户养成使用打包的应用的习惯。 下面是一些可以执行的操作。

* 将现有“开始”磁贴和任务栏按钮指向打包的应用。
* 将打包的应用程序与一组文件类型相关联。
* 使打包的应用程序默认打开特定类型的文件。

有关扩展的完整列表以及如何使用它们的指南，请参阅[将用户切换到应用](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，请考虑向打包的应用程序添加用于完成这些任务的代码：

* 将与桌面应用程序关联的用户数据迁移至打包的应用的相应文件夹位置。
* 为用户提供卸载桌面版应用的选项。

下面逐个介绍这些任务。 首先是迁移用户数据。

### <a name="migrate-user-data"></a>迁移用户数据

如果要添加迁移用户数据的代码，最好只在首次启动应用程序时运行该代码。 迁移用户数据前，向用户显示一个对话框，说明目前的状况、建议迁移的原因以及其现有数据的去处。

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

最好不要在不经用户许可的情况下卸载其桌面应用程序。 显示一个对话框，请用户同意这么做。 用户可能会决定不卸载桌面版应用。 如果发生这种情况，你需要确定是要阻止使用桌面应用程序还是支持并行使用两个应用。

下面的示例介绍如何在基于 .NET 的打包的应用中执行此操作。

要查看此代码片段的完整上下文，请参阅此示例 [WPF 图片查看器与转换/迁移/卸载](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)的 MainWindow.cs  文件。

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
