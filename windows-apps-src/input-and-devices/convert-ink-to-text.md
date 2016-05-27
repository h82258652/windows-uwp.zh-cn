---
author: Karl-Bridge-Microsoft
Description: 使用手写识别将笔划墨迹转换为文本，或使用自定义识别转换为形状。
title: 将 Windows 笔划墨迹视作文本
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, handwriting recognition
---

# 将 Windows 笔划墨迹视作文本

使用手写识别将笔划墨迹转换为文本，或使用自定义识别转换为形状。

**重要的 API**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


手写识别内置于 Windows 墨迹平台，并支持一组广泛的区域设置和语言。

对于此处的所有示例，请添加墨迹功能所需的命名空间引用。 这包括“Windows.UI.Input.Inking”。

## <span id="Basic_handwriting_recognition"></span><span id="basic_handwriting_recognition"></span><span id="BASIC_HANDWRITING_RECOGNITION"></span>基本手写识别


我们在此处演示如何使用与默认安装语言包相关联的手写识别引擎来解释 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一组笔划。

用户通过在完成书写时单击某个按钮来启动识别。

1.  首先，我们设置 UI。

    此 UI 包括一个“识别”按钮、[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 以及一个用于显示识别结果的区域。    
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink recognition sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas" 
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult" 
                       Grid.Row="1" 
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  然后，我们设置一些基本墨迹输入行为。

    将 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 配置为将来自笔和鼠标的输入数据解释为笔划墨迹 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 笔划墨迹使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上呈现。 还声明一个用于“识别”按钮上的单击事件的侦听器。
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

3.  最后，执行基本手写识别。 在本例中，我们使用“识别”按钮的单击事件处理程序来执行手写识别。

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 将所有笔划墨迹存储在 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 对象中。 笔划通过 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 属性公开，并使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法检索。
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.
```    CSharp
// Create a manager for the InkRecognizer object 
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer = 
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults = 
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer, 
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (var result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
```    CSharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // Create a manager for the InkRecognizer object 
            // used in handwriting recognition.
            InkRecognizerContainer inkRecognizerContainer = 
                new InkRecognizerContainer();

            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults = 
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer, 
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (var result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <span id="International_recognition"></span><span id="international_recognition"></span><span id="INTERNATIONAL_RECOGNITION"></span>国际识别


Windows 支持的一个语言综合子集可用于手写识别。

下表列出了 [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478) 支持的语言。 第一列包含 [**Name**](https://msdn.microsoft.com/library/windows/apps/br208484) 的可能值。

| 名称                                                            | IETF 语言标记 | 范围                                                                      |
|-----------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------|
| Microsoft English (US) Handwriting Recognizer                   | en-US             | 美国和菲律宾的英语                                       |
| Microsoft English (UK) Handwriting Recognizer                   | en-GB             | 英国和此表中未指定的所有其他地区的英语 |
| Microsoft English (Canada) Handwriting Recognizer               | en-CA             | 加拿大的英语                                                             |
| Microsoft English (Australia) Handwriting Recognizer            | en-AU             | 澳大利亚的英语                                                          |
| Microsoft-Handschrifterkennung - Deutsch                        | de-DE             | 德国、奥地利、卢森堡和纳米比亚的德语                           |
| Microsoft-Handschrifterkennung - Deutsch (Schweiz)              | de-CH             | 瑞士和列支敦士登的德语                                       |
| Reconocimiento de escritura a mano en español de Microsoft      | es-ES             | 西班牙和此表中未指定的所有其他地区的西班牙语         |
| Reconocedor de escritura en Español (México) de Microsoft       | es-MX             | 墨西哥和美国的西班牙语                                       |
| Reconocedor de escritura en Español (Argentina) de Microsoft    | es-AR             | 阿根廷、巴拉圭和乌拉圭的西班牙语                                   |
| Reconnaissance d'écriture Microsoft - Français                  | fr                | 法国、加拿大、比利时、瑞士和所有其他地区的法语       |
| Microsoft 日本語手書き認識エンジン                              | ja                | 所有地区的日语                                                     |
| Riconoscimento grafia italiana Microsoft                        | it                | 意大利、瑞士和所有其他地区的意大利语                        |
| Microsoft Nederlandstalige handschriftherkenning                | nl-NL             | 荷兰、苏里南和安的列斯的荷兰语                           |
| Microsoft Nederlandstalige (België) handschriftherkenning       | nl-BE             | 比利时的荷兰语(佛兰德语)                                                    |
| Microsoft 中文(简体)手写识别器                                  | zh                | 简体中文                                                  |
| Microsoft 中文(繁體)手寫辨識器                                  | zh-Hant           | 繁体中文                                                 |
| Microsoft система распознавания русского рукописного ввода      | ru                | 所有地区的俄语                                                      |
| Reconhecedor de Manuscrito da Microsoft para Português (Brasil) | pt-BR             | 巴西的葡萄牙语                                                          |
| Reconhecedor de escrita manual da Microsoft para português      | pt-PT             | 葡萄牙和所有其他地区的葡萄牙语                               |
| Microsoft 한글 필기 인식기                                      | ko                | 所有地区的朝鲜语                                                       |
| System rozpoznawania polskiego pisma odręcznego firmy Microsoft | pl                | 所有地区的波兰语                                                       |
| Microsoft Handskriftstolk för svenska                           | sv                | 所有地区的瑞典语                                                      |
| Microsoft rozpoznávač rukopisu pro český jazyk                  | cs                | 所有地区的捷克语                                                        |
| Microsoft Genkendelse af dansk håndskrift                       | da                | 所有地区的丹麦语                                                       |
| Microsoft Håndskriftsgjenkjenner for norsk                      | nb                | 所有地区的挪威语(博克马尔语)                                           |
| Microsoft Håndskriftsgjenkjenner for nynorsk                    | nn                | 所有地区的挪威语(尼诺斯克语)                                          |
| Microsoftin suomenkielinen käsinkirjoituksen tunnistus          | fi                | 所有地区的芬兰语                                                      |
| Microsoft recunoaştere grafie - Română                          | ro                | 所有地区的罗马尼亚语                                                     |
| Microsoftov hrvatski rukopisni prepoznavač                      | hr                | 所有地区的克罗地亚语                                                     |
| Microsoft prepoznavač rukopisa za srpski (latinica)             | sr-Latn           | 所有地区的塞尔维亚语(拉丁文)                                           |
| Microsoft препознавач рукописа за српски (ћирилица)             | sr                | 所有地区的塞尔维亚语(西里尔文)                                        |
| Reconeixedor d'escriptura manual en català de Microsoft         | ca                | 所有地区的加泰罗尼亚语                                                      |

 

你的应用可以查询已安装的手写识别引擎的集合，并使用其中一个引擎或让用户选择其首选语言。

**注意**  
用户可以通过转到“设置”-&gt;“时间和语言”来查看已安装语言的列表。 已安装的语言在“语言”下列出。

若要安装新语言包并为该语言启用手写识别：

1.  转到“设置”&gt;“时间和语言”&gt;“区域和语言”****。
2.  选择“添加语言”****。
3.  从列表中选择某种语言，然后选择区域版本。 该语言现在在“区域和语言”****页面上列出。
4.  单击该语言，然后选择“选项”****。
5.  在“语言选项”****页面上，下载“手写识别引擎”****（也可以在此处下载完整的语言包、语音识别引擎和键盘布局）。

 

我们在此处演示如何使用手写识别引擎基于所选的识别器来解释 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一组笔划。

用户通过在完成书写时单击某个按钮来启动识别。

1.  首先，我们设置 UI。

    UI 包含一个“识别”按钮、一个列出已安装手写识别器的组合框、[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 和一个用于显示识别结果的区域。
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Advanced international ink recognition sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers" 
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize" 
                    Content="Recognize" 
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas" 
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult" 
                       Grid.Row="1" 
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  然后，我们设置一些基本墨迹输入行为。

    将 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 配置为将来自笔和鼠标的输入数据解释为笔划墨迹 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 笔划墨迹使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上呈现。

    我们调用 `InitializeRecognizerList` 函数以使用已安装手写识别器的列表来填充识别器组合框。

    我们还声明“识别”按钮上的单击事件的侦听器和识别组合框上的选择更改事件。
```    CSharp
 public MainPage()
     {
         this.InitializeComponent();

         // Set supported inking device types.
         inkCanvas.InkPresenter.InputDeviceTypes =
             Windows.UI.Core.CoreInputDeviceTypes.Mouse |
             Windows.UI.Core.CoreInputDeviceTypes.Pen;

         // Set initial ink stroke attributes.
         InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
         drawingAttributes.Color = Windows.UI.Colors.Black;
         drawingAttributes.IgnorePressure = false;
         drawingAttributes.FitToCurve = true;
         inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged += 
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  我们使用已安装手写识别器的列表来填充识别器组合框。

    创建一个 [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) 来管理手写识别过程。 使用此对象调用 [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480) 并检索已安装识别器的列表来填充识别器组合框。
```    CSharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers = 
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  如果识别器组合框选择发生更改，请更新手写识别器。

    基于识别器组合框中所选的识别器使用 [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) 调用 [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328)。
```    CSharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  最后，我们基于所选的手写识别器执行手写识别。 在本例中，我们使用“识别”按钮的单击事件处理程序来执行手写识别。

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 将所有笔划墨迹存储在 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 对象中。 笔划通过 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 属性公开，并使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法检索。
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = 
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = 
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
```    CSharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = 
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = 
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = 
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <span id="Dynamic_handwriting_recognition"></span><span id="dynamic_handwriting_recognition"></span><span id="DYNAMIC_HANDWRITING_RECOGNITION"></span>动态手写识别


前面的两个示例要求用户按下按钮来开始识别。 你的应用还可以执行动态识别，方法是使用与基本计时函数配对的笔划输入。

在此示例中，我们将使用与前面的国际识别示例相同的 UI 和笔划设置。

1.  和前面的示例一样，配置 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 以将笔和鼠标的输入数据解释为笔划墨迹 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))，并且笔划墨迹使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上呈现。

    我们将添加两个 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 笔划事件（[**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) 和 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)）的侦听器（而不是用于启动识别的按钮），并设置一个带有一秒 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 间隔的基本计时器 ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250))。    
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Populate the recognizer combo box with installed recognizers.
        InitializeRecognizerList();

        // Listen for combo box selection.
        comboInstalledRecognizers.SelectionChanged += 
            comboInstalledRecognizers_SelectionChanged;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += 
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted += 
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }    
```

2.  下面是我们在第一步中添加的三个事件的处理程序。

    <span id="StrokesCollected"></span><span id="strokescollected"></span><span id="STROKESCOLLECTED"></span>[**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    当用户通过抬起笔或手指或释放鼠标按钮来停止墨迹书写时，启动识别计时器。 在无墨迹输入的一秒后，启动识别。

    <span id="StrokeStarted"></span><span id="strokestarted"></span><span id="STROKESTARTED"></span>[**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    如果新的笔划在下一个计时器滴答事件前开始，则停止计时器，因为新的笔划可能是单次手写输入的延续。

    <span id="Tick"></span><span id="tick"></span><span id="TICK"></span>[**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    在无墨迹输入的一秒后，调用识别函数。
```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  最后，我们基于所选的手写识别器执行手写识别。 在此示例中，我们使用 [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) 的 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 事件处理程序来启动手写识别。

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 将所有笔划墨迹存储在 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 对象中。 笔划通过 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 属性公开，并使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法检索。
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the recognition function, in full.
```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## <span id="related_topics"></span>相关文章

* [笔和触笔交互](pen-and-stylus-interactions.md)

**示例**
* [墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [简单墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [复杂墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 






<!--HONumber=May16_HO2-->


