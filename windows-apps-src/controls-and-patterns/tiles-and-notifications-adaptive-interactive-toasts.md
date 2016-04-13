---
自适应和交互式 Toast 通知可使你创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。
自适应和交互式 Toast 通知
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
自适应和交互式 Toast 通知
template: detail.hbs
---

# 自适应和交互式 Toast 通知


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


自适应和交互式 Toast 通知可使你创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。

自适应和交互式 Toast 通知模型具有以下针对传统 Toast 模板目录的更新：

-   在通知上包含按钮和输入的选项。
-   主 Toast 通知和每个操作的三个不同的激活类型。
-   为特定方案创建通知的选项，包括闹钟、提醒和来电。

**注意** 若要查看 Windows 8.1 和 Windows Phone 8.1 中的传统模板，请参阅[传统 Toast 模板目录](https://msdn.microsoft.com/library/windows/apps/hh761494)。

 

## <span id="toast_structure"> </span> <span id="TOAST_STRUCTURE"> </span>Toast 通知结构


Toast 通知使用 XML 构建，这通常包含以下关键元素：

-   <visual> 涵盖可供用户从视觉上查看的内容，包括文本和图像
-   <actions> 包含开发人员希望在通知内添加的按钮/输入
-   <audio> 指定在通知弹出时播放的声音

下面是代码示例：

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Sample</text>
      <text>This is a simple toast notification example</text>
      <image placement="AppLogoOverride" src="oneAlarm.png" />
    </binding>
  </visual>
  <actions>
    <action content="check" arguments="check" imageUri="check.png" />
    <action content="cancel" arguments="cancel" />
  </actions>
  <audio src="ms-winsoundevent:Notification.Reminder"/>
</toast>
```

以及该结构的可视化表示形式：

![Toast 通知结构](images/adaptivetoasts-structure.jpg)

### <span id="Visual"> </span> <span id="visual"> </span> <span id="VISUAL"> </span>视觉

在视觉元素内，你必须正好有一个包含 Toast 的可视内容的绑定元素。

通用 Windows 平台 (UWP) 应用中的磁贴通知支持基于不同磁贴大小的多个模板。 但是，Toast 通知只有一个模板名称：**ToastGeneric**。 只有一个模板名称意味着：

-   你可以更改 Toast 内容，例如添加另一行文本、添加嵌入式图像或将缩略图图像从显示应用图标更改为其他内容，并且在执行任意上述操作时无需担心更改整个模板或由于模板名称和内容之间的不匹配而创建无效的负载。
-   你可以使用相同代码为专用于传递到不同类型的 Microsoft Windows 设备（包括手机、平板电脑、PC 和 Xbox One）的 **Toast 通知**构建相同的负载。 其中每台设备都将接受通知，并使用相应的视觉提示和交互模型根据其 UI策略向用户显示通知。

有关视觉部分及其子元素中受支持的所有属性，请参阅下面的“架构”部分。 有关更多示例，请参阅下面的 XML 示例部分。

### <span id="Actions"> </span> <span id="actions"> </span> <span id="ACTIONS"> </span>操作

在 UWP 应用中，你可以将按钮和其他输入添加到 Toast 通知，这可使用户在应用外执行更多操作。 这些操作在 <actions> 元素下指定，其中有两个可以指定的类型：

-   <action> 这将显示为桌面和移动设备上的按钮。 你可以在 Toast 通知内指定最多五个自定义或系统操作。
-   <input> 这允许用户提供输入，如对消息的快速回复或从下拉菜单中选择一个选项。

<action> 和 <input> 在 Windows 设备系列内均为自适应。 例如，在移动或桌面设备上，面向用户的 <action> 是可点击/单击的按钮。 文本 <input> 是用户可使用物理键盘或屏幕键盘输入文本的框。 这些元素还会适应将来的交互方案，如通过语音宣布的操作或通过听写获取的文本输入。

当用户采取某个操作时，你可以通过在 <action> 元素内指定 [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) 属性来执行以下操作之一：

-   使用特定于操作的参数在前台激活应用，该参数可用于导航到特定页面/上下文。
-   在不影响用户的情况下激活应用的后台任务。
-   通过协议启动激活另一个应用。
-   执行要执行的系统操作。 当前可用系统操作将推迟和解除计划的警报/提醒，这将在下面部分中进一步介绍。

有关视觉部分及其子元素中受支持的所有属性，请参阅下面的“架构”部分。 有关更多示例，请参阅下面的 XML 示例部分。

### <span id="Audio"> </span> <span id="audio"> </span> <span id="AUDIO"> </span>音频

自定义声音当前在面向桌面平台的 UWP 应用上不受支持；相反，你可以为你的桌面上的应用从 ms-winsoundevents 列表中进行选择。 移动平台上的 UWP 应用支持 ms-winsoundevents 以及采用以下格式的自定义声音：

-   ms-appx:///
-   ms-appdata:///

有关 Toast 通知中的音频的信息（其中包括 ms-winsoundevents 的完整列表），请参阅[音频架构页面](https://msdn.microsoft.com/library/windows/apps/br230842)。

## <span id="Alarms__reminders__and_incoming_calls"> </span> <span id="alarms__reminders__and_incoming_calls"> </span> <span id="ALARMS__REMINDERS__AND_INCOMING_CALLS"> </span>闹钟、提醒和来电


你可以将 Toast 通知用于闹钟、提醒和来电。 这些特殊 Toasts 具有与标准 Toast 一致的外观，尽管特殊 Toast 具有某些自定义、基于方案的 UI 和模式：

-   提醒 Toast 通知将保留在屏幕上，直到用户取消它或执行操作。 在 Windows 移动版上，提醒 Toast 通知还会以预先展开的形式显示。
-   除了与提醒通知共享上述行为，闹钟通知还会自动播放循环音频。
-   来电通知将在 Windows Mobile 设备上全屏显示。 这通过在 Toast 通知的根元素内指定方案属性来实现 - <toast>：
    <toast scenario=" { default | alarm | reminder | incomingCall } " >

## <span id="xml_examples"> </span> <span id="XML_EXAMPLES"> </span>XML 示例


**注意** 这些示例的 Toast 通知屏幕截图取自桌面上的应用。 在移动设备上，Toast 通知可在弹出时处于折叠状态，并且 Toast 底部带有用于展开它的捕获器。

 

**带有丰富视觉内容的通知**

此示例向你介绍如何具有多行文本、一个用于替代应用程序徽标的可选小图像和一个可选的嵌入式图像缩略图。

```XML
<toast launch="app-defined-string">
  <visual>
<binding template="ToastGeneric">
    <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image placement="appLogoOverride" src="A.png" />
    <image placement="inline" src="hiking.png" />
    </binding>
  </visual>
</toast>
```

![带有丰富视觉内容的通知](images/adaptivetoasts-xmlsample01.png)

 

**带有操作的通知，示例 1**

此示例显示...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="see more details" arguments="details" imageUri="check.png"/>
    <action activationType="background" content="remind me later" arguments="later" imageUri="cancel.png"/>
  </actions>
</toast>
```

![带有操作的通知，示例 1](images/adaptivetoasts-xmlsample02.png)

 

**带有操作的通知，示例 2**

此示例显示...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Cortana</text>
      <text>We noticed that you are near Wasaki.</text>
      <text>Thomas left a 5 star rating after his last visit, do you want to try?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="reviews" arguments="reviews" />
    <action activationType="protocol" content="show map" arguments="bingmaps:?q=sushi" />
  </actions>
</toast>
```

![带有操作的通知，示例 2](images/adaptivetoasts-xmlsample03.png)

 

**带有文本输入和操作的通知，示例 1**

此示例显示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="reply here" />
    <action activationType="background" content="reply" arguments="reply" />
    <action activationType="foreground" content="video call" arguments="video" />
  </actions>
</toast>
```

![带有文本和输入操作的通知](images/adaptivetoasts-xmlsample04.png)

 

**带有文本输入和操作的通知，示例 2**

此示例显示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="reply here" />
    <action activationType="background" content="reply" arguments="reply" imageUri="send.png" hint-inputId="message"/>
  </actions>
</toast>
```

![带有文本输入和操作的通知](images/adaptivetoasts-xmlsample05.png)

 

**带有选择输入和操作的通知**

此示例显示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
  <selection id="1" content="Breakfast" />
  <selection id="2" content="Lunch" />
  <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="background" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

![带有选择输入和操作的通知](images/adaptivetoasts-xmlsample06.png)

 

**提醒通知**

此示例显示...

```XML
<toast scenario="reminder" launch="developer-pre-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Adam&#39;s Hiking Camp</text>
      <text>You have an upcoming event for this Friday!</text>
      <text>RSVP before it"s too late.</text>
      <image placement="appLogoOverride" src="A.png" />
      <image placement="inline" src="hiking.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="background" content="RSVP" arguments="rsvp" />
    <action activationType="background" content="Reminder me later" arguments="later" />
  </actions>
</toast>
```

![提醒通知](images/adaptivetoasts-xmlsample07.png)

 

## <span id="Activation_samples"> </span> <span id="activation_samples"> </span> <span id="ACTIVATION_SAMPLES"> </span>激活示例


如上所述，Toast 中的正文和操作能够以不同方式激活应用。 下面的示例向你介绍如何处理 Toast 正文和/或 Toast 操作中的不同类型的激活。

**前台**

在此方案中，应用使用前台激活来响应可操作的 Toast 通知内的操作，方法是启动应用并导航到正确的内容。

用于调用 OnLaunched() 的 Toast 通知中的激活。 在 Windows 10 中，Toast 具有其自己的激活类型，并将调用 OnActivated()。

```
async protected override void OnActivated(IActivatedEventArgs args)
{
        //Initialize your app if it&#39;s not yet initialized;
    //Find out if this is activated from a toast;
    If (args.Kind == ActivationKind.ToastNotification)
    {
                //Get the pre-defined arguments and user inputs from the eventargs;
        var toastArgs = args as ToastNotificationActivatedEventArgs;
        var arguments = toastArgs.Arguments;
        var input = toastArgs.UserInput["1"]; 
}
     
    //...
}
```

**后台**

在此方案中，应用使用后台任务处理交互式 Toast 通知内的操作。 下面的代码显示如何声明此后台任务以处理应用清单内的 Toast 激活，以及如何在单击按钮时获取操作参数和用户输入。

```
<!-- Manifest Declaration -->
<!-- A new task type toastNotification is added -->
<Extension Category = "windows.backgroundTasks" 
EntryPoint = "Tasks.BackgroundTaskClass" >
  <BackgroundTasks>
    <Task Type="systemEvent" />
  </BackgroundTasks>
</Extension>
```

```
namespace ToastNotificationTask
{
    public sealed class ToastNotificationBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        //Inside here developer can retrieve and consume the pre-defined 
        //arguments and user inputs;
        var details = taskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
        var arguments = details.Arguments;
        var input = details.Input.Lookup("1");

            // ...
        }        
    }
}
```

## <span id="Schemas___visual__and__audio_"> </span> <span id="schemas___visual__and__audio_"> </span> <span id="SCHEMAS___VISUAL__AND__AUDIO_"> </span>架构：<visual> 和 <audio>


在以下架构中，“?”后缀表示属性为可选属性。

```
<toast launch? duration? activationType? scenario? >
    <visual version? lang? baseUri? addImageQuery? >
        <binding template? lang? baseUri? addImageQuery? >
            <text lang? >content</text>
            <text />
            <image src placement? alt? addImageQuery? hint-crop? />
        </binding>
    </visual>
    <audio src? loop? silent? />
    <actions>
    </actions>
