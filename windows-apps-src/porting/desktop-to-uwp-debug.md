---
author: awkoren
Description: "使用桌面到 UWP 桥部署和调试从 Windows 桌面应用程序（Win32、WPF 和 Windows 窗体）转换的通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: "调试使用桌面桥转换的应用"
translationtype: Human Translation
ms.sourcegitcommit: 8429e6e21319a03fc2a0260c68223437b9aed02e
ms.openlocfilehash: 9dcc39c51e61b24c25bcbfa216c6e51b49bbfd3a

---

# 调试使用桌面桥转换的应用

本主题包含有助于你在使用桌面到 UWP 桥转换应用后成功调试应用的信息。 有几个用于调试已转换应用的选项。

## 附加到进程

当 Microsoft Visual Studio“以管理员身份”运行时，“开始调试”和“开始执行”命令将适用于转换应用的项目，但已启动的应用将以[中等完整性级别](https://msdn.microsoft.com/library/bb625963)运行（即，它不会具有提升的权限）。 若要向已启动的应用授予管理员权限，首先你需要通过快捷方式或磁贴启动“以管理员身份”。 应用运行后，从“以管理员身份”运行的 Microsoft Visual Studio 的实例调用__附加到进程__，然后从对话框中选择应用的进程。

## F5 调试

Visual Studio 现在支持新的打包项目。 新项目可使你在将应用程序生成到从应用程序安装程序上的转换器创建的 Appx 包中时自动复制任何更新。 配置打包项目后，你现在还可以使用 F5 直接在 AppX 程序包中调试。 

下面介绍了如何开始使用： 

1. 首先，确保已经过设置，可以使用桌面应用转换器。 有关说明，请参阅[桌面应用转换器](desktop-to-uwp-run-desktop-app-converter.md)。

2. 依次运行转换器和 Win32 应用程序的安装程序。 转换器会捕获布局以及对注册表所做的任何更改，并输出一个带有清单和 registery.dat 的 Appx 以虚拟化该注册表：

![alt](images/desktop-to-uwp/debug-1.png)

3. 安装并启动 [Visual Studio "15" Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx)。 

4. 从[Visual Studio 库](http://go.microsoft.com/fwlink/?LinkId=797871)中安装“桌面到 UWP 打包 VSIX 项目”。 

5. 打开已在 Visual Studio 中转换的相应 Win32 解决方案。
 
6. 将新的打包项目添加到你的解决方案，方法是右键单击该解决方案并选择“添加新项目”。 然后在“安装和部署”下选取“桌面到 UWP 打包项目”：

    ![alt](images/desktop-to-uwp/debug-2.png)

    所产生的项目将添加到你的解决方案：

    ![alt](images/desktop-to-uwp/debug-3.png)

    在打包项目中，AppXFileList 提供文件到 AppX 布局的映射。 引用开始为空，但应手动设置为 .exe 项目以用于生成排序。 

7. DesktopToUWPPackaging 项目具有一个属性页，该页允许你配置 AppX 程序包根以及要执行的磁贴：

    ![alt](images/desktop-to-uwp/debug-4.png)

    将 PackageLayout 设置为转换器（上述）所创建的 AppX 的根位置。 然后选取要执行的磁贴。

8.  打开并编辑 AppXFileList.xml。 此文件定义如何将 Win32 调试生成的输出复制到转换器所生成的 AppX 布局中。 默认情况下，文件中有一个带有示例标签和注释的占位符：

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    下面是创建映射的示例。 在此情况下，我们将 .exe 和 .dll 从 Win32 生成位置复制到程序包布局位置中。 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    此文件的定义如下： 

    首先，我们将 *MyProjectOutputPath* 定义为指向 Win32 项目将生成到的位置：

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    然后，每个 *LayoutFile* 都指定要从 Win32 生成位置复制到 Appx 程序包布局的文件。 在此情况下，依次复制 .exe 和 .dll。 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. 将打包项目设置为启动项目。 这会将 Win32 文件复制到 AppX，然后在生成和运行该项目时启动调试程序。  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. 最后，你现在可以在 Win32 代码中设置断点，并按 F5 启动调试程序。 它会将你对 Win32 应用程序所做的所有更新复制到 AppX 程序包，并允许你直接从 Visual Studio 内进行调试。

11. 如果你更新应用程序，将需要使用 MakeAppX 重新打包你的应用。 有关详细信息，请参阅[应用包生成工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)。 

如果你有多个生成配置（例如用于发布和调试），可以将以下内容添加到 AppXFileList.xml 文件以从不同的位置复制 Win32 生成：

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

如果你将应用程序更新到 UWP，但仍然希望也针对 Win32 生成它，则还可以使用条件编译来启用特定代码路径。 

1.  在以下示例中，代码仅针对 DesktopUWP 进行编译，并且将使用 WinRT API 显示磁贴。 

    ```C#
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

2.  你可以使用“配置管理器”添加新的生成配置：

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  然后在项目属性下，添加对条件编译符号的支持：

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  如果要面向你添加的 UWP API 进行生成，你现在可以将生成目标切换为 DesktopUWP。

## PLMDebug 

如果要在应用运行时进行调试，Visual Studio F5 和“附加到进程”将很有用。 但是，在某些情况下，你可能希望更精细的控制调试过程，包括能够在应用启动前调试应用。 在这些更高级的方案中，使用 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)。 此工具允许使用 Windows 调试程序调试转换的应用，并提供对应用生命周期（包括暂停、恢复和终止）的完全控制。 

PLMDebug 包含在 Windows SDK 中。 有关详细信息，请参阅 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)。 

## 在完全信任的容器内运行另一个进程 

你可以在指定应用包的容器内调用自定义进程。 这对于测试方案（例如，如果你有自定义测试工具并想要测试应用的输出）很有用。 若要执行此操作，请使用 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet： 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```



<!--HONumber=Nov16_HO1-->


