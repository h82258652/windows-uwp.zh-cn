---
author: Karl-Bridge-Microsoft
Description: "了解如何管理由音频输入的质量所导致的语音识别准确度的问题。"
title: "管理音频输入问题"
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 4f926969b5f9f20de904aeaf749e69c0ded3c052

---

# 管理音频输入问题

了解如何管理由音频输入的质量所导致的语音识别准确度的问题。

**重要的 API**

-   [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)
-   [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)
-   [**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)




## 评估音频输入质量


当语音识别处于活动状态时，使用语音识别器的 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 事件，确定一个或多个音频问题是否会干扰语音输入。 事件参数 ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) 可提供 [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) 属性，该属性描述了检测到的音频输入问题。

背景噪音太大、麦克风设为静音以及扬声器的音量或语速都可能会影响语音识别。

在此情况下，我们可以配置一个语音识别器并开始侦听 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 事件。

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

## 管理语音识别体验


使用由 [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) 属性提供的说明可帮助用户改善识别的条件。

在此处，我们将为 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 事件创建一个处理程序，用于检查较低的音量级别。 然后，我们将使用 [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) 对象建议用户尝试提高音量。

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## 相关文章


* [语音交互](speech-interactions.md)

**示例**
* [语音识别和语音合成示例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO4-->


