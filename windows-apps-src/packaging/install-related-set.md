---
author: laurenhughes
title: 使用应用安装程序文件安装相关集
description: 在此部分中，我们将查看允许通过应用安装程序来安装相关集所需执行的步骤。 我们还将完成相应的步骤以构建一个将定义相关集的 *.appinstaller 文件。
ms.author: lahugh
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.openlocfilehash: 4caf4333bb3d442779aedac2028b0996cbd17645
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6851794"
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>使用应用安装程序文件安装相关集

如果你刚开始使用 UWP 可选包或相关集，那么以下文章是开始入门的优良资源。 

1.  [使用可选包扩展你的应用程序](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [生成你的第一个可选包](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [从可选包加载代码](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [用于创建相关集的工具](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [可选包和相关集的创作](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

使用 Windows 10 Fall Creators Update，现在可以通过应用安装程序来安装相关集。 这样，可以向用户分发和部署相关集的应用包。 

## <a name="how-to-install-a-related-set"></a>如何安装相关集 
相关集不是一个实体，而是主要包和可选包的组合。 为了能够将相关集作为一个实体来安装，我们必须能够将主要包和可选包指定为一个整体。 若要执行此操作，我们将需要创建一个扩展名为“.appinstaller”的 XML 文件以定义相关集。 应用安装程序使用 *.appinstaller 文件，并允许用户通过一次单击来安装所有定义的包。 

在我们详述之前，下面给出了一个完整的 *.appinstaller 示例文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

在部署期间，会根据 `Uri` 元素中引用的应用包来验证应用安装程序文件。 因此，`Name`、`Publisher` 和 `Version` 应与应用程序包清单中的 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素匹配。 

## <a name="how-to-create-an-app-installer-file"></a>如何创建应用安装程序文件 

若要作为一个实体来分配相关集，你必须创建一个应用安装程序文件，其中包含该 [appinstaller 架构](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)所需的元素。

### <a name="step-1-create-the-appinstaller-file"></a>第 1 步：创建 *.appinstaller 文件
使用文本编辑器，创建一个文件（其中将包含 XML）并将其命名为 &lt;文件名&gt;.appinstaller 

### <a name="step-2-add-the-basic-template"></a>第 2 步：添加基本模板
基本模板包括应用安装程序文件信息。 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>第 3 步：添加主要包信息 
如果主应用包是.appxbundle 或.msixbundle 文件，则使用`<MainBundle>`如下所示。 如果主应用包是.appx 或.msix 文件，则使用`<MainPackage>`替代`<MainBundle>`的代码片段中。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
`<MainBundle>` 或 `<MainPackage>` 属性中的信息应该分别与应用程序包清单或应用包清单中的 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素匹配。 

### <a name="step-4-add-the-optional-packages"></a>第 4 步：添加可选包 
类似于主应用包属性，如果可选包可以是应用包或应用程序包，则 `<OptionalPackages>` 属性中的子元素应该分别是 `<Package>` 或 `<Bundle>`。 子元素中的包信息应该与程序包或程序包清单中的 Identity 元素相匹配。 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>第 5 步：添加依赖项 
在依赖项元素中，你可以指定主要包或可选包所需的框架包。 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>第 6 步：添加更新设置 
应用安装程序文件还可以指定更新设置，以便在发布较新的应用安装程序文件时可以自动更新相关集。 **<UpdateSettings>** 是可选元素。 在 **<UpdateSettings>** 中，OnLaunch 选项指定应在应用启动时进行更细你检查，HoursBetweenUpdateChecks="12" 则指定应每隔 12 小时执行更新检查。 如果未指定 HoursBetweenUpdateChecks，则用于检查更新的默认时间间隔为 24 小时。
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

有关 XML 架构的所有详细信息，请参阅[应用安装程序文件参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)。

> [!NOTE]
> 
> 应用安装程序文件类型是 Windows 10 Fall Creators Update 中的新类型。 在以前版本的 Windows 10 上，不支持使用应用安装程序文件来部署 UWP 应用。
> 还应注意的是，**HoursBetweenUpdateChecks** 元素是 Windows 10 的下一个主要更新中的新增功能。
