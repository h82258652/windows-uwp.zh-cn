---
author: normesta
Description: "本指南介绍了如何配置 Visual Studio 解决方案来编辑、调试和打包用于桌面桥的桌面应用。"
Search.Product: eADQiWindows 10XVcnh
title: "使用 Visual Studio 打包应用（桌面桥）"
ms.author: normesta
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.openlocfilehash: d8919448b965f18ff7f8fdaeda325889e495ef85
ms.sourcegitcommit: f6dd9568eafa10ee5cb2b849c0d82d84a1c5fb93
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2017
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>使用 Visual Studio 打包应用（桌面桥）

可以使用 Visual Studio 为你的桌面应用生成一个包。 然后，可将该包发布到 Windows 应用商店或将其侧向加载到一台或多台个人电脑中。

本指南演示如何设置你的解决方案，然后为桌面应用程序生成包。

## <a name="first-consider-how-youll-distribute-your-app"></a>首先，请考虑你要如何分发应用

如果你打算将应用发布到 [Windows 应用商店](https://www.microsoft.com/store/apps)，请先填写[此表单](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)。 Microsoft 将联系你以开始加入过程。 在此过程中，你需要在应用商店中预留一个名称，然后获取对应用打包所需的信息。

## <a name="add-a-packaging-project-to-your-solution"></a>向你的解决方案中添加一个打包项目

1. 在 Visual Studio 中，打开包含你的桌面应用程序项目的解决方案。

2. 向你的解决方案中添加一个 JavaScript **空白应用（通用 Windows）**项目。

   你无需向该项目中添加任何代码。 该项目只用于为你生成包。 我们将此项目称为“打包项目”。

   ![javascript UWP 项目](images/desktop-to-uwp/javascript-uwp-project.png)

   >[!IMPORTANT]
   >通常，应使用此项目的 JavaScript 版本。  C#、VB.NET 和 C++ 版本存在一些问题，但如果要使用这些版本，请在使用前参阅[已知问题](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor)指南。

## <a name="add-the-desktop-application-binaries-to-the-packaging-project"></a>向打包项目中添加桌面应用程序二进制文件

将二进制文件直接添加到打包项目中。

1. 在**解决方案资源管理器**中，展开打包项目文件夹，创建一个子文件夹，并将其命名为任意名称（例如，**win32**）。

2. 右键单击该子文件夹，然后选择**添加现有项目**。

3. 在**添加现有项目**对话框中，从桌面应用程序的输出文件夹中查找并添加文件。 这不只包括可执行文件，还包括位于该文件夹内的任何 dll 或 .config 文件。

   ![引用可执行文件](images/desktop-to-uwp/cpp-exe-reference.png)

   每当你对桌面应用程序项目进行更改时，都必须将这些文件的新版本复制到打包项目中。 你可以通过向打包项目的项目文件中添加生成后事件来自动完成此操作。 下面提供了一个示例。

   ```XML
   <Target Name="PostBuildEvent">
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe.config"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.pdb"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.dll"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.pdb"
       DestinationFolder="win32" />
   </Target>
   ```

## <a name="modify-the-package-manifest"></a>修改包清单

打包项目包含一个用于描述包设置的文件。 默认情况下，该文件会描述一个 UWP 应用，因此你必须对其进行修改，以使系统了解你的包含有以完全信任方式运行的桌面应用程序。  

1. 在**解决方案资源管理器**中，展开打包项目，右键单击 **package.appxmanifest** 文件，然后选择**查看代码**。

   ![引用 dotnet 项目](images/desktop-to-uwp/reference-dotnet-project.png)

2. 将此命名空间添加到文件顶部，并将命名空间前缀添加到 ``IgnorableNamespaces`` 列表中。

   ```XML
   xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
   ```
   完成后，你的命名空间声明将如下所示：

   ```XML
   <Package
     xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
     xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
     xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
     xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
     IgnorableNamespaces="uap mp rescap">
   ```

3. 找到 ``TargetDeviceFamily`` 元素，将 ``Name`` 属性设置为 **Windows.Desktop**，将 ``MinVersion`` 属性设置为打包项目的最低版本，并将 ``MaxVersionTested`` 设置为打包项目的目标版本。

   ```XML
   <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.10586.0" MaxVersionTested="10.0.15063.0" />
   ```

   可在打包项目的属性页中找到最低版本和目标版本。

   ![最低和目标版本设置](images/desktop-to-uwp/min-target-version-settings.png)


4. 从 ``Application`` 元素中删除 ``StartPage`` 属性。 然后添加 ``Executable`` 和 ``EntryPoint`` 属性。

   ``Application`` 元素将类似如下。

   ```XML
   <Application Id="App"  Executable=" " EntryPoint=" ">
   ```

5. 将 ``Executable`` 属性设置为桌面应用程序的可执行文件的名称。 然后，将 ``EntryPoint`` 属性设置为 **Windows.FullTrustApplication**。

   ``Application`` 元素将类似如下。

   ```XML
   <Application Id="App"  Executable="win32\MyWindowsFormsApplication.exe" EntryPoint="Windows.FullTrustApplication">
   ```
6. 将 ``runFullTrust`` 功能添加到 ``Capabilities`` 元素中。

   ```XML
     <rescap:Capability Name="runFullTrust"/>
   ```
   此声明下方可能会显示蓝色的波浪标记，但可以安全地忽略。

   >[!IMPORTANT]
   如果要为 C++ 桌面应用程序创建包，则需对清单文件进行一些额外的更改，以便将 Visual C++ 运行时与你的应用一起部署。 请参阅[在桌面桥项目中使用 Visual C++ 运行时](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

7. 生成打包项目，以确保未显示任何错误。

8. 如果要对包进行测试，请参阅[运行、调试和测试打包的桌面应用（桌面桥）](desktop-to-uwp-debug.md)。

   然后，返回本指南并查看下一部分来生成包。

## <a name="generate-a-package"></a>生成包

要为你的应用生成包，请按照本主题中所述的指南操作：[打包 UWP 应用](..\packaging\packaging-uwp-apps.md)。

在到达**选择并配置程序包**屏幕时，在选择任何复选框之前请花些时间考虑要在包中包含哪种二进制文件。

* 如果你已通过向解决方案中添加基于 C#、C++ 或 VB.NET 的通用 Windows 平台项目来[扩展](desktop-to-uwp-extend.md)桌面应用程序，请选中 **x86** 和 **x64** 复选框。  

* 否则，请选中**中性**复选框。

>[!NOTE]
你必须明确选择每个受支持平台的原因是，你已扩展的解决方案包含两种类型的二进制文件；一种用于 UWP 项目，另一种用于桌面项目。 由于这些是不同类型的二进制文件，因此 .NET Native 需要为每个平台显式生成本机二进制文件。

如果你在尝试生成包时收到错误，请参阅[已知问题](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor)指南；如果该列表中未显示你的问题，请[在此处](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)与我们分享该问题。

## <a name="next-steps"></a>后续步骤

**运行应用/查找和修复问题**

请参阅[运行、调试和测试打包的桌面应用（桌面桥）](desktop-to-uwp-debug.md)

**通过添加 UWP API 来增强桌面应用**

请参阅[增强用于 Windows 10 的桌面应用程序](desktop-to-uwp-enhance.md)

**通过添加 UWP 组件来扩展桌面应用**

请参阅[使用新式 UWP 组件扩展桌面应用程序](desktop-to-uwp-extend.md)。

**分发应用**

请参阅[分发打包的桌面应用（桌面桥）](desktop-to-uwp-distribute.md)

**查找特定问题的答案**

我们的团队会监视这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供关于本文的反馈**

请使用下面的评论区。
