---
author: awkoren
Description: "本指南介绍了如何配置 Visual Studio 解决方案来编辑、调试和打包转换的桌面桥应用。"
Search.Product: eADQiWindows 10XVcnh
title: "适用于使用 Visual Studio 创建的 .NET 桌面应用的桌面桥打包指南"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8aa68312d6ce81c809c79ddcafe7732944a628be
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-packaging-guide-for-net-desktop-apps-with-visual-studio"></a>适用于使用 Visual Studio 创建的 .NET 桌面应用的桌面桥打包指南

Windows 10 周年更新允许开发人员使用桌面桥打包使用新包模型 (.appx) 的现有 Win32 应用，其中启用了应用商店发布或轻松旁加载。 本指南介绍了如何配置 Visual Studio 解决方案，以便编辑、调试和打包你的应用。 

若要开始使用，请在[使用桌面桥将现有应用和游戏发布到 Windows 应用商店](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)中填写表格。 Microsoft 将联系你以开始培训过程。 你的帐户已被批准提交桌面桥应用后，请按照本文档中的说明准备 appxupload 程序包以上传。 

> 在使用桌面桥期间是否反馈了遇到的问题？ 最好在 [Windows 开发人员 UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) 上提出功能建议。 有关问题和错误报告，请转到[开发通用 Windows 应用论坛](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop)。

## <a name="default-universal-windows-platform-packages"></a>默认通用 Windows 平台包

通过 Visual Studio 可以生成可使用 Windows 应用商店或应用旁加载分发的调试和发布包。 为了便于创建包，Visual Studio 可帮助你创建一个 已准备就绪可提交到应用商店的 appxupload 文件。 有关详细信息，请参阅[打包 UWP 应用](..\packaging\packaging-uwp-apps.md)。

## <a name="desktop-bridge-packages"></a>桌面桥包

[桌面桥](desktop-to-uwp-root.md)允许设置其他配置以将 Win32 二进制文件集成到应用程序包 (appx) 中。 我们可以将跨桌面桥的进展看作是一个旅程，该旅程通过四步即可完成。 

- **第 1 步 - 转换**：打包现有 Win32 二进制文件（代码未做改动或有微小改动）。
- **第 2 步 - 增强**：通过从现有 Win32 代码中引用 Windows.winmd，将一些基本的 UWP 功能（例如动态磁贴）添加到现有应用。
- **第 3 步 - 扩展**：为应用添加高级 UWP 功能（如后台任务）。 如果使用托管的语言（如 C# 或 VB.Net）构建 UWP 和 Win32 组件，则生成的包将包含混合二进制文件；此时你需要小心处理这些文件，从而保证正确的 .NET Native 处理。 
- **步骤 4 - 迁移**：你已将 UI 迁移到现代的 XAML 和 C#/VB.NET，但仍具有旧的 Win32 代码。 入口点现在是一个 UWP.NET 可执行文件，但使用某些 Win32 API 的包仍包含二进制文件。

下表总结了每个步骤中应用的不同之处。 

| 步骤 | 二进制文件 | 入口点 | .NET Native | F5 调试 |
|---|---|---|---|---|
| 1（转换） | Win32 | Win32 | 不适用 | VS 扩展 |
| 2（增强） | Refs WinMD | Win32 | 不适用 | VS 扩展 |
| 3（扩展） | Win32 + CoreCLR（*） | Win32 | 通过用户 (**) | VS 扩展 |
| 4（迁移）    | CoreCLR (*) + Win32 | UWP | 通过用户 (**) | VS |
| 5 (UWP) | CoreCLR | UWP |通过应用商店 | VS |

