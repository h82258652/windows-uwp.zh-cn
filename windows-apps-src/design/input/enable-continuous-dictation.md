---
Description: Learn how to capture and recognize long-form, continuous dictation speech input.
title: 启用连续听写
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: 语音，语音，语音识别，自然语言，听写，输入，用户交互
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 839dc024204ec9b76ffe621a35cbbbaffc248d02
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7705029"
---
# <a name="continuous-dictation"></a>连续听写

了解如何捕获和识别较长的连续听写语音输入。

> **重要 API**：[**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896)、[**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)

在[语音识别](speech-recognition.md)中，你已了解如何使用 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653244) 对象的 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 或 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653226) 方法捕获和识别相对较短的语音输入。例如，撰写短信 (SMS) 或进行提问时。

对于较长的连续语音识别会话（例如听写或电子邮件），则使用 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn913913) 的 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn653226) 属性以获取 [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) 对象。

> [!NOTE]
> 听写语言支持取决于你的应用正在运行其中的[设备](https://docs.microsoft.com/windows/uwp/design/devices/)。 电脑和笔记本电脑，识别仅为 EN-US，而 Xbox 和手机可以识别支持语音识别的所有语言。 有关详细信息，请参阅[指定语音识别器语言](specify-the-speech-recognizer-language.md)。

## <a name="set-up"></a>设置

若要管理连续听写会话，你的应用需要几个对象：

- [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 对象的示例。
- 对要在听写期间更新 UI 的 UI 调度程序的引用。
- 用于跟踪用户累积说出的字词的方式。

此处，我们将一个 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 实例声明为代码隐藏类的私有字段。 如果你希望连续听写在单个可扩展应用程序标记语言 (XAML) 页面之后持续，则你的应用需要将引用存储在其他位置。

```CSharp
private SpeechRecognizer speechRecognizer;
```

在听写期间，识别器会从后台线程引发事件。 由于后台线程不能直接在 XAML 中更新 UI，你的应用必须使用调度程序才能更新 UI 以响应识别事件。

此处，我们声明一个私有字段，它将在之后通过 UI 调度程序进行初始化。

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

若要跟踪用户说出的内容，你需要处理由语音识别器所引发的识别事件。 这些事件提供用户话语块的识别结果。

此处，我们使用 [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx) 对象保留在会话期间获取的所有识别结果。 新结果将在处理后追加到 **StringBuilder**。

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>初始化

在连续语音识别初始化期间，你必须：

- 提取 UI 线程的调度程序（如果在连续识别事件处理程序中更新你的应用的 UI）。
- 初始化语音识别器。
- 编译内置的听写语法。
    **注意**语音识别需要至少一个约束，才能定义可识别的词汇。 如果未指定任何约束，将使用预定义的听写语法。 请参阅[语音识别](speech-recognition.md)。
- 为识别事件设置事件侦听器。

在此示例中，我们将在 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 页面事件中初始化语音识别。

1. 因为由语音识别器引发的事件在后台线程上发生，所以请创建一个对调度程序的引用以更新 UI 线程。 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 始终在 UI 线程上调用。
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  然后，我们初始化 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 实例。
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  我们再添加和编译语法，该语法定义所有可通过 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 识别的字词和短语。

    如果未显式指定语法，则默认使用预定义听写语法。 通常，默认语法最适用于常规听写。

    此处，我们立即调用 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) 而无需添加语法。

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>处理识别事件

你可以通过调用 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 或 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 捕获单一、简要的话语或短语。 

但是，为了捕获较长的连续识别会话，我们将指定在用户说话时要在后台运行的事件侦听器，并定义处理程序以构建听写字符串。

然后，我们使用识别器的 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) 属性来获取 [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) 对象，该对象提供用于管理连续识别会话的方法和事件。

两个事件尤其关键：

- [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)，在识别器已生成一些结果时发生。
- [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899)，在连续识别会话已结束时发生。

当用户说话时引发 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件。 识别器持续侦听用户，并定期引发一个传递语音输入块的事件。 你必须使用事件参数的 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) 属性检查语音输入，并在事件处理程序中采取相应操作，例如将文本追加到 StringBuilder 对象。

