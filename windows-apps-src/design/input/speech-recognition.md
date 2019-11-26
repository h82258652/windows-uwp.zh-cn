---
Description: 使用语音识别提供输入内容、指定操作或命令并完成任务。
title: 语音识别
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: 语音，语音，语音识别，自然语言，听写，输入，用户交互
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1979e16853fc288b83f5e4216c970440300fc597
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258319"
---
# <a name="speech-recognition"></a>语音识别


使用语音识别提供输入内容、指定操作或命令并完成任务。

> **重要 API**：[**Windows.Media.SpeechRecognition**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition)

语音识别由以下部分构成：语音运行时、用于为运行时编程的识别 API、用于听写和 Web 搜索的现成语法，以及帮助用户发现和使用语音识别功能的默认系统 UI。

## <a name="configure-speech-recognition"></a>配置语音识别

若要支持应用的语音识别，用户必须在其设备上连接并启用麦克风，并接受 Microsoft 隐私策略授予你的应用程序使用的权限。

若要自动提示用户使用系统对话框，该对话框请求访问和使用麦克风的音频源（如下面所示的[语音识别和语音合成示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)），只需在[应用程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中设置**麦克风**[设备功能](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability)即可。 有关更多详细信息，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

![麦克风访问隐私策略](images/speech/privacy.png)

如果用户单击 "是" 将访问权限授予麦克风，你的应用将添加到 "设置-> 隐私-> 麦克风" 页上的 "已批准的应用程序" 列表中。 但是，用户可以随时选择关闭此设置，因此，在尝试使用它之前，应确认你的应用程序有权访问麦克风。

如果还想要支持听写、Cortana 或其他语音识别服务（如主题约束中定义的[预定义语法](#predefined-grammars)），还必须确认启用了**联机语音识别**（设置-> 的隐私 > 语音）。

此代码段显示了你的应用程序可以如何检查是否存在麦克风以及是否有权使用它。

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>识别语音输入

*约束*可定义该应用在语音输入中识别出的字词和短语（词汇）。 约束是语音识别的核心，它除了能提高语音识别准确度，还能为你的应用带来其他优势。

您可以使用以下类型的约束来识别语音输入。

### <a name="predefined-grammars"></a>预定义的语法

预定义的听写和 Web 搜索语法在无需你创作语法的情况下为你的应用提供语音识别。 使用这些语法时，语音识别由远程 Web 服务执行，并且结果将返回到设备。

默认自由文本听写语法可以识别用户以特定语言说出的大部分字词或短语，并且为识别短语进行了优化。 如果没有为 [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 对象指定任何约束，将使用预定义的听写语法。 当你不希望限制用户可说内容的种类时，自由文本听写非常有用。 典型用法包括为一条消息创建笔记或听写其内容。

诸如听写语法等 Web 搜索语法包含了用户可能说出的大量字词和短语。 但是，优化它的目的是识别用户搜索 Web 时通常使用的术语。

**请注意**  因为预定义听写和 web 搜索语法可能会很大，并且由于它们处于联机状态（不在设备上），因此性能可能不像在设备上安装了自定义语法那样快。     

可以使用这些预定义语法识别长达 10 秒的语音输入，并且不要求你进行任何创作。 然而，它们确实需要连接到网络。

若要使用 Web 服务约束，必须在**设置**中启用语音输入和听写支持，方法是在**设置 -> 隐私 -> 语音、墨迹书写和键入**中打开“了解我”选项。

下面我们将介绍如何测试是否已启用语音输入，如果未启用，则打开“设置”->“隐私”->“语音、墨迹书写和键入”页面。

首先，我们将全局变量 (HResultPrivacyStatementDeclined) 初始化为 0x80045509 的 HResult 值。 请参阅[C\# 或 Visual Basic 中的异常处理](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10))。

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

如果 [**HResult**](https://docs.microsoft.com/uwp/api/Windows.Foundation.HResult) 值等于 HResultPrivacyStatementDeclined 变量的值，则我们会在识别和测试过程中发现任何标准异常。 如果情况如此，我们将会显示一则警告并调用 `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` 以打开“设置”页。

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
    // Open the privacy/speech, inking, and typing settings page.
    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
  }
  else
  {
    var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
    await messageDialog.ShowAsync();
  }
}
```

请参阅[**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)。

### <a name="programmatic-list-constraints"></a>编程列表约束 

编程列表约束提供一种轻型方法，用于使用字词或短语的列表创建一种简单的语法。 列表约束非常适用于识别清晰的短语。 因为语音识别引擎仅须处理语音即可确认匹配，所以采用某种语法明确指定所有字词也可提高识别准确度。 也可以以编程方式更新该列表。

列表约束由字符串数组组成，此数组表示你的应用将为识别操作接受的语音输入。 你可以通过创建语音识别列表约束对象并传递字符串数组在应用中创建列表约束。 然后，将该对象添加到识别器的约束集合。 当语音识别器识别数组中的任何一个字符串时，识别成功。

请参阅[**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)。

### <a name="srgs-grammars"></a>SRGS 语法

语音识别语法规范 (SRGS) 语法是一个静态文档，与编程列表约束不同，它使用由 [SRGS 版本 1.0](https://www.w3.org/TR/speech-grammar/) 定义的 XML 格式。 SRGS 语法提供了对语音识别体验的最大控制，方法是让你在单个识别中捕获多个语义含义。

 请参阅[**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)。

### <a name="voice-command-constraints"></a>语音命令约束

使用语音命令定义 (VCD) XML 文件定义用户可以在激活应用时说出以启动操作的命令。 有关更多详细信息，请参阅[通过 Cortana 使用语音命令激活前台应用](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana)。

请参阅[**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**请注意**  所使用的约束类型类型取决于要创建的识别体验的复杂性。 对于特定识别任务，任一类型都可能是最佳选择，你也可能在应用中发现所有类型的约束的用途。
要开始使用约束，请参阅[定义自定义识别约束](define-custom-recognition-constraints.md)。

预定义的通用 Windows 应用听写语法可识别使用某种语言的大部分字词和短语。 如果语音识别器对象在没有自定义约束的情况下实例化，它会自动激活。

在该示例中，我们展示如何：

- 创建语音识别器。
- 编译默认 Universal Windows App 约束（未向语音识别器的语法集添加任何语法）。
- 开始使用 [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) 方法提供的基本识别 UI 和 TTS 反馈侦听语音。 如果不需要默认 UI，则使用 [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) 方法。

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>自定义识别 UI


当你的应用通过调用 [**SpeechRecognizer.RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) 来尝试进行语音识别时，多个屏幕将按以下顺序显示。

如果你使用基于预定义语法的约束（听写或 Web 搜索）：

-   **侦听**屏幕。
-   **思考**屏幕。
-   **听到你说**屏幕或错误屏幕。

如果你使用的约束基于字词或短语列表，或者基于 SRGS 语法文件：

-   **侦听**屏幕。
-   **你说的是**屏幕，如果用户所说的内容可以解释为不止一种可能性结果。
-   **听到你说**屏幕或错误屏幕。

下图演示了语音识别器在不同屏幕间的流程的示例，该识别器使用基于 SRGS 语法文件的约束。 在本例中，语音识别是成功的。

![基于 sgrs 语法文件的约束的初始识别屏幕](images/speech-listening-initial.png)

![基于 sgrs 语法文件的约束的中间识别屏幕](images/speech-listening-intermediate.png)

![基于 sgrs 语法文件的约束的最终识别屏幕](images/speech-listening-complete.png)

**侦听**屏幕可提供应用可识别的字词或短语的示例。 下面我们介绍如何使用 [**SpeechRecognizerUIOptions**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions) 类的属性（通过调用 [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions) 属性获取）自定义**侦听**屏幕上的内容。

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
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

## <a name="related-articles"></a>相关文章


**商**
* [语音交互](speech-interactions.md)
**设计人员**
* [语音设计指南](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)
**示例**
* [语音识别和语音合成示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




