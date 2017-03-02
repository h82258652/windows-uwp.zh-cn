---
author: mijacobs
Description: "借助自适应和交互式 Toast 通知，你可以创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。"
title: "自适应和交互式 Toast 通知"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b1962e58d3513ddff908a0d556731d83cce20af4
ms.lasthandoff: 02/07/2017

---
# <a name="adaptive-and-interactive-toast-notifications"></a>自适应和交互式 Toast 通知

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

借助自适应和交互式 Toast 通知，你可以创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。

自适应和交互式 Toast 通知模型具有以下针对传统 Toast 模板目录的更新：

-   在通知上包含按钮和输入的选项。
-   主 Toast 通知和每个操作的三个不同的激活类型。
-   为特定方案创建通知的选项，包括闹钟、提醒和来电。

**注意**   若要查看 Windows 8.1 和 Windows Phone 8.1 中的传统模板，请参阅 [传统 Toast 模板目录](https://msdn.microsoft.com/library/windows/apps/hh761494)。


## <a name="getting-started"></a>入门

**安装通知库。** 如果希望使用 C# 而不是 XML 来生成通知，请安装名为 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 程序包（搜索“notifications uwp”）。 本文中提供的 C# 示例使用 NuGet 程序包的版本 1.0.0。

**安装通知可视化工具。** 此免费 UWP 应用通过在你编辑时提供 toast 的即时可视预览来帮助你设计交互 toast 通知，类似于 Visual Studio 的 XAML 编辑器/设计视图。 你可以阅读[此博客文章](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)获取详细信息，并且可以在[此处](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)下载通知可视化工具。


## <a name="toast-notification-structure"></a>Toast 通知结构


Toast 通知使用 XML 构建，这通常包含以下关键元素：

-   &lt;visual&gt; 涵盖可供用户从视觉上查看的内容，包括文本和图像
-   &lt;actions&gt; 包含开发人员希望在通知内添加的按钮/输入
-   &lt;audio&gt; 指定在通知弹出时播放的声音

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

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Sample"
                },
 
                new AdaptiveText()
                {
                    Text = "This is a simple toast notification example"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "oneAlarm.png"
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("check", "check")
            {
                ImageUri = "check.png"
            },
 
            new ToastButton("cancel", "cancel")
            {
                ImageUri = "cancel.png"
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = new Uri("ms-winsoundevent:Notification.Reminder")
    }
};
```

以及该结构的可视化表示形式：

![Toast 通知结构](images/adaptivetoasts-structure.jpg)

### <a name="visual"></a>视觉

在视觉元素内，你必须正好有一个包含 Toast 的视觉内容的绑定元素。

通用 Windows 平台 (UWP) 应用中的磁贴通知支持基于不同磁贴大小的多个模板。 但是，Toast 通知只有一个模板名称：**ToastGeneric**。 只有一个模板名称意味着：

-   你可以更改 Toast 内容，例如添加另一行文本、添加嵌入式图像或将缩略图图像从显示应用图标更改为其他内容，并且在执行任意上述操作时无需担心更改整个模板或由于模板名称和内容之间的不匹配而创建无效的负载。
-   你可以使用相同代码为专用于传递到不同类型的 Microsoft Windows 设备（包括手机、平板电脑、PC 和 Xbox One）的 **Toast 通知**构建相同的负载。 其中每台设备都将接受通知，并使用相应的视觉提示和交互模型根据其 UI策略向用户显示通知。

有关视觉部分及其子元素中受支持的所有属性，请参阅下面的“架构”部分。 有关更多示例，请参阅下面的 XML 示例部分。

应用标识通过应用图标传达。 但是，如果使用 appLogoOverride，我们将在文本行下显示应用名称。

| 正常 toast                                                                              | 使用 appLogoOverride 的 toast                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![无 appLogoOverride 的通知](images/adaptivetoasts-withoutapplogooverride.jpg) | ![有 appLogoOverride 的通知](images/adaptivetoasts-withapplogooverride.jpg) |

### <a name="actions"></a>操作

在 UWP 应用中，你可以将按钮和其他输入添加到 Toast 通知，这可使用户在应用外执行更多操作。 这些操作在 &lt;actions&gt; 元素下指定，其中有两个可以指定的类型：

-   &lt;action&gt; 这将显示为桌面和移动设备上的按钮。 你可以在 Toast 通知内指定最多五个自定义或系统操作。
-   &lt;input&gt; 这允许用户提供输入，如对消息的快速回复或从下拉菜单中选择一个选项。

&lt;action&gt; 和 &lt;input&gt; 在 Windows 设备系列内均是自适应的。 例如，在移动或桌面设备上，面向用户的 &lt;action&gt; 是可点击/单击的按钮。 文本 &lt;input&gt; 是用户可使用物理键盘或屏幕键盘输入文本的框。 这些元素还会适应将来的交互方案，如通过语音宣布的操作或通过听写获取的文本输入。

当用户采用某个操作时，你可以通过在 &lt;action&gt; 元素内指定 [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) 属性来执行以下操作之一：

-   使用特定于操作的参数在前台激活应用，该参数可用于导航到特定页面/上下文。
-   在不影响用户的情况下激活应用的后台任务。
-   通过协议启动激活另一个应用。
-   执行要执行的系统操作。 当前可用系统操作将推迟和解除计划的警报/提醒，这将在下面部分中进一步介绍。

有关视觉部分及其子元素中受支持的所有属性，请参阅下面的“架构”部分。 有关更多示例，请参阅下面的 XML 示例部分。

### <a name="audio"></a>Audio

移动版一直支持自定义音频，桌面版 1511（内部测试版本 10586）或更新版本也支持此功能。 可通过以下路径引用自定义音频：

-   ms-appx:///
-   ms-appdata:///

或者，你可以从 [ms winsoundevents 列表](https://msdn.microsoft.com/library/windows/apps/br230842)中选择，它始终在两种平台上受支持。

有关 Toast 通知中的音频的信息，请参阅[音频架构页面](https://msdn.microsoft.com/library/windows/apps/br230842)。 若要了解如何使用自定义音频发送 Toast，[请参阅此博客文章](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/)。

## <a name="alarms-reminders-and-incoming-calls"></a>闹钟、提醒和来电


你可以将 Toast 通知用于闹钟、提醒和来电。 这些特殊 Toasts 具有与标准 Toast 一致的外观，尽管特殊 Toast 具有某些自定义、基于方案的 UI 和模式：

-   提醒 Toast 通知将保留在屏幕上，直到用户取消它或执行操作。 在 Windows 移动版上，提醒 Toast 通知还会以预先展开的形式显示。
-   除了与提醒通知共享上述行为，闹钟通知还会自动播放循环音频。
-   来电通知将在 Windows Mobile 设备上全屏显示。 这通过在 Toast 通知的根元素内指定方案属性来实现 - &lt;toast&gt;：&lt;toast scenario=" { default | alarm | reminder | incomingCall } " &gt;

## <a name="xml-examples"></a>XML 示例


**注意**  这些示例的 Toast 通知屏幕截图取自桌面上的应用。 在移动设备上，Toast 通知可在弹出时处于折叠状态，并且 Toast 底部带有用于展开它的捕获器。

 

**带有丰富视觉内容的通知**

此示例向你介绍如何具有多行文本、一个用于替代应用程序徽标的可选小图像和一个可选的嵌入式图像缩略图。

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image src="https://unsplash.it/360/180?image=1043" />
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Photo Share"
                },
 
                new AdaptiveText()
                {
                    Text = "Andrew sent you a picture"
                },
 
                new AdaptiveText()
                {
                    Text = "See it in full size!"
                },
 
                new AdaptiveImage()
                {
                    Source = "https://unsplash.it/360/180?image=1043"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    }
};
```

