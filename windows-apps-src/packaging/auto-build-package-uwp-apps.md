---
title: 设置 UWP 应用的自动生成
description: 如何将自动生成配置为生成旁加载和/或存储包。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 70415c9f3d58625cfdc651ec67c8a9f37c23cffa
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "77089493"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>设置 UWP 应用的自动生成

可以使用 Azure Pipelines 为 UWP 项目创建自动化生成。 在本文中，我们将讨论执行该操作的方法。 此外，介绍如何使用命令行执行这些任务，以便可与任何其他生成系统相集成。

## <a name="create-a-new-azure-pipeline"></a>创建新的 Azure 管道

首先[注册 Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)（如果尚未这样做）。

接下来，创建一个可用于生成源代码的管道。 有关生成一个用于生成 GitHub 存储库的管道的教程，请参阅[创建第一个管道](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)。 Azure Pipelines 支持[此文](https://docs.microsoft.com/azure/devops/pipelines/repos)中列出的存储库类型。

## <a name="set-up-an-automated-build"></a>设置自动生成

首先，我们将使用 Azure Dev Ops 中提供的默认 UWP 生成定义，并向你展示如何配置管道。

在生成定义模板列表中，选择“通用 Windows 平台”  模板。

![选择 UWP 模板](images/select-yaml-template.png)

此模板包含用于生成 UWP 项目的基本配置：

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

默认模板尝试用 .csproj 文件中指定的证书对包进行签名。 如果要在生成过程中对包进行签名，必须有权访问私钥。 否则，可以在 YAML 文件中将参数 `/p:AppxPackageSigningEnabled=false` 添加到 `msbuildArgs` 部分，以禁用签名。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>将项目证书添加到安全文件库

应尽量避免将证书提交到存储库，Git 默认会忽略这些证书。 为了管理敏感文件（例如证书）的安全处理，Azure DevOps 支持[安全文件](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)功能。

若要为自动生成上传证书：

1. 在 Azure Pipelines 中，展开导航窗格中的“管道”并单击“库”。  
2. 依次单击“安全文件”选项卡、“+ 安全文件”。  

    ![如何上传安全文件](images/secure-file1.png)

3. 浏览到证书文件并单击“确定”。 
4. 上传证书后，选择该证书以查看其属性。 在“管道权限”下，将“授权在所有管道中使用”切换开关置于启用状态。  

    ![如何上传安全文件](images/secure-file2.png)

5. 如果证书中的私钥包含密码，则我们建议将密码存储在 [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) 中，然后将密码链接到某个[变量组](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 可以使用该变量来从管道访问密码。 请注意，只有私钥支持密码；当前不支持使用本身受密码保护的证书文件。

> [!NOTE]
> 从 Visual Studio 2019 开始，不再在 UWP 项目中生成临时证书。 若要创建或导出证书，请使用[此文](/windows/msix/package/create-certificate-package-signing)中所述的 PowerShell cmdlet。

## <a name="configure-the-build-solution-build-task"></a>配置生成解决方案生成任务

此任务将工作文件夹中的任何解决方案编译为二进制文件，并生成输出应用包文件。 此任务使用 MSBuild 参数。 你必须指定这些参数的值。 使用下表作为指南。

|**MSBuild 参数**|**值**|**描述**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定义要存储生成的项目的文件夹。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 用于定义要包含在捆绑包中的平台。 |
| AppxBundle | 始终 | 使用 .msix/.appx 文件为指定的平台创建 .msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 生成要旁加载的 .msixupload/.appxupload 文件和 **_Test** 文件夹。 |
| UapAppxPackageBuildMode | CI | 仅生成 .msixupload/.appxupload 文件。 |
| UapAppxPackageBuildMode | SideloadOnly | 仅生成要旁加载的 **_Test** 文件夹。 |
| AppxPackageSigningEnabled | true | 启用包签名。 |
| PackageCertificateThumbprint | 证书指纹 | 此值**必须**与签名证书中的指纹匹配，或者为空字符串。 |
| PackageCertificateKeyFile | 路径 | 要使用的证书的路径。 此值是从安全文件元数据中检索的。 |
| PackageCertificatePassword | 密码 | 证书中私钥的密码。 建议将密码存储在 [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) 中，并将密码链接到[变量组](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 可将变量传递到此参数。 |

### <a name="configure-the-build"></a>配置生成

如果需要通过使用命令行或使用任何其他生成系统生成解决方案，请使用这些参数运行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>配置包签名

若要为 MSIX（或 .appx）包签名，管道需要检索签名证书。 为此，请在 VSBuild 任务的前面添加 DownloadSecureFile 任务。
这样，就可以通过 ```signingCert``` 访问签名证书。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

接下来，更新 VSBuild 任务以引用签名证书：

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> PackageCertificateThumbprint 参数被有意设置为空字符串，目的是引以注意。 如果在项目中设置指纹，但该指纹与签名证书不匹配，则生成将会失败并出现错误：`Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>查看参数

使用 `$()` 语法定义的参数是在生成定义中定义的变量，在其他生成系统中将会更改。

![默认变量](images/building-screen5.png)

若要查看所有预定义的变量，请参阅[预定义的生成变量](https://docs.microsoft.com/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>配置“发布生成项目”任务

默认的 UWP 管道不会保存生成的项目。 若要将发布功能添加到 YAML 定义，请添加以下任务。

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

可以在生成结果页的“项目”选项中查看生成的项目。 

![项目](images/building-screen6.png)

由于我们已将 `UapAppxPackageBuildMode` 属性设置为 `StoreUpload`，项目文件夹将包含提交到应用商店的程序包 (.msixupload/.appxupload)。 请注意，还可以将常规应用包 (.msix/.appx) 或应用程序包 (.msixbundle/.appxbundle/) 提交到应用商店。 在本文中，我们将使用 .appxupload 文件。

## <a name="address-bundle-errors"></a>地址绑定错误

如果将多个 UWP 项目添加到解决方案，然后尝试创建程序包，则可能收到如下错误。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

出现此错误是因为，在解决方案级别上，哪个应用应出现在程序包中不明确。 若要解决此问题，请打开每个项目文件，并在第一个 `<PropertyGroup>` 元素的末尾添加以下属性。

|**项目**|**属性**|
|-------|----------|
|应用|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然后，从生成步骤中删除 `AppxBundle` MSBuild 参数。

## <a name="related-topics"></a>相关主题

- [生成适用于 Windows 的 .NET 应用](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)
- [在 Windows 10 中旁加载 LOB 应用](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [为包签名创建证书](/windows/msix/package/create-certificate-package-signing)
