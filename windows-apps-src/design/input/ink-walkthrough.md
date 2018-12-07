---
ms.assetid: ''
title: 在 UWP 应用中支持墨迹
description: 向你的 UWP 应用添加墨迹支持的分步教程。
keywords: 墨迹, 墨迹书写, 教程
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cc650c1f81fbcac5b62b090a6dc58b5f8709cd7a
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795064"
---
# <a name="tutorial-support-ink-in-your-uwp-app"></a>教程：在 UWP 应用中支持墨迹

![Surface 触控笔](images/ink/ink-hero-small.png)  
*Surface 触控笔*（可通过 [Microsoft 官方商城](https://aka.ms/purchasesurfacepen)购买）。

此教程分步介绍如何创建一个支持使用 Windows Ink 书写和绘制的基本通用 Windows 平台 (UWP) 应用。 我们使用可以从 GitHub 下载的示例应用中的代码段（参阅[示例代码](#sample-code)），来展示各个步骤所讨论的各种功能和关联的 Windows Ink API（参阅 [Windows Ink 平台的组件](#components-of-the-windows-ink-platform)）。

我们主要介绍以下内容：
* 添加基本的墨迹支持
* 添加墨迹工具栏
* 支持手写识别
* 支持基本的形状识别
* 保存和加载墨迹

有关实现这些功能的更多详细信息，请参阅 [UWP 应用中的笔交互和 Windows Ink](https://docs.microsoft.com/windows/uwp/input/pen-and-stylus-interactions)。

## <a name="introduction"></a>简介

使用 Windows Ink，你可以为客户提供能够想象的几乎任何一种笔纸体验的同等数字方式，从快速的手写便笺和注释到白板演示，从体系结构和工程绘图到个人作品。

## <a name="prerequisites"></a>必备条件

* 一台运行当前版本 Windows 10 的计算机（或虚拟机）
* [Visual Studio 2017 和 RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 具体取决于你的配置，你可能需要安装[Microsoft.NETCore.UniversalWindowsPlatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/6.1.9) NuGet 程序包和系统设置中启用**开发人员模式**（设置-> 更新和安全-> 对于开发人员->使用开发人员功能）。
* 如果你还不熟悉使用 Visual Studio 进行通用 Windows 平台 (UWP) 应用开发，请在开始此教程前浏览一下这些主题：  
    * [准备工作](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [创建“Hello, world”应用 \(XAML\)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[可选]** 数字笔和显示屏支持使用该数字笔输入的计算机。

> [!NOTE] 
> 虽然 Windows Ink 可以支持使用鼠标和触摸进行绘制（我们将在此教程的步骤 3 中介绍如何执行操作），以提供最佳的 Windows Ink 体验，但是我们仍建议使用数字笔和显示屏支持使用该数字笔输入的计算机。。

## <a name="sample-code"></a>示例代码
在本指南中，我们全部使用示例墨迹应用来演示所讨论的概念和功能。

在 [windows-appsample-get-started-ink 示例](https://aka.ms/appsample-ink)从 [GitHub](https://github.com/) 下载此 Visual Studio 示例和源代码：

1. 选择绿色的**克隆或下载**按钮  
![克隆存储库](images/ink/ink-clone.png)
2. 如果你有 GitHub 帐户，则可以选择**在 Visual Studio 中打开**，将存储库克隆到本地计算机 
3. 如果你没有 GitHub 帐户，或者只是想要项目的本地副本，则选择**下载 ZIP**（你需要以后定期查看以下载最新的更新）

> [!IMPORTANT]
> 示例中的大部分代码已被注释掉。在我们介绍各个步骤时，系统将要求你取消代码各个部分的注释。 在 Visual Studio 中，只需突出显示代码行，并按 CTRL-K，然后按 CTRL-U。

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 平台组件

这些对象提供 UWP 应用的大部分墨迹书写体验。

| 组件 | 描述 |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | XAMLUI 平台控件，默认情况下，接收和显示来自笔的所有输入作为笔划墨迹或擦除笔划进行处理。 |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | 代码隐藏对象，与 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件（通过 [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) 属性公开）一起进行实例化。 此对象提供 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 公开的所有默认墨迹书写功能以及适用于其他自定义和个性化的完整 API 集。 |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | XAMLUI 平台控件，包含激活关联的[**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)中与墨迹相关功能的按钮的可自定义、 可扩展集合。 |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)<br/>我们不在这里介绍此功能，有关详细信息，请参阅[复杂墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620314)。 | 支持将笔划墨迹呈现到通用 Windows 应用的指定 Direct2D 设备上下文，而非默认的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件。 |

## <a name="step-1-run-the-sample"></a>步骤 1：运行示例

下载 RadialController 示例应用后，确认它在运行：
1. 在 Visual Studio 中打开示例项目。
2. 将**解决方案平台**下拉列表设置为非 ARM 选择。
3. 按 F5 编译、部署和运行。  

   > [!NOTE]
   > 或者，你可以选择**调试** > **开始调试**菜单项，或选择此处显示的**本地计算机**运行按钮。
   > ![Visual Studio 生成项目按钮](images/ink/ink-vsrun-small.png)

应用窗口打开，在初始屏幕出现几秒钟后，你将看到此初始屏幕。

![空应用](images/ink/ink-app-step1-empty-small.png)

好了，现在我们有了基本的 UWP 应用，在此教程接下来的所有部分我们都会用到它。 在以下步骤中，我们添加墨迹功能。

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>步骤 2：使用 InkCanvas 支持基本墨迹书写

也许你可能已注意到，该应用在它的初始窗体中，不允许你使用触控笔进行任何绘制（尽管你可以使用触控笔作为标准指针设备与应用进行交互）。 

我们在这一步中修复这个小缺点。

若要添加基本的墨迹书写功能，只需将 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) UWP 平台控件放在应用的相应页面上。

> [!NOTE]
> InkCanvas 的默认 [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) 和 [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) 属性为零，除非它是自动调整其子元素大小的元素的子项。 

### <a name="in-the-sample"></a>在示例中：
1. 打开 MainPage.xaml.cs 文件。
2. 找到标有此步骤标题的代码 ("// Step 2: Use InkCanvas to support basic inking")。
3. 取消以下各行的注释。 （后续步骤使用的功能需要这些引用）。  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. 打开 MainPage.xaml 文件。
5. 找到标有此步骤标题的代码 ("\<!-- Step 2: Basic inking with InkCanvas -->")。
6. 取消以下行的注释。  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

就这么简单！ 

现在，再次运行应用。 继续乱写一气，写下你的姓名，或（如果你有一面镜子或记忆力很好）为自己画一幅自画像。

![基本墨迹书写](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>步骤 3：使用触摸和鼠标支持墨迹书写

你将注意到，默认情况下，墨迹仅支持使用触控笔输入。 如果你尝试使用手指、鼠标或触摸板书写或绘画，你会失望。

若要解决这一问题，你需要再添加一行代码。 这一次是在你声明 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 的 XAML 文件的代码隐藏中。 

在此步骤中，我们介绍 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 对象，它为在 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 上输入、处理和呈现墨迹输入（标准和修改后）提供精细管理。

> [!NOTE]
> 标准墨迹输入（笔尖或橡皮擦尖/按钮）不使用辅助硬件提供功能修改，如笔桶按钮、鼠标右键按钮或类似机制。 

若要启用鼠标和触摸墨迹书写，将 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 的 [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 属性设置为你需要的 [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) 值的组合。

### <a name="in-the-sample"></a>在示例中：
1. 打开 MainPage.xaml.cs 文件。
2. 找到标有此步骤标题的代码 ("// Step 3: Support inking with touch and mouse")。
3. 取消以下各行的注释。  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

再次运行应用，你将发现你在计算机屏幕上用手指画画的所有梦想全部成真啦！

> [!NOTE]
> 指定输入设备类型时，你必须为每个特定输入类型（包括触控笔）指出支持，因为设置此属性将覆盖默认的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 设置。

## <a name="step-4-add-an-ink-toolbar"></a>步骤 4：添加墨迹工具栏

[**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 是一个 UWP 平台控件，提供激活墨迹相关功能的按钮的可自定义、可扩展集合。 

默认情况下，[**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 包含一组基本按钮，让用户可以快速选择触控笔、铅笔、荧光笔或橡皮擦，这些工具均可以与模具（标尺或量角器）一起使用。 触控笔、铅笔和荧光笔按钮各自还提供浮出控件，用于选择墨迹颜色和笔划大小。

若要在墨迹书写应用中添加默认的 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)，只需将其放在与 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 页面相同的页面上，并关联这两个控件。

### <a name="in-the-sample"></a>在示例中
1. 打开 MainPage.xaml 文件。
2. 找到标有此步骤标题的代码 ("\<!-- Step 4: Add an ink toolbar -->")。
3. 取消以下各行的注释。  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> 为了让 UI 和代码尽量保持整齐、简单，我们将使用基本的网格布局，并网格行中在 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 后面声明 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)。 如果在 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 之前声明，[**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 将首先呈现在画布下方，用户无法访问。  

现在，再次运行应用，查看 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)，并试用一下一些工具。

![来自 Ink 工作区草图板的 InkToolbar](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>挑战：添加一个自定义按钮
<table class="wdg-noborder">
<tr>
<td>

![来自 Ink 工作区草图板的 InkToolbar](images/challenge-icon.png)

</td>
<td>

下面是一个自定义 **[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)** 的示例（来自 Windows Ink 工作区的草图板）。

![来自 Ink 工作区草图板的 InkToolbar](images/ink/ink-inktoolbar-sketchpad-small.png)

有关如何自定义 [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 的更多详细信息，请参阅[将 InkToolbar 添加到通用 Windows 平台 (UWP) 墨迹书写应用](ink-toolbar.md)。

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>步骤 5：支持手写识别

既然你可以在应用中书写和绘画了，我们来尝试使用这些操作做一些有用的事。

在此步骤，我们使用 Windows Ink 手写识别功能来尝试识别你书写的内容。

> [!NOTE]
> 书写识别可以通过**笔和 Windows Ink** 设置改进：
> 1. 打开“开始”菜单，然后选择**设置**。
> 2. 从”设置“屏幕中选择**设备** > **笔和 Windows Ink**。
> ![来自 Ink 工作区草图板的 InkToolbar](images/ink/ink-settings-small.png)
> 3. 选择**了解我的书写**打开**手写个性化**对话框。
> ![来自 Ink 工作区草图板的 InkToolbar](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>在示例中：
1. 打开 MainPage.xaml 文件。
2. 找到标有此步骤标题的代码 ("\<!-- Step 5: Support handwriting recognition -->")。
3. 取消以下各行的注释。  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. 打开 MainPage.xaml.cs 文件。
5. 找到标有此步骤标题的代码 (" Step 5: Support handwriting recognition")。
6. 取消以下各行的注释。  

- 以下是此步骤所需的全局变量。

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- 这是**识别文本**按钮的处理程序，我们用于执行识别处理。

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. 再次运行应用，随便写点什么，然后单击**识别文本**按钮
8. 识别结果显示在按钮旁边

### <a name="challenge-1-international-recognition"></a>挑战 1：国际识别
<table class="wdg-noborder">
<tr>
<td>

![来自 Ink 工作区草图板的 InkToolbar](images/challenge-icon.png)

</td>
<td>

Windows Ink 支持对 Windows 支持的很多语言进行文本识别。 每个语言包包括一个手写识别引擎，可以随语言包一起安装。

通过查询安装的手写识别引擎将特定语言定为目标。

有关国际手写识别的更多详细信息，请参阅[将 Windows Ink 笔划识别为文本](convert-ink-to-text.md)。

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>挑战 2：动态识别
<table class="wdg-noborder">
<tr>
<td>

![来自 Ink 工作区草图板的 InkToolbar](images/challenge-icon.png)

</td>
<td>

对于此教程，我们需要按下按钮来启动识别。 你还可以通过使用基本计时函数执行动态识别。

有关动态识别的更多详细信息，请参阅[将 Windows Ink 笔划识别为文本](convert-ink-to-text.md)。

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>步骤 6：识别形状

好了，现在你可以将手写便笺转换为更清晰一些的内容。 但是那些来自清晨流程图程序匿名会议、冒着咖啡因气息的摇摇晃晃的涂鸦会怎样？

使用墨迹分析，你的应用还可以识别一些核心形状，包括：

- 圆形
- 菱形
- 绘图
- 椭圆形
- 等边三角形
- 六边形
- 等腰三角形
- 平行四边形
- 五角形
- 四边形
- 矩形
- 直角三角形
- 正方形
- 梯形
- 三角形

在此步骤中，我们使用 Windows Ink 的形状识别功能尝试清理你的涂鸦。

对于此示例，我们不尝试重绘笔划墨迹（尽管可以这样做）。 而是在 InkCanvas 下添加一个标准画布，我们在画布中绘制源自原始墨迹的等效椭圆或多角形物体。 然后，我们删除相应的笔划墨迹。

### <a name="in-the-sample"></a>在示例中：
1. 打开 MainPage.xaml 文件
2. 找到标有此步骤标题的代码 ("\<!-- Step 6: Recognize shapes -->")
3. 取消此行的注释。  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. 打开 MainPage.xaml.cs 文件
5. 找到标有此步骤标题的代码 ("// Step 6: Recognize shapes")
6. 取消这些行的注释：  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. 运行应用，绘制一些形状，然后单击**识别形状**按钮

这里是一个数字创意的基本流程图示例。

![原始墨迹流程图](images/ink/ink-app-step6-shapereco1-small.png)

这里是形状识别后的同一个流程图。

![原始墨迹流程图](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>步骤 7：保存和加载墨迹

现在，你已完成了涂鸦，而且很喜欢所看到的效果，但你想过你以后可能会喜欢对一些地方进行调整吗？ 你可以将笔划墨迹保存到一个墨迹序列化格式 (ISF) 文件，每当灵感闪现，你便可以加载这些文件进行编辑。 

ISF 文件是一种基本的 GIF 图像，包含描述笔划墨迹属性和行为的其他元数据。 不支持墨迹的应用可以忽略额外的元数据，但仍可以加载基本 GIF 图像（包括 alpha 通道背景透明度）。

在此步骤中，我们关联墨迹工具栏旁边的**保存**和**加载**按钮。

### <a name="in-the-sample"></a>在示例中：
1. 打开 MainPage.xaml 文件。
2. 找到标有此步骤标题的代码 ("\<!-- Step 7: Saving and loading ink -->")。
3. 取消以下各行的注释。 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. 打开 MainPage.xaml.cs 文件。
5. 找到标有此步骤标题的代码 ("// Step 7: Save and load ink")。
6. 取消以下各行的注释。  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. 运行应用并绘制一些图画。
8. 选择**保存**按钮，保存你的绘图。
9. 擦除墨迹或重启应用。
10. 选择**加载**按钮，打开刚刚保存的墨迹文件。

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>挑战：使用剪贴板复制并粘贴笔划墨迹 
<table class="wdg-noborder">
<tr>
<td>

![来自 Ink 工作区草图板的 InkToolbar](images/challenge-icon.png)

</td>

<td>

Windows Ink 还支持从剪贴板复制和粘贴笔划墨迹。

有关使用带有墨迹的剪贴板的更多详细信息，请参阅[存储和检索 Windows Ink 笔划数据](save-and-load-ink.md)。

</td>
</tr>
</table>

## <a name="summary"></a>小结

恭喜，你已完成了**输入：在 UWP 应用中支持墨迹**教程！ 我们向你展示了在 UWP 应用中支持墨迹所需的基本代码，以及如何提供 Windows Ink 平台支持的一些更加丰富的用户体验。

## <a name="related-articles"></a>相关文章

* [UWP 应用中的笔交互和 Windows Ink](pen-and-stylus-interactions.md)

### <a name="samples"></a>示例

* [墨迹分析示例（基本）(C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [墨迹手写识别示例 (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [保存并从墨迹序列化格式 (ISF) 文件加载笔划墨迹](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [保存并从剪贴板加载笔划墨迹](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [墨迹工具栏位置和方向示例（基本）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [墨迹工具栏位置和方向示例（动态）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [简单墨迹示例 (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [复杂墨迹示例 (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [墨迹示例 (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [入门教程：在 UWP 应用中支持墨迹](https://aka.ms/appsample-ink)
* [Coloring Book 示例](https://aka.ms/cpubsample-coloringbook)
* [系列说明示例](https://aka.ms/cpubsample-familynotessample)