![带有丰富视觉内容的通知](images/adaptivetoasts-xmlsample01.jpg)

 

**带有操作的通知，示例 1**

此示例显示...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="See more details" arguments="details"/>
    <action activationType="background" content="Remind me later" arguments="later"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Microsoft Company Store"
                },
 
                new AdaptiveText()
                {
                    Text = "New Halo game is back in stock!"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "details"),
 
            new ToastButton("Remind me later", "later")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![带有操作的通知，示例 1](images/adaptivetoasts-xmlsample02.jpg)

 

**带有操作的通知，示例 2**

此示例显示...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Restaurant suggestion...</text>
      <text>We noticed that you are near Wasaki. Thomas left a 5 star rating after his last visit, do you want to try it?</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="Reviews" arguments="reviews" />
    <action activationType="protocol" content="Show map" arguments="bingmaps:?q=sushi" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Restaurant suggestion..."
                },
 
                new AdaptiveText()
                {
                    Text = "We noticed that you are near Wasaki. Thomas left a 5 star rating after his last visit, do you want to try it?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("Reviews", "reviews"),
 
            new ToastButton("Show map", "bingmaps:?q=sushi")
            {
                ActivationType = ToastActivationType.Protocol
            }
        }
    }
};
```

![带有操作的通知，示例 2](images/adaptivetoasts-xmlsample03.jpg)

 

**带有文本输入和操作的通知，示例 1**

此示例显示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" />
    <action activationType="foreground" content="Video call" arguments="video" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Video call", "video")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![带有文本和输入操作的通知](images/adaptivetoasts-xmlsample04.jpg)

 

**带有文本输入和操作的通知，示例 2**

此示例显示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" hint-inputId="message" imageUri="Assets/Icons/send.png"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                TextBoxId = "message",
                ImageUri = "Assets/Icons/send.png",
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![带有文本输入和操作的通知](images/adaptivetoasts-xmlsample05.jpg)

 

**带有选择输入和操作的通知**

此示例显示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
      <selection id="1" content="Breakfast" />
      <selection id="2" content="Lunch" />
      <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="foreground" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Spicy Heaven"
                },
 
                new AdaptiveText()
                {
                    Text = "When do you plan to come in tomorrow?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "2",
                Items =
                {
                    new ToastSelectionBoxItem("1", "Breakfast"),
                    new ToastSelectionBoxItem("2", "Lunch"),
                    new ToastSelectionBoxItem("3", "Dinner")
                }
            }
        },
 
        Buttons =
        {
            new ToastButton("Reserve", "reserve")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Call Restaurant", "call")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![带有选择输入和操作的通知](images/adaptivetoasts-xmlsample06.jpg)

 

**提醒通知**

此示例显示...

```XML
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  <visual>
    <binding template="ToastGeneric">
      <text>Adaptive Tiles Meeting</text>
      <text>Conf Room 2001 / Building 135</text>
      <text>10:00 AM - 10:30 AM</text>
    </binding>
  </visual>
 
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

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "action=viewEvent&eventId=1983",
    Scenario = ToastScenario.Reminder,
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Adaptive Tiles Meeting"
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
    },
 
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

