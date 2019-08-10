---
title: 设置 UWP 应用的自动生成
description: 如何配置自动生成以生成旁加载和/或应用商店程序包。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 08ad21d3ddc73499bb2b97b300e635fe0a6c148d
ms.sourcegitcommit: 698a86640b365dc1ca772fb6f53ca556dc284ed6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935778"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>设置 UWP 应用的自动生成

您可以使用 Azure Pipelines 创建 UWP 项目的自动生成。 本文介绍了执行此操作的不同方式。 我们还将演示如何使用命令行执行这些任务, 以便可以与任何其他生成系统集成。

## <a name="create-a-new-azure-pipeline"></a>创建新的 Azure 管道

首先[注册 Azure Pipelines (](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)如果尚未这样做)。

接下来, 创建可用于生成源代码的管道。 有关生成用于构建 GitHub 存储库的管道的教程, 请参阅[创建第一个管道](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)。 Azure Pipelines 支持[本文中](https://docs.microsoft.com/azure/devops/pipelines/repos)列出的存储库类型。

## <a name="set-up-an-automated-build"></a>设置自动生成

首先, 我们将使用 Azure Dev Ops 提供的默认 UWP 生成定义, 并向您展示如何配置管道。

在生成定义模板列表中，选择**通用 Windows 平台**模板。

![选择 UWP 模板](images/select-yaml-template.png)

此模板包含用于构建 UWP 项目的基本配置:

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

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

默认模板尝试用 .csproj 文件中指定的证书对包进行签名。 如果要在生成过程中对包进行签名, 则必须有权访问私钥。 否则, 可以通过将参数`/p:AppxPackageSigningEnabled=false`添加到 YAML 文件的`msbuildArgs`部分来禁用签名。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>将项目证书添加到安全文件库

应尽量避免将证书提交到存储库 (如果可能), 并且默认情况下, git 将忽略它们。 为了管理敏感文件 (如证书) 的安全处理, Azure DevOps 支持[安全文件](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)功能。

为自动生成上载证书:

1. 在 Azure Pipelines 中, 展开导航窗格中的 "**管道**", 然后单击 "**库**"。
2. 单击 "**安全文件**" 选项卡, 然后单击 " **+ Secure file**"。

    ![如何上传安全文件](images/secure-file1.png)

3. 浏览到证书文件, 然后单击 **"确定"** 。
4. 上载证书后, 选择它以查看其属性。 在 "**管道权限**" 下, 启用 "**授权在所有管道中使用**" 切换。

    ![如何上传安全文件](images/secure-file2.png)

5. 如果证书中的私钥具有密码, 则建议你将密码存储在[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) , 然后将密码链接到[变量组](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 您可以使用该变量来访问管道中的密码。 请注意, 私钥仅支持密码;当前不支持使用本身受密码保护的证书文件。

> [!NOTE]
> 从 Visual Studio 2019 开始, 不会再在 UWP 项目中生成临时证书。 若要创建或导出证书, 请使用[本文](/windows/msix/package/create-certificate-package-signing)中介绍的 PowerShell cmdlet。

## <a name="configure-the-build-solution-build-task"></a>配置生成解决方案生成任务

此任务将工作文件夹中的任何解决方案编译为二进制文件, 并生成输出应用包文件。 此任务使用 MSBuild 参数。 你必须指定这些参数的值。 使用下表作为指南。

|**MSBuild 参数**|**ReplTest1**|**说明**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定义要存储生成的项目的文件夹。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 使您能够定义要包含在捆绑中的平台。 |
| AppxBundle | Always | 使用指定平台的. .msix/.appx 文件创建 .msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 生成 msixupload/.appxupload 文件和 **_Test**文件夹以进行旁加载。 |
| UapAppxPackageBuildMode | CI | 仅生成 msixupload/.appxupload 文件。 |
| UapAppxPackageBuildMode | SideloadOnly | 只为旁加载生成 **_Test**文件夹。 |
| AppxPackageSigningEnabled | true | 启用包签名。 |
| PackageCertificateThumbprint | 证书指纹 | 此值**必须**与签名证书中的指纹匹配, 或为空字符串。 |
| PackageCertificateKeyFile | Path | 要使用的证书的路径。 这是从安全文件元数据中检索到的。 |
| PackageCertificatePassword | 密码 | 证书中的私钥的密码。 建议在[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates)中存储密码, 并将密码链接到[变量组](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 可以将变量传递给此参数。 |

### <a name="configure-the-build"></a>配置生成

如果要使用命令行或使用任何其他生成系统来生成解决方案, 请使用这些参数运行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>配置包签名

若要对 .MSIX (或 APPX) 包进行签名, 管道需要检索签名证书。 为此, 请在 VSBuild 任务之前添加 DownloadSecureFile 任务。
这将允许你通过```signingCert```访问签名证书。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

接下来, 更新 VSBuild 任务以引用签名证书:

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
> PackageCertificateThumbprint 参数被有意设置为空字符串, 作为预防措施。 如果在项目中设置指纹但不匹配签名证书, 则生成将失败, 并出现以下错误: `Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>查看参数

用`$()`语法定义的参数是在生成定义中定义的变量, 将在其他生成系统中发生更改。

![默认变量](images/building-screen5.png)

若要查看所有预定义的变量, 请参阅[预定义的生成变量](https://docs.microsoft.com/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>配置 "发布生成项目" 任务

默认 UWP 管道不保存生成的项目。 若要向 YAML 定义添加发布功能, 请添加以下任务。

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

可以在 "生成结果" 页的 "**项目**" 选项中查看生成的项目。

![项目](images/building-screen6.png)

由于我们已将`UapAppxPackageBuildMode`参数设置为`StoreUpload`, 因此项目文件夹包含用于提交到应用商店的包 (. msixupload/. .appxupload)。 请注意, 还可以将常规应用包 (. .msix/.appx) 或应用捆绑包 (. .msixbundle/.appxbundle/) 提交到存储区。 在本文中，我们将使用 .appxupload 文件。

## <a name="address-bundle-errors"></a>地址绑定错误

如果将多个 UWP 项目添加到解决方案中, 然后尝试创建捆绑包, 可能会收到类似于下面的错误。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

出现此错误是因为，在解决方案级别上，哪个应用应出现在程序包中不明确。 若要解决此问题, 请打开每个项目文件, 并在第一个`<PropertyGroup>`元素的末尾添加以下属性。

|**投影**|**属性**|
|-------|----------|
|应用|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然后, 删除生成`AppxBundle`步骤中的 MSBuild 参数。

## <a name="related-topics"></a>相关主题

- [生成适用于 Windows 的 .NET 应用](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)
- [Windows 10 中的旁加载 LOB 应用](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [为包签名创建证书](/windows/msix/package/create-certificate-package-signing)
