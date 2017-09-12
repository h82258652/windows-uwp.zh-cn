---
author: normesta
Description: "运行打包的应用并查看其状况，无需对其进行签名。 然后，设置断点并单步执行代码。 准备好在生产环境中测试应用后，对应用进行签名，然后安装它。"
Search.Product: eADQiWindows 10XVcnh
title: "运行、调试和测试打包的桌面应用（桌面桥）"
ms.author: normesta
ms.date: 06/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: c160fecc530a6366de48f4f2ecc24df2463c0469
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>运行、调试和测试打包的桌面应用（桌面桥）

运行打包的应用并查看其状况，无需对其进行签名。 然后，设置断点并单步执行代码。 准备好在生产环境中测试应用后，对应用进行签名，然后安装它。 本主题演示如何执行以下各项操作。

<span id="run-app" />
## <a name="run-your-app"></a>运行应用

可运行应用在本地进行测试，无需获得证书并对应用进行签名。

如果已使用 Visual Studio 中的 UWP 项目创建包，只需将打包项目设置为启动项目，然后按 CTRL+F5 启动应用即可。

如果使用 Desktop App Converter 或手动打包应用，则打开 Windows PowerShell 命令提示符，从输出文件夹的 **PacakgeFiles** 子文件夹运行此 cmdlet：

```
Add-AppxPackage –Register AppxManifest.xml
```
要启动应用，请在 Windows“开始”菜单中找到它。