</toast>
```

**<toast> 中的属性**

launch?

-   launch? = string
-   这是可选属性。
-   当应用程序由 Toast 激活时向其传递的字符串。
-   根据 activationType 的值，此值可由前台中的应用在后台任务内接收或由从原始应用协议启动的另一个应用接收。
-   此字符串的格式和内容由应用根据其自身用途定义。
-   当用户点击或单击 Toast 来启动其关联应用时，启动字符串会向应用提供上下文，以允许该应用向用户显示与 Toast 内容相关的视图，而不是以其默认方式启动。
-   如果由于用户单击某个操作（而不是 Toast 的正文）而发生激活，开发人员会检索回在该 <action> 标记中预定义的“arguments”，而不是在 <toast> 标记中预定义的“launch”。

duration?

-   duration? = "short|long"
-   这是可选属性。 默认值为“short”。
-   此属性只适用于特定方案和 appCompat。 对于闹钟方案，你不再需要此属性。
-   我们不建议使用此属性。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   这是可选属性。
-   默认值为“foreground”。

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   这是可选属性，默认值为“default”。
-   你不需要此属性，除非你的方案是弹出警报、提醒或来电。
-   不要将此属性仅用于使通知持续显示在屏幕上。

**<visual> 中的属性**

version?

-   version? = nonNegativeInteger
-   由于 <visual> 上将弃用版本控制，因此无需此属性。 敬请期待从更高层次结构指定的新版本控制模型（如果需要）。

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

baseUri?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**<binding> 中的属性**

template?

-   \[Important\] template? = "ToastGeneric"
-   如果你要使用任意新的自适应和交互式通知功能，请确保开始使用“ToastGeneric”模板而不是传统模板。
-   使用带有新操作的传统模板现在可能有效，但这不是预定用例，我们无法保证它会继续工作。

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

baseUri?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**<text> 中的属性**

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**<image> 中的属性**

src

-   有关此必需属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230844)。

placement?

-   placement? = "inline" | "appLogoOverride"
-   此属性是可选的。
-   这会指定将显示此图像的位置。
-   “inline”表示在 Toast 正文内部，在文本下； “appLogoOverride”表示替换应用程序图标（显示在 Toast 的左上角）。
-   对于每个位置值，你最多可以有一个图像。

alt?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230844)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230844)。

hint-crop?

-   hint-crop? = "none" | "circle"
-   此属性是可选的。
-   “none”是默认值，表示没有裁剪。
-   “circle”将图像裁剪为圆形。 将此属性用于联系人的头像、用户图像等。

**<audio> 中的属性**

src?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

loop?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

silent?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

## <span id="Schemas___action_"> </span> <span id="schemas___action_"> </span> <span id="SCHEMAS___ACTION_"> </span>架构：<action>


在以下架构中，“?”后缀表示属性为可选属性。

```
<toast>
    <visual>
    </visual>
    <audio />
    <actions>
        <input id type title? placeHolderContent? defaultInput? >
            <selection id content />
        </input>
        <action content arguments activationType? imageUri? hint-inputId />
    </actions>
