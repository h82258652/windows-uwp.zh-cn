---
Description: 了解如何向未打包的桌面应用授予标识，实现在这些应用中使用新式 Windows 10 功能。
title: 向未打包的桌面应用授予标识
ms.date: 02/28/2020
ms.topic: article
keywords: Windows 10, 桌面, 包, 标识, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d997c6109256974f17bc0f86a518e34ef55960a7
ms.sourcegitcommit: ecd7bce5bbe15e72588937991085dad6830cec71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "81224270"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>向未打包的桌面应用授予标识

许多 Windows 10 可扩展性功能需要在非 UWP 桌面应用中使用[程序包标识符](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)，包括后台任务、通知、动态磁贴和共享目标。 对于这些情况，OS 需要使用标识来识别相应 API 的调用方。

在 Windows 10 版本 2004 之前的 OS 发行版中，向桌面应用授予标识符的唯一方式是[将该应用打包到已签名的 MSIX 包中](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。 对于这些应用，将在程序包清单中指定标识，并由 MSIX 部署管道根据清单中的信息处理标识注册。 程序包清单中引用的所有内容都存在于 MSIX 包中。

从 Windows 10 版本 2004 开始，可以通过生成稀疏包并将其注册到应用  ，向未打包到 MSIX 包中的桌面应用授予包标识符。 通过此支持，尚不能采用 MSIX 打包方式进行部署的桌面应用可以使用需要程序包标识符的 Windows 10 可扩展性功能。 有关更多背景信息，请参阅[此博客文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)。

要生成并注册稀疏包以向桌面应用授予程序包标识符，请按照以下步骤操作。

1. [为稀疏包创建程序包清单](#create-a-package-manifest-for-the-sparse-package)
2. [生成稀疏包并为稀疏包签名](#build-and-sign-the-sparse-package)
3. [将程序包标识符元数据添加到桌面应用程序清单](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [在运行时注册稀疏包](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>重要概念

以下功能可使非打包桌面应用获取程序包标识符。

### <a name="sparse-packages"></a>稀疏包

稀疏包  包含一个程序包清单，但不包含其他应用二进制文件和内容。 稀疏包的清单可以在预定的外部位置引用包外部的文件。 这使仍无法对整个应用采用 MSIX 打包的应用可以按照某些 Windows 10 可扩展性功能的要求获取程序包标识符。

> [!NOTE]
> 使用稀疏包的桌面应用无法获得通过 MSIX 包进行完全部署的一些优势。 这些优势包括：防篡改、在锁定位置安装，以及在部署、运行时间和卸载时由 OS 进行全面管理。

### <a name="package-external-location"></a>包外部位置

为了支持稀疏包，程序包清单架构现在在 [\<Properties\>  ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 元素下支持可选的 \<AllowExternalContent\>  元素。 这使程序包清单可以在磁盘上的特定位置引用包外部的内容。

例如，如果你的现有非打包桌面应用在 C:\Program Files\MyDesktopApp\, 中安装应用可执行文件和其他内容，则可以创建一个在清单中包含 \<AllowExternalContent\>  元素的稀疏包。 在应用安装过程中或第一次安装应用时，可以安装稀疏包并将 C:\Program Files\MyDesktopApp\ 声明为应用将使用的外部位置。

## <a name="create-a-package-manifest-for-the-sparse-package"></a>为稀疏包创建程序包清单

在生成稀疏包之前，必须先创建一个[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)（一个名为 AppxManifest.xml 的文件），该文件声明桌面应用的程序包标识符元数据和其他所需的详细信息。 为稀疏包创建程序包清单的最简单方法是使用以下示例，并使用[架构引用](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)为应用自定义它。

确保程序包清单包含以下项：

* 一个 [\<Identity\>  ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素，用于描述桌面应用的标识属性。
* [\<Properties\>  ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 元素下的 \<AllowExternalContent\>  元素。 应为此元素分配 `true` 值，该值允许程序包清单在磁盘上的特定位置引用包外部的内容。 在后面的步骤中，在从安装程序或应用中运行的代码注册稀疏包时，将指定外部位置的路径。 在清单中引用的不属于包本身的所有内容都应安装到外部位置。
* 应将 [\<TargetDeviceFamily **\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 元素的 MinVersion  属性设置为 `10.0.19000.0` 或更高版本。
* [\<Application\>  ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素的 TrustLevel=mediumIL  和 RuntimeBehavior=Win32App  属性声明与稀疏包关联的桌面应用的运行方式与标准未打包桌面应用类似，无需注册表和文件系统虚拟化以及其他运行时更改。

下面的示例展示稀疏程序包清单 (AppxManifest.xml) 的完整内容。 此清单包含需要程序包标识符的 `windows.sharetarget` 扩展。

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

## <a name="build-and-sign-the-sparse-package"></a>生成稀疏包并为稀疏包签名

创建程序包清单后，请使用 Windows SDK 中的 [MakeAppx.exe 工具](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool)生成稀疏包。 由于稀疏包不包含清单中引用的文件，因此必须指定 `/nv` 选项，该选项将跳过对包的语义验证。

下面的示例演示如何从命令行创建稀疏包。  

```Console
MakeAppx.exe pack /d <path to directory that contains manifest> /p <output path>\MyPackage.msix /nv
```

在目标计算机上成功安装稀疏包之前，必须使用目标计算机上受信任的证书对其进行签名。 可以出于开发目的创建新的自签名证书，并使用 Windows SDK 中提供的 [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool) 为稀疏包签名。

下面的示例演示如何从命令行对稀疏包进行签名。

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx /p <certificate password> <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>将程序包标识符元数据添加到桌面应用程序清单

还必须在桌面应用中添加[并行应用程序清单](https://docs.microsoft.com/windows/win32/sbscs/application-manifests)，并添加一个 [&lt;msix&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) 元素，该元素的属性声明应用的标识属性。 OS 将使用这些属性的值在启动可执行文件时确定应用的标识。

下面的示例展示了包含 \<msix\>  元素的并行应用程序清单。

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

\<msix\>  元素的属性必须与稀疏包的程序包清单中的以下值匹配：

* packageName  和 publisher  属性必须分别与程序包清单中 [\<Identity\>  ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素中的 Name  和 Publisher  属性相匹配。
* applicationId  属性必须分别与程序包清单中 [\<Application\>  ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素的 Id  属性相匹配。

并行应用程序清单必须与桌面应用的可执行文件位于同一目录中，并且约定应与应用的可执行文件具有相同的名称，并带有 `.manifest` 扩展名。 例如，如果应用的可执行文件名为 `ContosoPhotoStore`，则应用程序清单文件名应为 `ContosoPhotoStore.exe.manifest`。

## <a name="register-your-sparse-package-at-run-time"></a>在运行时注册稀疏包

若要向桌面应用授予包标识符，应用必须使用 [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) 类的 AddPackageByUriAsync  方法注册稀疏包。 从 Windows 10 版本 2004 开始已提供此方法。 可以在首次运行应用时向应用添加代码以注册稀疏包，也可以在安装桌面应用时运行代码以注册该稀疏包（例如，如果你使用 MSI 安装桌面应用，可以通过自定义操作运行此代码）。

下面的示例演示如何注册稀疏包。 此代码创建一个 AddPackageOptions  对象，该对象包含程序包清单可以引用包外部内容的外部位置的路径。 然后，代码将此对象传递给 AddPackageByUriAsync  方法以注册稀疏包。 此方法还以 URI 形式接收已签名的稀疏包的位置。 有关更完整的示例，请参阅相关[示例](#sample)的 `StartUp.cs` 代码文件。

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

有关演示如何使用稀疏包向桌面应用授予程序包标识符的功能齐全的示例应用，请参阅 [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages)。 [此博客文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)中提供了有关生成和运行示例的详细信息。

此示例包含以下内容：

* 名为 PhotoStoreDemo 的 WPF 应用的源代码。 在启动过程中，应用将检查并确定它是否使用标识运行。 如果它没有使用标识运行，则会注册稀疏包，然后重新启动应用。 有关执行这些步骤的代码，请参阅 `StartUp.cs`。
* 名为 `PhotoStoreDemo.exe.manifest` 的并行应用程序清单。
* 名为 `AppxManifest.xml` 的程序包清单。