![“开始”菜单中的打包应用](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> 打包的应用始终作为交互用户运行，任何安装已打包应用的驱动器都必须格式化为 NTFS 格式。

## <a name="debug-your-app"></a>调试应用

每次调试应用或安装扩展并调试应用时在对话框中选择你的软件包，而不必在每次启动会话时选择软件包。

### <a name="debug-your-app-by-selecting-the-package"></a>通过选择软件包来调试应用

此选项需要的设置时间量最少，但每次想要启动调试会话时都会要求执行一个额外步骤。


1. 确保至少启动一次打包应用，以便将其安装在本地机器上。

   请参阅上述[运行应用](#run-app)部分。

2. 启动 Visual Studio。

   如果希望使用提升的权限调试应用，请使用**以管理员身份运行**选项启动 Visual Studio。

3. 在 Visual Studio 中，选择**调试**->**其他调试目标**->**调试安装的应用程序包**。

4. 在**安装的应用程序包**列表中，选择你的应用包，然后选择**附加**按钮。


### <a name="debug-your-app-without-having-to-select-the-package"></a>调试应用，无需选择包

此选项需要的设置时间量最多，但无需在每次启动应用时选择已安装的包。 需要安装 [Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/) 才可使用此方法。

1. 首先，安装[桌面桥调试项目](http://go.microsoft.com/fwlink/?LinkId=797871)。

2. 启动 Visual Studio，打开桌面应用程序项目。

6. 将**桌面桥调试**项目添加至解决方案。

   可在已安装模板的**其他项目类型**组中找到项目模板。

    ![alt](images/desktop-to-uwp/debug-2.png)

    **桌面桥调试**项目将显示在解决方案中。

    ![alt](images/desktop-to-uwp/debug-3.png)

7. 打开**桌面桥调试**项目的属性页。

8. 将**程序包布局**字段设置为程序包清单文件 (AppxManifest.xml) 的位置，然后从**启动磁贴**下拉列表选择应用的可执行文件。

     ![alt](images/desktop-to-uwp/debug-4.png)

8. 在代码编辑器中打开 AppXPackageFileList.xml 文件。

9. 取消 XML 块的注释，并为以下元素添加值：

   **MyProjectOutputPath**：桌面应用程序调试文件夹的相对路径。

   **LayoutFile**：桌面应用程序调试文件夹中的可执行文件。

   **PackagePath**：转换过程中复制到 Windows 应用包文件夹的桌面应用程序可执行文件的完全限定文件名。

    下面是一个示例：

    ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
     <MyProjectOutputPath>..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    </PropertyGroup>
    <ItemGroup>
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.exe">
        <PackagePath>$(PackageLayout)\MyDesktopApp.exe</PackagePath>
      </LayoutFile>
    </ItemGroup>
  </Project>
    ```

  如果应用使用生成自解决方案中其他项目的 dll 文件，并且你希望单步执行包含在这些 dll 中的代码，请为每个 dll 文件包含一个 **LayoutFile** 元素。

  ```XML
  ...
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.Models.dll">
      <PackagePath>$(PackageLayout)\MyDesktopApp.Models.dll</PackagePath>
      </LayoutFile>
  ...
  ```

10. 将打包项目设置为启动项目。  

    ![alt](images/desktop-to-uwp/debug-5.png)

11. 在桌面应用程序代码中设置断点，然后启动调试程序。

  ![“调试”按钮](images/desktop-to-uwp/debugger-button.png)

  Visual Studio 会将 XML 文件中指定的可执行文件和 dll 文件复制到 Windows 应用包，然后启动调试程序。

#### <a name="handle-multiple-build-configurations"></a>处理多个生成配置

如果已定义多个生成配置（例如：发布和调试），可以修改 AppXPackageFileList.xml 文件，仅复制与启动调试程序时在 Visual Studio 中选择的生成配置匹配的文件。

下面提供了一个示例。

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'Debug'">..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'Release'"> ..\MyDesktopApp\bin\Release</MyProjectOutputPath>
</PropertyGroup>
```

#### <a name="debug-uwp-enhancements-to-your-app"></a>调试应用的 UWP 增强功能

你可能希望通过现代体验（例如动态磁贴）来增强应用。 如果希望执行此操作，可使用条件编译启用带有特定生成配置的代码路径。

1. 首先，在 Visual Studio 中定义生成配置并为其命名，如“DesktopUWP”。

2. 在项目的项目属性**生成**选项卡中，将该名称添加到**条件编译符号**字段。

     ![alt](images/desktop-to-uwp/debug-8.png)

3. 添加条件代码块。 此代码仅对 **DesktopUWP** 生成配置进行编译。

    ```csharp
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

### <a name="debug-the-entire-app-lifecycle"></a>调试整个应用生命周期

某些情况下，你可能希望更精细地控制调试过程，包括能够在应用启动前进行调试。

可以使用 [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 获取对应用生命周期的完全控制，包括暂停、恢复以及终止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 包含在 Windows SDK 中。


### <a name="modify-your-app-in-between-debug-sessions"></a>在调试会话之间修改应用

如果更改应用来修复 bug，请使用 MakeAppx 工具重新打包。 请参阅[运行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

## <a name="test-your-app"></a>测试应用

若要在准备分发时在现实环境中测试应用，最好对应用进行签名，然后安装它。

如果使用 Visual Studio 打包应用，可以运行脚本对应用进行签名，然后安装它。 请参阅[旁加载包](../packaging/packaging-uwp-apps.md#sideload-your-app-package)。

如果使用 Desktop App Converter 对应用打包，可以使用 ``sign`` 参数通过生成的证书自动对应用进行签名。 必须安装该证书，然后再安装应用。 请参阅[运行打包的应用](desktop-to-uwp-run-desktop-app-converter.md#run-app)。   

你还可以手动对应用进行签名。 下面是操作方法

1. 创建证书。 请参阅[创建证书](../packaging/create-certificate-package-signing.md)。

2. 将该证书安装到系统上**受信任根**或**受信任人**证书存储区中。

3. 使用该证书对应用进行签名，请参阅[使用 SignTool 对应用包进行签名](../packaging/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 确保证书上的发布者名称与应用的发布者名称匹配。

### <a name="related-sample"></a>相关示例

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>测试 Windows 10 S 应用

在发布你的应用之前，请确保它可在运行 Windows 10 S 的设备上正常运行。实际上，如果你打算将应用发布到 Windows 应用商店，则必须执行此操作，因为这是应用商店要求。 无法在运行 Windows 10 S 的设备上正常运行的应用将不会通过认证。 

请参阅[测试适用于 Windows 10 S 的 Windows 应用](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器内运行另一个进程

你可以在指定应用包的容器内调用自定义进程。 这对于测试方案（例如，如果你有自定义测试工具并想要测试应用的输出）很有用。 若要执行此操作，请使用 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>后续步骤

**查找特定问题的答案**

我们的团队监视这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供关于本文的反馈**

请使用下面的评论区。
