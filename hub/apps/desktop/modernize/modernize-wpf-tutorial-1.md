---
description: 本教程演示如何添加 UWP XAML 用户界面、 创建 MSIX 包和其他新式组件合并到 WPF 应用。
title: 将 Contoso 迁移到.NET Core 3 Expenses 应用程序
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e718de7a22873ccf347e60c661f724ce3abdd2cf
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420125"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>第 1 部分：将 Contoso 迁移到.NET Core 3 Expenses 应用程序

这是本教程演示如何实现名为 Contoso 费用的示例 WPF 桌面应用的现代化的第一部分。 教程、 先决条件和说明下载示例应用程序的概述，请参阅[教程：使 WPF 应用现代化](modernize-wpf-tutorial.md)。
  
在本教程的此部分中，将从.NET Framework 4.7.2 到迁移整个 Contoso Expenses 应用程序[.NET Core 3](modernize-wpf-tutorial.md#net-core-3)。 在开始本教程的此部分之前，请确保您执行以下：

* [打开并生成 ContosoExpenses 示例](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)在 Visual Studio 2019。
* 如果使用 Visual Studio 2019 的发行版，启用.NET Core SDK 的预览版本。 在 Visual Studio 中，转到**工具 > 选项**，在搜索框中，键入"预览版"并选择**使用的.NET Core SDK 预览**。 如果您使用的[预览版本的 Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)，无需选择此选项，因为默认情况下启用.NET Core 预览版。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>将 ContosoExpenses 项目迁移到.NET Core 3

在本部分中，将迁移到.NET Core 3 ContosoExpenses 项目中，Contoso Expenses 应用程序。 通过创建包含现有 ContosoExpenses 项目但而不是在.NET Framework 4.7.2 的目标.NET Core 3 相同的文件的新项目文件，将执行此操作。 这使您可以维护单个解决方案与.NET Framework 和.NET Core 版本的应用程序。

1. 验证 ContosoExpenses 当前项目面向.NET Framework 4.7.2。 在解决方案资源管理器中右键单击**ContosoExpenses**项目中，选择**属性**，然后确认**目标框架**属性**应用程序**选项卡设置为在.NET Framework 4.7.2。

    ![.NET framework 4.7.2 项目](images/wpf-modernize-tutorial/NETFramework472.png)

3. 在 Windows 资源管理器，导航到**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses**文件夹，并创建一个名为的新文本文件**ContosoExpenses.Core.csproj**。

4. 右键单击该文件中，选择**使用打开**，然后在您的选择，例如记事本、 Visual Studio Code 或 Visual Studio 文本编辑器中打开它。

5. 将以下文本复制到该文件并将其保存。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 关闭该文件并返回到**ContosoExpenses** Visual Studio 中的解决方案。

7. 右键单击**ContosoExpenses**解决方案，然后选择**添加-> 现有项目**。 选择**ContosoExpenses.Core.csproj**文件中只需创建`C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses`文件夹以将其添加到解决方案。

**ContosoExpenses.Core.csproj**包括以下元素：

* **项目**元素指定的 SDK 版本**Microsoft.NET.Sdk.WindowsDesktop**。 这是指为 Windows 桌面.NET 应用程序并包括 WPF 和 Windows 窗体应用程序的组件。
* **PropertyGroup**元素包含子元素，指示项目输出是一个可执行文件 (不是一个 DLL)，面向.NET Core 3，并使用 WPF。 对于 Windows 窗体应用中，将使用**UseWinForms**元素而不是**UseWPF**元素。

> [!NOTE]
> 在使用.NET Core 3.0 中引入了.csproj 格式，.csproj 所在的文件夹中的所有文件被都视为该项目的一部分。 因此，您无需指定项目中包含的每个文件。 必须指定要排除那些想要定义自定义生成操作或你的文件。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>将 ContosoExpenses.Data 项目迁移到.NET Standard

**ContosoExpenses**解决方案包括**ContosoExpenses.Data**类库，其中包含模型和服务接口并面向.NET 4.7.2。 .NET core 3.0 应用程序可以使用.NET Framework 库，只要它们不使用 Api 在.NET Core 中不可用。 但是，最佳的现代化途径是将您的库移动到.NET Standard。 这将确保你的库完全支持.NET Core 3.0 应用程序。 此外，您可以重复使用还与其他平台，如 （通过 ASP.NET Core) 的 web 和移动 （通过 Xamarin) 库。

