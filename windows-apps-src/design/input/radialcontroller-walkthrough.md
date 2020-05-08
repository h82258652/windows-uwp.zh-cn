---
ms.assetid: ''
title: 支持 Windows 应用中的外围网络（和其他轮设备）
description: 添加对你的 Windows 应用的 Surface 拨号（和其他轮设备）的支持的分步教程。
keywords: 转盘, 径向, 教程
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 74bb75fb6bced451daeb6f03fba78636d0998cec
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970272"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-windows-app"></a>教程：在 Windows 应用程序中支持 Surface 拨号（和其他轮设备）

![适配 Surface Studio 的 Surface Dial 的图像](images/radialcontroller/dial-pen-studio-600px.png)  
*适配 Surface Studio 和 Surface 触控笔的 Surface Dial*（可通过 [Microsoft 官方商城](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)购买）。

此教程逐步介绍如何自定义 Surface Dial 等滚轮设备支持的用户交互体验。 我们使用可以从 GitHub 下载的示例应用中的代码段（参阅[示例代码](#sample-code)），来展示各个步骤所讨论的各种功能和关联的 [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) API。

我们主要介绍以下内容：
* 指定哪些内置工具在 [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 菜单中显示
* 将自定义工具添加到菜单
* 控制触觉反馈
* 自定义单击交互
* 自定义旋转交互

有关实现这些功能和其他功能的详细信息，请参阅[Windows 应用中的外围网络交互](windows-wheel-interactions.md)。

## <a name="introduction"></a>介绍

Surface Dial 是一款辅助输入设备，与主要输入设备（如触控笔、触摸或鼠标）一起使用可帮助用户提高效率。 作为辅助输入设备，Dial 通常与非惯用手结合使用，提供对系统命令和其他与上下文更相关的工具和功能的访问。 

Dial 支持三种基本手势： 
- 长按显示命令的内置菜单。
- 旋转以突出显示菜单项（如果菜单处于活动状态）或在应用中修改当前操作（如果菜单处于非活动状态）。
- 单击以选择突出显示的菜单项（如果菜单处于活动状态）或在应用中调用命令（如果菜单处于非活动状态）。

## <a name="prerequisites"></a>必备条件

* 运行 Windows 10 创意者更新或更高版本的计算机（或虚拟机）
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 滚轮设备（现在仅限 [Surface Dial](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)）
* 如果你不熟悉 Visual Studio 的 Windows 应用应用开发，请在开始本教程之前，先了解以下主题：  
    * [准备工作](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [创建“Hello, world”应用 \(XAML\)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>设置设备

1. 确保 Windows 设备开启。
2. 转到**开始**，选择**设置** > **设备** > **蓝牙和其他设备**，然后打开**蓝牙**。
3. 取下 Surface Dial 底部，打开电池盒，确保内装两节 AAA 电池。
4. 如果 Dial 底面出现电池标签，将其去除。
5. 长按电池旁边的小嵌入按钮，直到蓝牙灯闪烁。
6. 返回到 Windows 设备，选择**添加蓝牙或其他设备**。
7. 在**设备添加**对话框中，选择**蓝牙** > **Surface Dial**。 Surface Dial 现在应已连接，并被添加到**蓝牙和其他设备**设置页中**鼠标、键盘和笔**下的设备列表中。
8. 长按 Dial 几秒钟以显示内置菜单，通过此方法对 Dial 进行测试。
9. 如果屏幕上未显示菜单（"拨号" 也应振动），请返回蓝牙设置，删除设备，并尝试重新连接设备。

> [!NOTE]
> 可以通过**滚轮**设置配置滚轮设备：
> 1. 在**开始**菜单上，选择**设置**。
> 2. 选择**设备** > **滚轮**。    
> ![滚轮设置屏幕](images/radialcontroller/wheel-settings.png)

现在，你已准备好开始此教程了。 

## <a name="sample-code"></a>示例代码
在本指南中，我们全部使用示例应用来演示所讨论的概念和功能。

在 [windows-appsample-get-started-radialcontroller 示例](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController)从 [GitHub](https://github.com/) 下载此 Visual Studio 示例和源代码：

1. 选择绿色**克隆或下载**按钮。  
![克隆存储库](images/radialcontroller/wheel-clone.png)
2. 如果有 GitHub 帐户，则可以通过选择 "**在 Visual Studio 中打开**" 将存储库克隆到本地计算机。 
3. 如果没有 GitHub 帐户，或者只是想要项目的本地副本，请选择 "**下载 ZIP** " （需要定期检查以下载最新更新）。

> [!IMPORTANT]
> 示例中的大部分代码已被注释掉。在我们介绍本主题中的各个步骤时，系统将要求你取消代码各个部分的注释。 在 Visual Studio 中，只需突出显示代码行，并按 CTRL-K，然后按 CTRL-U。

## <a name="components-that-support-wheel-functionality"></a>支持滚轮功能的组件

这些对象为 Windows 应用程序提供了滚轮设备体验。

| 组件 | 说明 |
| --- | --- |
| [**RadialController** 类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)和相关项 | 表示滚轮输入设备或附件，例如 Surface Dial。 |
| [**IRadialControllerConfigurationInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop) / [**IRadialControllerInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>我们不在这里介绍此功能，有关详细信息，请参阅 [Windows 经典桌面示例](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)。 | 启用与 Windows 应用的互操作性。 |

## <a name="step-1-run-the-sample"></a>步骤 1：运行示例

下载 RadialController 示例应用后，确认它在运行：
1. 在 Visual Studio 中打开示例项目。
2. 将**解决方案平台**下拉列表设置为非 ARM 选择。
3. 按 F5 编译、部署和运行。 

> [!NOTE]
> 或者，可以选择 "**调试** > " "**开始调试**" 菜单项，或选择此处显示的 "**本地计算机**运行" 按钮： ![Visual Studio "生成项目" 按钮](images/radialcontroller/wheel-vsrun.png)

应用窗口打开，在初始屏幕出现几秒钟后，你将看到此初始屏幕。

![空应用](images/radialcontroller/wheel-app-step1-empty.png)

好了，我们现在有了基本的 Windows 应用程序，我们将在本教程的其余部分中使用。 在以下步骤中，我们添加 **RadialController** 功能。

## <a name="step-2-basic-radialcontroller-functionality"></a>步骤 2：基本 RadialController 功能

在前台运行和的应用程序时，按下图面拨号以显示**RadialController**菜单。

我们尚未对应用进行任何自定义设置，所以菜单包含一组默认的上下文工具。 

这些图像显示默认菜单的两个变体。 （还有许多其他工具，包括当 Windows 桌面处于活动状态且前台没有应用时呈现的基本系统工具、InkToolbar 呈现时显示的其他墨迹书写工具，以及当你使用“地图”应用时呈现的地图工具。

| RadialController 菜单（默认）  | RadialController 菜单（媒体播放时默认显示）  |
|---|---|
| ![默认 RadialController 菜单](images/radialcontroller/wheel-app-step2-basic-default.png) | ![随音乐显示的默认 RadialController 菜单](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

现在，我们将开始执行一些基本的自定义。

## <a name="step-3-add-controls-for-wheel-input"></a>步骤 3：添加滚轮输入的控件

首先，我们来为应用添加 UI：

1. 打开 MainPage_Basic.xaml 文件。
2. 查找标记有此步骤标题的代码（"\<!--步骤3：添加色轮输入的控件-->"）。
3. 取消以下各行的注释。

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
此时，仅启用**初始化示例**按钮、滑块和切换开关。 其他按钮在后续步骤中用于添加和删除 **RadialController** 菜单项（提供对滑块和切换开关的访问）。

![基本示例应用 UI](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>步骤 4：自定义基本 RadialController 菜单

现在我们来添加支持对我们的控件进行 **RadialController** 访问所需的代码。

1. 打开 MainPage_Basic.xaml.cs 文件。
2. 找到标有此步骤标题的代码 ("// Step 4: Basic RadialController menu customization")。
3. 取消以下各行的注释：
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) 和 [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) 类型引用用于后续步骤中的功能：  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - 这些全局对象（[RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)、[RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration)、[RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)）用于整个应用。
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - 在这里，我们为启用控件并初始化自定义 **RadialController** 菜单项的按钮指定 **Click** 处理程序。

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - 接下来，我们初始化 [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 对象，并设置 [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 和 [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 事件的处理程序。

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - 在这里，我们初始化自定义 RadialController 菜单项。 我们使用 [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) 获取对 [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 对象的引用，使用 [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) 属性将旋转灵敏度设置为“1”，然后使用 [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph) 创建 [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)，我们将菜单项添加到 **RadialController** 菜单项集合，最后，我们使用 [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) 清除默认菜单项，只保留我们的自定义工具。 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. 现在，再次运行应用。
5. 选择**初始化径向控制器**按钮。  
6. 当应用处于前台中时，长按 Surface Dial 显示菜单。 请注意，所有默认工具（使用 **RadialControllerConfiguration.SetDefaultMenuItems** 方法）均已删除，只保留了自定义工具。 下面是包含自定义工具的菜单。 

| RadialController 菜单（自定义）  | 
|---|
| ![自定义 RadialController 菜单](images/radialcontroller/wheel-app-step3-custom.png) |

7. 选择自定义工具，并试用一下现在通过 Surface Dial 支持的交互：
    * 旋转操作移动滑块。 
    * 单击将切换开关设置为开启或关闭。

好了，我们来关联这些按钮。

## <a name="step-5-configure-menu-at-runtime"></a>步骤 5：在运行时配置菜单

在此步骤中，我们将关联**添加/删除项目**和**重置 RadialController 菜单**按钮，以向你展示如何能够动态自定义菜单。

1. 打开 MainPage_Basic.xaml.cs 文件。
2. 找到标有此步骤标题的代码 ("// Step 5: Configure menu at runtime")。
3. 取消下列方法中代码的注释并重新运行应用，但不选择任何按钮（保存供下一步骤使用）。  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. 选择**删除项目**按钮，然后长按 Dial 再次显示菜单。

    请注意，菜单现在包含默认的工具集合。 回想一下，在步骤 3 中，在设置自定义菜单时，我们删除了所有默认工具，只添加了我们的自定义工具。 我们还注意到，当菜单设置为空集合时，当前上下文的默认项目都将恢复。 （我们在删除默认工具前添加了自定义工具。）

5. 选择**添加项目**按钮，然后长按 Dial。

    请注意，菜单现在包含默认工具集合和我们的自定义工具。

6. 选择**重置 RadialController 菜单**按钮，然后长按 Dial。

    请注意，菜单返回原始状态。

## <a name="step-6-customize-the-device-haptics"></a>步骤 6：自定义设备触觉
Surface Dial 和其他滚轮设备可以向用户提供与当前交互对应的触觉反馈（基于单击或旋转）。

在此步骤中，我们向你展示，如何通过关联滑块和切换开关控件并使用它们动态指定触觉反馈行为，来自定义触觉反馈。 对于此示例，切换开关必须设置为开启以已启用反馈，滑块值指定单击反馈的重复频率。 

> [!NOTE]
> 用户可以在 "**设置** >  " "**设备** > "**滚动**页中禁用 Haptic 反馈。

1. 打开 App.xaml.cs 文件。
2. 找到标有此步骤标题的代码 ("Step 6: Customize the device haptics")。
3. 为第一行和第三行（“MainPage_Basic”和“MainPage”）添加注释，取消第二行（“MainPage_Haptics”）的注释。  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. 打开 MainPage_Haptics.xaml 文件。
5. 查找此步骤标题标记的代码（"\<!--步骤6：自定义设备 haptics-->"）。
6. 取消以下各行的注释。 （此 UI 代码仅指示当前设备支持哪些触觉功能。）    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. 打开 MainPage_Haptics.xaml.cs 文件。
8. 找到标有此步骤标题的代码 ("Step 6: Haptics customization")
9. 取消以下各行的注释：  

    - [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics) 类型引用用于后续步骤中的功能。  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - 在这里，我们指定选择自定义 **RadialController** 菜单项时触发的 [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 事件的处理程序。

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - 接下来，我们定义 [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 处理程序，在这个过程中我们禁用默认的触觉反馈并初始化我们的触觉 UI。

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - 在 [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 和 [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 事件处理程序中，我们将相应的滑块和切换按钮控件连接到我们的自定义触觉。 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - 最后，我们为触觉反馈获得请求的 **[Waveform](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)**（如果受支持）。 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

现在重新运行应用，更改滑块值和切换开关的状态，尝试一下自定义触觉。

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>步骤 7：为 Surface Studio 和类似设备定义屏幕交互
与 Surface Studio 搭配使用，Surface Dial 可提供更为独特的用户体验。 

除了所述的默认长按菜单体验之外，还可以将 Surface Dial 直接置于 Surface Studio 的屏幕上。 这会启用一个特殊的“屏幕”菜单。 

系统通过检测 Surface Dial 的接触位置和边界来处理设备的遮挡，并环绕 Surface Dial 的外侧显示更大号的菜单。 应用还可以使用这一相同信息针对设备的存在及其预期用途（例如用户的手和臂的放置）排布 UI。 

此教程附带的示例中有一个略微复杂的示例，用来展示这些功能中的一部分功能。

若要查看操作过程（你需要一台 Surface Studio）：

1. 在 Surface Studio 设备（已安装 Visual Studio）上下载示例
2. 在 Visual Studio 中打开示例
3. 打开 App.xaml.cs 文件
4. 找到标有此步骤标题的代码 ("Step 7: Define on-screen interactions for Surface Studio and similar devices")
5. 为第一行和第二行（“MainPage_Basic”和“MainPage_Haptics”）添加注释，取消第三行（“MainPage”）的注释。  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. 运行应用，将 Surface Dial 放在两个控制区域，两个区域来回交替。    
![屏幕 RadialController](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    以下是此示例的操作过程的视频：  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>总结

恭喜，你已完成*入门教程：在 Windows 应用程序中支持 Surface 拨号（和其他轮设备）*！ 我们向您展示了在 Windows 应用程序中支持滑轮设备所需的基本代码，以及如何提供**RadialController** api 支持的一些更丰富的用户体验。

## <a name="related-articles"></a>相关文章

[Surface Dial 交互](windows-wheel-interactions.md)

### <a name="api-reference"></a>API 参考

- [**RadialController**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs**类](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon**枚举](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind**枚举](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>示例

#### <a name="topic-samples"></a>主题示例

[自定义 RadialController](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>其他示例
[着色书籍示例](https://github.com/Microsoft/Windows-appsample-coloringbook)

[通用 Windows 平台示例（C# 和 C++）](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Windows 经典桌面示例](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
