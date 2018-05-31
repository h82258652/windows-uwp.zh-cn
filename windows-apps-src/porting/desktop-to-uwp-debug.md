---
author: normesta
Description: Run your packaged app and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your app in a production environment, sign your app and then install it.
Search.Product: eADQiWindows 10XVcnh
title: 运行、调试和测试打包的桌面应用（桌面桥）
ms.author: normesta
ms.date: 08/31/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 0f8d443bc201d0e60387673edb7f9b73e2e47490
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2018
ms.locfileid: "1662677"
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>运行、调试和测试打包的桌面应用（桌面桥）

运行打包的应用并查看其状况，无需对其进行签名。 然后，设置断点并单步执行代码。 准备好在生产环境中测试应用后，对应用进行签名，然后安装它。 本主题演示如何执行以下各项操作。

<a id="run-app" />

## <a name="run-your-app"></a>运行应用

可运行应用以在本地进行测试，无需获得证书并对其进行签名。 运行应用的方式取决于你用于创建程序包的工具。

### <a name="you-created-the-package-by-using-visual-studio"></a>使用 Visual Studio 创建程序包

将打包项目设置为启动项目，然后按 CTRL+F5 启动应用。

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>你以手动方式或使用 Desktop App Converter 创建程序包

打开 Windows PowerShell 命令提示符，并从输出文件夹的 **PackageFiles** 子文件夹运行此 cmdlet：

```
Add-AppxPackage –Register AppxManifest.xml
```
要启动应用，请在 Windows“开始”菜单中找到它。

![“开始”菜单中的打包应用](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> 打包的应用始终作为交互用户运行，任何安装已打包应用的驱动器都必须格式化为 NTFS 格式。

## <a name="debug-your-app"></a>调试应用

调试应用的方式取决于你用于创建程序包的工具。

如果你使用 Visual Studio 2017 15.4 版中提供的[新的打包项目](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)创建了程序包，则只需将打包项目设置为启动项目，然后按 F5 即可调试应用。

如果你使用任何其他工具创建了程序包，请按照以下步骤操作。

1. 确保至少启动一次打包应用，以便将其安装在本地机器上。

   请参阅上述[运行应用](#run-app)部分。

2. 启动 Visual Studio。

   如果希望使用提升的权限调试应用，请使用**以管理员身份运行**选项启动 Visual Studio。

3. 在 Visual Studio 中，选择**调试**->**其他调试目标**->**调试安装的应用程序包**。

4. 在**安装的应用程序包**列表中，选择你的应用包，然后选择**附加**按钮。

#### <a name="modify-your-app-in-between-debug-sessions"></a>在调试会话之间修改应用

如果更改应用来修复 bug，请使用 MakeAppx 工具重新打包。 请参阅[运行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

### <a name="debug-the-entire-app-lifecycle"></a>调试整个应用生命周期

某些情况下，你可能希望更精细地控制调试过程，包括能够在应用启动前进行调试。

可以使用 [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 获取对应用生命周期的完全控制，包括暂停、恢复以及终止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 包含在 Windows SDK 中。

## <a name="test-your-app"></a>测试应用

若要在准备分发时在现实环境中测试应用，最好对应用进行签名，然后安装它。

### <a name="test-an-app-that-you-packaged-by-using-visual-studio"></a>测试使用 Visual Studio 打包的应用

Visual Studio 通过使用测试证书来对你的应用进行签名。 你会在**创建应用包**向导生成的输出文件夹中找到该证书。 证书文件具有 *.cer* 扩展名，必须将该证书安装到测试应用所在电脑上的**受信任的根证书颁发机构**存储中。 请参阅[旁加载包](../packaging/packaging-uwp-apps.md#sideload-your-app-package)。

### <a name="test-an-app-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>测试使用 Desktop App Converter (DAC) 打包的应用

如果使用 Desktop App Converter 对应用打包，可以使用 ``sign`` 参数通过生成的证书自动对应用进行签名。 必须安装该证书，然后再安装应用。 请参阅[运行打包的应用](desktop-to-uwp-run-desktop-app-converter.md#run-app)。   


### <a name="manually-sign-apps-optional"></a>手动对应用进行签名（可选）

你还可以手动对应用进行签名。 下面是操作方法

1. 创建证书。 请参阅[创建证书](../packaging/create-certificate-package-signing.md)。

2. 将该证书安装到系统上**受信任根**或**受信任人**证书存储区中。

3. 使用该证书对应用进行签名，请参阅[使用 SignTool 对应用包进行签名](../packaging/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 确保证书上的发布者名称与应用的发布者名称匹配。

    **相关示例**

    [SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>测试 Windows 10 S 应用

在发布你的应用之前，请确保它可在运行 Windows 10 S 的设备上正常运行。实际上，如果你打算将应用发布到 Microsoft Store，则必须执行此操作，因为这是 Microsoft Store 要求。 无法在运行 Windows 10 S 的设备上正常运行的应用将不会通过认证。

请参阅[测试适用于 Windows 10 S 的 Windows 应用](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器内运行另一个进程

你可以在指定应用包的容器内调用自定义进程。 这对于测试方案（例如，如果你有自定义测试工具并想要测试应用的输出）很有用。 若要执行此操作，请使用 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