若要迁移**ContosoExpenses.Data**到.NET Standard 项目：

1. 在 Visual Studio 中，右键单击**ContosoExpenses.Data**项目，然后选择**卸载项目**。 再次右键单击项目，然后选择**编辑 ContosoExpenses.Data.csproj**。

2. 删除项目文件的全部内容。

3. 复制并粘贴以下 XML，然后保存该文件。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 右键单击**ContosoExpenses.Data**项目，然后选择**重新加载项目**。

## <a name="configure-nuget-packages-and-dependencies"></a>配置 NuGet 包和依赖项

何时迁移**ContosoExpenses.Core**并**ContosoExpenses.Data**项目在前面部分中，删除 NuGet 包引用项目中。 在本部分中，将添加这些引用返回。

若要配置的 NuGet 包**ContosoExpenses.Data**项目：

1.  在中**ContosoExpenses.Data**项目中，展开**依赖项**节点。 请注意， **NuGet**部分缺少。

    ![NuGet 包](images/wpf-modernize-tutorial/NuGetPackages.png)

    如果您打开**Packages.config**中**解决方案资源管理器**您会发现它使用完整的.NET Framework 时，旧引用的 NuGet 包使用项目。

    ![依赖项和包](images/wpf-modernize-tutorial/Packages.png)

    下面是内容的**Packages.config**文件。 您将注意到所有 NuGet 包都面向完整.NET Framework 4.7.2:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 在中**ContosoExpenses.Data**项目中，删除**Packages.config**文件。

