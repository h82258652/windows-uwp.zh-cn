---
author: andrewleader
Description: Adaptive and interactive toast notifications let you create flexible pop-up notifications with more content, optional inline images, and optional user interaction.
title: toast 内容
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.author: mijacobs
ms.date: 11/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, toast 通知, 交互式 toast, 自适应 toast, toast 内容, toast 有效负载
ms.localizationpriority: medium
ms.openlocfilehash: de999528d07e6bd7d243e53708e9afc465004af7
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2018
ms.locfileid: "3851407"
---
# <a name="toast-content"></a>toast 内容

借助自适应和交互式 toast 通知，可使用文本、图像和按钮/输入创建灵活的通知。

> **重要 API**：[UWP 社区工具包通知 NuGet 程序包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> 要查看 Windows 8.1 和 Windows Phone 8.1 中的传统模板，请参阅[传统 Toast 模板目录](https://msdn.microsoft.com/library/windows/apps/hh761494)。


## <a name="getting-started"></a>入门

**安装通知库。** 如果希望使用 C# 而不是 XML 来生成通知，请安装名为 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 程序包（搜索“notifications uwp”）。 本文中提供的 C# 示例使用 NuGet 程序包的版本 1.0.0。

**安装通知可视化工具。** 此免费 UWP 应用通过在你编辑时提供 toast 的即时可视预览来帮助你设计交互 toast 通知，类似于 Visual Studio 的 XAML 编辑器/设计视图。 请参阅[通知可视化工具](notifications-visualizer.md)了解详细信息，或[从 Microsoft Store 下载通知可视化工具](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)。


## <a name="sending-a-toast-notification"></a>发送 toast 通知

要了解如何发送通知，请参阅[发送本地 Toast](send-local-toast.md)。 本文档仅介绍如何创建 Toast 内容。


## <a name="toast-notification-structure"></a>Toast 通知结构

Toast 通知是某些数据属性（如标记/组，用于标识通知）与 *Toast 内容*的组合。

Toast 内容的核心组件有
* **launch**：定义当用户单击你的 Toast 时将传回应用的参数，允许你深层链接到 Toast 所显示的正确内容。 有关详细信息，请参阅[发送本地 Toast](send-local-toast.md)。
* **visual**：toast 的可视部分，包括带有文本和图像的通用绑定。
* **actions**：toast 的交互部分，包括输入和操作。
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

每个 toast 均必须指定一个 visual，此处必须包含带文本、图像等的通用 toast 绑定。 这些元素将在各种 Windows 设备上呈现，包括桌面设备、手机、平板电脑和 Xbox。

有关可视部分支持的所有属性及其子元素，请[参阅架构文档](toast-schema.md#toastvisual)。

关于 Toast 通知的应用标识通过手机应用图标传达。 但是，如果使用应用徽标替代，文本行下面将显示应用名称。

| 正常 toast 的应用标识 | 带有 appLogoOverride 的应用标识 |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>文本元素

每个 toast 均必须具有至少一个文本元素，此外还可包含两个文本元素，类型均为 [**AdaptiveText**](toast-schema.md#adaptivetext)。

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

自 Windows 10 周年更新起，可使用文本的 **HintMaxLines** 属性控制显示的文本行数。 标题文本的默认（及最大）行数最多为 2 行，两个额外的说明元素（第二个和第三个 **AdaptiveText**）的行数最多为 4 行（合并后）。

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

默认情况下，toast 将显示应用徽标。 但可将此徽标替代为自己的 [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo) 图像。 例如，如果这是来自某人的通知，则建议将应用徽标替换为此人的照片。

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

可使用 **HintCrop** 属性更改图像的裁剪效果。 例如，**Circle** 会将图像裁剪为圆形。 否则，图像为方形。 图像尺寸在 100% 缩放时为 48x48 像素。

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://picsum.photos/48?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```


## <a name="hero-image"></a>主图

**周年更新的新增功能**：toast 可显示主图，这是在 toast 横幅中以及在操作中心时突出显示的特别  [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage)。 图像尺寸在 100% 缩放时为 364x180 像素。

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastGenericHeroImage()
    {
        Source = "https://picsum.photos/364/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>内联图像

可提供在展开 toast 时显示的全宽内联图像。

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://picsum.photos/360/202?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```


## <a name="image-size-restrictions"></a>图像大小限制

Toast 通知中使用的图像可源自以下位置...

 - http://
 - ms-appx:///
 - ms-appdata:///

对于 http 和 https 远程 Web 图像，每个单独图像的文件大小存在一定限制。 在 Fall Creators Update (16299) 中，我们将正常连接时的限制提升至 3 MB，并将按流量计费的连接上的限制提升至 1 MB。 之前，图像大小始终限制在 200 KB。

| 正常连接 | 按流量计费的连接 | Fall Creators Update 之前 |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

如果图像超过此文件大小、无法下载或超时，则该图像将被删除，但通知的剩余部分会显示。


## <a name="attribution-text"></a>署名文本

**周年更新中的新增功能**：如果你需要引用你的内容源，可以使用署名文本。 此文本始终显示在通知底部，与应用标识或通知时间戳一起显示。

在不支持署名文本的旧 Windows 版本中，该文本仅显示为另一文本元素（假设你还没有达到最多的三个文本元素）。

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

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

**创建者更新中的新增功能**：现在，你可以用自己的准确表示消息/信息/内容生成时间的时间戳替代系统提供的时间戳。 可在操作中心查看此时间戳。

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

若要详细了解如何使用自定义时间戳，请参阅 [toast 上的自定义时间戳](custom-timestamps-on-toasts.md)。

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


## <a name="progress-bar"></a>进度栏

**创意者更新的新增功能**：可在 Toast 通知中提供进度栏，让用户时刻了解操作进度，例如下载进度。

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

若要详细了解如何使用进度栏，请参阅 [Toast 进度栏](toast-progress-bar.md)。


## <a name="headers"></a>标题

**创意者更新的新增功能**：可在操作中心将通知分到不同的标题下。 例如，你可以将群聊中的组消息分到一个标题下，或将常见主题的通知分到一个标题下等等。

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

若要详细了解如何使用标题，请参阅 [Toast 标题](toast-headers.md)。


## <a name="adaptive-content"></a>自适应内容

**周年更新中的新增功能**：除了上面指定的内容外，你还可以显示在展开 Toast 时可见的附加自适应内容。

这些附加内容是用 Adaptive 指定的，阅读[自适应磁贴文档](create-adaptive-tiles.md)可了解更多内容。

请注意，任何自适应内容均必须包含在 [**AdaptiveGroup**](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema#adaptivegroup) 中。 否则将不会使用自适应呈现。


### <a name="columns-and-text-elements"></a>列和文本元素

下面的示例使用了列和高级自适应文本元素。 由于文本元素位于 **AdaptiveGroup** 中，因此它们支持所有富自适应样式属性。

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

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


## <a name="buttons"></a>按钮

按钮使你的 Toast 可交互，让用户无需中断其当前工作流即可就你的 Toast 通知采取快速操作。 例如，用户可以直接从 Toast 内回复邮件，甚至在不打开电子邮件应用的情况下删除电子邮件。 按钮显示在通知的扩展部分。

若要详细了解如何端到端操控按钮，请参阅[发送本地 Toast](send-local-toast.md)。

按钮可执行以下不同操作...

-   通过可用于导航到特定页面/内容的参数在前台激活应用。
-   激活应用的后台任务以便快速回复或类似的场景。
-   通过协议启动激活另一个应用。
-   执行系统操作，如推迟或取消通知。

> [!NOTE]
> 最多只能具有 5 个按钮（包括我们稍后将讨论的关联菜单项）。

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

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
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="buttons-with-icons"></a>带图标的按钮

可向按钮添加图标。 这些图标在 100% 缩放时为白色透明的 16x16 像素图像，且图像本身不应包含任何填充。 如果选择在一条 Toast 通知上提供图标，则必须为该通知中的所有按钮提供图标，因为该操作会将按钮样式转换为图标按钮。

> [!NOTE]
> 为实现辅助功能，请务必添加“白对比色”版本的图标（白色背景的黑色图标），以便用户在打开高对比度白色模式时，图标清晰可见。 有关详细信息，请参阅 [Toast 辅助功能页面](tile-toast-language-scale-contrast.md)。

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

```csharp
new ToastButton("Dismiss", "dismiss")
{
    ActivationType = ToastActivationType.Background,
    ImageUri = "Assets/ToastButtonIcons/Dismiss.png"
}
```


```xml
<action
    content="Dismiss"
    imageUri="Assets/ToastButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```


### <a name="buttons-with-pending-update-activation"></a>具有待更新激活的按钮

**Fall Creators Update 的新增功能**：在后台激活按钮上，可使用 **PendingUpdate** 的激活后行为在 Toast 通知中创建多步骤的交互。 用户单击按钮后，将激活后台任务，此时 Toast 处于“等待更新”状态。该状态下，屏幕上始终显示 Toast，直到后台任务将其替换为新的 Toast。

若要了解如何实现此操作，请参阅 [Toast 等待更新](toast-pending-update.md)。

![有待更新的 Toast](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>关联菜单操作

**周年更新的新增功能**：可向现有关联菜单（用户从操作中心右键单击 Toast 时会出现）添加其他关联菜单。 请注意：此菜单仅在用户从操作中心右键单击时显示。 右键单击 Toast 弹出式横幅时，该菜单不会出现。

> [!NOTE]
> 在较旧的设备上，上述其他关联菜单操作仅显示为 Toast 上的正常按钮。

添加的其他关联菜单操作（如“更改位置”）显示在两个默认系统项的上方。

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

> [!NOTE]
> 其他关联菜单项计入 Toast 按钮（数量上限为 5 个）。

其他关联菜单项的激活方式与 Toast 按钮的相同。


## <a name="inputs"></a>输入

输入是在 toast 的 toast 区域的 actions 区域中指定的，这表示它们仅在 toast 展开时可见。


### <a name="quick-reply-text-box"></a>快速回复文本框

要启用快速回复文本框，例如在消息传递方案中，请添加文本输入和按钮，并引用文本输入的 ID 以使按钮显示在输入旁。

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

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

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>使用按钮栏的输入

还可使用一个（或多个）输入，在输入下方显示正常按钮。

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

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

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>选择输入

除文本框外，还可使用选择菜单。

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

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


### <a name="snoozedismiss"></a>推迟/消除

使用一个选择菜单和两个按钮，可创建利用系统推迟和消除操作的提醒通知。 请确保将方案设置为“提醒”以便通知像提醒那样工作。

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

我们使用 toast 按钮的 **SelectionBoxId** 属性将“推迟”按钮链接到选择菜单输入。

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

若要使用系统推迟和消除操作：

-   指定 **ToastButtonSnooze** 或 **ToastButtonDismiss**
-   （可选）指定一个自定义内容字符串：
    -   如果你不提供字符串，我们将自动使用“推迟”和“消除”的本地化字符串。
-   选择指定 **SelectionBoxId**：
    -   如果你不希望用户选择推迟间隔，而只是希望你的通知仅在系统定义的时间间隔内推迟一次（这在整个操作系统上都一致），则不要构建任何 &lt;input&gt;。
    -   如果你希望提供推迟间隔选择：
        -   在推迟操作中指定 **SelectionBoxId**
        -   将输入的 ID 与推迟操作的 **SelectionBoxId** 匹配
        -   将 **ToastSelectionBoxItem** 的值指定为一个以分钟为单位表示推迟间隔的 nonNegativeInteger。



## <a name="audio"></a>音频

移动版一直支持自定义音频，桌面版 1511（内部测试版本 10586）或更新版本也支持此功能。 可通过以下路径引用自定义音频：

-   ms-appx:///
-   ms-appdata:///

或者，你可以从 [ms winsoundevents 列表](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements)中选择，它始终在两种平台上受支持。

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

若要了解 toast 通知中的音频，请参阅[音频架构页面](/uwp/schemas/tiles/toastschema/element-audio)。 若要了解如何使用自定义音频发送 toast，请参阅 [toast 中的自定义音频](custom-audio-on-toasts.md)。


## <a name="alarms-reminders-and-incoming-calls"></a>闹钟、提醒和来电

要创建闹钟、提醒和来电通知，只需使用正常的 Toast 通知并为它分配一个方案值。 方案会调整几个行为以创建一致且统一的用户体验。

> [!IMPORTANT]
> 使用提醒或闹钟时，必须在 Toast 通知上提供至少一个按钮。 否则，该 Toast 将被视为正常 Toast。

* **提醒**：通知将保留在屏幕上，直到用户消除它或采取操作。 在 Windows 移动版上，该 Toast 还会以预先展开的形式显示。 将播放提醒声音。
* **闹钟**：除提醒行为外，闹钟将用默认的闹钟声音自动循环音频。
* **来电**：来电通知在 Windows 移动设备上全屏显示。 否则，除使用铃声音频且设置了不同的按钮样式外，它们的行为与闹钟相同。

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


## <a name="localization-and-accessibility"></a>本地化和辅助功能

磁贴和 toast 可加载为显示语言、显示比例系数、高对比度和其他运行时上下文定制的字符串和图像。 有关详细信息，请参阅[磁贴和 toast 通知的语言、比例和高对比度支持](tile-toast-language-scale-contrast.md)。


## <a name="handling-activation"></a>处理激活
要了解如何处理 Toast 激活（用户单击你的 Toast 或 Toast 上的按钮），请参阅[发送本地 Toast](send-local-toast.md)。
 
## <a name="related-topics"></a>相关主题

* [发送本地 toast 和处理激活](send-local-toast.md)
* [GitHub 上的通知库（UWP 社区工具包的一部分）](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [磁贴和 toast 通知的语言、比例和高对比度支持](tile-toast-language-scale-contrast.md)
