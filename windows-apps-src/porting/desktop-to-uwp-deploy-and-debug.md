---
author: awkoren
Description: "使用桌面转换扩展部署和调试从 Windows 桌面应用程序（Win32、WPF 和 Windows 窗体）转换的通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: "部署和调试从 Windows 桌面应用程序转换的通用 Windows 平台 (UWP) 应用"
ms.sourcegitcommit: 606d5237cb67cb4439704f81b180c3c48cc1556f
ms.openlocfilehash: 14634c12435cd8d6d4471a65c0f8deb36e3b1c80

---

# 部署和调试转换的 UWP 应用 (Project Centennial)

\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

本主题包含有助于你在转换后成功部署和调试应用的信息。 此外，如果你对某些桌面转换扩展的内部组件感兴趣，则本主题适合你。

## 调试转换的 UWP 应用

使用 Visual Studio 调试转换的应用有两个主要选项。

### 附加到进程

当 Microsoft Visual Studio“以管理员身份”运行时，“开始调试”____和“开始执行(不调试)”____命令将适用于转换应用的项目，但已启用的应用将以[中等完整性级别](https://msdn.microsoft.com/library/bb625963)运行。 即，它_不_会具有提升的权限。 若要向已启动的应用授予管理员权限，首先你需要通过快捷方式或磁贴启动“以管理员身份”。 应用运行后，从“以管理员身份”运行的 Microsoft Visual Studio 的实例调用__附加到进程__，然后从对话框中选择应用的进程。

### F5 调试

Visual Studio 现在支持新的打包项目，可使你将在生成应用程序时创建的所有更新自动复制到在应用程序的安装程序上运行转换器时所创建的 AppX 程序包中。 配置打包项目后，你现在还可以使用 F5 直接在 AppX 程序包中调试。 

下面介绍了如何开始使用： 

1. 首先，确保你已做好使用 Centennial 的准备。 有关说明，请参阅[桌面应用转换器预览 (Project Centennial)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)。 

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

11. 如果你更新应用程序，将需要使用 MakeAppX 重新打包你的应用。 有关详细信息，请参阅[应用包生成工具 (MakeAppx.exe)](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446767(v=vs.85).aspx)。 

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

## 部署转换的 UWP 应用

若要在部署期间部署应用，请运行以下 PowerShell cmdlet： 

```Add-AppxPackage –Register AppxManifest.xml```

若要更新应用的 .exe 或 .dll 文件，只需将程序包中的现有文件替换为新文件、增加 AppxManifest.xml 中的版本号，然后再次运行上述命令。

注意以下情况： 

安装转换的应用的任何驱动器都必须设置为 NTFS 格式。

转换的应用始终以交互用户身份运行。 对于其清单指定 __requireAdministrator__ 的执行级别的 .NET 应用，这具有特定的意义。 如果交互用户具有管理员权限，则将在_每次启动应用时_显示 UAC 提示。 对于标准用户，应用将无法启动。

如果你尝试在未导入你所创建的证书的计算机上运行 Add-AppxPackage cmdlet，则你将收到错误。

在你部署应用前，你将需要使用证书对其进行签名。 有关创建证书的信息，请参阅[对 .Appx 程序包进行签名](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx)。 

下面介绍如何导入你之前创建的证书。 你可以直接安装它，或者可以从你已签名的 appx 中安装它，就像客户所做的那样。
1.  在“文件资源管理器”中，右键单击你已使用测试证书签名的 appx，然后从上下文菜单中选择“属性”****。
2.  单击或点击“数字签名”****选项卡。
3.  单击或点击该证书并选择“详细信息”****。
4.  单击或点击“查看证书”****。
5.  单击或点击“安装证书”****。
6.  在“存储位置”****组中，选择“本地计算机”****。
7.  依次单击或点击“下一步”****和“确定”****来确认 UAC 对话框。
8.  在证书导入向导的下一个屏幕中，将选定的选项更改为“将所有的证书放入下列存储”****。
9.  单击或点击“浏览”****。 在“选择证书存储”窗口中，向下滚动并选择“受信任人”****，然后单击或点击“确定”****。
10. 单击或点击“下一步”****。 将出现新屏幕。 单击或点击“完成”****。
11. 应出现确认对话框。 如果出现，请单击“确定”****。 如果出现另一个指示证书存在问题的对话框，则可能需要执行某些证书疑难解答操作。

若要使 Windows 信任证书，证书必须位于“证书(本地计算机)”&gt;“受信任根证书颁发机构”&gt;“证书”****节点或“证书(本地计算机)”&gt;“受信任人”&gt;“证书”****节点中。 仅这两个位置中的证书可以在本地计算机的上下文中验证证书信任。 否则，将显示类似于以下字符串的错误消息：
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

### 后台操作

当你运行转换的应用时，你的 UWP 应用包将从 \Program Files\WindowsApps\\&lt;_package name_&gt;\\&lt;_appname_&gt;.exe 启动。 如果你查看该处，你将看到你的应用具有一个应用程序包清单（名为 AppxManifest.xml），该清单引用用于转换的应用的特殊 xml 命名空间。 该清单文件内部是一个 __&lt;EntryPoint&gt;__ 元素，该元素引用完全信任应用。 当启动该应用时，它不会在应用容器内部运行，而是像往常一样以用户身份运行。

但是，应用在特殊环境下运行，在该环境中，应用对文件系统和注册表所做的所有访问都将重定向。 名为 Registry.dat 的文件用于注册表重定向。 它实际上是一个注册表配置单元，因此你可以在 Windows 注册表编辑器 (Regedit) 中查看它。 请注意，此机制意味着你无法为进程间通信使用注册表。 注册表在任何情况下都不旨在用于（并且不太适合）该做法。 当涉及到文件系统时，重定向的唯一内容是 AppData 文件夹，并且它重定向到为所有 UWP 应用存储应用数据的相同位置。 此位置称为本地应用数据存储，你可以使用 [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621) 属性访问它。 通过这种方式，已经移植你的代码，以便你无需执行任何操作即可在正确的位置读取和写入应用数据。 并且你还可以在该处直接写入。 文件系统重定向的一个好处是更简洁的卸载体验。

在一个名为 VFS 的文件夹内，你将看到包含你的应用具有依赖项的 DLL 的文件夹。 对于应用的经典桌面版本，这些 DLL 将安装到系统文件夹中。 但是，作为 UWP 应用，DLL 位于应用本地。 这样，在安装和卸载 UWP 应用时不会有版本控制问题。

## 另请参阅
[将桌面应用程序转换为通用 Windows 平台 (UWP) 应用](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)

[桌面应用转换器预览 (Project Centennial)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)

[将 Windows 桌面应用程序手动转换为通用 Windows 平台 (UWP) 应用](https://msdn.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-manual-conversion)

[GitHub 上的 UWP 代码示例的桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


<!--HONumber=Jun16_HO5-->


