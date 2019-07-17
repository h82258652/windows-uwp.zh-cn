---
title: 设置 UWP 应用的自动生成
description: 如何配置自动生成以生成旁加载和/或应用商店程序包。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5837674f2cb20710a59eeac0af59498bf28b197e
ms.sourcegitcommit: a86d0bd1c2f67e5986cac88a98ad4f9e667cfec5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68229381"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>设置 UWP 应用的自动生成

Azure 管道可用于创建 UWP 项目的自动的生成。 在本文中，我们将介绍多种不同方式执行此操作。 我们将还展示你如何执行这些任务使用命令行，以便您可以与任何其他生成系统集成。

## <a name="create-a-new-azure-pipeline"></a>创建新的 Azure 管道

首先[注册 Azure 管道](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)如果尚未这样做已经。

接下来，创建的管道，可用于生成源代码。 有关生成管道生成的 GitHub 存储库的教程，请参阅[创建第一个管道](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)。 Azure 管道支持列出的存储库类型[这篇文章中](https://docs.microsoft.com/azure/devops/pipelines/repos)。

## <a name="set-up-an-automated-build"></a>设置自动生成

我们将开始 UWP 生成现已推出 Azure 开发运营的定义以及然后演示如何配置管道的默认值。

在生成定义模板列表中，选择**通用 Windows 平台**模板。

![选择 UWP 模板](images/select-yaml-template.png)

此模板包括要生成 UWP 项目的基本配置：

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

默认模板会尝试使用在.csproj 文件中指定的证书对包进行签名。 如果你想要在生成过程中登录您的包必须具有访问私钥。 否则，禁用签名通过添加参数`/p:AppxPackageSigningEnabled=false`到`msbuildArgs`YAML 文件中的部分。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>将你项目的证书添加到安全文件库

应避免提交到存储库如有可能，证书和 git 将其忽略默认情况下。 若要管理安全处理的敏感文件与证书一样，Azure DevOps 支持[保护文件](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)。

若要上传在自动生成的证书：

1. 在 Azure 管道中，展开**管道**在导航窗格中单击**库**。
2. 单击**保护的文件**选项卡，然后单击 **+ 安全文件**。

    ![如何上传安全文件](images/secure-file1.png)

3. 浏览到证书文件，然后单击**确定**。
4. 上传证书后，选择它以查看其属性。 下**管道的权限**，启用**以便在所有管道中使用 Authorize**切换。

    ![如何上传安全文件](images/secure-file2.png)

## <a name="configure-the-build-solution-build-task"></a>配置生成解决方案生成任务

此任务将编译到二进制文件在工作文件夹中，并产生输出应用包文件的任何解决方案。
此任务使用 MSBuild 参数。 你必须指定这些参数的值。 使用下表作为指南。

|**MSBuild 参数**|**ReplTest1**|**说明**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定义要存储生成的项目的文件夹。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 可以定义要包含在绑定中的平台。 |
| AppxBundle | Always | 使用指定的平台的.msix/.appx 文件创建.msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 生成.msixupload/.appxupload 文件和 **_Test**文件夹以进行旁加载。 |
| UapAppxPackageBuildMode | CI | 生成仅.msixupload/.appxupload 文件。 |
| UapAppxPackageBuildMode | SideloadOnly | 将生成 **_Test**仅旁加载的文件夹。 |
| AppxPackageSigningEnabled | true | 启用包签名。 |
| PackageCertificateThumbprint | 证书指纹 | 此值**必须**匹配中的签名证书的指纹，或为空字符串。 |
| PackageCertificateKeyFile | Path | 指向要使用的证书的路径。 这是从安全的文件元数据检索。 |

### <a name="configure-the-build"></a>配置生成

如果你想要使用命令行中，或使用任何其他生成系统生成您的解决方案，请使用这些自变量运行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>配置包签名

MSIX （或 APPX） 程序包进行签名管道需要检索签名证书。 若要执行此操作，添加 DownloadSecureFile 任务之前 VSBuild 任务。
这将授予你访问的签名证书通过```signingCert```。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

接下来，更新引用的签名证书的 VSBuild 任务：

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
> 有意将 PackageCertificateThumbprint 参数设置为空字符串作为预防措施。 如果指纹在项目中设置，但不匹配的签名证书，生成将失败并出现错误： `Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>检查参数

使用定义的参数`$()`语法是在生成定义中定义的变量，将在其他的更改生成系统。

![默认变量](images/building-screen5.png)

若要查看所有预定义的变量，请参阅[预定义生成变量](https://docs.microsoft.com/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>配置发布生成项目任务

默认 UWP 管道不会保存生成的项目。 若要将发布功能添加到你的 YAML 定义，添加以下任务。

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

您可以看到在生成的产物**项目**选项生成的结果页。

![项目](images/building-screen6.png)

因为我们设置了`UapAppxPackageBuildMode`自变量`StoreUpload`，项目文件夹包括提交到应用商店 (.msixupload/.appxupload) 的包。 请注意，您还可以提交常规应用包 (.msix/.appx) 或应用程序捆绑 (.msixbundle/.appxbundle/) 到存储区。 在本文中，我们将使用 .appxupload 文件。

## <a name="address-bundle-errors"></a>处理绑定错误

如果将多个 UWP 项目添加到你的解决方案，然后尝试创建捆绑包时，可能会收到如下错误。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

出现此错误是因为，在解决方案级别上，哪个应用应出现在程序包中不明确。 若要解决此问题，请打开每个项目文件，并在第一个的末尾添加以下属性`<PropertyGroup>`元素。

|**项目**|**属性**|
|-------|----------|
|应用|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然后，删除`AppxBundle`MSBuild 参数，从生成步骤。

## <a name="related-topics"></a>相关主题

- [为 Windows 构建.NET 应用程序](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [打包 UWP 应用](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [在 Windows 10 中的旁加载 LOB 应用](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [创建进行包签名证书](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
