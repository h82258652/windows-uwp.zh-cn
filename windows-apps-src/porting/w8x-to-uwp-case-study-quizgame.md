---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: 本主题介绍了案例研究的正常运行对等测验游戏 WinRT 8.1 示例将应用移植到 Windows 10 通用 Windows 平台 (UWP) 应用。
title: Windows 运行时 8.x 到 UWP 案例研究，QuizGame 对等示例应用
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d2d1b2b4e6875730d5a6bfa8dd711e11ac5d049c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642912"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Windows 运行时 8.x 到 UWP 案例研究：QuizGame 示例应用




本主题介绍了案例研究的正常运行对等测验游戏 WinRT 8.1 示例将应用移植到 Windows 10 通用 Windows 平台 (UWP) 应用。

一个通用的 8.1 应用程序是指生成两个版本的相同应用程序： 一个应用程序包以进行 Windows 8.1 和 Windows Phone 8.1 的88不同的应用包。 QuizGame 的 WinRT 8.1 版本使用 Universal Windows App 项目安排，但它采用不同的方式，并且为两个平台构建功能不同的应用。 Windows 8.1 应用包作为主机测验游戏会话，而 Windows Phone 8.1 应用包到主机充当客户端的角色。 测验游戏会话的两个部分通过对等网络进行通信。

针对电脑和手机定制两个部分，每个部分都很有意义。 但是，如果你刚好可以在你选择的任何设备上运行主机和客户端不是更好吗？ 在这种情况下研究中，我们将移植到 Windows 10，它们将每个生成放在一个用户可以安装到各种设备上的单个应用程序包的这两个应用。

该应用使用可充分利用视图和视图模型的模式。 由于此明确的分离，此应用的移植过程如你所将看到的一样，非常直观简单。

**请注意**  本示例假定你的网络配置为发送和接收自定义 UDP 多播的数据包数 （大多数家庭网络是，尽管可能不会在工作网络） 进行分组。 该示例还发送和接收 TCP 数据包。

 

**请注意**  时在 Visual Studio 中，打开 QuizGame10，如果看到消息"Visual Studio 更新所需"，然后按照中的步骤[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)。

 

## <a name="downloads"></a>下载

