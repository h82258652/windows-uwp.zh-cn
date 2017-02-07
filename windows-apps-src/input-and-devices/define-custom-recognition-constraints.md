---
author: Karl-Bridge-Microsoft
Description: "了解如何为语音识别定义和使用自定义约束。"
title: "定义自定义识别约束"
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: "语音, 语音, 语音识别, 自然语言, 听写, 输入, 用户交互"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: c75ee725ce4bc9fc33d9ea13e4ab92ed60bfb04d

---

# <a name="define-custom-recognition-constraints"></a>定义自定义识别约束

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

了解如何为语音识别定义和使用自定义约束。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)</li>
<li>[**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)</li>
<li>[**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)</li>
</ul>
</div>


语音识别至少需要一个约束，才能定义可识别的词汇。 如果未指定任何约束，将使用通用 Windows 应用的预定义听写语法。 请参阅[语音识别](speech-recognition.md)。


## <a name="add-constraints"></a>添加约束


使用 [**SpeechRecognizer.Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) 属性向语音识别器添加约束。

我们在此处介绍三种可在应用内使用的语音识别约束。 （有关语音命令约束，请参阅[在 Cortana 中使用语音命令启动前台应用](launch-a-foreground-app-with-voice-commands-in-cortana.md)。）

-   [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) - 基于预定义语法的约束（听写或 Web 搜索）。
-   [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) - 基于字词或短语列表的约束。
-   [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412) - 在语音识别语法规范 (SRGS) 文件中定义的约束。

每个语音识别器都可具有一个约束集合。 只有以下约束组合有效：

-   单个主题的约束，也称预定义语法（听写或 Web 搜索）。 不允许使用其他约束。
-   列表约束和/或语法文件约束的组合。

**记住**：先调用 [**SpeechRecognizer.CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) 方法编译约束，然后再开始识别过程。

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>指定 Web 搜索语法 (SpeechRecognitionTopicConstraint)


必须将主题约束（听写或 Web 搜索语法）添加到语音识别器的约束集合。

在此，我们将 Web 搜索语法添加到约束集合。

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>指定编程列表约束 (SpeechRecognitionListConstraint)


必须将列表约束添加到语音识别器的约束集合。

请牢记以下几点：

-   你可以将多个列表约束添加到约束集合。
-   你可以使用为字符串值实现 **IIterable&lt;String&gt;** 的任何集合。

我们在此处以编程方式将一组字词指定为列表约束，并将其添加到语音识别器的约束集合中。

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;yes&#39;, &#39;no&#39;";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>指定 SRGS 语法约束 (SpeechRecognitionGrammarFileConstraint)


必须将 SRGS 语法文件添加到语音识别器的约束集合。

SRGS 版本 1.0 是用于为语音识别创建 XML 格式语法的行业标准标记语言。 尽管 Universal Windows App 为创建语音识别提供了使用 SRGS 的替代项，但是你可能会发现，使用 SRGS 创建语法产生的结果最好，尤其是对较复杂的语音识别方案。

SRGS 提供一组完整的功能，可帮助你为应用构建复杂的语音交互。 例如，你可以使用 SRGS 语法执行以下操作：

-   指定字词和短语必须说出的顺序以供识别。
-   从多个列表和短语组合字词以供识别。
-   链接到其他语法。
-   对替代字词或短语分配权重以增加或减少它将被用于匹配语音输入的可能性。
-   包括可选字词或短语。
-   使用帮助筛选掉未指定或非预期输入（例如不匹配语法的随机语音或背景噪音）的特殊规则。
-   使用语义定义语音识别对你的应用的意义。
-   在语法中以内联方式或通过指向词典的链接来指定发音。

有关 SRGS 元素和属性的详细信息，请参阅 [SRGS 语法 XML 参考](http://go.microsoft.com/fwlink/p/?LinkID=269886)。 若要开始创建 SRGS 语法，请参阅[如何创建基本的 XML 语法](http://go.microsoft.com/fwlink/p/?LinkID=269887)。

请牢记以下几点：

-   你可以将多个语法文件约束添加到约束集合。
-   为符合 SRGS 规则的基于 XML 的语法文件使用 .grxml 文件扩展名。

该示例使用在名为 srgs.grxml 的文件（稍后说明）中定义的 SRGS 语法。 在文件属性中，**数据包操作**设置为**内容**，**复制到输出目录**则设置为**始终复制**。

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;blue background&#39;, &#39;green text&#39;";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

该 SRGS 文件 (srgs.grxml) 包含语义翻译标记。 这些标记提供了用于将语法匹配数据返回应用的机制。 语法必须符合万维网联合会 (W3C) [用于语音识别的语义翻译 (SISR) 1.0](http://go.microsoft.com/fwlink/p/?LinkID=201765) 规范。

我们在此处侦听“是”和“否”的变体。

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>管理约束


在加载约束集合以供识别后，你的应用可以通过将约束的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn631402) 属性设置为 **true** 或 **false** 来管理启用哪些约束以供识别操作。 默认设置为 **true**。

通常，相比为每次识别操作加载、卸载和编译约束，加载一次约束并按需启用或禁用它们更有效率。 按照需要使用 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn631402) 属性。

限制约束数量可以限制语音识别器针对语音输入进行搜索和匹配所需的数据量。 这既可以提高性能，又可以增加语音识别的准确性。

根据应用在当前识别操作的上下文中预期的短语，决定启用哪些约束。 例如，如果当前的应用上下文是显示一种颜色，那么你可能无需启用识别动物名称的约束。

要提示用户可说的内容，你可以使用 [**SpeechRecognizerUIOptions.AudiblePrompt**](https://msdn.microsoft.com/library/windows/apps/dn653235) 和 [**SpeechRecognizerUIOptions.ExampleText**](https://msdn.microsoft.com/library/windows/apps/dn653236) 属性，这些属性通过 [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254) 属性的方法进行设置。 预先提示用户在识别操作期间可说的内容可以提高他们说出的短语匹配可用约束的几率。

## <a name="related-articles"></a>相关文章


* [语音交互](speech-interactions.md)

**示例**
* [语音识别和语音合成示例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Dec16_HO3-->


