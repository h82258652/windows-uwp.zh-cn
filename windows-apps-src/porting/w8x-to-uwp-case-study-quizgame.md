---
author: mcleblanc
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: "本主题介绍了一个将正在运行的对等测验游戏 WinRT 8.1 示例应用移植到 Windows 10 通用 Windows 平台 (UWP) 应用的案例研究。"
title: "Windows 运行时 8.x 到 UWP 案例研究，QuizGame 对等示例应用"
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: cd05c3edbc254cceb00c55caba698d21998f5594

---

# Windows 运行时 8.x 到 UWP 案例研究：QuizGame 对等示例应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题介绍了一个将正在运行的对等测验游戏 WinRT 8.1 示例应用移植到 Windows 10 通用 Windows 平台 (UWP) 应用的案例研究。

通用 8.1 应用是生成同一应用的两个版本的应用：一个应用包用于 Windows 8.1，另一个应用包用于 Windows Phone 8.1。 QuizGame 的 WinRT 8.1 版本使用 Universal Windows App 项目安排，但它采用不同的方式，并且为两个平台构建功能不同的应用。 Windows 8.1 应用包可用作测验游戏会话的主机，而 Windows Phone 8.1 应用包用作该主机的客户端。 测验游戏会话的两个部分通过对等网络进行通信。

针对电脑和手机定制两个部分，每个部分都很有意义。 但是，如果你刚好可以在你选择的任何设备上运行主机和客户端不是更好吗？ 在此案例研究中，我们将两个应用都移植到 Windows 10，它们将在其中分别生成到单个应用包中，用户可将该应用包安装到种类广泛的设备上。

该应用使用可充分利用视图和视图模型的模式。 由于此明确的分离，此应用的移植过程如你所将看到的一样，非常直观简单。

**注意** 此示例假定你的网络已配置为发送和接收自定义 UDP 组多播数据包（大部分家庭网络都是如此，尽管你的工作网络可能不是）。 该示例还发送和接收 TCP 数据包。

 

