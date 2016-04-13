---
Description: 使用语音命令，将你的应用提供的功能加入到 Cortana 中以对其进行扩展。
title: Cortana 设计指南
ms.assetid: A92C084B-9913-4718-9A04-569D51ACE55D
label: 指南
template: detail.hbs
---

# Cortana 设计指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]




这些指南和建议描述了你的应用可以如何充分利用 **Cortana** 与用户交互、帮助他们完成任务，以及清楚地表明一切是如何发生的。

**Cortana** 允许在后台运行的应用程序提示用户进行确认或消除歧义，转而为用户提供语音命令状态的反馈。 该过程轻型、快速，不强制用户退出 **Cortana** 体验或将上下文切换到应用程序。

虽然用户应该感觉到 **Cortana** 有助于使该过程尽可能轻而易举，但是你可能希望 **Cortana** 也表明完成该任务的是你的应用。

我们使用此处显示的旅行规划和管理应用（名称为 **Adventure Works** 且集成到 **Cortana** UI）来展示所讨论的许多概念和功能。

![Cortana 画布概述](images/speech/cortana-overview.png)

## <span id="Conversational_writing_"> </span> <span id="conversational_writing_"> </span> <span id="CONVERSATIONAL_WRITING_"> </span>对话编写


成功的 **Cortana** 交互要求你在创建文本到语音转换 (TTS) 和 GUI 字符串时，遵循一些基本原则。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">原则</th>
<th align="left">错误示例</th>
<th align="left">正确示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt><span id="Efficient"></span><span id="efficient"></span><span id="EFFICIENT"></span>高效</dt>
<dd><p>使用尽可能少的字词，并将最重要的信息放在最前面。</p>
</dd>
</dl></td>
<td align="left"><p>当然可以，今天你希望搜索哪部电影？ 我们有大量搜索结果。</p></td>
<td align="left"><p>当然，你在寻找哪部电影？</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt><span id="Relevant"></span><span id="relevant"></span><span id="RELEVANT"></span>相关</dt>
<dd><p>提供仅与任务、内容和上下文相关的信息。</p>
</dd>
</dl></td>
<td align="left"><p>我已将它添加到你的播放列表。 提示：电量越来越低。</p></td>
<td align="left"><p>我已将它添加到你的播放列表。</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt><span id="Clear"></span><span id="clear"></span><span id="CLEAR"></span>清楚</dt>
<dd><p>避免混淆。 使用日常生活语言，而不是技术行话。</p>
</dd>
</dl></td>
<td align="left"><p>没有“到拉斯维加斯的旅行”的查询结果。</p></td>
<td align="left"><p>我找不到任何到拉斯维加斯的旅行。</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt><span id="Trustworthy_"></span><span id="trustworthy_"></span><span id="TRUSTWORTHY_"></span>可信 </dt>
<dd><p>尽量准确。 对在后台运行的任务了如指掌：如果某个任务尚未完成，则不说它已完成。 尊重隐私：不朗读私人信息。</p>
</dd>
</dl></td>
<td align="left"><p>我找不到该电影，它肯定尚未发行。</p></td>
<td align="left"><p>我无法在我们的目录中找到该电影。</p></td>
</tr>
</tbody>
</table>

 

编写人们的说话方式。 不要使语法准确的重要性高于听起来自然。 例如，TTS 可以读出“要”或“得”等顺口的口头禅。

只要有可能并且自然，请使用隐式的第一人称时态。 例如，“寻找你的下一个 Adventure Works 之旅”意味着有人在进行查找，但不使用字词“我”来指定。

使用一些变化来帮助使你的应用听起来更自然。 提供不同版本的 TTS 和 GUI 字符串，有效地说出相同的话。 例如“你想要看哪部电影？” 可以替换为“你希望观看哪部电影？”等。 人们每次说的话不可能完全相同。 只需确保使 TTS 版本和 GUI 版本保持同步即可。

在你的响应中果断地使用诸如“可以”和“好”等短语。 虽然它们可以进行确认并提供合理的进度，但是如果使用过于频繁且无变化，它们也会变得重复。

**注意** 仅在 TTS 中使用确认短语。 由于 **Cortana** 画布上的空间限制，不要在相应的 GUI 字符串中重复它们。

 

