---
title: 设置 UWP 应用的自动生成
description: 如何配置自动生成以生成旁加载和/或应用商店程序包。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 61525e2a4a088e37184bb93526722e0bf23fbd56
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319811"
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

## <a name="add-your-project-certificate-to-a-repository"></a>将你项目的证书添加到存储库

管道适用于 Azure 存储库的 Git 和 TFVC 存储库。 如果使用 Git 存储库，请将项目的证书文件添加到存储库，以便生成代理可以对应用包签名。 如果不执行此操作，则 Git 存储库将忽略证书文件。 若要将证书文件添加到你的存储库中，右键单击中的证书文件**解决方案资源管理器**，然后在快捷菜单中，选择**将忽略文件添加到源代码管理**命令。

![如何包含证书](images/building-screen1.png)

## <a name="configure-the-build-solution-build-task"></a>配置生成解决方案生成任务

此任务将编译到二进制文件在工作文件夹中，并产生输出应用包文件的任何解决方案。
此任务使用 MSBuild 参数。 你必须指定这些参数的值。 使用下表作为指南。

|**MSBuild 参数**|**ReplTest1**|**说明**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定义要存储生成的项目的文件夹。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 可以定义要包含在绑定中的平台。 |
| AppxBundle | 始终 | 使用指定的平台的.msix/.appx 文件创建.msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 生成.msixupload/.appxupload 文件和 **_Test**文件夹以进行旁加载。 |
| UapAppxPackageBuildMode | CI | 生成仅.msixupload/.appxupload 文件。 |
| UapAppxPackageBuildMode | SideloadOnly | 将生成 **_Test**文件夹以进行旁加载只 |

如果你想要使用命令行中，或使用任何其他生成系统生成您的解决方案，请使用这些自变量运行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

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

|**Project**|**属性**|
|-------|----------|
|应用|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然后，删除`AppxBundle`MSBuild 参数，从生成步骤。

## <a name="related-topics"></a>相关主题

- [为 Windows 构建.NET 应用程序](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [打包 UWP 应用](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [在 Windows 10 中的旁加载 LOB 应用](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [创建进行包签名证书](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
