---
author: awkoren
Description: "介绍如何将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）手动转换为通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: "将 Windows 桌面应用程序手动转换为通用 Windows 平台 (UWP) 应用"
translationtype: Human Translation
ms.sourcegitcommit: ee697323af75f13c0d36914f65ba70f544d046ff
ms.openlocfilehash: f55f3bd6479cdf076c51cf574b07bfb5ce3a805c

---

# <a name="manually-convert-your-app-to-uwp-using-the-desktop-bridge"></a>使用桌面桥手动将你的应用转换到 UWP

使用[桌面应用转换器 (DAC)](desktop-to-uwp-run-desktop-app-converter.md) 方便并且自动，如果你不确定安装程序的用途，则它很有用。 但是，如果应用使用 xcopy 安装，或者你熟悉应用的安装程序对系统所做的更改，可能想要手动创建应用包和清单。 此文章包含开始使用的步骤。 它还介绍了如何将 DAC 未包含的更新的资源添加到你的应用中。 

下面介绍了如何开始使用：

## <a name="create-a-manifest-by-hand"></a>手动创建清单

你的 _appxmanifest.xml_ 文件需要具有以下内容（至少）。 将格式类似于 \*\*\*THIS\*\*\* 的占位符更改为应用程序的实际值。

```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
```

## <a name="add-unplated-assets"></a>添加更新的资源

下面介绍了如何为应用配置显示在任务栏中的 44x44 资源。

1. 获取正确的 44x44 图像并将它们复制到包含图像（即资源）的文件夹。

2. 对于每个 44x44 图像，在相同的文件夹中创建副本，并将 *.targetsize 44_altform unplated* 附加到文件名。 你应该有每个图标的两个副本，每个都以特定方式命名。 例如，完成该过程后，你的资源文件夹可能包含 *MYAPP_44x44.png* 和 *MYAPP_44x44.targetsize-44_altform unplated.png*（注意：前者是在 VisualElements 属性 *Square44x44Logo* 下的 appxmanifest 中引用的图标）。 

3.  在 AppXManifest 中，为每个固定为透明的图标设置 BackgroundColor。 此属性可在每个应用程序的 VisualElements 下找到。

4.  打开 CMD，将目录更改为程序包的根文件夹，并通过运行命令 ```makepri createconfig /cf priconfig.xml /dq en-US``` 创建 priconfig.xml 文件。

5.  使用 CMD 并保持在程序包的根文件夹中，使用命令 ```makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``` 创建 resources.pri 文件。 例如，应用的命令可能如下所示 ```makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml```。 

6.  使用下一步中的说明打包 AppX 以查看结果。

## <a name="run-the-makeappx-tool"></a>运行 MakeAppX 工具

使用[应用包生成工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) 为你的项目生成 AppX。 MakeAppx.exe 包含在 Windows 10 SDK 中。 

若要运行 MakeAppx，请先确保你已创建清单文件，如上所述。 

接下来，创建一个映射文件。 该文件应以 **[Files]** 开头，然后列出磁盘上的每个源文件，后跟其在程序包中的目标路径。 下面是一个示例： 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

最后，运行以下命令： 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## <a name="sign-your-appx-package"></a>对 AppX 程序包进行签名

Add-AppxPackage cmdlet 要求必须对要部署的应用程序包 (.appx) 进行签名。 使用 Microsoft Windows 10 SDK 附带的 [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx) 对 .appx 程序包进行签名。

示例用法： 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

当你运行 MakeCert.exe 并且系统要求你输入密码时，请选择“无”。 有关证书和签名的详细信息，请参阅以下内容： 

- [操作方法：创建在部署期间使用的临时证书](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe（签名工具）](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Dec16_HO1-->


