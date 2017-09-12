---
author: mijacobs
Description: "自适应和交互式 Toast 通知可使你创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。"
title: "自适应和交互式 Toast 通知"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c8e77773b9118c3177dc958ddc7b51d32a452fa5
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2017
---
# <a name="adaptive-and-interactive-toast-notifications"></a>自适应和交互式 Toast 通知

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

自适应和交互式 Toast 通知允许你使用文本、图像和按钮/输入创建灵活的通知。

> **重要 API**：[UWP 社区工具包通知 NuGet 程序包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> 要查看 Windows 8.1 和 Windows Phone 8.1 中的传统模板，请参阅[传统 Toast 模板目录](https://msdn.microsoft.com/library/windows/apps/hh761494)。


## <a name="getting-started"></a>入门

**安装通知库。** 如果希望使用 C# 而不是 XML 来生成通知，请安装名为 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 程序包（搜索“notifications uwp”）。 本文中提供的 C# 示例使用 NuGet 程序包的版本 1.0.0。

**安装通知可视化工具。** 此免费 UWP 应用通过在你编辑时提供 toast 的即时可视预览来帮助你设计交互 toast 通知，类似于 Visual Studio 的 XAML 编辑器/设计视图。 你可以阅读[此博客文章](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)获取详细信息，并且可以在[此处](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)下载通知可视化工具。


## <a name="sending-a-toast-notification"></a>发送 Toast 通知

要了解如何发送通知，请参阅[发送本地 Toast](tiles-and-notifications-send-local-toast.md)。 本文档仅介绍如何创建 Toast 内容。


## <a name="toast-notification-structure"></a>Toast 通知结构

Toast 通知是某些数据属性（如标记/组，用于标识通知）与 *Toast 内容*的组合。

Toast 内容的核心组件有
* **launch**：定义当用户单击你的 Toast 时将传回应用的参数，允许你深层链接到 Toast 所显示的正确内容。 有关详细信息，请参阅[发送本地 Toast](tiles-and-notifications-send-local-toast.md)。
* **visual**：Toast 的可视部分，包括包含文本、图像和应用徽标的通用绑定。
* **actions**：Toast 的交互部分，包括输入和操作。
* **audio**：控制向用户显示 Toast 时播放的音频。

Toast 内容在原始 XML 中定义，但你可以使用我们的 [NuGet 库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)获取 C#（或 C++）对象模型来构造 Toast 内容。 本文介绍了 Toast 内容中的所有内容。

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

下面是 Toast 内容的可视化表示形式：

![Toast 通知结构](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Visual

每个 Toast 必须指定可视，你必须在这里提供通用 Toast 绑定，其中可以包含文本、图像、徽标等。 这些元素将在各种 Windows 设备上呈现，包括台式机、手机、平板电脑和 Xbox。

有关可视部分支持的所有属性及其子元素，请[参阅架构文档](tiles-and-notifications-toast-schema.md#toastvisual)。

关于 Toast 通知的应用标识通过手机应用图标传达。 但是，如果你使用应用徽标替代，我们将在文本行下显示应用名称。

| 正常 toast                                                                              | 使用 appLogoOverride 的 toast                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![无 appLogoOverride 的通知](images/adaptivetoasts-withoutapplogooverride.jpg) | ![有 appLogoOverride 的通知](images/adaptivetoasts-withapplogooverride.jpg) |


## <a name="text-elements"></a>文本元素

每个 Toast 必须具有至少一个文本元素，还可以包含两个另外的文本元素，类型均为 [AdaptiveText](tiles-and-notifications-toast-schema.md#adaptivetext)。

![带有标题和描述的 Toast](images/toast-title-and-description.jpg)

自从周年更新开始，你就可以使用文本的 **HintMaxLines** 属性控制显示的文本行数。 默认情况下，标题最多显示 2 行文本，每个描述行最多显示 4 行文本。

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>应用徽标替代

默认情况下，你的 Toast 将显示应用徽标。 但你可以用自己的 [ToastGenericAppLogo](tiles-and-notifications-toast-schema.md#toastgenericapplogo) 图像替代此徽标。 例如，如果这是来自一个人的通知，我们建议你将应用徽标替换为此人的照片。

![使用应用徽标替代的 Toast](images/toast-applogooverride.jpg)

你可以使用 **HintCrop** 属性更改图像的裁剪。 例如，*circle* 会将图像裁剪为圆形。 否则，图像为方形。 图像尺寸在 100% 缩放时为 64x64 像素。

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://unsplash.it/64?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://unsplash.it/64?image=883"/>
</binding>
```


## <a name="hero-image"></a>主图

**周年更新中的新增功能**：Toasts 可以显示主图，这是在 Toast 横幅和操作中心内突出显示的特别推荐的[ToastGenericHeroImage](tiles-and-notifications-toast-schema.md#toastgenericheroimage)。 图像尺寸在 100% 缩放时为 360x180 像素。

![使用主图的 Toast](images/toast-heroimage.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastHeroImage()
    {
        Source = "https://unsplash.it/360/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://unsplash.it/360/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>内联图像

你可以提供在展开 Toast 时显示的全宽内联图像。

![使用其他图像的 Toast](images/toast-additionalimage.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://unsplash.it/360/180?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://unsplash.it/360/180?image=1043" />
</binding>
```


## <a name="attribution-text"></a>署名文本

**周年更新中的新增功能**：如果你需要引用你的内容源，可以使用署名文本。 此文本始终显示在通知底部，与应用标识或通知时间戳一起显示。

在不支持署名文本的旧 Windows 版本中，该文本仅显示为另一文本元素（假设你还没有达到最多的三个文本元素）。

![带有署名文本的 Toast](images/toast-attributiontext.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>自定义时间戳

**创建者更新中的新增功能**：现在，你可以用自己的准确表示消息/信息/内容生成时间的时间戳替代系统提供的时间戳。 此时间戳在操作中心可见。

![带有自定义时间戳的 Toast](images/toast-customtimestamp.jpg)

要了解有关如何使用自定义时间戳的详细信息，请[参阅此博客文章](https://blogs.msdn.microsoft.com/tiles_and_toasts/2017/01/09/custom-timestamp-on-toast-notifications-windows-10-creators-update/)。

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="adaptive-content"></a>自适应内容

**周年更新中的新增功能**：除了上面指定的内容外，你还可以显示在展开 Toast 时可见的附加自适应内容。

这些附加内容是用 Adaptive 指定的，阅读[自适应磁贴文档](tiles-and-notifications-create-adaptive-tiles.md)可了解更多内容。

请注意，任何自适应内容都必须包含在 AdaptiveGroup 中。 否则将不会使用自适应呈现。


### <a name="columns-and-text-elements"></a>列和文本元素

下面的示例使用了列和高级自适应文本元素。 由于文本元素位于 AdaptiveGroup 中，所以它们支持所有富自适应样式属性。

![带有附加文本的 Toast](images/toast-additionaltext.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="inputs-and-buttons"></a>输入和按钮

输入和按钮是在 Toast 的 Toast 区域的 Actions 区域中指定的，表示它们仅在 Toast 展开时可见。


### <a name="quick-reply-text-box"></a>快速回复文本框

要启用快速回复文本框，例如在消息传递方案中，请添加文本输入和按钮，并引用文本输入的 ID 以使按钮显示在输入旁。

![带有文本输入和操作的通知](images/adaptivetoasts-xmlsample05.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Send",
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>使用按钮栏的输入

你也可以使用一个（或多个）输入，在输入下方显示正常按钮。

![带有文本和输入操作的通知](images/adaptivetoasts-xmlsample04.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Reply",
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call",
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>选择输入

除文本框外，你还可以使用选择菜单。

![带有选择输入和操作的通知](images/adaptivetoasts-xmlsample06.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


## <a name="buttons"></a>按钮

按钮使你的 Toast 可交互，让用户无需中断其当前工作流即可就你的 Toast 通知采取快速操作。 例如，用户可以直接从 Toast 内回复邮件，甚至在不打开电子邮件应用的情况下删除电子邮件。

按钮可以执行以下不同操作：

-   通过可用于导航到特定页面/内容的参数在前台激活应用。
-   激活应用的后台任务以便快速回复或类似的场景。
-   通过协议启动激活另一个应用。
-   执行系统操作，如推迟或消除通知。

请注意，你最多只能有 5 个按钮（包括我们稍后将讨论的上下文菜单项）。

![带有操作的通知，示例 1](images/adaptivetoasts-xmlsample02.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details",
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later",
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="snoozedismiss-buttons"></a>推迟/消除按钮

使用一个选择菜单和两个按钮，我们可以创建利用系统推迟和消除操作的提醒通知。 请确保将方案设置为“提醒”以便通知像提醒那样工作。

![提醒通知](images/adaptivetoasts-xmlsample07.jpg)

我们使用 Toast 按钮的 *SepectionBoxId* 属性将“推迟”按钮链接到选择菜单输入。

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

要使用系统推迟和消除操作，请执行以下步骤：

-   指定 ToastButtonSnooze 或 ToastButtonDismiss
-   选择指定一个自定义内容字符串：
    -   如果你不提供字符串，我们将自动使用“推迟”和“消除”的本地化字符串。
-   选择指定 *SelectionBoxId*：
    -   如果你不希望用户选择推迟间隔，而只是希望你的通知仅在系统定义的时间间隔内推迟一次（这在整个操作系统上都一致），则不要构建任何 &lt;input&gt;。
    -   如果你希望提供推迟间隔选择：
        -   在推迟操作中指定 *SelectionBoxId*
        -   将输入的 ID 与推迟操作的 *SelectionBoxId* 匹配
        -   将 *ToastSelectionBoxItem* 的值指定为一个以分钟为单位表示推迟间隔的 nonNegativeInteger。



## <a name="audio"></a>音频

移动版一直支持自定义音频，桌面版 1511（内部测试版本 10586）或更新版本也支持此功能。 可通过以下路径引用自定义音频：

-   ms-appx:///
-   ms-appdata:///

或者，你可以从 [ms winsoundevents 列表](https://msdn.microsoft.com/library/windows/apps/br230842)中选择，它始终在两种平台上受支持。

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

有关 Toast 通知中的音频的信息，请参阅[音频架构页面](https://msdn.microsoft.com/library/windows/apps/br230842)。 若要了解如何使用自定义音频发送 Toast，[请参阅此博客文章](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/)。


## <a name="alarms-reminders-and-incoming-calls"></a>闹钟、提醒和来电

要创建闹钟、提醒和来电通知，只需使用正常的 Toast 通知并为它分配一个方案值。 方案会调整几个行为以创建一致且统一的用户体验。

* **提醒**：通知将保留在屏幕上，直到用户消除它或采取操作。 在 Windows 移动版上，该 Toast 还会以预先展开的形式显示。 将播放提醒声音。
* **闹钟**：除提醒行为外，闹钟将用默认的闹钟声音自动循环音频。
* **来电**：来电通知在 Windows 移动设备上全屏显示。 否则，除使用铃声音频外，它们的行为与闹钟相同。

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="handling-activation"></a>处理激活
要了解如何处理 Toast 激活（用户单击你的 Toast 或 Toast 上的按钮），请参阅[发送本地 Toast](tiles-and-notifications-send-local-toast.md)。


 
## <a name="related-topics"></a>相关主题

* [发送本地 Toast 和处理激活](tiles-and-notifications-send-local-toast.md)
* [GitHub 上的通知库](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)