在你的响应中使用缩略语，获得更自然的交互效果并使 **Cortana** 画布节省更多空间。 例如，“我找不到该电影”而不是“我无法找到这部电影”。 针对听觉而不是视觉进行编写。

使用系统可以识别的语言。 用户通常会重复已向其展示的措辞。 了解你所显示的内容。

通过从备用响应的集合中进行循环或随机选择，在你的响应中使用稍有变化的措辞。 例如“你想要看哪部电影？” 和“你想看什么电影？” 这将使你的应用听起来更自然、更独特。

## <span id="Localization_"> </span> <span id="localization_"> </span> <span id="LOCALIZATION_"> </span>本地化


若要使用语音命令启动某项操作，你的应用必须使用用户在其设备上所选的语言（“设置”>“系统”>“语音”>“语音语言”）注册语音命令。

你应该将你的应用所对应的语音命令和所有 TTS 和 GUI 字符串都本地化。

应该避免较长的 GUI 字符串。 **Cortana** 画布提供三行以供响应，并将截断长于这三行的字符串。

有关详细信息，请参阅[全球化和本地化部分](../globalizing/globalizing-portal.md)。

## <span id="Image_resources_and_scaling"> </span> <span id="image_resources_and_scaling"> </span> <span id="IMAGE_RESOURCES_AND_SCALING"> </span>图像资源和缩放


通用 Windows 平台 (UWP) 应用可以基于特定设置和设备功能（高对比度、有效像素、区域设置等）自动选择最合适的应用徽标图像。 你只需提供图像，并确保在不同资源版本的应用项目中使用相应的命名约定和文件夹组织。 如果未能提供推荐的资源版本，辅助功能、本地化和图像质量将受到影响，具体取决于用户首选项、功能、设备类型和位置。

有关高对比度和比例系数的图像资源的更多详细信息，请参阅[磁贴和图标资源指南](../controls-and-patterns/tiles-and-notifications-app-assets.md)。

使用限定符命名资源。 资源限定符是一种文件夹和文件名修饰符，可标识应使用一种特定资源版本的上下文。

