---
description: 本文演示如何使用 XAML 岛在 WPF 应用中托管自定义 UWP 控件。
title: 使用 XAML 岛在 WPF 应用中托管自定义 UWP 控件
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10，uwp，windows 窗体，wpf，xaml 岛，自定义控件，用户控件，托管控件
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b1ac53e0a6b6e01cd2129e2b1893f91fae2ef0fe
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218597"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>使用 XAML 岛在 WPF 应用中托管自定义 UWP 控件

本文演示了如何使用 Windows 社区工具包中的 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件在面向 .NET Core 3 的 WPF 应用中托管自定义 UWP 控件。 此自定义控件包含 Windows SDK 中的多个第三方 UWP 控件，并将其中一个 UWP 控件的属性绑定到 WPF 应用中的字符串。 此外，本文还说明了如何托管 [WinUI 库](https://docs.microsoft.com/uwp/toolkits/winui/)中的 UWP 控件。

尽管本文介绍了如何在 WPF 应用中实现此操作，但该过程类似于在 Windows 窗体中实现。 有关在 WPF 和 Windows 窗体应用中托管 UWP 控件的概述，请参阅[此文](xaml-islands.md#wpf-and-windows-forms-applications)。

## <a name="required-components"></a>必需的组件

若要在 WPF（或 Windows 窗体）应用中托管自定义 UWP 控件，解决方案中需要有以下组件。 本文提供了创建各个组件的说明。

* **项目和应用源代码**。 只能在面向 .NET Core 3 的应用中使用 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件来托管自定义 UWP 控件。 面向 .NET Framework 的应用不支持此方案。

* **自定义 UWP 控件**。 必须有要托管的自定义 UWP 控件的源代码，才能通过应用程序对其进行编译。 通常，在 UWP 类库项目中定义自定义控件，WPF 或 Windows 窗体项目的同一解决方案会引用此项目。

* **定义派生自 XamlApplication 的根应用程序类的 UWP 应用项目**。 WPF 或 Windows 窗体项目必须有权访问 Windows 社区工具包提供的 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 类的实例。 建议使用 WPF 或 Windows 窗体应用解决方案中单独的 UWP 应用项目定义此对象，来实现访问。 此对象充当根元数据提供程序，用于在应用程序当前目录的程序集中加载自定义 UWP XAML 类型的元数据。

    > [!NOTE]
    > 解决方案只能包含一个定义 `XamlApplication` 对象的项目。 应用中的所有自定义 UWP 控件共享同一个 `XamlApplication` 对象。 定义 `XamlApplication` 对象的项目必须包含在 XAML 岛上托管 UWP 控件所用的所有其他 UWP 库和项目的引用。

## <a name="create-a-wpf-project"></a>创建 WPF 项目

开始操作前，先按照下面的说明创建 WPF 项目，并对它进行配置，用于托管 XAML 岛。 如果你有现有的 WPF 项目，则可以将这些步骤和代码示例用于你的项目。

> [!NOTE]
> 若有现有的面向 .NET Framework 的项目，需要将此项目迁移到 .NET Core 3。 有关详细信息，请参阅[此博客系列文章](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

1. 若尚未创建项目，请安装 [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) 的最新版本。

2. 在 Visual Studio 2019 中，新建一个“WPF 应用(.NET Core)”项目  。

3. 确保已启用[包引用](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，单击“工具”->“NuGet 程序包管理器”->“程序包管理器设置”  。
    2. 确保为“默认程序包管理格式”选择了“PackageReference”   。

4. 在“解决方案资源管理器”中，右键单击相应的 WPF 项目并选择“管理 NuGet 包”   。

5. 在“NuGet 程序包管理器”窗口中，确保已选中“包括预发行版”   。

6. 选择“浏览”选项卡，搜索 [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) 包（版本 v6.0.0 或更高版本），并安装此包  。 此包会提供使用 WindowsXamlHost 控件托管 UWP 控件所需的全部内容，包括其他相关的 NuGet 程序包  。
    > [!NOTE]
    > Windows 窗体应用必须使用 [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) 包（版本 v6.0.0 或更高版本）。

7. 将解决方案配置为面向特定平台，例如 x86 或 x64。 面向任意 CPU 的项目不支持自定义 UWP 控件  。

    1. 在“解决方案资源管理器”中，右键单击相应的解决方案节点，选择“属性” -> “配置属性” -> “配置管理器”     。
    2. 在“活动解决方案平台”  下，选择“新建”  。 
    3. 在“新建解决方案平台”对话框中，选择“x64”或“x86”，并按“确认”     。 
    4. 关闭打开的对话框。

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>在 UWP 应用项目中定义 XamlApplication 类

接下来，将 UWP 应用项目添加到解决方案，并将此项目中的默认 `App` 类修改为派生自 Windows 社区工具包提供的 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 类。 应用会将此类用作根元数据提供程序，用于在应用程序当前目录的程序集中加载自定义 UWP XAML 类型的元数据。

1. 在“解决方案资源管理器”中，右键单击解决方案节点，然后选择“添加” -> “新建项目”    。
2. 向你的解决方案中添加一个空白应用（通用 Windows）  项目。 确保目标版本和最低版本均设置为 Windows 10 版本 1903 或更高版本  。
3. 在 UWP 应用项目中，安装 [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 程序包（版本 v6.0.0 或更高版本）。
4. 打开 App.xaml 文件，将此文件的内容替换为以下 XAML  。 将 `MyUWPApp` 替换为 UWP 应用项目的命名空间。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. 打开 App.xaml.cs 文件，将此文件的内容替换为以下代码  。 将 `MyUWPApp` 替换为 UWP 应用项目的命名空间。

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. 从 UWP 应用项目删除 MainPage.xaml 文件  。
7. 清理 UWP 应用项目，然后生成它。
8. 在 WPF 项目中，右键单击“依赖项”节点，添加对 UWP 应用项目的引用  。

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>在 WPF 应用入口点中实例化 XamlApplication 对象

接下来，将代码添加到 WPF 应用入口点，用于创建刚刚在 UWP 项目中定义的 `App` 类的实例（现在此类派生自 `XamlApplication`）。 此对象充当根元数据提供程序，用于在应用程序当前目录的程序集中加载自定义 UWP XAML 类型的元数据。

1. 在 WPF 项目中，右键单击项目节点，选择“添加” -> “新建项目”，然后选择“类”    。 将类命名为“Program”，然后单击“添加”   。

2. 将生成的 `Program` 类替换为以下代码，然后保存文件。 将 `MyUWPApp` 替换为 UWP 应用项目的命名空间，并将 `MyWPFApp` 替换为 WPF 应用项目的命名空间。

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. 右键单击项目节点，然后选择“属性”  。

4. 在属性的“应用程序”选项卡中，单击“启动对象”下拉列表，选择在上一步中添加的 `Program` 类的完全限定的名称   。 
    > [!NOTE]
    > 默认情况下，WPF 项目在不可修改的生成代码文件中定义 `Main` 入口点函数。 此步骤将项目的入口点更改为新 `Program` 类的 `Main` 方法，这样添加的代码即可在应用启动过程中尽可能早地运行。 

5. 保存对项目属性的更改。

## <a name="create-a-custom-uwp-control"></a>创建自定义 UWP 控件

若要在 WPF 应用中托管自定义 UWP 控件，必须有此控件的源代码，才能通过应用对其进行编译。 通常在 UWP 类库项目中定义自定义控件，以便于移植。

此部分将在新的类库项目中定义一个简单的自定义 UWP 控件。 你也可以在上一部分中创建的 UWP 应用项目中定义自定义 UWP 控件。 不过，出于说明目的，以下步骤将在单独的类库中完成，因为为便于移植通常会以此方式实现自定义控件。

若已有自定义控件，可以使用它替代本文所示的控件。 不过，你仍需按照下面的步骤所示配置包含此控件的项目。

1. 在“解决方案资源管理器”中，右键单击解决方案节点，然后选择“添加” -> “新建项目”    。
2. 向解决方案添加一个“类库(通用 Windows)”  项目。 确保目标版本和最低版本均设置为 Windows 10 版本 1903 或更高版本  。
3. 右键单击该项目文件，然后选择“上传项目”  。 再次右键单击该项目文件，然后选择“编辑”。 
4. 在结尾的 `</Project>` 元素前面，添加以下 XML，以禁用多个属性，然后保存项目文件。 若要在 WPF（或 Windows 窗体）应用中托管自定义 UWP 控件，必须启用这些属性。

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. 右键单击该项目文件，然后选择“重新加载项目”  。
6. 删除默认的 Class1.cs 文件，将新的“用户控件”项目添加到项目。  
7. 在此用户控件的 XAML 文件中，添加下面的 `StackPanel` 作为默认 `Grid` 的子级。 此示例添加 ``TextBlock`` 控件，然后将该控件的 ``Text`` 属性绑定到 ``XamlIslandMessage`` 字段。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. 在用户控件的代码隐藏文件中，将 `XamlIslandMessage` 字段添加到用户控件类中，如下所示。

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
10. 在 WPF 项目中，右键单击“依赖项”节点，添加对 UWP 类库项目的引用。 
11. 在先前配置的 UWP 应用项目中，右键单击“引用”节点，添加对 UWP 类库项目的引用。 
12. 重新生成整个解决方案，确保所有项目成功生成。

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>在 WPF 应用中托管自定义 UWP 控件

1. 在“解决方案资源管理器”中，展开相应的 WPF 项目，打开 MainWindow.xaml 文件或想要用于托管自定义控件的其他窗口。 
2. 在 XAML 文件中，将以下命名空间声明添加到 `<Window>` 元素。

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 在同一个文件中，将以下控件添加到 `<Grid>` 元素。 将 `InitialTypeName` 属性更改为 UWP 类库项目中用户控件的完全限定名称。

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 打开代码隐藏文件，然后将以下代码添加到 `Window` 类。 此代码定义一个 `ChildChanged` 事件处理程序，来将 UWP 自定义控件的 ``XamlIslandMessage`` 字段的值分配到 WPF 应用的 `WPFMessage` 字段的值。 将 `UWPClassLibrary.MyUserControl` 更改为 UWP 类库项目中用户控件的完全限定名称。

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

6. 生成并运行应用，确认 UWP 用户控件按预期方式显示。

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>将 WinUI 库中的控件添加到自定义控件

按照传统，UWP 控件已作为 Windows 10 操作系统的一部分发布，并且已通过 Windows SDK 向开发人员提供。 [WinUI 库](https://docs.microsoft.com/uwp/toolkits/winui/)是备用方法，它将 Windows SDK 中 UWP 控件的更新版分发在未与 Windows SDK 版本关联的 NuGet 程序包中。 此库还包含不属于 Windows SDK 和默认 UWP 平台的新控件。 有关详细信息，请参阅 [WinUI 路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

此部分演示了如何将 WinUI 库中的 UWP 控件添加到用户控件，以便于将此控件托管在 WPF 应用中。

1. 在 UWP 应用项目中，安装 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 程序包的最新版本。

2. 在此项目的 App.xaml 文件中，将以下子元素添加到 `<xaml:XamlApplication>` 元素。

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    添加此元素后，现在此文件的内容应如下所示。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. 在 UWP 类库项目中，安装 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 程序包的最新版本（与安装到 UWP 应用项目的版本相同）。

4. 在同一个项目中，打开用户控件的 XAML 文件，将以下命名空间声明添加到 `<UserControl>` 元素。

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 在同一个文件中，添加 `<winui:RatingControl />` 元素作为 `<StackPanel>` 的子级。 此元素会添加 WinUI 库中 [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2) 类的实例。 添加此元素后，现在 `<StackPanel>` 应如下所示。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. 生成并运行应用，确认新评分控件按预期方式显示。

## <a name="package-the-app"></a>打包应用

可以选择在 [MSIX 包](https://docs.microsoft.com/windows/msix)中打包 WPF 应用以供部署。 MSIX 是适用于 Windows 的新式应用打包技术，并且基于 MSI、.appx、App-V 和 ClickOnce 安装技术的组合。

下面的说明介绍了如何在 Visual Studio 2019 中使用 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)将解决方案中的所有组件打包到 MSIX 包。 只有在需要将 WPF 应用打包到 MSIX 包时，才需要使用这些步骤。 请注意，目前这些步骤包含一些特定于托管自定义 UWP 控件方案的解决方法。

> [!NOTE]
> 如果选择不在 [MSIX 包](https://docs.microsoft.com/windows/msix)中打包应用程序以供部署，则运行应用的计算机必须安装有 [Visual C++ 运行时](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)。

1. 向解决方案中添加一个新的 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 创建项目时，针对“目标版本”和“最低版本”选择“Windows 10 版本 1903 (10.0；版本 18362)”    。

2. 在打包项目中，右键单击“应用程序”节点，然后选择“添加引用”   。 在项目列表中，选择解决方案中的 WPF 项目，然后单击“确认”  。

3. 编辑此 WPF 项目文件。 若要打包托管自定义 UWP 控件的 WPF 应用，目前需要完成以下更改。

    1. 在“解决方案资源管理器”中右键单击 WPF 项目节点，然后选择“卸载项目”。 
    2. 右键单击 WPF 项目节点，选择“编辑”。 
    3. 找到文件的最后一个 `</PropertyGroup>` 结束标记，紧随该标记之后添加以下 XML。

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. 保存并关闭项目文件。
    5. 右键单击 WPF 项目节点，选择“重新加载项目”。 

4. 生成并运行打包项目。 确认 WPF 按预期运行，并且 UWP 自定义控件按预期方式显示。

## <a name="related-topics"></a>相关主题

* [在桌面应用中托管 UWP XAML 控件（XAML 岛）](xaml-islands.md)
* [XAML 岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
