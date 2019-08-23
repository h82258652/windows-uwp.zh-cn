---
description: 本文演示如何使用 XAML 孤岛在 WPF 应用程序中托管自定义 UWP 控件。
title: 使用 XAML 孤岛在 WPF 应用程序中托管自定义 UWP 控件
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛、自定义控件、用户控件、宿主控件
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: a22846c2b0499b990a27b1c445ad36f2ff1a0437
ms.sourcegitcommit: e9dc2711f0a0758727468f7ccd0d0f0eee3363e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69979311"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>使用 XAML 孤岛在 WPF 应用程序中托管自定义 UWP 控件

本文演示如何使用 Windows 社区工具包中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件在面向 .net Core 3 的 WPF 应用程序中托管自定义 UWP 控件。 自定义控件包含几个第一方 UWP 控件, 并将其中一个 UWP 控件中的属性绑定到 WPF 应用程序中的字符串。

尽管本文演示了如何在 WPF 应用程序中执行此操作, 但此过程与 Windows 窗体应用程序类似。 有关在 WPF 中承载 UWP 控件和 Windows 窗体应用的概述, 请参阅[此文](xaml-islands.md#wpf-and-windows-forms-applications)。

## <a name="overview"></a>概述

若要在 WPF 应用程序中托管自定义 UWP 控件, 你将需要以下组件。 本文提供了有关创建每个组件的说明。

* **用于 WPF 应用程序的项目和源代码**。 仅在 WPF 和面向 .NET Core 3 的 Windows 窗体应用中, 才支持使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件托管自定义 UWP 控件。 面向 .NET Framework 的应用不支持此方案。

* **自定义 UWP 控件**。 你需要承载自定义 UWP 控件的源代码, 以便可以将其与你的应用进行编译。 通常, 自定义控件在与 WPF (或 Windows 窗体) 项目相同的解决方案中引用的 UWP 类库项目中定义。

* **定义 XamlApplication 对象的 UWP 应用项目**。 WPF (或 Windows 窗体) 项目必须有权访问 Windows 社区工具包提供的`Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication`类的实例。 此对象用作根元数据提供程序, 用于为应用程序的当前目录中的程序集中的自定义 UWP XAML 类型加载元数据。 执行此操作的建议方法是将**空白应用 (通用 Windows)** 项目添加到与 WPF (或 Windows 窗体) 项目相同的解决方案, 并修改此项目中`App`的默认类。
  > [!NOTE]
  > 你的解决方案只能包含一个定义`XamlApplication`对象的项目。 应用中的所有自定义 UWP 控件共享同`XamlApplication`一个对象。 定义`XamlApplication`对象的项目必须包括对在 XAML 岛中承载 uwp 控件的所有其他 UWP 库和项目的引用。

## <a name="create-a-wpf-project"></a>创建 WPF 项目

在开始之前, 请按照以下说明创建 WPF 项目, 并将其配置为承载 XAML 孤岛。 如果你有现有 WPF 项目, 则可以修改项目的这些步骤和代码示例。

> [!NOTE]
> 如果有一个面向 .NET Framework 的现有项目, 则需要将项目迁移到 .NET Core 3。 有关详细信息, 请参阅[此博客系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

1. 如果尚未这样做, 请安装最新版本的[.Net Core 3 预览版 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)。

2. 在 Visual Studio 2019 中, 创建一个新的**WPF 应用程序 (.Net Core)** 项目。

3. 确保启用[包引用](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files):

    1. 在 Visual Studio 中, 单击 "**工具"-> "NuGet 包管理器-> 包管理器设置**"。
    2. 请确保已为**默认包管理格式**选择**PackageReference** 。

4. 在**解决方案资源管理器**中右键单击 WPF 项目, 然后选择 "**管理 NuGet 包**"。

5. 在 " **NuGet 包管理器**" 窗口中, 确保选择 "**包括预发行**版"。

6. 选择 "**浏览**" 选项卡, 搜索[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)包 (版本 v 6.0.0-preview7 或更高版本), 然后安装包。 此包提供了使用**WindowsXamlHost**控件承载 UWP 控件所需的所有内容, 包括其他相关的 NuGet 包。
    > [!NOTE]
    > Windows 窗体应用必须使用[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)包 (版本 v 6.0.0-preview7 或更高版本)。

7. 配置解决方案以面向特定的平台, 例如 x86 或 x64。 对于以**任何 CPU**为目标的项目, 不支持自定义 UWP 控件。

    1. 在**解决方案资源管理器**中, 右键单击解决方案节点, 然后选择 "**属性** -> " "**配置属性** -> "**Configuration Manager**。 
    2. 在 "**活动解决方案平台**" 下, 选择 "**新建**"。 
    3. 在 "**新建解决方案平台**" 对话框中, 选择 " **X64**或**X86** ", 并按 **"确定"** 。 
    4. 关闭 "打开" 对话框。

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>在 UWP 应用项目中创建 XamlApplication 对象

接下来, 将 UWP 应用项目添加到与 WPF 项目相同的解决方案中。 您将修改此项目`App`中的默认类以派生自 Windows `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication`社区工具包提供的类。 WPF 应用中的**WindowsXamlHost**对象需要此`XamlApplication`对象来承载自定义 UWP 控件。

1. 在**解决方案资源管理器**中, 右键单击解决方案节点, 然后选择 "**添加** -> **新项目**"。
2. 向你的解决方案中添加一个**空白应用（通用 Windows）** 项目。 请确保目标版本和最低版本均设置为**Windows 10 1903 版**或更高版本。
3. 在 UWP 应用项目中, 安装[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 包 (版本 v 6.0.0-preview7 或更高版本)。
4. 打开**app.config**文件, 并将此文件的内容替换为以下 xaml。 替换`MyUWPApp`为 UWP 应用项目的命名空间。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. 打开**App.xaml.cs**文件, 并将此文件的内容替换为以下代码。 替换`MyUWPApp`为 UWP 应用项目的命名空间。

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. 从 UWP 应用项目中删除**MainPage**文件。
7. 生成 UWP 应用项目。
8. 在 WPF 项目中, 右键单击 "**依赖项**" 节点, 并添加对 UWP 应用项目的引用。

## <a name="create-a-custom-uwp-control"></a>创建自定义 UWP 控件

若要在 WPF 应用程序中托管自定义 UWP 控件, 你必须具有控件的源代码, 以便可以使用你的应用进行编译。 自定义控件通常是在 UWP 类库项目中定义的, 以方便实现可移植性。

在本部分中, 将在新的类库项目中定义一个简单的自定义 UWP 控件。 您也可以在上一节中创建的 UWP 应用项目中定义自定义 UWP 控件。 不过, 这些步骤在单独的类库项目中执行此操作是为了便于演示, 因为这通常是实现自定义控件以实现可移植性的方式。 

如果已经有一个自定义控件, 则可以使用它, 而不是此处所示的控件。 但是, 您仍需要配置包含该控件的项目, 如以下步骤所示。

1. 在**解决方案资源管理器**中, 右键单击解决方案节点, 然后选择 "**添加** -> **新项目**"。
2. 将**类库 (通用 Windows)** 项目添加到解决方案。 请确保目标版本和最低版本均设置为**Windows 10 1903 版**或更高版本。
3. 右键单击项目文件, 然后选择 "**卸载项目**"。 再次右键单击项目文件, 然后选择 "**编辑**"。
4. 在结束`</Project>`元素之前添加以下 XML, 以禁用多个属性, 然后保存该项目文件。 必须启用这些属性才能在 WPF (或 Windows 窗体) 应用程序中托管自定义 UWP 控件。

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. 右键单击项目文件, 然后选择 "**重新加载项目**"。
6. 删除默认的**Class1.cs**文件, 并向项目中添加新的**用户控件**项。
7. 在用户控件的 XAML 文件中, 将以下`StackPanel`项添加为默认`Grid`的子项。 此示例将添加``TextBlock``一个控件, 然后将``Text``该控件的属性绑定到``XamlIslandMessage``该字段。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. 在用户控件的代码隐藏文件中, 将`XamlIslandMessage`字段添加到用户控件类, 如下所示。

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. 生成 UWP 类库项目。
10. 在 WPF 项目中, 右键单击 "**依赖项**" 节点, 并添加对 UWP 类库项目的引用。
11. 在之前配置的 UWP 应用项目中, 右键单击 "**引用**" 节点, 然后添加对 UWP 类库项目的引用。
12. 重新生成整个解决方案并确保所有项目都已成功生成。

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>在 WPF 应用程序中托管自定义 UWP 控件

1. 在**解决方案资源管理器**中, 展开 WPF 项目, 然后打开 mainwindow.xaml 文件或您要在其中承载自定义控件的其他窗口。
2. 在 XAML 文件中, 将以下命名空间声明添加到`<Window>`元素。

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 在同一文件中, 将以下控件添加到`<Grid>`元素。 `InitialTypeName`将属性更改为 UWP 类库项目中的用户控件的完全限定名称。

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 打开代码隐藏文件并将以下代码添加到`Window`类。 此代码定义一个`ChildChanged`事件处理程序, 该事件处理程序``XamlIslandMessage``将 UWP 自定义控件的字段值分配`WPFMessage`给 WPF 应用程序中的字段的值。 更改`UWPClassLibrary.MyUserControl`为 UWP 类库项目中的用户控件的完全限定名称。

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. 生成并运行应用, 并确认 UWP 用户控件按预期显示。

## <a name="package-the-app"></a>打包应用程序

可以选择将 WPF 应用打包在[.msix 包](https://docs.microsoft.com/windows/msix)中进行部署。 .MSIX 是适用于 Windows 的新式应用打包技术, 它基于 MSI、APPX、App-v 和 ClickOnce 安装技术的组合。

以下说明介绍了如何使用 Visual Studio 2019 中的[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)将解决方案中的所有组件打包到 .msix 包中。 仅当要将 WPF 应用打包到 .MSIX 包时, 才需要执行这些步骤。 请注意, 这些步骤当前包括特定于托管自定义 UWP 控件的方案的一些解决方法。

1. 向解决方案添加新的[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 创建项目时, 请选择 " **Windows 10, 版本 1903 (10.0;版本 18362)** 用于**目标版本**和**最低版本**。

2. 在打包项目中, 右键单击 "**应用程序**" 节点, 然后选择 "**添加引用**"。 在项目列表中, 选择解决方案中的 WPF 项目, 然后单击 **"确定"** 。

3. 编辑打包项目文件。 这些更改当前需要打包面向 .NET Core 3 且托管 XAML 孤岛的 WPF 应用。

    1. 在解决方案资源管理器中, 右键单击打包项目节点, 然后选择 "**编辑项目文件**"。
    2. 在文件中找到 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。 将此元素替换为以下 XML。 这些更改当前是打包面向 .NET Core 3 并承载 UWP 控件的 WPF 应用程序所必需的。

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. 保存并关闭项目文件。

4. 编辑包清单以引用正确的默认初始屏幕图像。 此解决方法目前是打包自定义 UWP 控件的 WPF 应用程序所必需的。

    1. 在打包项目中, 右键单击**appxmanifest.xml**文件, 然后单击 "**查看代码**"。
    2. 在文件中找到以下元素。

        ```<uap:SplashScreen Image="Images\SplashScreen.png" />```

    3. 将此元素更改为:

        ```<uap:SplashScreen Image="Images\SplashScreen.scale-200.png" />```

    4. 保存**appxmanifest.xml**文件并将其关闭。

5. 编辑 WPF 项目文件。 这些更改当前是打包自定义 UWP 控件的 WPF 应用程序所必需的。

    1. 在解决方案资源管理器中, 右键单击 WPF 项目节点, 然后选择 "**卸载项目**"。
    2. 右键单击 WPF 项目节点, 然后选择 "**编辑**"。
    3. 定位到文件`</PropertyGroup>`中的最后一个结束标记, 并在该标记的后面添加以下 XML。

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. 保存并关闭项目文件。
    5. 右键单击 WPF 项目节点, 然后选择 "**重新加载项目**"。

6. 生成并运行打包项目。 确认 WPF 运行, UWP 自定义控件按预期方式显示。

## <a name="related-topics"></a>相关主题

* [桌面应用程序中的 UWP 控件](xaml-islands.md)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
