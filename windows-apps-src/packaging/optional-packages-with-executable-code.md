---
author: laurenhughes
title: 包含可执行代码的可选包
description: 了解如何使用 Visual Studio 创建包含可执行代码的可选包。
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.openlocfilehash: f5660649b6f82135cdb45a8678a3f871a0f5e61d
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "5162493"
---
# <a name="optional-packages-with-executable-code"></a>包含可执行代码的可选包
 
包含可执行代码的可选包对于划分大型或复杂的应用，或者添加到已发布的应用很有用。 使用 Visual Studio 2017 版本 15.7 和 .NET Native 2.1，你可以从 C++ 和 C# 可选包加载可执行代码。

## <a name="prerequisites"></a>必备软件
- Visual Studio 2017，版本 15.7
- Windows 10 版本 1709
- Windows 10，版本 1709 SDK

若要获取最新的开发工具，请参阅[适用于 Windows 10 的下载和工具](https://developer.microsoft.com/windows/downloads)。 

> [!NOTE]
> 若要将使用可选包和/或相关集的应用提交到 Microsoft Store，你需要权限。 如果未将可选包和相关集提交到 Microsoft Store，则无需开发人员中心权限即可以将它们用于业务线 (LOB) 或企业应用。 请参阅 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)，获取提交使用可选包和相关集的应用的权限。

## <a name="c-optional-packages-with-executable-code"></a>包含可执行代码的 C++ 可选包

若要从 C++ 可选包加载代码，请参阅 GitHub 上的 [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) 存储库。 [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL) 展示如何使用可从主要包执行的代码创建项目。 MyMainApp 项目演示如何从 OptionalPackageDLL.dll 文件[加载代码](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182)。

## <a name="c-optional-packages-with-executable-code"></a>包含可执行代码的 C# 可选包

若要开始生成使用 C# 的可选代码包，请按照以下步骤来配置你的解决方案：

1. 创建新 UWP 应用程序，将最低版本设置为 Windows 10 Fall Creators Update SDK（版本 16299）或更高版本。

2. 将新的**可选代码包（通用 Windows）** 项目添加到解决方案。 确保**最低版本**和**目标版本**与主应用相匹配。

3. 如果你计划将应用提交到 Microsoft Store，右键单击两个项目并选择 **Microsoft Store -> 将应用与 Microsoft Store 关联…**

4. 打开主应用的 `Package.appxmanifest` 文件并查找 `Identity Name` 值。 记下此值以供下一步使用。

5. 打开可选应用包的 `Package.appxmanifest` 文件并查找 `uap3:MainAppPackageDependency Name` 值。 更新 `uap3:MainAppPackageDependency Name` 值以匹配上一步中主应用包的 `Identity Name` 值。 

    以下是主应用的 `Package.appxmanifest` 的 `Identity` 示例。
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    可选应用包的 `uap3:MainPackageDependency` 需要更新以匹配主应用的 `Identity`。
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. 将 `Bundle.mapping.txt` 文件添加到主应用。 请按照此[相关集](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets)部分的步骤创建包含两个应用的相关集。 

7. 生成可选包项目，然后导航到生成输出中的包参考文件夹，位于 `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference`。 请注意，你可以在参考文件夹路径中选择任何体系结构，因为 `.winmd` 文件（步骤 8）是体系结构独立文件。

8. 将主应用项目的参考添加到在此文件夹中找到的 `.winmd` 文件。 每次更改可选包项目中的 API 图面区域时，此 `.winmd` 文件**必须**进行更新。 此参考为主应用项目提供要编译的必要信息。

9. 在主应用项目中，导航到项目生成属性，然后选择**使用 .NET Native 工具链编译**。 目前，仅 .NET Native 中的调试支持在 C# 中创建可选代码包。 转到项目调试属性，然后选择**部署可选包**。 这将确保不论你何时部署主应用项目，这两个包均保持同步。

完成这些步骤后，你可以向可选包项目添加代码，就像托管的 WinRT 组件项目一样。 若要访问主应用项目中的代码，调用可选包项目中公开的公共方法。