---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 MSIX 包以及将其他现代组件合并到 WPF 应用中。
title: 将 Contoso Expenses 应用迁移到 .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 9d08dd0f43f1c505124203028c69326e10eea26c
ms.sourcegitcommit: 6cdba316bdbd85a2429259ebfb59ff94440e234a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85882882"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>第 1 部分：将 Contoso Expenses 应用迁移到 .NET Core 3

本文是演示如何现代化名为 Contoso Expenses 的示例 WPF 桌面应用的教程的第 1 部分。 有关本教程的概述、先决条件以及有关下载示例应用的说明，请参阅[教程：现代化 WPF 应用](modernize-wpf-tutorial.md)。
  
在本教程部分，你要将整个 Contoso Expenses 应用从 .NET Framework 4.7.2 迁移到 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3)。 在开始学习本教程部分之前，请确保在 Visual Studio 2019 中[打开并生成 ContosoExpenses 示例](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)。

> [!NOTE]
> 有关将 WPF 应用程序从 .NET Framework 迁移到 .NET Core 3 的详细信息，请参阅[此博客系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>将 ContosoExpenses 项目迁移到 .NET Core 3

在本部分，你要将 Contoso Expenses 应用中的 ContosoExpenses 项目迁移到 .NET Core 3。 为此，你将创建一个新项目文件，其中包含的文件与现有 ContosoExpenses 项目的文件相同，但该项目文件面向 .NET Core 3 而不是 .NET Framework 4.7.2。 这样，便可以同时使用应用的 .NET Framework 和 .NET Core 版本来维护单个解决方案。

1. 验证 ContosoExpenses 项目当前是否面向 .NET Framework 4.7.2。 在解决方案资源管理器中右键单击“ContosoExpenses”项目，选择“属性”，确认“应用程序”选项卡上的“目标框架”属性是否设置为 .NET Framework 4.7.2。   

    ![项目的 .NET Framework 版本 4.7.2](images/wpf-modernize-tutorial/NETFramework472.png)

3. 在 Windows 资源管理器中导航到“C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses”文件夹，创建名为“ContosoExpenses.Core.csproj”的新文本文件。 

4. 右键单击该文件，选择“打开方式”，然后在所选的文本编辑器（例如记事本、Visual Studio Code 或 Visual Studio）中将其打开。

5. 将以下文本复制到该文件，然后保存该文件。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 关闭该文件并返回到 Visual Studio 中的“ContosoExpenses”解决方案。

7. 右键单击“ContosoExpenses”解决方案，并选择“添加”->“现有项目”。  选择刚刚在 `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` 文件夹中创建的“ContosoExpenses.Core.csproj”文件，将其添加到解决方案中。

ContosoExpenses.Core.csproj 包含以下元素：

* Project 元素指定某个 SDK 版本的 Microsoft.NET.Sdk.WindowsDesktop。  此版本引用 Windows 桌面版的 .NET 应用程序，包含 WPF 和 Windows 窗体应用的组件。
* PropertyGroup 元素包含用于指示项目输出是可执行文件（而不是 DLL）的子元素，它面向 .NET Core 3 并使用 WPF。 对于 Windows 窗体应用，请使用 UseWinForms 元素而不是 UseWPF 元素。 

> [!NOTE]
> 使用 .NET Core 3.0 中引入的 .csproj 格式时，.csproj 所在的同一文件夹中的所有文件被视为项目的一部分。 因此，无需指定项目中包含的每个文件。 只能指定要为其定义自定义生成操作的文件或者要排除的文件。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>将 ContosoExpenses.Data 项目迁移到 .NET Standard

ContosoExpenses 解决方案包含一个 ContosoExpenses.Data 类库，该库包含服务的模型和接口，面向 .NET 4.7.2。  .NET Core 3.0 应用可以使用 .NET Framework 库，前提是它们未使用在 .NET Core 中不可用的 API。 但是，最佳现代化路径是将库迁移到 .NET Standard。 这可以确保 .NET Core 3.0 应用完全支持你的库。 此外，还可以在 Web（通过 ASP.NET Core）和移动（通过 Xamarin）平台等其他平台中重复使用库。

若要将 ContosoExpenses.Data 项目迁移到 .NET Standard：

1. 在 Visual Studio 中右键单击“ContosoExpenses.Data”项目，并选择“卸载项目”。  再次右键单击该项目，并选择“编辑 ContosoExpenses.Data.csproj”。

2. 删除项目文件的整个内容。

3. 复制并粘贴以下 XML，然后保存该文件。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 右键单击“ContosoExpenses.Data”项目，并选择“重载项目”。 

## <a name="configure-nuget-packages-and-dependencies"></a>配置 NuGet 包和依赖项

在前面的部分中迁移 ContosoExpenses.Core 和 ContosoExpenses.Data 项目时，已从项目中删除了 NuGet 包引用。  在本部分，你将重新添加这些引用。

若要为 ContosoExpenses.Data 项目配置 NuGet 包：

1. 在“ContosoExpenses.Data”项目中，展开“依赖项”节点。  请注意缺少 NuGet 节。

    ![NuGet 包](images/wpf-modernize-tutorial/NuGetPackages.png)

    如果在“解决方案资源管理器”中打开“Packages.config”，将会发现，NuGet 包的“旧”引用使用了该项目，但该项目使用的是完整 .NET Framework。 

    ![依赖项和包](images/wpf-modernize-tutorial/Packages.png)

    下面是 Packages.config 文件的内容。 将会看到，所有 NuGet 包面向完整 .NET Framework 4.7.2：

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 在“ContosoExpenses.Data”项目中删除“Packages.config”文件。 

4. 在“ContosoExpenses.Data”项目中，右键单击“依赖项”节点并选择“管理 NuGet 包”。  

  ![管理 NuGet 包...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 在“NuGet 包管理器”窗口中，单击“浏览”。  搜索 `Bogus` 包并安装最新稳定版本。

    ![Bogus NuGet 包](images/wpf-modernize-tutorial/Bogus.png)

6. 搜索 `LiteDB` 包并安装最新稳定版本。

    ![LiteDB NuGet 包](images/wpf-modernize-tutorial/LiteDB.png)

    你可能想知道 NuGet 包列表的存储位置，因为项目不再包含 packages.config 文件。 引用的 NuGet 包直接存储在 .csproj 文件中。 可以通过在文本编辑器中查看 ContosoExpenses.Data.csproj 项目文件的内容进行检查。 你将发现，该文件的末尾添加了以下行：

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 你还可能会注意到，要为此 .NET Core 3 项目安装的包与 .NET Framework 4.7.2 项目使用的包相同。 NuGet 包支持多目标。 库作者可以在同一个包中包含某个库的不同版本，这些版本是针对不同体系结构和平台编译的。 这些包支持完整 .NET Framework，以及与 .NET Core 3 项目兼容的 .NET Standard 2.0。 有关 .NET Framework、.NET Core 与 .NET Standard 之间的差异的详细信息，请参阅 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)。

若要为 ContosoExpenses.Core 项目配置 NuGet 包：

1. 在“ContosoExpenses.Core”项目中打开“packages.config”文件。  请注意，它当前包含面向 .NET Framework 4.7.2 的以下引用。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    以下步骤将使用 `MvvmLightLibs` 和 `Unity` 包的 .NET Standard 版本。 另外两个文件是安装这两个库时，NuGet 自动下载的依赖项。

2. 在“ContosoExpenses.Core”项目中删除“Packages.config”文件。 

3. 右键单击“ContosoExpenses.Core”项目并选择“管理 NuGet 包”。 

4. 在“NuGet 包管理器”窗口中，单击“浏览”。  搜索 `Unity` 包并安装最新稳定版本。

    ![Unity 包](images/wpf-modernize-tutorial/UnityPackage.png)

5. 搜索 `MvvmLightLibsStd10` 包并安装最新稳定版本。 这是 `MvvmLightLibs` 包的 .NET Standard 版本。 对于此包，作者已选择在不包含 .NET Framework 版本的单独包中打包库的 .NET Standard 版本。

    ![MvvmLightsLibs 包](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 在“ContosoExpenses.Core”项目中，右键单击“依赖项”节点并选择“添加引用”。  

7. 在“项目”>“解决方案”类别中，选择“ContosoExpenses.Data”并单击“确定”。  

    ![添加引用](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>禁用自动生成的程序集特性

在迁移过程的此阶段，如果尝试生成 ContosoExpenses.Core 项目，将会看到一些错误。

![.NET Core 3 生成新错误](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

发生此问题的原因是，.NET Core 3.0 中引入的新 .csproj 格式会将程序集信息存储在项目文件中，而不是存储在 AssemblyInfo.cs 文件中。 若要修复这些错误，请禁用此行为，并让项目继续使用 AssemblyInfo.cs 文件。

1. 在 Visual Studio 中右键单击“ContosoExpenses.Core”项目，并选择“卸载项目”。  再次右键单击该项目，并选择“编辑 ContosoExpenses.Core.csproj”。

1. 在 PropertyGroup 节中添加以下元素，然后保存文件。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    添加此元素后，PropertyGroup 节现在应如下所示：

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 右键单击“ContosoExpenses.Core”项目，并选择“重载项目”。 

4. 右键单击“ContosoExpenses.Data”项目，并选择“卸载项目”。  再次右键单击该项目，并选择“编辑 ContosoExpenses.Data.csproj”。

5. 在 PropertyGroup 节中添加相同的条目，然后保存文件。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    添加此元素后，PropertyGroup 节现在应如下所示：

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 右键单击“ContosoExpenses.Data”项目，并选择“重载项目”。 

## <a name="add-the-windows-compatibility-pack"></a>添加 Windows 兼容性包

如果现在尝试编译 ContosoExpenses.Core 和 ContosoExpenses.Data 项目，将会发现前面的错误现已修复，但 ContosoExpenses.Data 库中仍出现如下所示的一些错误。  

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

这些错误是在将 ContosoExpenses.Data 项目从（Windows 特有的）.NET Framework 库转换为可在多个平台（包括 Linux、Android、iOS 等）上运行的 .NET Standard 库后出现的。 ContosoExpenses.Data 项目包含一个名为 RegistryService 的类，该类与注册表（仅在 Windows 中出现的概念）交互。 

若要解决这些错误，请安装 [Windows 兼容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 包。 此包为在 .NET Standard 库中使用的许多 Windows 特定 API 提供支持。 使用此包后，库将不再跨平台，但它仍面向 .NET Standard。 

1. 右键单击“ContosoExpenses.Data”项目。
2. 选择“管理 NuGet 包”。
3. 在“NuGet 包管理器”窗口中，单击“浏览”。  搜索 `Microsoft.Windows.Compatibility` 包并安装最新稳定版本。

    ![安装 NuGet 包](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 现在，请右键单击“ContosoExpenses.Data”项目并选择“生成”，以再次尝试编译该项目。 

这一次，生成过程将会完成且不出错。

## <a name="test-and-debug-the-migration"></a>测试和调试迁移

成功生成项目后，接下来可以运行并测试应用，以确定是否会出现任何运行时错误。

1. 右键单击“ContosoExpenses.Core”项目，并选择“设为启动项目”。 

2. 按 F5 在调试器中启动“ContosoExpenses.Core”项目。 将看到如下所示的异常。

    ![Visual Studio 中显示的异常](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    引发此异常的原因是，在迁移过程中最初删除 .csproj 文件中的内容时，删除了有关映像文件的生成操作的信息。 以下步骤可解决此问题。

3. 停止调试器。

4. 右键单击“ContosoExpenses.Core”项目，并选择“卸载项目”。  再次右键单击该项目，并选择“编辑 ContosoExpenses.Core.csproj”。

5. 在 /Project 元素的前面添加以下条目：

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 右键单击“ContosoExpenses.Core”项目，并选择“重载项目”。 

7. 若要将 Contoso.ico 分配到应用，请右键单击“ContosoExpenses.Core”项目并选择“属性”。  在打开的页面中，单击“图标”下的下拉列表并选择 `Images\contoso.ico`。

    ![项目“属性”中的 Contoso 图标](images/wpf-modernize-tutorial/ContosoIco.png)

8. 单击 **“保存”** 。

9. 按 F5 在调试器中启动“ContosoExpenses.Core”项目。 确认应用现已运行。

## <a name="next-steps"></a>后续步骤

在本教程的此阶段，你已成功将 Contoso Expenses 应用迁移到 .NET Core 3。 现在，可以继续学习[第 2 部分：使用 XAML Islands 添加 UWP InkCanvas 控件](modernize-wpf-tutorial-2.md)。

> [!NOTE]
> 如果使用高分辨率屏幕，可能会发现应用看起来很小。 本教程的下一步骤将介绍如何解决此问题。