**注意** 在 Visual Studio 中打开 QuizGame10 时，如果你看到消息“需要 Visual Studio 更新”，则按照 [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md#targetplatformversion) 中的步骤进行操作。

 

## 下载

[下载 QuizGame 通用 8.1 应用](http://go.microsoft.com/fwlink/?linkid=532953)。 这是应用在移植之前的初始状态。 

[下载 QuizGame10 Windows 10 应用](http://go.microsoft.com/fwlink/?linkid=532954)。 这是应用刚移植后的状态。 

[在 GitHub 上查看此示例的最新版本](https://github.com/Microsoft/Windows-appsample-quizgame)。

## WinRT 8.1 解决方案


QuizGame（我们将移植的应用）的外观如下。

![在 Windows 上运行的 QuizGame 主机应用](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

在 Windows 上运行的 QuizGame 主机应用

 

![在 Windows Phone 上运行的 QuizGame 客户端应用](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

在 Windows Phone 上运行的 QuizGame 客户端应用

## 正在使用的 QuizGame 的操作实例

这是对正在使用的应用的简短假设描述，但是如果你希望在无线网络上亲自试用该应用，它将提供有用的信息。

酒吧中正在举行一场有趣的测验。 酒吧中有一台每个人都可以看到的大电视机。 测验主持人有一台电脑，其输出正显示在电视机上。 该电脑上有一个“主机应用”正在运行。 任何想要参与测验的人只需在他们的手机或 Surface 上安装“客户端应用”。

主机应用处于大厅模式，在大电视机上，它宣告客户端应用可以连接。 Joan 在她的移动设备上启动客户端应用。 她将她的名字键入“玩家名称”****文本框并点击“加入游戏”****。 主机应用通过显示 Joan 的名字来示意她已加入，并且 Joan 的客户端应用指示它正在等待游戏开始。 接下来，Maxwell 在他的移动设备上完成这些相同的步骤。

测验主持人单击“开始游戏”****，然后主机应用显示一个问题和可能的答案（它还以正常字体粗细和灰色显示已加入玩家的列表）。 同时，答案显示在已加入客户端设备的按钮上。 Joan 点击上面带有答案“1975”的按钮，此时她的所有按钮都变为禁用状态。 在主机应用上，Joan 的名字被涂成绿色（并且变为粗体），以此示意已收到她的答案。 Maxwell 也给出了答案。 测验主持人注意到所有玩家的名字都变为绿色后，单击“下一个问题”****。

以这一相同的循环方式继续提出问题和回答问题。 当最后一个问题显示在主机应用上时，按钮的内容是“显示结果”****，而不是“下一个问题”****。 当单击“显示结果”****时，将显示结果。 通过单击“返回大厅”****可返回到游戏开始时的状态，只是已加入的玩家仍然保持加入状态。 但是返回大厅为新玩家提供了加入的机会，甚至为已加入的玩家提供了便于离开的时间（尽管已加入玩家可随时通过点击“离开游戏”****离开）。

## 本地测试模式

若要在单台电脑上而不是跨设备分布地电脑上试用应用及其交互，你可以在本地测试模式下构建主机应用。 通过此模式，完全不必使用网络。 相反，主机应用的 UI 在窗口左侧显示主机部分，在右侧显示两个垂直堆叠的客户端应用 UI 的副本（请注意，在此版本中，对于电脑显示，本地测试模式 UI 是固定的；它不适应小型设备）。 UI 的这些分段（全部在同一个应用中）通过模拟客户端通信器互相通信，这可模拟本应在网络上发生的交互。

若要激活本地测试模式，请将 **LOCALTESTMODEON**（在项目属性中）定义为条件编译符号，并执行重新生成。

## 移植到 Windows 10 项目

QuizGame 具有以下部分。

-   P2PHelper。 这是一个包含对等网络逻辑的可移植类库。
-   QuizGame.Windows。 这是一个为主机应用构建应用包且面向 Windows 8.1的项目。
-   QuizGame.WindowsPhone。 这是一个为客户端应用构建应用包且面向 Windows Phone 8.1的项目。
-   QuizGame.Shared。 这是包含由其他两个项目同时使用的源代码、标记文件以及其他资源的项目。

对于此案例研究，我们针对要支持的设备提供[如果你有通用 8.1 应用](w8x-to-uwp-root.md#if-you-have-an-81-universal-windows-app)中所述的常用选项。

基于这些选项，我们会将 QuizGame.Windows 移植到名为 QuizGameHost 的新 Windows 10 项目。 并且我们会将 QuizGame.WindowsPhone 移植到名为 QuizGameClient 的新 Windows 10 项目。 这些项目将面向通用设备系列，因此它们将在任何设备上运行。 我们会将 QuizGame.Shared 源文件等保留在它们自己的文件夹中，并且我们会将这些共享文件链接到两个新的项目中。 和以前一样，我们会将所有内容保留在一个解决方案中并将其命名为 QuizGame10。

**QuizGame10 解决方案**

-   创建一个新的解决方案（“新建项目”****&gt;“其他项目类型”****&gt;“Visual Studio 解决方案”****）并将其命名为 QuizGame10。

**P2PHelper**

-   在解决方案中，创建一个新的 Windows 10 类库项目（“新建项目”****&gt;“Windows 通用”****&gt;“类库(Windows 通用)”****）并将其命名为 P2PHelper。
-   从新项目中删除 Class1.cs。
-   将 P2PSession.cs、P2PSessionClient.cs 和 P2PSessionHost.cs 复制到新项目的文件夹中，并在新项目中包括已复制的文件。
-   该项目无需其他更改即可构建。

**共享文件**

-   将文件夹 Common、Model、View 和 ViewModel 从 \\QuizGame.Shared\\ 复制到 \\QuizGame10\\。
-   Common、Model、View 和 ViewModel 是我们在提到磁盘上的共享文件夹时所指的文件夹。

**QuizGameHost**

-   创建一个新的 Windows 10 应用项目（“添加”****&gt;“新建项目”****&gt;“Windows 通用”****&gt;“空白应用程序(Windows 通用)”****）并将其命名为 QuizGameHost。
-   添加对 P2PHelper 的引用（“添加引用”****&gt;“项目”****&gt;“解决方案”****&gt;“P2PHelper”****）。
-   在“解决方案资源管理器”****中，为磁盘上的每个共享文件夹创建一个新文件夹。 反过来，右键单击你刚刚创建的每个文件夹，然后依次单击“添加”****&gt;“现有项”****并向上导航文件夹。 打开相应的共享文件夹、选择所有文件，然后单击“添加为链接”****。
-   将 MainPage.xaml 从 \\QuizGame.Windows\\ 复制到 \\QuizGameHost\\ ，并将命名空间更改为 QuizGameHost。
-   将 App.xaml 从 \\QuizGame.Shared\\ 复制到 \\QuizGameHost\\，并将命名空间更改为 QuizGameHost。
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

-   在“属性”****&gt;“生成”****&gt;“条件编译符号”****中，添加 LOCALTESTMODEON。
-   现在可以返回到你向 app.xaml.cs 添加的代码并解析 TestView 类型。
-   在 package.appxmanifest 中，将功能名称从 internetClient 更改为 internetClientServer。

**QuizGameClient**

-   创建一个新的 Windows 10 应用项目（“添加”****&gt;“新建项目”****&gt;“Windows 通用”****&gt;“空白应用程序(Windows 通用)”****）并将其命名为 QuizGameClient。
-   添加对 P2PHelper 的引用（“添加引用”****&gt;“项目”****&gt;“解决方案”****&gt;“P2PHelper”****）。
-   在“解决方案资源管理器”****中，为磁盘上的每个共享文件夹创建一个新文件夹。 反过来，右键单击你刚刚创建的每个文件夹，然后依次单击“添加”****&gt;“现有项”****并向上导航文件夹。 打开相应的共享文件夹、选择所有文件，然后单击“添加为链接”****。
-   将 MainPage.xaml 从 \\QuizGame.WindowsPhone\\ 复制到 \\QuizGameClient\\，并将命名空间更改为 QuizGameClient。
-   将 App.xaml 从 \\QuizGame.Shared\\ 复制到 \\QuizGameClient\\，并将命名空间更改为 QuizGameClient。
-   在 package.appxmanifest 中，将功能名称从 internetClient 更改为 internetClientServer。

你现在可以进行构建并运行。

## 自适应 UI

当 QuizGameHost Windows 10 应用在宽窗口（只可能出现在带有大屏幕的设备上）中运行时，它看起来很美观。 但是，当应用的窗口较窄时（会出现在小型设备上，但也可能出现在大型设备上），UI 会因过分拥挤而无法阅读。

我们可以使用自适应视觉状态管理器功能修复此问题，正如在我们在[案例研究：Bookstore2](w8x-to-uwp-case-study-bookstore2.md) 中所说明的。 首先，在视觉元素上设置属性，以便在默认情况下以较窄的状态设置 UI 布局。 所有这些更改都发生在 \\View\\HostView.xaml 中。

-   在主 **Grid** 中，将第一个 **RowDefinition** 的 **Height** 从“140”更改为“Auto”。
-   在包含名为 `pageTitle` 的 **TextBlock** 的 **Grid** 上，设置 `x:Name="pageTitleGrid"` 和 `Height="60"`。 其中前两个步骤是为了使我们可以通过视觉状态中的资源库有效控制该 **RowDefinition** 的高度。
-   在 `pageTitle` 上，设置 `Margin="-30,0,0,0"`。
-   在由注释 `<!-- Content -->` 指示的 **Grid** 上，设置 `x:Name="contentGrid"` 和 `Margin="-18,12,0,0"`。
-   在紧挨在注释 `<!-- Options -->` 上方的 **TextBlock** 上，设置 `Margin="0,0,0,24"`。
-   在默认 **TextBlock** 样式（文件中的第一个资源）中，将 **FontSize** 资源库的值更改为“15”。
-   在 `OptionContentControlStyle` 中，将 **FontSize** 资源库的值更改为“20”。 此步骤和上一个步骤将为我们提供一个可在所有设备上良好工作的良好字型渐变。 这些大小比我们为 Windows 8.1 应用使用的“30”要灵活很多。
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

## 通用样式设置


你会注意到在 Windows 10 中，按钮不具有其模板中面向触摸的相同填充。 可以通过两个小更改修复该问题。 第一，将此标记添加到 QuizGameHost 和 QuizGameClient 中的 app.xaml。

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

第二，将此资源库添加到 \\View\\ClientView.xaml 中的 `OptionButtonStyle`。

```xml
<Setter Property="Margin" Value="6"/>
```

通过最后一项调整，该应用的行为和外观将与移植前完全相同，此外它现在可在任意位置运行。

## 总结

我们在此案例研究中移植的应用是涉及到多个项目、一个类库以及大量代码和用户界面的相对复杂的应用。 即便如此，该移植仍然很简单。 移植的一些便利之处可以直接归功于 Windows 10 开发人员平台与 Windows 8.1 和 Windows Phone 8.1 平台之间的相似性。 另一些是由于原始应用的设计方式使模型、视图模型和视图保持分离。



<!--HONumber=Jun16_HO4-->