![提醒通知](images/adaptivetoasts-xmlsample07.jpg)

 

## <a name="handling-activation-foreground-and-background"></a>处理激活（前台和后台）

若要了解如何处理 toast 激活（用户单击 toast 或 toast 上的按钮），请参阅[快速入门：发送本地 toast 通知和处理激活](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/)。


## <a name="schemas-ltvisualgt-and-ltaudiogt"></a>架构：&lt;visual&gt; 和 &lt;audio&gt;


在以下 XML 架构中，“?”后缀表示属性为可选属性。

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Launch = ?,
    Duration = ?,
    ActivationType = ?,
    Scenario = ?,
 
    Visual = new ToastVisual()
    {
        Language = ?,
        BaseUri = ?,
        AddImageQuery = ?,
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = ?,
                    Language = ?,
                    HintMaxLines = ?
                },
 
                new AdaptiveGroup()
                {
                    Children =
                    {
                        new AdaptiveSubgroup()
                        {
                            HintWeight = ?,
                            HintTextStacking = ?,
                            Children =
                            {
                                new AdaptiveText(),
                                new AdaptiveImage()
                            }
                        }
                    }
                },
 
                new AdaptiveImage()
                {
                    Source = ?,
                    AddImageQuery = ?,
                    AlternateText = ?,
                    HintCrop = ?
                }
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = ?,
        Loop = ?,
        Silent = ?
    }
};
```

**&lt;toast 中的属性&gt;**

launch?

-   launch? = string
-   这是可选属性。
-   当应用程序由 Toast 激活时向其传递的字符串。
-   根据 activationType 的值，此值可由前台中的应用在后台任务内接收或由从原始应用协议启动的另一个应用接收。
-   此字符串的格式和内容由应用根据其自身用途定义。
-   当用户点击或单击 Toast 来启动其关联应用时，启动字符串会向应用提供上下文，以允许该应用向用户显示与 Toast 内容相关的视图，而不是以其默认方式启动。
-   如果由于用户单击某个操作（而不是 Toast 的正文）而发生激活，开发人员会检索回在该 &lt;action&gt; 标记中预定义的“arguments”，而不是在 &lt;toast&gt; 标记中预定义的“launch”。

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

**&lt;visual 中的属性&gt;**

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

baseUri?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**&lt;binding 中的属性&gt;**

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

**&lt;text 中的属性&gt;**

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**&lt;image 中的属性&gt;**

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

**&lt;audio 中的属性&gt;**

src?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

loop?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

silent?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

## <a name="schemas-ltactiongt"></a>架构：&lt;action&gt;


在以下 XML 架构中，“?”后缀表示属性为可选属性。

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

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("id")
            {
                Title = ?
                DefaultSelectionBoxItemId = ?,
                Items =
                {
                    new ToastSelectionBoxItem("id", "content")
                }
            },
 
            new ToastTextBox("id")
            {
                Title = ?,
                PlaceholderContent = ?,
                DefaultInput = ?
            }
        },
 
        Buttons =
        {
            new ToastButton("content", "args")
            {
                ActivationType = ?,
                ImageUri = ?,
                TextBoxId = ?
            },
 
            new ToastButtonSnooze("content")
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss("content")
        }
    }
};
```