标准命名约定为“foldername/qualifiername-value\[\_qualifiername-value\]/filename.qualifiername-value\[\_qualifiername-value\].ext”。 例如：images/en-US/logo.scale-100\_contrast-white.png 仅是指使用根文件夹和文件名 images/logo.png 的代码。 请参阅[全球化和本地化](../globalizing/globalizing-portal.md)和[如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。

我们建议在字符串资源文件（如“en-US\\resources.resw”）上标记默认语言，在图像（如“logo.scale-100.png”）上标记默认比例系数，即使你当前不计划提供本地化或多种分辨率的资源也是如此。 但是，我们建议你至少为 100、200 和 400 比例系数提供资源。

**重要提示**  
在 Cortana 画布的标题区域使用的应用图标是在“Package.appxmanifest”文件中指定的 44x44 方形徽标图标。 

你还可以在 Cortana 画布的内容区域显示的查询中指定每个结果的图标。 结果图标的有效图像大小为：

-   68w x 68h
-   68w x 92h
-   280w x 140h


## <span id="Example"> </span> <span id="example"> </span> <span id="EXAMPLE"> </span>示例


此示例演示了在 **Cortana** 中的后台应用的端到端任务流。 我们将使用 **Adventure Works** 应用取消拉斯维加斯之旅。

![端到端的 Cortana 后台应用流](images/speech/e2e-canceltrip.png)

下面是此图中所概括的步骤：

1.  用户点击麦克风启动 **Cortana**。
2.  用户说出“取消 Adventure Works 拉斯维加斯之旅”，启动后台中的 **Adventure Works** 应用。 应用同时使用 **Cortana** 语音和画布与用户进行交互。
3.  **Cortana** 转换到可为用户提供确认反馈（“我将通知 Adventure Works 进行处理。”）、状态栏和取消按钮的切换屏幕。
4.  在此情况下，用户具有多个与查询相匹配的行程，因此该应用将提供一个列出了所有匹配结果的歧义消除屏幕，然后询问：“你想要取消哪一个？”
5.  用户指定“拉斯维加斯技术大会”项。
6.  由于取消不能撤消，该应用将提供一个确认屏幕，要求用户确认其意图。
7.  用户说“是”。
8.  然后，该应用提供一个显示操作结果的完成屏幕。

我们可以在此处详细了解这些步骤。

### <span id="Handoff"> </span> <span id="handoff"> </span> <span id="HANDOFF"> </span>切换

|                                                                                                          |
|----------------------------------------------------------------------------------------------------------|
| ![端到端：不使用切换屏幕查找行程 ](images/speech/cortana-backgroundapp-result.png)              |
| 不使用切换屏幕查找行程                                                                              |
| ![端到端：使用切换屏幕取消行程。 ](images/speech/cortana-backgroundapp-progress-result.png) |
| 使用切换屏幕取消行程                                                                          |

 

使应用响应耗时不超过 500 毫秒、不要求用户提供额外信息、可以在 **Cortana** 不进一步参与的情况下完成而不显示完成屏幕的任务。

如果你的应用程序需要超过 500 毫秒才能响应，**Cortana** 将提供切换屏幕。 将显示应用图标和名称，并且必须同时提供 GUI 和 TTS 切换字符串，以指示已正确理解语音命令。 切换屏幕最多将显示 5 秒；如果你的应用在此期间内没有响应，**Cortana** 将显示一般性错误屏幕。

### <span id="GUI_and_TTS_guidelines_for_handoff_screens"> </span> <span id="gui_and_tts_guidelines_for_handoff_screens"> </span> <span id="GUI_AND_TTS_GUIDELINES_FOR_HANDOFF_SCREENS"> </span>适用于切换屏幕的 GUI 和 TTS 指南

清晰地指示任务正在进行中。

使用现在时。

使用确认启动的是哪一任务的动词并引用特定实体。

使用非肯定执行已请求的未完成动作的一般动词。 例如使用“查找行程”而非“取消行程”。 在此情况下，如果没有返回任何结果，用户不会听到“取消拉斯维加斯之旅… 我找不到任何到拉斯维加斯的旅行”。

你应该清楚，如果应用仍然需要解析请求的实体，则该任务尚未进行。 例如，注意我们说的是“正在查找你的行程”而不是“正在取消你的行程”，因为零个或多个行程都可能是匹配的，而我们还不知道结果。

GUI 和 TTS 字符串可以相同，但这不是必需的。 尝试使 GUI 字符串的长度保持简短，避免其他可视资产的截断和重复。

| TTS                                                    | GUI                                 |
|--------------------------------------------------------|-------------------------------------|
| 查找你的下一个 Adventure Works 行程。            | 正在查找你的下一个行程…         |
| 在 Adventure Works 中搜索你的瀑布之城之旅。 | 正在搜索到瀑布之城的旅行... |

 

### <span id="Progress"> </span> <span id="progress"> </span> <span id="PROGRESS"> </span>进度

|                                                                                             |
|---------------------------------------------------------------------------------------------|
| ![端到端：使用进度屏幕取消行程 ](images/speech/e2e-canceltrip-progress.png) |
| 使用进度屏幕取消行程                                                            |

 

当两个步骤之间的某个任务花费了一些时间时，你的应用需要介入，并向用户通知进度屏幕上所发生情况的最新进展。 将显示应用图标，并且你必须同时提供 GUI 和 TTS 进度字符串，以指示该任务正在进行中。

你应该向你的应用提供一个包含启动参数的链接，以在适当的状态下启动该应用。 这样，用户便可以自行查看或完成任务。 **Cortana** 提供链接文本（如“转到 Adventure Works”）。

每个进度屏幕都将显示 5 秒，5 秒后，它们后面必须紧跟另一个屏幕，否则任务将超时。

进度屏幕后面可以紧跟以下屏幕：

-   进度
-   确认（显式，稍后说明）
-   消除歧义
-   完成

### <span id="GUI_and_TTS_guidelines_for_progress_screens"> </span> <span id="gui_and_tts_guidelines_for_progress_screens"> </span> <span id="GUI_AND_TTS_GUIDELINES_FOR_PROGRESS_SCREENS"> </span>进度屏幕的 GUI 和 TTS 指南

使用现在时。

使用可确认该任务正在进行中的行为动词。

**GUI**：如果显示实体，则使用对它的引用（“正在取消此行程...”）；如果没有显示实体，则显式调出实体（“正在取消‘拉斯维加斯技术大会’”）。

**TTS**：在第一个进度屏幕上，只应包含一个 TTS 字符串。 如果需要更多进度屏幕，请将空字符串 {} 作为 TTS 字符串发送，并仅提供一个 GUI 字符串。

| 条件                                              | TTS                            | GUI                            |
|---------------------------------------------------------|--------------------------------|--------------------------------|
| 在上一轮读取的实体/ 在显示器上显示的实体     | 正在取消此行程...          | 正在取消此行程...          |
| 在上一轮未读取的实体/ 在显示器上显示的实体 | 正在取消你的拉斯维加斯之旅… | 正在取消此行程...          |
| 在上一轮未读取的实体/ 未显示的实体        | 正在取消你的拉斯维加斯之旅… | 正在取消你的拉斯维加斯之旅… |

 

### <span id="Confirmation"> </span> <span id="confirmation"> </span> <span id="CONFIRMATION"> </span>确认

|                                                                                                     |
|-----------------------------------------------------------------------------------------------------|
| ![端到端：使用确认屏幕取消行程 ](images/speech/e2e-canceltrip-confirmation.png) |
| 使用确认屏幕取消行程                                                                |

 

某些任务可根据用户命令的性质隐式确认；其他任务可能更为敏感，需要显式确认。 下面是有关何时使用隐式和显式确认的一些指南。

确认屏幕上的 GUI 和 TTS 字符串都由你的应用指定，并将显示应用图标（如果提供）而不显示 **Cortana** 头像。

客户响应确认后，你的应用程序必须在 500 毫秒内提供下一个屏幕，以避免转到进度屏幕。

在以下情况下使用显式确认：

-   用户要发送内容（例如文本消息、电子邮件或社交帖子）
-   操作无法撤消（例如购买或删除某些内容）
-   结果可能会令人尴尬（例如打错电话）
-   需要更复杂的识别（例如开放式转录）

在以下情况下使用隐式确认：

-   内容仅为用户保存（例如自我提醒）
-   存在一种简单的退出方法（例如打开或关闭闹钟）
-   任务需要快速执行（例如，在忘记之前快速捕获想法）
-   准确性较高（例如简单菜单）

### <span id="GUI_and_TTS_guidelines_for_confirmation_screens"> </span> <span id="gui_and_tts_guidelines_for_confirmation_screens"> </span> <span id="GUI_AND_TTS_GUIDELINES_FOR_CONFIRMATION_SCREENS"> </span>适用于确认屏幕的 GUI 和 TTS 指南

使用现在时。

向用户询问一个可以使用“是”或“否”回答的明确问题。 该问题应显式确认用户要尝试执行的操作，并且不应该存在任何其他明显选项。

提供该问题的另一种问法以重新提示，以防第一次没有读懂语音命令。

**GUI**：如果显示实体，则使用对它的引用。 如果没有显示实体，则显式调出实体。

**TTS**：为清楚起见，始终引用特定项目或实体，除非系统已在上一轮读出它。

| 条件                                              | TTS                                        | GUI                                           |
|---------------------------------------------------------|--------------------------------------------|-----------------------------------------------|
| 在上一轮未读取的实体/ 在显示器上显示的实体 | 是否要取消拉斯维加斯技术大会？ | 是否取消此行程？                             |
| 在上一轮未读取的实体/ 未显示的实体        | 是否要取消拉斯维加斯技术大会？ | 是否取消拉斯维加斯技术大会？                 |
| 在上一轮读取的实体/ 未显示的实体            | 是否要取消此行程？             | 是否取消此行程？                             |
| 显示实体的重新提示                              | 是否曾打算取消此行程？            | 是否曾想要取消此行程？             |
| 未显示实体的重新提示                          | 是否曾打算取消此行程？            | 是否曾想要取消拉斯维加斯技术大会？ |

 

### <span id="Disambiguation"> </span> <span id="disambiguation"> </span> <span id="DISAMBIGUATION"> </span>消除歧义

|                                                                                                        |
|--------------------------------------------------------------------------------------------------------|
| ![端到端：使用消除歧义屏幕取消行程](images/speech/cortana-disambiguation-screen.png) |
| 使用消除歧义屏幕取消行程                                                                 |

 

某些任务可能会要求用户从实体列表做出选择，以完成任务。

消除歧义屏幕上的 GUI 和 TTS 字符串都由你的应用指定，并将显示应用图标（如果提供）而不显示 **Cortana** 头像。

客户响应消除歧义问题后，你的应用程序必须在 500 毫秒内提供下一个屏幕，以避免转到进度屏幕。

### <span id="GUI_and_TTS_guidelines_for_disambiguation_screens"> </span> <span id="gui_and_tts_guidelines_for_disambiguation_screens"> </span> <span id="GUI_AND_TTS_GUIDELINES_FOR_DISAMBIGUATION_SCREENS"> </span>适用于消除歧义屏幕的 GUI 和 TTS 指南

使用现在时。

向用户询问一个可以使用任何已显示实体的标题或文本行回答的明确问题。

最多可以显示 10 个实体。

每个实体都应具有唯一的标题。

提供该问题的另一种问法以重新提示，以防第一次没有读懂语音命令。

**TTS**：为清楚起见，始终引用特定项目或实体，除非已在上一轮说出它。

**TTS**：不要读出实体列表，除非只存在三个或更少实体并且它们长度很短。

| 条件                 | TTS                                                                            | GUI                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| 提示 - 3 个或更少项目  | 你打算取消哪趟拉斯维加斯之旅？ 是拉斯维加斯技术大会还是拉斯维加斯聚会？ | 你想要取消哪一个？ |
| 提示 - 超过 3 项 | 你打算取消哪趟拉斯维加斯之旅？                                          | 你想要取消哪一个？ |
| 重新提示                   | 你曾打算取消哪趟拉斯维加斯之旅？                                         | 你想要取消哪一个？ |

 

### <span id="Completion"> </span> <span id="completion"> </span> <span id="COMPLETION"> </span>完成

|                                                                                                 |
|-------------------------------------------------------------------------------------------------|
| ![端到端：使用完成屏幕取消行程 ](images/speech/e2e-canceltrip-completion.png) |
| 使用完成屏幕取消行程                                                              |

 

在成功完成任务时，你的应用应该通知用户已成功完成请求的任务。

完成屏幕上的 GUI 和 TTS 字符串都由你的应用指定，并将显示应用图标（如果提供）而不显示 **Cortana** 头像。

你应该向你的应用提供一个包含启动参数的链接，以在适当的状态下启动该应用。 这样，用户便可以自行查看或完成任务。 **Cortana** 提供链接文本（如“转到 Adventure Works”）。

### <span id="GUI_and_TTS_guidelines_for_completion_screens"> </span> <span id="gui_and_tts_guidelines_for_completion_screens"> </span> <span id="GUI_AND_TTS_GUIDELINES_FOR_COMPLETION_SCREENS"> </span>适用于完成屏幕的 GUI 和 TTS 指南

使用过去时。

使用行为动词显式声明该任务已完成。

如果显示实体，或在上一轮引用过它，则仅引用它。

| 条件                                       | TTS                                             | GUI                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| 显示的实体/在上一轮读取的实体         | 我已取消此行程。                       | 已取消此行程。               |
| 未显示的实体 /在上一轮未读取的实体 | 我已取消你的拉斯维加斯技术大会行程。 | 已取消“拉斯维加斯技术大会。” |

 

### <span id="Error"> </span> <span id="error"> </span> <span id="ERROR"> </span>错误

|                                                                                      |
|--------------------------------------------------------------------------------------|
| ![端到端：使用错误屏幕取消行程](images/speech/e2e-canceltrip-error.png) |
| 使用错误屏幕取消行程                                                        |

 

当发生以下错误之一时，**Cortana** 将显示相同的一般性错误消息。

-   应用服务意外终止。
-   **Cortana** 无法与应用服务进行通信。
-   在 **Cortana** 显示切换屏幕或进度屏幕 5 秒钟后，应用无法提供屏幕。

## <span id="related_topics"> </span>相关文章


* [语音交互](speech-interactions.md)
**开发人员**
* [Cortana 交互](https://msdn.microsoft.com/library/windows/apps/mt185598)
* [语音交互](https://msdn.microsoft.com/library/windows/apps/mt185614)
 

 






<!--HONumber=Mar16_HO1-->