[下载 QuizGame 通用 8.1 应用](https://go.microsoft.com/fwlink/?linkid=532953)。 这是应用在移植之前的初始状态。 

[下载 QuizGame10 Windows 10 应用](https://go.microsoft.com/fwlink/?linkid=532954)。 这是应用刚移植后的状态。 

[在 GitHub 上查看此示例的最新版本](https://github.com/Microsoft/Windows-appsample-quizgame)。

## <a name="the-winrt-81-solution"></a>WinRT 8.1 解决方案


QuizGame（我们将移植的应用）的外观如下。

![在 Windows 上运行的 QuizGame 主机应用](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

在 Windows 上运行的 QuizGame 主机应用

 

![在 Windows Phone 上运行的 QuizGame 客户端应用](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

在 Windows Phone 上运行的 QuizGame 客户端应用

## <a name="a-walkthrough-of-quizgame-in-use"></a>正在使用的 QuizGame 的操作实例

这是对正在使用的应用的简短假设描述，但是如果你希望在无线网络上亲自试用该应用，它将提供有用的信息。

酒吧中正在举行一场有趣的测验。 酒吧中有一台每个人都可以看到的大电视机。 测验主持人有一台电脑，其输出正显示在电视机上。 该电脑上有一个“主机应用”正在运行。 任何想要参与测验的人只需在他们的手机或 Surface 上安装“客户端应用”。

主机应用处于大厅模式，在大电视机上，它宣告客户端应用可以连接。 Joan 在她的移动设备上启动客户端应用。 她将她的名字键入**玩家名称**文本框并点击**加入游戏**。 主机应用通过显示 Joan 的名字来示意她已加入，并且 Joan 的客户端应用指示它正在等待游戏开始。 接下来，Maxwell 在他的移动设备上完成这些相同的步骤。

测验主持人单击**开始游戏**，然后主机应用显示一个问题和可能的答案（它还以正常字体粗细和灰色显示已加入玩家的列表）。 同时，答案显示在已加入客户端设备的按钮上。 Joan 点击上面带有答案“1975”的按钮，此时她的所有按钮都变为禁用状态。 在主机应用上，Joan 的名字被涂成绿色（并且变为粗体），以此示意已收到她的答案。 Maxwell 也给出了答案。 测验主持人注意到所有玩家的名字都变为绿色后，单击**下一个问题**。

以这一相同的循环方式继续提出问题和回答问题。 当最后一个问题显示在主机应用上时，按钮的内容是**显示结果**，而不是**下一个问题**。 当单击**显示结果**时，将显示结果。 通过单击**返回大厅**可返回到游戏开始时的状态，只是已加入的玩家仍然保持加入状态。 但是返回大厅为新玩家提供了加入的机会，甚至为已加入的玩家提供了便于离开的时间（尽管已加入玩家可随时通过点击**离开游戏**离开）。

## <a name="local-test-mode"></a>本地测试模式

若要在单台电脑上而不是跨设备分布地电脑上试用应用及其交互，你可以在本地测试模式下构建主机应用。 通过此模式，完全不必使用网络。 相反，主机应用的 UI 在窗口左侧显示主机部分，在右侧显示两个垂直堆叠的客户端应用 UI 的副本（请注意，在此版本中，对于电脑显示，本地测试模式 UI 是固定的；它不适应小型设备）。 UI 的这些分段（全部在同一个应用中）通过模拟客户端通信器互相通信，这可模拟本应在网络上发生的交互。

若要激活本地测试模式，请将 **LOCALTESTMODEON**（在项目属性中）定义为条件编译符号，并执行重新生成。

## <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 项目

QuizGame 具有以下部分。

-   P2PHelper。 这是一个包含对等网络逻辑的可移植类库。
-   QuizGame.Windows。 这是生成主机应用程序，面向 Windows 8.1 应用包的项目。
-   QuizGame.WindowsPhone。 这是一个为客户端应用构建应用包且面向 Windows Phone 8.1的项目。
-   QuizGame.Shared。 这是包含由其他两个项目同时使用的源代码、标记文件以及其他资源的项目。

对于此案例研究，我们针对要支持的设备提供[如果你有通用 8.1 应用](w8x-to-uwp-root.md)中所述的常用选项。

根据这些选项，我们将向新的 Windows 10 项目调用 QuizGameHost 端口 QuizGame.Windows。 而且，我们将端口 QuizGame.WindowsPhone 连接到名为 QuizGameClient 的新 Windows 10 项目。 这些项目将面向通用设备系列，因此它们将在任何设备上运行。 我们会将 QuizGame.Shared 源文件等保留在它们自己的文件夹中，并且我们会将这些共享文件链接到两个新的项目中。 和以前一样，我们会将所有内容保留在一个解决方案中并将其命名为 QuizGame10。

**QuizGame10 解决方案**

-   创建新的解决方案 (**新的项目** &gt; **其他项目类型** &gt; **Visual Studio 解决方案**) 并将其命名 QuizGame10。

**P2PHelper**

-   在解决方案中，创建一个新 Windows 10 类库项目 (**新的项目** &gt; **Windows Universal** &gt; **类库 （Windows 通用）**) 并将其命名 P2PHelper。
-   从新项目中删除 Class1.cs。
-   将 P2PSession.cs、P2PSessionClient.cs 和 P2PSessionHost.cs 复制到新项目的文件夹中，并在新项目中包括已复制的文件。
-   该项目无需其他更改即可构建。

**共享的文件**

-   常见、 模型、 视图和 ViewModel 中的文件夹复制\\QuizGame.Shared\\到\\QuizGame10\\。
-   Common、Model、View 和 ViewModel 是我们在提到磁盘上的共享文件夹时所指的文件夹。

**QuizGameHost**

-   创建一个新的 Windows 10 应用程序项目 (**外** &gt; **新建项目** &gt; **Windows 通用** &gt; **空白应用程序 （Windows 通用）**) 并将其命名 QuizGameHost。
-   添加对 P2PHelper 的引用 (**添加引用** &gt; **项目** &gt; **解决方案** &gt; **P2PHelper**).
-   在“解决方案资源管理器”中，为磁盘上的每个共享文件夹创建一个新文件夹。 反过来，右键单击刚创建的每个文件夹，然后单击**外** &gt; **现有项**和导航到上一个文件夹。 打开相应的共享文件夹、选择所有文件，然后单击“添加为链接”。
-   复制从 MainPage.xaml \\QuizGame.Windows\\到\\QuizGameHost\\和命名空间更改为 QuizGameHost。
-   复制从 App.xaml \\QuizGame.Shared\\到\\QuizGameHost\\和命名空间更改为 QuizGameHost。
-   我们会将该版本保留在新项目中，并只进行一个定向更改以支持本地测试模式，而不是覆盖 app.xaml.cs。 在 app.xaml.cs 中，将此行代码

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

替换为：

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   在中**属性** &gt; **生成** &gt; **条件编译符号**，添加 LOCALTESTMODEON。
-   现在可以返回到你向 app.xaml.cs 添加的代码并解析 TestView 类型。
-   在 package.appxmanifest 中，将功能名称从 internetClient 更改为 internetClientServer。

**QuizGameClient**

-   创建一个新的 Windows 10 应用程序项目 (**外** &gt; **新建项目** &gt; **Windows 通用** &gt; **空白应用程序 （Windows 通用）**) 并将其命名 QuizGameClient。
-   添加对 P2PHelper 的引用 (**添加引用** &gt; **项目** &gt; **解决方案** &gt; **P2PHelper**).
-   在“解决方案资源管理器”中，为磁盘上的每个共享文件夹创建一个新文件夹。 反过来，右键单击刚创建的每个文件夹，然后单击**外** &gt; **现有项**和导航到上一个文件夹。 打开相应的共享文件夹、选择所有文件，然后单击“添加为链接”。
-   复制从 MainPage.xaml \\QuizGame.WindowsPhone\\到\\QuizGameClient\\和命名空间更改为 QuizGameClient。
-   复制从 App.xaml \\QuizGame.Shared\\到\\QuizGameClient\\和命名空间更改为 QuizGameClient。
-   在 package.appxmanifest 中，将功能名称从 internetClient 更改为 internetClientServer。

你现在可以进行构建并运行。

## <a name="adaptive-ui"></a>自适应 UI

在一个宽窗口 （这是仅可能具有较大屏幕设备上） 中运行应用时，QuizGameHost Windows 10 应用看起来很正常。 但是，当应用的窗口较窄时（会出现在小型设备上，但也可能出现在大型设备上），UI 会因过分拥挤而无法阅读。

我们可以使用自适应的可视状态管理器功能解决相关问题，如我们所述[案例研究：Bookstore2](w8x-to-uwp-case-study-bookstore2.md)。 首先，在视觉元素上设置属性，以便在默认情况下以较窄的状态设置 UI 布局。 所有这些更改会在中召开\\视图\\HostView.xaml。

-   在主 **Grid** 中，将第一个 **RowDefinition** 的 **Height** 从“140”更改为“Auto”。
-   在包含名为 `pageTitle` 的 **TextBlock** 的 **Grid** 上，设置 `x:Name="pageTitleGrid"` 和 `Height="60"`。 其中前两个步骤是为了使我们可以通过视觉状态中的资源库有效控制该 **RowDefinition** 的高度。
-   在 `pageTitle` 上，设置 `Margin="-30,0,0,0"`。
-   在由注释 `<!-- Content -->` 指示的 **Grid** 上，设置 `x:Name="contentGrid"` 和 `Margin="-18,12,0,0"`。
-   在紧挨在注释 `<!-- Options -->` 上方的 **TextBlock** 上，设置 `Margin="0,0,0,24"`。
-   在默认 **TextBlock** 样式（文件中的第一个资源）中，将 **FontSize** 资源库的值更改为“15”。
-   在 `OptionContentControlStyle` 中，将 **FontSize** 资源库的值更改为“20”。 此步骤和上一个步骤将为我们提供一个可在所有设备上良好工作的良好字型渐变。 这些是比我们所使用的 Windows 8.1 应用程序"30"灵活得多的大小。
-   最后，将相应的视觉状态管理器标记添加到根 **Grid**。

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>通用样式设置


您会注意到，在 Windows 10 中，按钮没有其模板中填充相同触摸目标。 可以通过两个小更改修复该问题。 第一，将此标记添加到 QuizGameHost 和 QuizGameClient 中的 app.xaml。

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

其次，将添加到此资源库和`OptionButtonStyle`中\\视图\\ClientView.xaml。

```xml
<Setter Property="Margin" Value="6"/>
```

通过最后一项调整，该应用的行为和外观将与移植前完全相同，此外它现在可在任意位置运行。

## <a name="conclusion"></a>结论

我们在此案例研究中移植的应用是涉及到多个项目、一个类库以及大量代码和用户界面的相对复杂的应用。 即便如此，该移植仍然很简单。 一些移植的易用性是直接由引起 Windows 10 开发人员平台和 Windows 8.1 和 Windows Phone 8.1 平台之间的相似性。 另一些是由于原始应用的设计方式使模型、视图模型和视图保持分离。
