---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 .MSIX 包以及将其他新式组件合并到 WPF 应用。
title: 将 Contoso Expenses 应用迁移到 .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 97488a913605916c067861b5941d7aa127b00917
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643413"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>第 1 部分：将 Contoso Expenses 应用迁移到 .NET Core 3

这是教程的第一部分, 该教程演示如何将名为 Contoso 支出的示例 WPF 桌面应用实现现代化。 有关下载示例应用的教程、先决条件和说明的概述, 请参阅[教程:现代化 WPF 应用](modernize-wpf-tutorial.md)。
  
在本教程的此部分中, 你要将整个 Contoso 支出应用从 .NET Framework 4.7.2 迁移到[.Net Core 3](modernize-wpf-tutorial.md#net-core-3)。 在开始本教程部分之前, 请确保执行以下操作:

* [打开并生成](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)Visual Studio 2019 中的 ContosoExpenses 示例。
* 如果使用的是 Visual Studio 2019 的发布版本, 请启用 .NET Core SDK 的预览版本。 在 Visual Studio 中, 依次单击 "**工具" > "选项**", 在搜索框中键入 "Preview", 然后选择 **"使用 .NET Core SDK 的预览**"。 如果使用[的是 Visual Studio 2019 的预览版本](https://visualstudio.microsoft.com/vs/preview/), 则无需选择此选项, 因为默认情况下会启用 .net Core 预览。

> [!NOTE]
> 有关将 WPF 应用程序从 .NET Framework 迁移到 .NET Core 3 的详细信息, 请参阅[此博客系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>将 ContosoExpenses 项目迁移到 .NET Core 3

在本部分中, 你要将 Contoso 支出应用中的 ContosoExpenses 项目迁移到 .NET Core 3。 为此, 可以创建一个新的项目文件, 其中包含与现有 ContosoExpenses 项目相同的文件, 但面向 .NET Core 3 而不是 .NET Framework 4.7.2。 这使您可以使用应用程序的 .NET Framework 和 .NET Core 版本来维护单个解决方案。

1. 验证 ContosoExpenses 项目当前是否以 .NET Framework 4.7.2 为目标。 在解决方案资源管理器中, 右键单击**ContosoExpenses**项目, 选择 "**属性**", 然后确认 "**应用程序**" 选项卡上的 "**目标框架**" 属性设置为 .NET Framework 4.7.2。

    ![项目的 .NET Framework 版本4.7。2](images/wpf-modernize-tutorial/NETFramework472.png)

3. 在 Windows 资源管理器中, 导航到**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses**文件夹并创建一个名为**ContosoExpenses**的新文本文件。

4. 右键单击该文件, 选择 "**打开方式**", 然后在所选的文本编辑器 (如记事本、Visual Studio Code 或 Visual Studio) 中打开它。

5. 将以下文本复制到该文件中并保存该文件。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 关闭文件并返回到 Visual Studio 中的**ContosoExpenses**解决方案。

7. 右键单击**ContosoExpenses**解决方案, 然后选择 "**外接 > 现有项目**"。 选择刚刚在`C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses`文件夹中创建的 ContosoExpenses 文件, 以将其添加到解决方案中。

**ContosoExpenses**包括以下元素:

* **项目**元素指定了**WindowsDesktop**的 SDK 版本。 这是指适用于 Windows 桌面的 .NET 应用程序, 它包括用于 WPF 和 Windows 窗体应用程序的组件。
* **PropertyGroup**元素包含子元素, 这些子元素指示项目输出是一个可执行文件 (而不是 DLL), 面向 .net Core 3, 并使用 WPF。 对于 Windows 窗体应用, 请使用**UseWinForms**元素, 而不是**UseWPF**元素。

> [!NOTE]
> 使用 .NET Core 3.0 引入的 .csproj 格式时, 与 .csproj 相同的文件夹中的所有文件都被视为项目的一部分。 因此, 您无需指定项目中包含的每个文件。 必须仅指定要为其定义自定义生成操作的文件或要排除的文件。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>将 ContosoExpenses 项目迁移到 .NET Standard

**ContosoExpenses**解决方案包括一个**ContosoExpenses**类库, 其中包含用于服务和目标的模型和接口 .net 4.7.2。 .NET Core 3.0 应用程序可使用 .NET Framework 库, 前提是它们不使用 .NET Core 中不可用的 Api。 但是, 最佳的现代化途径是将库移动到 .NET Standard。 这将确保 .NET Core 3.0 应用完全支持你的库。 此外, 还可以将该库与其他平台一起使用, 例如 web (通过 ASP.NET Core) 和移动 (通过 Xamarin)。

若要将**ContosoExpenses**项目迁移到 .NET Standard:

1. 在 Visual Studio 中, 右键单击**ContosoExpenses**项目, 然后选择 "**卸载项目**"。 再次右键单击该项目, 然后选择 "**编辑 ContosoExpenses**"。

2. 删除项目文件的全部内容。

3. 复制并粘贴以下 XML 并保存该文件。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**重新加载项目**"。

## <a name="configure-nuget-packages-and-dependencies"></a>配置 NuGet 包和依赖项

当你在前面的部分中迁移**ContosoExpenses**和**ContosoExpenses**项目时, 将从项目中删除 NuGet 包引用。 在本部分中, 您将重新添加这些引用。

为**ContosoExpenses**项目配置 NuGet 包:

1. 在**ContosoExpenses**项目中, 展开 "**依赖项**" 节点。 请注意, **NuGet**部分丢失。

    ![NuGet 包](images/wpf-modernize-tutorial/NuGetPackages.png)

    如果在**解决方案资源管理器**中打开了**包**, 则在使用完整 .NET Framework 时, 将会发现 NuGet 包的 "旧" 引用使用的是该项目。

    ![依赖项和包](images/wpf-modernize-tutorial/Packages.png)

    下面是**包 .config**文件的内容。 你会注意到, 所有 NuGet 包都以完整 .NET Framework 4.7.2 为目标:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 在**ContosoExpenses**项目中, 删除**包 .config**文件。

4. 在**ContosoExpenses**项目中, 右键单击 "**依赖关系**" 节点, 然后选择 "**管理 NuGet 包**"。

  ![管理 NuGet 包 。](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 在 " **NuGet 包管理器**" 窗口中, 单击 "**浏览**"。 `Bogus`搜索包并安装最新的稳定版本。

    ![虚假 NuGet 包](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB`搜索包并安装最新的稳定版本。

    ![LiteDB NuGet 包](images/wpf-modernize-tutorial/LiteDB.png)

    您可能想知道这些 NuGet 包的存储位置, 因为该项目不再具有包 .config 文件。 引用的 NuGet 包直接存储在 .csproj 文件中。 可以通过在文本编辑器中查看**ContosoExpenses**项目文件的内容来进行检查。 你将在文件末尾添加以下行:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 你可能还会注意到, 你要将此 .NET Core 3 项目的包安装 .NET Framework 4.7.2 项目所使用的包中。 NuGet 包支持多目标。 库作者可以在同一包中包含不同版本的库, 并针对不同的体系结构和平台进行了编译。 这些包支持完整 .NET Framework 以及与 .NET Core 3 项目兼容的 .NET Standard 2.0。 有关 .NET Framework、.NET Core 和 .NET Standard 之间的差异的详细信息, 请参阅[.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)。

若要为**ContosoExpenses**项目配置 NuGet 包:

1. 在**ContosoExpenses**项目中, 打开**包 .config**文件。 请注意, 它当前包含面向 .NET Framework 4.7.2 的以下引用。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    在下面的步骤中, 你将 .NET Standard `MvvmLightLibs`和`Unity`包的版本。 另外两个是安装这两个库时, NuGet 自动下载的依赖项。

2. 在**ContosoExpenses**项目中, 删除**包 .config**文件。

3. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**管理 NuGet 包**"。

4. 在 " **NuGet 包管理器**" 窗口中, 单击 "**浏览**"。 `Unity`搜索包并安装最新的稳定版本。

    ![Unity 包](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10`搜索包并安装最新的稳定版本。 这是`MvvmLightLibs`包的 .NET Standard 版本。 对于此包, 作者选择将库的 .NET Standard 版本打包到不同于 .NET Framework 版本的包中。

    ![MvvmLightsLibs 包](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 在**ContosoExpenses**项目中, 右键单击 "**依赖关系**" 节点, 然后选择 "**添加引用**"。

7. 在 "**项目 > 解决方案**" 类别中, 选择 " **ContosoExpenses** ", 然后单击 **"确定"** 。

    ![添加引用](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>禁用自动生成的程序集特性

此时在迁移过程中, 如果尝试生成**ContosoExpenses**项目, 则会出现一些错误。

![.NET Core 3 生成新错误](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

发生此问题的原因在于, .NET Core 3.0 引入的新 .csproj 格式将程序集信息存储在项目文件中, 而不是**AssemblyInfo.cs**文件中。 若要修复这些错误, 请禁用此行为, 并让项目继续使用**AssemblyInfo.cs**文件。

1. 在 Visual Studio 中, 右键单击**ContosoExpenses**项目, 然后选择 "**卸载项目**"。 再次右键单击该项目, 然后选择 "**编辑 ContosoExpenses**"。

1. 在**PropertyGroup**节中添加以下元素, 并保存该文件。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    添加此元素后, **PropertyGroup**节现在应如下所示:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**重新加载项目**"。

4. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**卸载项目**"。 再次右键单击该项目, 然后选择 "**编辑 ContosoExpenses**"。

5. 在**PropertyGroup**节中添加同一条目, 并保存该文件。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    添加此元素后, **PropertyGroup**节现在应如下所示:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**重新加载项目**"。

## <a name="add-the-windows-compatibility-pack"></a>添加 Windows 兼容性包

如果你现在尝试编译**ContosoExpenses**和**ContosoExpenses**项目, 你会发现以前的错误现已修复, 但**ContosoExpenses**中仍有一些错误, 类似于这些错误。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

这些错误是将 ContosoExpenses 项目从 .NET Framework 库 (特定于 Windows) 转换为 .NET Standard 库 (可在多个平台 (包括 Linux、Android、iOS 等) 上运行的结果 **。** **ContosoExpenses**项目包含一个名为**RegistryService**的类, 该类与注册表 (仅限 Windows 的概念) 交互。

若要解决这些错误, 请安装[Windows 兼容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)NuGet 包。 此包为在 .NET Standard 库中使用的许多特定于 Windows 的 Api 提供支持。 使用此包后, 库将不再跨平台, 但它仍将以 .NET Standard 为目标。 

1. 右键单击 " **ContosoExpenses** " 项目。
2. 选择 "**管理 NuGet 包**"。
3. 在 " **NuGet 包管理器**" 窗口中, 单击 "**浏览**"。 `Microsoft.Windows.Compatibility`搜索包并安装最新的稳定版本。

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 现在, 右键单击**ContosoExpenses**项目并选择 "**生成**", 再次尝试编译项目。

这次, 生成过程将完成而不发生错误。

## <a name="test-and-debug-the-migration"></a>测试和调试迁移

现在, 项目已成功生成, 你可以运行和测试应用程序, 以查看是否存在任何运行时错误。

1. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**设为启动项目**"。

2. 按 F5 在调试器中启动**ContosoExpenses**项目。 你将看到类似于以下内容的异常。

    ![Visual Studio 中显示的异常](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    引发此异常的原因是, 当你从迁移开始时删除 .csproj 文件中的内容时, 你删除了有关映像文件的**生成操作**的信息。 以下步骤解决此问题。

3. 停止调试器。

4. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**卸载项目**"。 再次右键单击该项目, 然后选择 "**编辑 ContosoExpenses**"。

5. 在结束**项目**元素之前, 添加以下条目:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**重新加载项目**"。

7. 若要将 Contoso 指定给应用, 请右键单击**ContosoExpenses**项目, 然后选择 "**属性**"。 在打开的页面中, 单击**图标**下的 "下拉" `Images\contoso.ico`, 然后选择。

    ![项目属性中的 Contoso 图标](images/wpf-modernize-tutorial/ContosoIco.png)

8. 单击“保存”。

9. 按 F5 在调试器中启动**ContosoExpenses**项目。 确认应用程序现已运行。

## <a name="next-steps"></a>后续步骤

在本教程的此阶段, 已成功将 Contoso 支出应用迁移到 .NET Core 3。 你现在已准备好[第2部分:使用 XAML 孤岛](modernize-wpf-tutorial-2.md)添加 UWP InkCanvas 控件。

> [!NOTE]
> 如果有高分辨率屏幕, 可能会发现应用看起来非常小。 你将在本教程的下一步中解决此问题。