</toast>
```

**<input> 中的属性**

id

-   id = string
-   此属性是必需的。
-   id 属性是必需的，并由开发人员用于在激活应用（在前台或后台）后检索用户输入。

type

-   type = "text | selection"
-   此属性是必需的。
-   它用于指定文本输入或预定义的选择列表中的输入。
-   在移动或桌面上，这用于指定你需要文本框输入还是列表框输入。

title?

-   title? = string
-   title 属性是可选的，它由开发人员用于为输入指定在出现提示时可供 shell 呈现的标题。
-   对于移动和桌面，此标题将显示在输入上方。

placeHolderContent?

-   placeHolderContent? = string
-   placeHolderContent 属性是可选的，并且是文本输入类型的灰色提示文本。 当输入类型不是“text”时，忽略此属性。

defaultInput?

-   defaultInput? = string
-   defaultInput 属性是可选的，用于提供默认输入值。
-   如果输入类型是“text”，将其视为字符串输入。
-   如果输入类型是“selection”，这应是此输入的元素内的可用选择之一的 id。

**<selection> 中的属性**

id

-   此属性是必需的。 它用于标识用户选择。 id 将返回到你的应用。

content

-   此属性是必需的。 它为此选择元素提供要显示的字符串。

**<action> 中的属性**

content

-   content = string
-   content 属性是必需的。 它提供在按钮上显示的文本字符串。

arguments

-   arguments = string
-   arguments 属性是必需的。 它描述应用定义的数据，在执行此操作的用户激活它以后应用可检索该数据。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   activationType 属性是可选的，其默认值为“foreground”。
-   它描述此操作将导致的激活类型：前台、后台，或者通过协议启动来启动另一个应用或调用系统操作。

imageUri?

-   imageUri? = string
-   imageUri 是可选的，用于针对此操作提供一个图像图标，以在按钮内仅与文本内容一起显示。

hint-inputId

-   hint-inputId = string
-   hint-inpudId 属性是必需的。 它专门用于快速回复方案。
-   此值需要是要关联的输入元素的 id。
-   在移动和桌面中，这会将按钮放置在输入框的右侧。

## <span id="Attributes_for_system-handled_actions"> </span> <span id="attributes_for_system-handled_actions"> </span> <span id="ATTRIBUTES_FOR_SYSTEM-HANDLED_ACTIONS"> </span>系统处理操作的属性


如果你不希望应用将通知的推迟/重新计划作为后台任务处理，则系统可以处理推迟和取消通知的操作。 系统处理的操作可以组合（或单独指定），但我们不建议在没有取消操作的情况下实现推迟操作。

系统命令组合：SnoozeAndDismiss

```
<toast>
    <visual>
    </visual>
    <audio />
    <actions hint-systemCommands? = "SnoozeAndDismiss" />
