---
Description: 了解如何向非打包桌面应用程序授予标识，以便可以在这些应用程序中使用新式 Windows 10 功能。
title: 向未打包的桌面应用授予标识
ms.date: 10/25/2019
ms.topic: article
keywords: windows 10、desktop、package、identity、.MSIX、Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 10ed6b8e1bd5efce4c9d4429d91849b1333505b6
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521348"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>向未打包的桌面应用授予标识

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

许多 Windows 10 扩展性功能都需要从非 UWP 桌面应用（包括后台任务、通知、动态磁贴和共享目标）使用[包标识](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)。 对于这些情况，操作系统需要标识，以便它能够识别相应 API 的调用方。

在 Windows 10 预览体验版10.0.19000.0 之前的操作系统版本中，向桌面应用授予标识的唯一方式是将[其打包到已签名的 .msix 包中](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。 对于这些应用程序，将在包清单中指定标识，并根据清单中的信息，由 .MSIX 部署管道处理标识注册。 包清单中引用的所有内容都出现在 .MSIX 包中。

从 Windows 10 预览体验内部版本10.0.19000.0 开始，你可以通过在你的应用程序中生成和注册*稀疏包*，将包标识授予未打包在 .msix 包中的桌面应用程序。 此支持使无法采用 .MSIX 打包进行部署的桌面应用使用需要包标识的 Windows 10 扩展性功能。 有关更多背景信息，请参阅[此博客文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)。

若要生成并注册将包标识授予桌面应用的稀疏包，请执行以下步骤。

1. [为稀疏包创建包清单](#create-a-package-manifest-for-the-sparse-package)
2. [生成并签署稀疏包](#build-and-sign-the-sparse-package)
3. [将包标识元数据添加到桌面应用程序清单](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [在运行时注册稀疏包](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>重要概念

以下功能可让非打包桌面应用获取包标识。

### <a name="sparse-packages"></a>稀疏包

*稀疏包*包含包清单，但不包含其他应用程序二进制文件和内容。 稀疏包的清单可以在预先确定的外部位置引用包之外的文件。 这允许仍无法对整个应用采用 .MSIX 打包的应用程序，以获取某些 Windows 10 扩展性功能所需的包标识。

> [!NOTE]
> 使用稀疏包的桌面应用程序不会获得通过 .MSIX 包进行完全部署的一些优点。 这些优势包括：篡改保护、在锁定的位置安装，以及在部署、运行时间和卸载时由操作系统进行完全管理。

### <a name="package-external-location"></a>包外部位置

为了支持稀疏包，包清单架构现在支持[ **\<属性\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)元素下的可选 **\<AllowExternalContent\>** 元素。 这允许包清单在磁盘上的特定位置引用包之外的内容。

例如，如果你的现有非打包桌面应用程序在 C:\Program Files\MyDesktopApp 中安装了应用可执行文件和其他内容\, 你可以创建一个包含清单中的 **\<AllowExternalContent\>** 元素的稀疏包。 在应用的安装过程中或首次应用时，可以安装稀疏包，并将 C:\Program Files\MyDesktopApp\ 声明为应用将使用的外部位置。

## <a name="create-a-package-manifest-for-the-sparse-package"></a>为稀疏包创建包清单

在生成稀疏包之前，你必须首先创建一个[包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)（一个名为 appxmanifest.xml 的文件），该清单为你的桌面应用程序声明包标识元数据和其他所需的详细信息。 若要为稀疏包创建包清单，最简单的方法是使用下面的示例，并使用[架构引用](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)为应用程序自定义它。

确保包清单包含以下项：

* 一个[ **\<标识\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素，用于描述桌面应用的标识特性。
* [ **\<属性\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)元素下的 **\<AllowExternalContent\>** 元素。 应为此元素分配值 `true`，这允许包清单在磁盘上的特定位置引用包外的内容。 在后面的步骤中，你将在从安装程序或应用程序中运行的代码注册稀疏包时指定外部位置的路径。 在清单中引用的不在包本身中的任何内容都应安装到外部位置。
* [ **\<y\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)元素的**MinVersion**特性应设置为 `10.0.19000.0` 或更高版本。
* [ **\<应用程序\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的**TrustLevel = mediumIL**和**RuntimeBehavior = Win32App**属性声明与稀疏包关联的桌面应用程序的运行方式与标准未打包桌面应用程序类似，无需注册表和文件系统虚拟化以及其他运行时更改。

下面的示例显示稀疏包清单（Appxmanifest.xml）的完整内容。 此清单包含需要包标识的 `windows.sharetarget` 扩展。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>生成并签署稀疏包

创建包清单后，使用 Windows SDK 中的[makeappx.exe 工具](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool)生成稀疏包。 因为稀疏包不包含清单中引用的文件，所以必须指定 `/nv` 选项，该选项将跳过对包的语义验证。

下面的示例演示如何从命令行创建稀疏包。  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

在目标计算机上成功安装稀疏包之前，必须使用目标计算机上受信任的证书对其进行签名。 你可以创建新的自签名证书进行开发，并使用[SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool)为你的稀疏包签名，Windows SDK 中提供了此证书。

下面的示例演示如何从命令行对稀疏包进行签名。

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>将包标识元数据添加到桌面应用程序清单

还必须在桌面应用中包括[并行应用程序清单](https://docs.microsoft.com/windows/win32/sbscs/application-manifests)，并包含一个 **\<.msix\>** 元素，其中包含声明应用的标识特性的属性。 这些属性的值由 OS 用于在启动可执行文件时确定应用的标识。

下面的示例演示一个具有 **\<.msix\>** 元素的并行应用程序清单。

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

**\<.msix\>** 元素的属性必须与稀疏包的包清单中的这些值匹配：

* **PackageName**和**publisher**特性必须分别匹配包清单中[ **\<标识\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素中的**名称**和**发布服务器**特性。
* **ApplicationId**特性必须与包清单中[ **\<应用程序\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的**Id**特性匹配。

并行应用程序清单必须与桌面应用程序的可执行文件位于同一目录中，并且约定应与应用的可执行文件具有相同的名称，并将 `.manifest` 扩展名追加到该文件中。 例如，如果您的应用程序的可执行文件名称为 `ContosoPhotoStore`，则应该 `ContosoPhotoStore.exe.manifest`应用程序清单文件名。

## <a name="register-your-sparse-package-at-run-time"></a>在运行时注册稀疏包

若要将包标识授予桌面应用，应用必须使用[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)类注册稀疏包。 您可以向应用程序添加代码，以便在应用程序第一次运行时注册稀疏包，也可以在安装桌面应用程序时运行代码来注册包（例如，如果使用 MSI 来安装桌面应用程序，可以从自定义操作运行此代码）。

下面的示例演示如何注册稀疏包。 此代码将创建一个**AddPackageOptions**对象，该对象包含包清单可以引用包外内容的外部位置的路径。 然后，该代码将此对象传递给**PackageManager. AddPackageByUriAsync**方法，以注册稀疏包。 此方法还会将已签名的稀疏包的位置作为 URI 接收。 有关更完整的示例，请参阅相关[示例](#sample)中的 `StartUp.cs` 代码文件。

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>示例

有关演示如何使用稀疏包将包标识授予桌面应用的完全功能的示例应用，请参阅[https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages)。 [此博客文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)提供了有关生成和运行示例的详细信息。

此示例包含以下内容：

* 名为 PhotoStoreDemo 的 WPF 应用程序的源代码。 在启动过程中，应用程序将进行检查以确定它是否正在运行标识。 如果它没有以标识运行，则会注册稀疏包，然后重新启动应用程序。 有关执行这些步骤的代码，请参阅 `StartUp.cs`。
* 并行应用程序清单，名为 `PhotoStoreDemo.exe.manifest`。
* 名为 `AppxManifest.xml`的包清单。
