---
description: 本文演示如何使用 XAML 孤岛在 WPF 应用程序中承载标准 UWP 控件。
title: 使用 XAML 孤岛在 WPF 应用中托管标准 UWP 控件
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛、包装控件、标准控件、InkCanvas、InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: cdaaa20b28a7f181467f6047bc93350ec40b366a
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317074"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>使用 XAML 孤岛在 WPF 应用中托管标准 UWP 控件

本文演示了在 WPF 应用中使用[XAML 孤岛](xaml-islands.md)承载标准 UWP 控件（即，由 Windows SDK 或 WinUI 库提供的第一方 uwp 控件）的两种方式：

* 它演示了如何使用 Windows 社区工具包中的[包装控件](xaml-islands.md#wrapped-controls)来承载 UWP [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)和[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)控件。 这些控件包装一小部分有用 UWP 控件的界面和功能。 您可以将它们直接添加到您的 WPF 或 Windows 窗体项目的设计图面上，然后在设计器中将其与任何其他 WPF 或 Windows 窗体控件一起使用。

* 它还演示了如何通过使用 Windows 社区工具包中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件来承载 UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)控件。 因为只有一小部分 UWP 控件作为包装控件提供，所以可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)托管任何其他标准 UWP 控件。

若要在 WPF 应用程序中承载 UWP 控件，你将需要以下组件。 本文提供了有关创建每个组件的说明。

* 用于 WPF 应用程序的项目和源代码。
* 一个 UWP 应用项目，该项目通过 Windows 社区`Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication`工具包定义类的实例。
  > [!NOTE]
  > 若要确保你的应用程序在所有 XAML 应用场景中都能正常工作，WPF （或 Windows 窗体）项目必须`XamlApplication`具有对对象的访问权限。 此对象用作根元数据提供程序，用于在应用程序的当前目录中的程序集中加载 UWP XAML 类型的元数据。 执行此操作的建议方法是将**空白应用（通用 Windows）** 项目添加到与 WPF （或 Windows 窗体）项目相同的解决方案，并修改此项目中`App`的默认类以派生自。 `XamlApplication`
  >
  > 虽然此步骤不是常用 xaml 岛方案（如承载第一方 uwp 控件）所必需的，但 WPF 应用`XamlApplication`需要此对象来支持全套 xaml 岛方案，包括托管自定义 uwp 控件。 建议你始终添加 UWP 项目，并在使用 XAML 孤岛`XamlApplication`的任何解决方案中定义对象。 你的解决方案只能包含一个定义`XamlApplication`对象的项目。 应用中的所有自定义 UWP 控件共享同`XamlApplication`一个对象。

虽然本文演示如何在 WPF 应用程序中托管 UWP 控件，但该过程与 Windows 窗体应用程序类似。

## <a name="create-a-wpf-project"></a>创建 WPF 项目

在开始之前，请按照以下说明创建 WPF 项目，并将其配置为承载 XAML 孤岛。 如果你有现有 WPF 项目，则可以修改项目的这些步骤和代码示例。

1. 在 Visual Studio 2019 中，创建一个新的**Wpf 应用程序（.NET Framework）** 或**wpf 应用程序（.net Core）** 项目。 若要创建**WPF 应用（.Net core）** 项目，必须首先安装最新版本的[.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)。