**&lt;input 中的属性&gt;**

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

**&lt;selection 中的属性&gt;**

id

-   此属性是必需的。 它用于标识用户选择。 id 将返回到你的应用。

content

-   此属性是必需的。 它为此选择元素提供要显示的字符串。

**&lt;action 中的属性&gt;**

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

## <a name="attributes-for-system-handled-actions"></a>用于系统处理的操作的属性


如果你不希望应用将通知的推迟/重新计划作为后台任务处理，系统可以处理推迟和取消通知的操作。 系统处理的操作可以组合（或单独指定），但我们不建议在没有取消操作的情况下实现推迟操作。

系统命令组合：SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsSnoozeAndDismiss()
};
```

单独系统处理的操作

```
<toast>
  <visual>
  </visual>
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

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
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
                    new ToastSelectionBoxItem("10", "10 minutes"),
                    new ToastSelectionBoxItem("20", "20 minutes"),
                    new ToastSelectionBoxItem("30", "30 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour")
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

若要构建单独的推迟和取消操作，请执行以下步骤：

-   Specify activationType = "system"
-   Specify arguments = "snooze" | "dismiss"
-   指定内容：
    -   如果你希望“snooze”和“dismiss”的本地化字符串在操作上显示，请将内容指定为空字符串：&lt;action content = ""/&gt;
    -   如果你需要自定义字符串，只需提供其值：&lt;action content="Remind me later" /&gt;
-   指定输入：
    -   如果你不希望用户选择推迟间隔，而只是希望你的通知仅在系统定义的时间间隔内推迟一次（这在整个操作系统上都一致），则不要构建任何 &lt;input&gt;。
    -   如果你希望提供推迟间隔选择：
        -   在推迟操作中指定 hint-inputId
        -   将输入的 id 与推迟操作的 hint-inputId 相匹配：&lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   将选择 id 指定为以分钟为单位表示推迟间隔的 nonNegativeInteger：&lt;selection id="240" /&gt; 表示推迟 4 小时
        -   请确保 &lt;input&gt; 中的 defaultInput 值与 &lt;selection&gt; 子元素的 id 之一相匹配
        -   提供最多（但不多于）5 个 &lt;selection&gt; 值

 

 
## <a name="related-topics"></a>相关主题

* [快速入门：发送本地 toast 和句柄激活](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [GitHub 上的通知库](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)
