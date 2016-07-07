---
Description: "介绍如何将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）手动转换为通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: "将 Windows 桌面使用程序手动转换为通用 Windows 平台 (UWP) 应用"
ms.sourcegitcommit: 606d5237cb67cb4439704f81b180c3c48cc1556f
ms.openlocfilehash: be7599403e78c8db7ba91fe5a7c25b1c1a1ab644

---

# 将 Windows 桌面应用程序手动转换为通用 Windows 平台 (UWP) 应用

\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

使用转换器方便并且自动，如果你不确定安装程序的用途，则它很有用。 但是如果应用使用 xcopy 安装，或者你熟悉应用的安装程序对系统所做的更改，可以选择手动创建应用包和清单。

以下是手动创建 Centennial 程序包的步骤：

## 手动创建清单。

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

## 运行 MakeAppX 工具

使用[应用包生成工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) 为你的项目生成 AppX。 MakeAppx.exe 包含在 Windows 10 SDK 中。 

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

## 对 AppX 程序包进行签名

Add-AppxPackage cmdlet 要求必须对要部署的应用程序包 (.appx) 进行签名。 使用 Microsoft Windows 10 SDK 附带的 [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx) 对 .appx 程序包进行签名。

示例用法： 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

当你运行 MakeCert.exe 并且系统要求你输入密码时，请选择“无”****。 有关证书和签名的详细信息，请参阅以下内容： 

- [操作方法：创建在部署期间使用的临时证书](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe（签名工具）](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Jun16_HO5-->