4. 在中**ContosoExpenses.Data**项目，右键单击**依赖关系**节点，然后选择**管理 NuGet 包**。

  ![管理 NuGet 包...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 在中**NuGet 包管理器**窗口中，单击**浏览**。 搜索`Bogus`程序包并安装它。

    ![虚假的 NuGet 包](images/wpf-modernize-tutorial/Bogus.png)

6. 搜索`LiteDB`程序包并安装它。

    ![LiteDB NuGet 包](images/wpf-modernize-tutorial/LiteDB.png)

    你可能想知道存储这些列表中的 NuGet 包的位置，因为项目不会再有 packages.config 文件。 直接在.csproj 文件中存储引用的 NuGet 包。 可以查看的内容检查这**ContosoExpenses.Data.csproj**在文本编辑器中的项目文件。 您将找到该文件末尾添加以下行：

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 您可能还注意到，要安装的.NET Framework 4.7.2 项目使用此.NET Core 3 项目相同的包。 NuGet 包支持多目标。 库作者可以在同一个包中，为不同的体系结构和平台编译包含不同版本的库。 这些包支持完整的.NET Framework 以及.NET Standard 2.0，这是与.NET Core 3 项目兼容。 有关.NET Framework、.NET Core 和.NET Standard 的区别的详细信息，请参阅[.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)。

若要配置的 NuGet 包**ContosoExpenses.Core**项目：

1. 在中**ContosoExpenses.Core**项目中，打开**packages.config**文件。 请注意它当前包含以下面向.NET Framework 4.7.2 的引用。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    在以下步骤将.NET Standard 版本的`MvvmLightLibs`和`Unity`包。 其他两个安装这两个库时自动下载 nuget 的依赖项。

2. 在中**ContosoExpenses.Core**项目中，删除**Packages.config**文件。

3. 右键单击**ContosoExpenses.Core**项目，然后选择**管理 NuGet 包**。

4. 在中**NuGet 包管理器**窗口中，单击**浏览**。 搜索`Unity`程序包并安装它。

    ![Unity 包](images/wpf-modernize-tutorial/UnityPackage.png)

5. 搜索`MvvmLightLibsStd10`程序包并安装它。 这是.NET Standard 版本`MvvmLightLibs`包。 此包的作者选择包中的.NET Framework 版本比单独的包的库的.NET Standard 版本。

    !MvvmLightsLibs package[](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 在中**ContosoExpenses.Core**项目，右键单击**依赖关系**节点，然后选择**添加引用**。

7. 在中**项目 > 解决方案**类别中，选择**ContosoExpenses.Data**然后单击**确定**。

    ![添加引用](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>禁用自动生成的程序集特性

在这一时刻在迁移过程中，如果你尝试生成**ContosoExpenses.Core**项目您将看到一些错误。

![.NET core 3 生成新的错误](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

因为新.csproj 格式引入与.NET Core 3.0 是存储在项目文件中的程序集信息发生了这一问题而不是**AssemblyInfo.cs**文件。 若要修复这些错误，禁用此行为，并允许继续使用项目**AssemblyInfo.cs**文件。

1. 在 Visual Studio 中，右键单击**ContosoExpenses.Core**项目，然后选择**卸载项目**。 再次右键单击项目，然后选择**编辑 ContosoExpenses.Core.csproj**。

1. 将以下元素中的添加**PropertyGroup**部分，并保存该文件。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    添加此元素后**PropertyGroup**部分现在应如下所示：

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 右键单击**ContosoExpenses.Core**项目，然后选择**重新加载项目**。

4. 右键单击**ContosoExpenses.Data**项目，然后选择**卸载项目**。 再次右键单击项目，然后选择**编辑 ContosoExpenses.Data.csproj**。

5. 添加中的同一条目**PropertyGroup**部分，并保存该文件。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    添加此元素后**PropertyGroup**部分现在应如下所示：

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 右键单击**ContosoExpenses.Data**项目，然后选择**重新加载项目**。

## <a name="add-the-windows-compatibility-pack"></a>添加 Windows 兼容性包

如果现在尝试进行编译**ContosoExpenses.Core**并**ContosoExpenses.Data**项目中，您将看到现已修复以前的错误，但仍有中的某些错误**ContosoExpenses.Data**类似于这些库。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

这些错误是将转换的结果**ContosoExpenses.Data**项目的.NET Standard 库，可以在多个平台包括 Linux、 Android、 iOS 运行，从.NET Framework 库 （这是特定于 Windows）和的详细信息。 **ContosoExpenses.Data**项目包含一个名为类**RegistryService**，这与注册表中，仅限 Windows 的概念进行交互。

若要解决这些错误，安装[Windows 兼容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)NuGet 包。 此包提供对要使用.NET Standard 库中的许多特定于 Windows 的 Api 的支持。 后使用此包，但它将仍定目标到.NET Standard 库不再将跨平台。 

1. 右键单击**ContosoExpenses.Data**项目。
2. 选择**管理 NuGet 包**。
3. 在中**NuGet 包管理器**窗口中，单击**浏览**。 搜索`Microsoft.Windows.Compatibility`程序包并安装它。 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 现在尝试编译项目，右键单击**ContosoExpenses.Data**项目，然后选择**生成**。

这一次生成过程将完成且未出错。

## <a name="test-and-debug-the-migration"></a>测试和调试迁移

现在，已成功生成项目，已准备好运行和测试应用程序以查看是否存在任何运行时错误。

1. 右键单击**ContosoExpenses.Core**项目，然后选择**设为启动项目**。

2. 按 F5 启动**ContosoExpenses.Core**在调试器中的项目。 你将看到类似于下面的异常。

    ![Visual Studio 中显示的异常](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    因为从迁移的开始处的.csproj 文件中删除内容，删除了信息有关正在引发此异常**生成操作**的图像文件。 以下步骤解决此问题。

3. 停止调试器。

4. 右键单击**ContosoExpenses.Core**项目，然后选择**卸载项目**。 再次右键单击项目，然后选择**编辑 ContosoExpenses.Core.csproj**。

5. 在关闭前**项目**元素中，添加以下条目：

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 右键单击**ContosoExpenses.Core**项目，然后选择**重新加载项目**。

7. 若要将 Contoso.ico 分配到应用，请右键单击**ContosoExpenses.Core**项目，然后选择**属性**。 在打开的页中，单击下拉列表下**图标**，然后选择`Images\contoso.ico`。

    ![在项目的属性中的 Contoso 图标](images/wpf-modernize-tutorial/ContosoIco.png)

8. 单击“保存”  。

9. 按 F5 启动**ContosoExpenses.Core**在调试器中的项目。 确认现在运行应用程序。

## <a name="next-steps"></a>后续步骤

此时在教程中，您成功迁移 Contoso Expenses 应用程序到.NET Core 3。 你现已准备好[第 2 部分：添加 UWP InkCanvas 控件使用 XAML 群岛](modernize-wpf-tutorial-2.md)。

> [!NOTE]
> 如果您有高分辨率屏幕，你可能会注意到该应用程序看起来很小。 将解决此问题在本教程的下一步。