作为 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) 的实例，[**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) 属性可用于确定是否希望接受语音输入。 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) 为此提供了两个属性：

- [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) 指示识别是否成功。 识别失败的原因有多种。
- [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) 指示识别器正确理解字词的相对置信度。

下面是支持连续识别的基本步骤：  

1. 此处，我们在 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/dn913900) 页面事件中注册 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/br227508) 连续识别事件的处理程序。
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  然后检查 [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) 属性。 如果 Confidence 的值是 [**Medium**](https://msdn.microsoft.com/library/windows/apps/dn631409) 或更好，我们便将文本追加到 StringBuilder。 我们还在收集输入时更新 UI。

    **注意** [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)事件在不能直接更新 UI 在后台线程上引发。 如果一个处理程序需要更新 UI（如同 \[语音和 TTS 示例\] 那样），则必须通过调度程序的 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法调度对 UI 线程的更新。
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  然后处理 [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) 事件，该事件指示连续听写的结尾。

    当你调用 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) 或 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) 方法时会话结束（在下一部分介绍）。 在发生错误或用户停止说话时，会话也可以结束。 检查事件参数的 [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) 属性以确定会话结束的原因 ([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433))。

    此处，我们在 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/dn913899) 页面事件中注册 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br227508) 连续识别事件的处理程序。
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  事件处理程序检查“Status”属性，以确定识别是否成功。 它还可处理用户已停止说话的情况。 通常，将 [**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433) 视为成功的识别，因为这意味着用户已结束说话。 你应该在代码中对这种情况进行处理以提供良好体验。

    **注意** [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)事件在不能直接更新 UI 在后台线程上引发。 如果一个处理程序需要更新 UI（如同 \[语音和 TTS 示例\] 那样），则必须通过调度程序的 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法调度对 UI 线程的更新。
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>提供正在进行的识别反馈


当用户对话时，他们通常将依赖上下文才能完全理解所说内容。 同样，语音识别器通常需要上下文才能提供高可信度的识别结果。 例如，除非可从前后的词语中收集到更多的上下文，否则词语“包含”和“包涵”本身是无法区分的。 除非识别器已具有一定的置信度来确保字词已正确识别，否则它将不会引发 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件。

这可能会导致不理想的用户体验，因为他们仍在继续说话，但在识别器不足以具有能够引发 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件的置信度之前，不会提供任何结果。

处理 [**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914) 事件以改进这种明显的响应缺乏问题。 只要识别器为要处理的字词生成一组新的潜在匹配，就会引发此事件。 事件参数提供包含当前匹配的 [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911) 属性。 在用户继续说话时向其展示这些匹配，向他们保证处理仍在进行。 当置信度较高并已确定识别结果时，使用 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913895) 事件中提供的最终 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913900) 替换临时的 **Hypothesis** 结果。

此处，我们将假设文本和一个省略号（“…”）追加到输出 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 的当前值。 从生成新的假设直到从 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件获取最终结果后，文本框内容才会更新。

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>启动和停止识别


启动识别会话之前，检查语音识别器 [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915) 属性的值。 语音识别器必须处于 [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227) 状态。

在检查语音识别器的状态之后，我们通过调用语音识别器的 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913901) 属性的 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913913) 方法启动会话。

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

可以采用两种方法停止识别：

-   [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908)允许任何挂起的识别事件完成（直到所有识别操作完成之前，都将继续引发 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)）
-   [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898)立即终止识别会话并放弃任何挂起的结果。

在检查语音识别器的状态之后，我们通过调用语音识别器的 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913898) 属性的 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913913) 方法停止会话。

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件可在调用 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) 后发生。  
> 由于多线程处理，当调用 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913900) 时，[**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913898) 事件可能仍保留在堆栈上。 如果如此，则仍引发 **ResultGenerated** 事件。  
> 如果在取消识别会话时设置任何私有字段，请始终在 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 处理程序中确认它们的值。 例如，如果在取消会话时将字段设置为 null，请勿假定字段在处理程序中进行初始化。

 

## <a name="related-articles"></a>相关文章


* [语音交互](speech-interactions.md)

**示例**
* [语音识别和语音合成示例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