(*) [CoreCLR](https://github.com/dotnet/coreclr) 指用托管语言 (C#/VB.NET) 编写的 UWP 组件依赖的 .NET Core 运行时。 这些组件还将需要 .NET Native 处理。

(**) 在步骤 3 和 4 中，用户应该处理 CoreCLR 程序集以产生 .NET Native 二进制文件和相应的符号，然后再发布到应用商店。

## <a name="configure-your-visual-studio-solution"></a>配置 Visual Studio 解决方案

Visual Studio 包含配置应用程序包所需的工具，例如清单编辑器和包创建向导。 若要使用这些工具，则需要将为你的应用充当 appx 容器的 UWP 项目。 虽然你可以使用任意 UWP 项目（包括 C#、VB.NET、C++ 或 JavaScript），但是 C#、VB.NET 和 C++ 项目却有些已知问题（请参阅本文档后面的[已知问题](#known-issues-anchor)部分），所以在此示例中，我们将使用 JavaScript。 

若要在 appx 应用程序模型上下文中调试你的应用，则将需要添加另一个将启用 F5 appx 调试的项目。 有关详细信息，请参阅[调试桌面桥应用](#debugging-anchor)部分。

让我们从该旅程的第 1 步开始。

### <a name="step-1-convert"></a>第 1 步：转换

这一步介绍了如何从现有 Win32 项目中创建桌面桥应用。 在此示例中，我们将使用基本的 WinForms 项目，该项目对注册表执行读取和写入操作。

![](images/desktop-to-uwp/net-1.png)

#### <a name="add-the-uwp-project"></a>添加 UWP 项目 

若要创建桌面桥包，请将 JavaScript UWP 项目添加到同一个解决方案。

> 注意：尽管我们使用的是 JavaScript UWP 模板，但并不将编写任何 JavaScript 代码。 我们只是将该项目用作一个工具。

![](images/desktop-to-uwp/net-2.png)

#### <a name="add-the-win32-binaries-to-the-win32-folder"></a>将 Win32 二进制文件添加到 win32 文件夹

将在一个名为 win32（不要求必须使用此名称，你可以使用你喜欢的任何名称）的文件夹中的 UWP 项目中存储所有 Win32 二进制文件。

如果你使用的是 Visual Studio，你可以将项目设置为每个内部版本后自动复制文件，从而改善开发工作流程。 编辑你的项目文件（在本示例中为 .csproj），以包含复制所有 Win32 输出文件到 UWP 项目中的 win32 文件夹的 AfterBuild 目标，如下所示： 

```xml
  <Target Name="AfterBuild">
    <PropertyGroup>
      <TargetUWP>..\MyDesktopApp.Package\win32\</TargetUWP>
    </PropertyGroup>
     <ItemGroup>
       <Win32Binaries Include="$(TargetDir)\*" />
     </ItemGroup>
    <Copy SourceFiles="@(Win32Binaries)" DestinationFolder="$(TargetUWP)" />
  </Target>
```

如果你使用另一工具产生 Win32 二进制文件，则只需将运行时需要的所有文件复制到 win32 文件夹即可。 

#### <a name="edit-the-app-manifest-to-enable-the-desktop-bridge-extensions"></a>编辑应用清单以启用桌面桥扩展

此模板包含可用于添加桌面桥扩展的 package.appxmanifest。 若要编辑该文件，请右键单击并选择“视图代码”，然后添加或修改这些项目： 

- `<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" IgnorableNamespaces="uap rescap">`

- `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" MaxVersionTested="10.0.14393.0" />`

- `<rescap:Capability Name="runFullTrust" />`

- `<Application Id="MyDesktopAppStep1" Executable="win32\MyDesktopApp.exe" EntryPoint="Windows.FullTrustApplication">`

下面是完整的清单文件示例： 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"

        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
        IgnorableNamespaces="uap rescap mp">
  <Identity Name="MyDesktopAppStep1"
            ProcessorArchitecture="x64"
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
            Version="1.0.0.5" />
  <mp:PhoneIdentity PhoneProductId="6f6600a4-6da1-4d91-b493-35808d01f8de" PhonePublisherId="00000000-0000-0000-0000-000000000000" />
  <Properties>
    <DisplayName>MyDesktopAppStep1</DisplayName>
    <PublisherDisplayName>CN=Test</PublisherDisplayName>
    <Logo>Assets\SampleAppx.150x150.png</Logo>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" 
                        MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
  <Applications>
    <Application Id="MyDesktopAppStep1" 
                 Executable="win32\MyDesktopApp.exe" 
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyDesktopAppStep1" 
                          Description="MyDesktopAppStep1" 
                          BackgroundColor="#777777" 
                          Square150x150Logo="Assets\SampleAppx.150x150.png" 
                          Square44x44Logo="Assets\SampleAppx.44x44.png">
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

#### <a name="configure-the-win32-binaries"></a>配置 Win32 二进制文件

若要将应用所需的二进制文件添加到输出包，请在 Visual Studio 中选择每个文件。 将其“内容”属性和内部版本行为设置为“如果较新则复制”。 

![](images/desktop-to-uwp/net-3.png)

如果要避免将二进制文件提交到源代码存储库，则可以使用 .gitignore 文件排除 win32 文件夹中的所有文件。 

#### <a name="optional-use-wildcards-to-specify-the-files-in-your-win32-folder"></a>可选：使用通配符指定 win32 文件夹中的文件

如果 Win32 应用所需多个文件，你可以编辑你的项目文件，指定通配符，从而基于通配符表达式指定哪些文件应被标记为“内容”。 你需要使用文本编辑器打开 . Jsproj 文本并包括所需文件，如下所示：

```xml
<Content Include="win32\*.dll">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.exe">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.config">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.pdb">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
```

### <a name="step-2-enhance"></a>第 2 步：增强

如果要从 Win32 代码调用可用的 UWP API，你需要添加对 `\Program Files (x86)\Windows Kits\10\UnionMetadata\Windows.winmd` 的引用。 你的应用可使用的 UWP API 完整列表已在文章[使用桌面桥转换的应用支持的 UWP API](desktop-to-uwp-supported-api.md) 中列出。  

因为在 Windows 10 中不需要该文件，因此你无需分发。 在引用属性中，将“复制本地”属性设置为 false。

![](images/desktop-to-uwp/net-4.png)

若要添加 Win32 二进制文件，请使用第 1 步中的说明进行操作。 

### <a name="step-3-extend"></a>第 3 步：扩展 

例如，我们将使用后台任务扩展一个 Win32 应用。 这需要在 UWP 应用的 package.appxmanifest 中注册后台任务并添加对实现后台任务的项目的引用，如下所示。

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" 
              EntryPoint="BackgroundTasks.MyBackgroundTask">
    <BackgroundTasks>
      <Task Type="timer" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

如果使用 C# 或 VB.NET 实现后台任务，则所生成的输出将包含需要先由 .NET Native 工具链处理，再提交到应用商店的 CoreCLR 二进制文件，如第 3 步和第 4 步中所述。 创建含混合二进制文件的 appxupload。

### <a name="step-4-migrate"></a>第 4 步：迁移

此方案已经具有一个 C# UWP 入口点，因此无需添加其他 UWP 项目。 但是，你需要按照第 1 步中的步骤添加并配置 Win32 二进制文件。

若要执行 Win32 过程，请使用 [**FullTrustProcessLauncher**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.FullTrustProcessLauncher) API。 你需要将桌面扩展和 *fullTrustProcess* 功能添加到应用清单以使用此 API，如下： 

```xml
..
xmlns:desktop=http://schemas.microsoft.com/appx/manifest/desktop/windows10
..
<desktop:Extension Category="windows.fullTrustProcess" 
                    Executable="win32\MyDesktopApp.exe" />
```

## <a name="generate-packages-for-your-desktop-bridge-app"></a>生成桌面桥应用包

按照上面的说明进行操作后，你应准备好使用 Visual Studio 生成你的包，如[打包 UWP 应用](..\packaging\packaging-uwp-apps.md)中所述。 

### <a name="steps-1-and-2-create-appxupload-with-win32-binaries"></a>第 1 步和第 2 步：创建含 Win32 二进制文件的 appxupload

若要提交具有 *fullTrust* 功能的包，你需要生成 appxupload 文件，此文件需包含 appxsym 文件中的各平台符号和一个包含 appx 平台包的捆绑包。

在第 1 步和第 2 步中，你的包不包含任何 CoreCLR 二进制文件，因此无需担心选择哪个平台。 选择“中性”和“版本（任何 CPU）”，如下图所示。

![](images/desktop-to-uwp/net-5.png)

选择“生成应用商店包”选项后，向导将生成准备好提交到应用商店的 appxupload 文件。

### <a name="step-3-and-4-create-appxupload-with-mixed-binaries"></a>第 3 步和第 4 步：创建含混合二进制文件的 appxupload

你还应该构建发布版，并且在这种情况下，必须指定目标平台，因为 .NET Native 需要为每个平台生成本机二进制文件。

![](images/desktop-to-uwp/net-6.png)

若要新建 appxupload 文件，则需创建一个新的 zip 存档以包含 _Test 文件夹中生成的 appxsym 和 appxbundle。

创建包含 appxsym 和 appxbundle 文件的新 zip 文件，然后将扩展名重命名为 appxupload。

![](images/desktop-to-uwp/net-7.png)

<span id="debugging-anchor" />
## <a name="debugging-your-desktop-bridge-app"></a>调试桌面桥应用

尽管无需调试 (Ctrl + F5) 就能从 Visual Studio 中启动你的项目，但有一个已知问题，即 Visual Studio 无法自动附加到正在运行的进程。 但是，你可以将稍后使用后续附加方法之一进行附加：

### <a name="attach-to-the-running-app"></a>附加到正在运行的应用

#### <a name="attach-to-an-existing-process"></a>附加到现有进程

在使用 Ctrl + F5 成功启动应用后，便可以附加到 Win32 进程；但是，你将无法调试 .NET Native 模块。 

![](images/desktop-to-uwp/net-8.png)

#### <a name="attach-to-an-installed-app"></a>附加到安装的应用

你还可以使用选项“调试”->“其他调试目标”->“调试安装的应用包”来附加到任意现有 Appx 包。

![](images/desktop-to-uwp/net-9.png)

可以选择本地计算机或连接到远程计算机。

![](images/desktop-to-uwp/net-10.png)

使用此选项，你应该能够调试 .NET Native 代码。

### <a name="use-visual-studio-extension-to-debug-your-desktop-bridge-app"></a>使用 Visual Studio 扩展调试桌面桥应用 

如果你偏好使用 F5 应用进行调试，则需要从 Visual Studio 库安装 Visual Studio 2017 扩展[桌面桥调试项目](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.DesktoptoUWPPackagingProject)。

此项目允许你调试使用 Visual Studio（如本文档中所述）或者使用 Desktop App Converter 迁移到 UWP 的任何 Win32 应用。

#### <a name="add-the-debugging-project-to-your-solution"></a>将调试项目添加到你的解决方案

首先将新的桌面桥调试项目添加你解决方案中的项目。

![](images/desktop-to-uwp/net-11.png)

若要配置此项目，你需要在用于调试的每个配置/平台的属性窗口中定义 PackageLayout 属性。
若要配置调试/x86，我们会将包布局属性设置为使用相对路径 `..\MyDesktopApp.Package\bin\x86\Debug` 的 UWP 项目的 bin\x86\debug 文件夹。 

![](images/desktop-to-uwp/net-12.png)

然后编辑 AppXFileLayout.xml 文件以指定入口点：

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" 
         xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <MyProjectOutputPath>$(PackageLayout)</MyProjectOutputPath>
  </PropertyGroup>
  <ItemGroup>
    <LayoutFile Include="$(MyProjectOutputPath)\win32\MyDesktopApp.exe">
      <PackagePath>$(PackageLayout)\win32\MyDesktopApp.exe</PackagePath>
    </LayoutFile>
  </ItemGroup>
</Project>
```

最后，配置解决方案相关性，确保项目按正确顺序生成。 

作为一个示例，让我们来看一下在第 3 步中创建的解决方案。

![](images/desktop-to-uwp/net-13.png)

若要配置生成顺序，你可以使用项目相关性配置。 右键单击你的解决方案，然后选择“项目相关性”选项。 设置正确的相关性后，便可以验证生成顺序，如下所示（适用于步骤 3）：

![](images/desktop-to-uwp/net-14.png)

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>C#/VB.NET 和 C++ UWP 项目的已知问题

如果你偏好使用 C# 项目打包你的应用，则需要注意以下已知问题。 

- **在调试中生成应用导致错误：Microsoft.Net.CoreRuntime.targets(235,5)：错误：不支持含自定义入口点可执行文件的应用程序。 查看程序包清单中的应用程序元素的 Executable 属性。** 请改为使用发布模式，以寻求解决。

- **在发布中删除存储在 UWP 项目根文件夹中的 Win32 二进制文件**。 如果你不使用文件夹存储 Win32 二进制文件，则 .NET Native 编译器将从最终程序包中删除这些文件，继而导致清单验证错误，因为无法找到可执行文件入口点。