2. 确保启用[包引用](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，单击 "**工具"-> "NuGet 包管理器-> 包管理器设置**"。
    2. 请确保已为**默认包管理格式**选择**PackageReference** 。

3. 在**解决方案资源管理器**中右键单击 WPF 项目，然后选择 "**管理 NuGet 包**"。

4. 在 " **NuGet 包管理器**" 窗口中，确保选择 "**包括预发行**版"。

5. 选择 "**浏览**" 选项卡，搜索 "6.0.0 [" 包（](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)版本 v preview7 或更高版本），然后安装包。 此包提供使用适用于 WPF 的已包装 UWP 控件（包括[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)以及[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件）所需的所有内容。
    > [!NOTE]
    > Windows 窗体应用必须使用 6.0.0[包（](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)版本 v preview7 或更高版本）。

6. 配置解决方案以面向特定的平台，例如 x86 或 x64。 大多数 XAML 孤岛方案在面向**任何 CPU**的项目中不受支持。

    1. 在**解决方案资源管理器**中，右键单击解决方案节点，然后选择 "**属性** -> " "**配置属性** -> "**Configuration Manager**。 
    2. 在 "**活动解决方案平台**" 下，选择 "**新建**"。 
    3. 在 "**新建解决方案平台**" 对话框中，选择 " **X64**或**X86** "，并按 **"确定"** 。 
    4. 关闭 "打开" 对话框。

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>在 UWP 应用项目中创建 XamlApplication 对象

接下来，将 UWP 应用项目添加到与 WPF 项目相同的解决方案中。 您将修改此项目`App`中的默认类以派生自 Windows `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication`社区工具包提供的类。 虽然此步骤不是日常 xaml 岛方案所必需的（例如托管单个第一方 UWP 控件），但 WPF 应用`XamlApplication`需要此对象来支持各种 xaml 岛方案。 建议你始终将此项目添加到使用 XAML 孤岛的任何解决方案。

1. 在**解决方案资源管理器**中，右键单击解决方案节点，然后选择 "**添加** -> **新项目**"。
2. 向你的解决方案中添加一个**空白应用（通用 Windows）** 项目。 请确保目标版本和最低版本均设置为**Windows 10 1903 版**或更高版本。
3. 在 UWP 应用项目中，安装[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 包（版本 v 6.0.0-preview7 或更高版本）。
4. 打开**app.config**文件，并将此文件的内容替换为以下 xaml。 替换`MyUWPApp`为 UWP 应用项目的命名空间。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. 打开**App.xaml.cs**文件，并将此文件的内容替换为以下代码。 替换`MyUWPApp`为 UWP 应用项目的命名空间。

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
8. 在 WPF 项目中，右键单击 "**依赖项**" 节点，并添加对 UWP 应用项目的引用。

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>使用包装的控件承载 InkCanvas 和 InkToolbar

现在，已将项目配置为使用 UWP XAML 孤岛，接下来可以将[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)包装的 UWP 控件添加到应用中。

1. 在**解决方案资源管理器**中，打开**mainwindow.xaml**文件。

2. 在 XAML 文件顶部附近的**Window**元素中, 添加以下属性。 这将引用[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)包装的 UWP 控件的 XAML 命名空间。

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    添加此属性后， **Window**元素现在应类似于此。

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. 在**mainwindow.xaml**文件中，将现有`<Grid>`元素替换为以下 xaml。 此 XAML 向添加[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)控件（前面作为命名空间定义的**Controls**关键字的前缀） `<Grid>`。

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > 还可以通过将这些控件和其他已包装控件从**工具箱**的 " **Windows 社区工具包**" 部分拖到设计器来将它们添加到该窗口中。

4. 保存**mainwindow.xaml**文件。

    如果设备支持数字笔（如表面），并且在物理计算机上运行此实验室，则现在可以使用笔生成并运行应用，并在屏幕上绘制数字墨迹。 但是, 如果没有支持笔的设备, 并且尝试使用鼠标进行签名, 则不会发生任何事情。 发生这种情况的原因是, 默认情况下, **InkCanvas**控件仅对数字笔启用。 但是，您可以更改此行为。

5. 打开**MainWindow.xaml.cs**文件。

6. 将以下命名空间声明添加到文件的顶部：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. 找到该`MainWindow()`构造函数。 在方法的`InitializeComponent()`后面添加以下代码行, 并保存代码文件。

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    你可以使用**InkPresenter**对象自定义默认墨迹体验。 此代码使用**InputDeviceTypes**属性启用鼠标和笔输入。

8. 再次按 F5 以在调试器中重新生成并运行应用。 如果使用的是带鼠标的计算机，请确认您可以用鼠标在 ink 画布空间中绘制一些内容。

## <a name="host-a-calendarview-by-using-the-host-control"></a>使用宿主控件托管 CalendarView

现在，已将[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)包装的 UWP 控件添加到该应用程序，现在可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件将[CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)添加到该应用程序。

> [!NOTE]
> [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件是由[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)包提供的控件。 此包包含在前面安装的 " [Microsoft 工具包](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)" 包中。

1. 在**解决方案资源管理器**中，打开**mainwindow.xaml**文件。

2. 在 XAML 文件顶部附近的**Window**元素中, 添加以下属性。 这将引用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件的 XAML 命名空间。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    添加此属性后， **Window**元素现在应类似于此。

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. 在**mainwindow.xaml**文件中，将现有`<Grid>`元素替换为以下 xaml。 此 XAML 向网格中添加一行，并将[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)对象添加到最后一行。 若要承载 UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)控件，此 XAML 会将`InitialTypeName`属性设置为控件的完全限定名称。 此 XAML 还定义`ChildChanged`事件的事件处理程序，该事件是在呈现托管控件时引发的。

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. 保存**mainwindow.xaml**文件并打开**MainWindow.xaml.cs**文件。

7. 将以下命名空间声明添加到文件的顶部：

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. 向`MainWindow`类添加`ChildChanged`以下事件处理程序方法，并保存代码文件。 当宿主控件已呈现时，此事件处理程序将`SelectedDatesChanged`运行并为日历控件的事件创建一个简单的事件处理程序。

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. 再次按 F5 以在调试器中重新生成并运行应用。 确认 "日历" 控件现在显示在窗口的底部。

## <a name="package-the-app"></a>打包应用程序

可以选择将 WPF 应用打包在[.msix 包](https://docs.microsoft.com/windows/msix)中进行部署。 .MSIX 是适用于 Windows 的新式应用打包技术，它基于 MSI、APPX、App-v 和 ClickOnce 安装技术的组合。

以下说明介绍了如何使用 Visual Studio 2019 中的[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)将解决方案中的所有组件打包到 .msix 包中。 仅当要将 WPF 应用打包到 .MSIX 包时，才需要执行这些步骤。

1. 向解决方案添加新的[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 创建项目时，请选择 " **Windows 10，版本1903（10.0;版本18362）** 用于**目标版本**和**最低版本**。

2. 在打包项目中，右键单击 "**应用程序**" 节点，然后选择 "**添加引用**"。 在项目列表中，选择解决方案中的 WPF 项目，然后单击 **"确定"** 。

3. 如果 WPF 项目面向 .NET Core 3，则必须编辑打包项目文件。 这些更改当前是打包面向 .NET Core 3 并承载 UWP 控件的 WPF 应用程序所必需的。

    1. 在解决方案资源管理器中, 右键单击打包项目节点, 然后选择 "**编辑项目文件**"。
    2. 在文件中找到 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。 将此元素替换为以下 XML。

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

4. 配置解决方案以面向特定的平台，例如 x86 或 x64。 这是使用 Windows 应用程序打包项目将 WPF 应用程序生成到 .MSIX 包所必需的。

    1. 在**解决方案资源管理器**中，右键单击解决方案节点，然后选择 "**属性** -> " "**配置属性** -> "**Configuration Manager**。
    2. 在 "**活动解决方案平台**" 下，选择 " **x64**或**x86**"。
    3. 在 WPF 项目的行中，在 "**平台**" 列中选择 "**新建**"。
    4. 在 "**新建解决方案平台**" 对话框中，选择 " **x64** " 或 " **X86** " （您为**活动解决方案平台**选择的平台），然后单击 **"确定"** 。
    5. 关闭 "打开" 对话框。

5. 生成并运行打包项目。 确认 WPF 运行，UWP 自定义控件按预期方式显示。

## <a name="related-topics"></a>相关主题

* [桌面应用程序中的 UWP 控件](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