</toast>
```

单独系统处理的操作

```
<toast>
    <visual>
    </visual>
    <audio />
<actions>
<input id="snoozeTime" type="selection" defaultInput="10">
  <selection id="5" content="5 minutes" />
  <selection id="10" content="10 minutes" />
  <selection id="20" content="20 minutes" />
  <selection id="30" content="30 minutes" />
  <selection id="60" content="1 hour" />
</input>
<action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
<action activationType="system" arguments="dismiss" content=""/>
</actions>
</toast>
```

若要构建单独的推迟和取消操作，请执行以下步骤：

-   Specify activationType = "system"
-   Specify arguments = "snooze" | "dismiss"
-   指定内容：
    -   如果你希望“snooze”和“dismiss”的本地化字符串在操作上显示，请将内容指定为空字符串：<action content = ""/>
    -   如果你需要自定义字符串，只需提供其值：<action content="Remind me later" />
-   指定输入：
    -   如果你不希望用户选择推迟间隔，而只是希望你的通知仅在系统定义的时间间隔内推迟一次（这在整个操作系统上都一致），则不要构建任何 <input>。
    -   如果你希望提供推迟间隔选择：
        -   在推迟操作中指定 hint-inputId
        -   将输入的 id 与推迟操作的 hint-inputId 相匹配：<input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/>
        -   将选择 id 指定为以分钟为单位表示推迟间隔的 nonNegativeInteger：<selection id="240" /> 表示推迟 4 小时
        -   请确保 <input> 中的 defaultInput 值与 <selection> 子元素的 id 之一相匹配
        -   提供最多（但不多于）5 个 <selection> 值

 

 






<!--HONumber=Mar16_HO1-->


