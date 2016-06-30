---
author: mijacobs
Description: "本文介绍了如何使用自适应磁贴模板将本地磁贴通知发送到主要磁贴和辅助磁贴。"
title: "发送本地磁贴通知"
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: cc2f86f2a56aae5ee9e3019dafa3417a25e7d610

---

# 发送本地磁贴通知





在 Windows 10 中，主要应用磁贴在应用清单中定义，而辅助磁贴由应用代码以编程方式创建和定义。 本文介绍了如何使用自适应磁贴模板将本地磁贴通知发送到主要磁贴和辅助磁贴。 （本地通知是从应用代码发送的通知，而不是从 Web 服务器推送或提取的通知。）

![默认磁贴和带有通知的磁贴](images/sending-local-tile-01.png)

**注意** 了解[创建自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)和[自适应磁贴模板架构](tiles-and-notifications-adaptive-tiles-schema.md)。

 

## <span id="Install_the_NuGet_package"></span><span id="install_the_nuget_package"></span><span id="INSTALL_THE_NUGET_PACKAGE"></span>安装 NuGet 程序包


我们建议安装 [NotificationsExtensions NuGet 程序包](https://www.nuget.org/packages/NotificationsExtensions.Win10/)，它可以通过生成带有对象而不是原始 XML 的磁贴负载来简化一些操作流程。

本文中的内联代码示例适用于安装了 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) NuGet 程序包的 C#。 （如果你希望创建自己的 XML，你可以在本文末尾找到不包含 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) 的代码示例。）

## <span id="Add_namespace_declarations"></span><span id="add_namespace_declarations"></span><span id="ADD_NAMESPACE_DECLARATIONS"></span>添加命名空间声明


若要访问磁贴 API，请包含 [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661) 命名空间。 我们还建议包含 **NotificationsExtensions.Tiles** 命名空间，以便你可以充分利用我们的磁贴帮助程序 API（必须安装 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) NuGet 程序包才能访问这些 API）。

```
using Windows.UI.Notifications;
using NotificationsExtensions.Tiles; // NotificationsExtensions.Win10
```

## <span id="Create_the_notification_content"></span><span id="create_the_notification_content"></span><span id="CREATE_THE_NOTIFICATION_CONTENT"></span>创建通知内容


在 Windows 10 中，使用自适应磁贴模板定义磁贴负载，这允许你为通知创建自定义视觉布局。 （若要了解自适应磁贴的功能，请参阅[创建自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)和[自适应磁贴模板](tiles-and-notifications-adaptive-tiles-schema.md)文章。）

此代码示例会为中等和加宽磁贴创建自适应磁贴内容。

```
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = from
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        },
 
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = from,
                        Style = TileTextStyle.Subtitle
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

通知内容在中磁贴上显示时如下所示：

![中磁贴上的通知内容](images/sending-local-tile-02.png)

## <span id="Create_the_notification"></span><span id="create_the_notification"></span><span id="CREATE_THE_NOTIFICATION"></span>创建通知


在拥有通知内容之后，你将需要创建一个新的 [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)。 **TileNotification** 构造函数会采用一个 Windows 运行时 [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br208620) 对象，如果你使用的是 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)，则可以通过 **TileContent.GetXml** 方法获取该对象。

此代码示例会为新磁贴创建一个通知。

```
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <span id="Set_an_expiration_time_for_the_notification__optional_"></span><span id="set_an_expiration_time_for_the_notification__optional_"></span><span id="SET_AN_EXPIRATION_TIME_FOR_THE_NOTIFICATION__OPTIONAL_"></span>设置通知的过期时间（可选）


默认情况下，本地磁贴和锁屏提醒通知不会过期，而推送通知、定期通知和计划通知会在三天之后过期。 因为磁贴内容的保留时间不应超过必要时间，因此最佳做法是设置对于你的应用合理的到期时间，对于本地磁贴和锁屏提醒通知尤其如此。

此代码示例创建了会在十分钟后到期并从磁贴中删除的通知。

```
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);</code></pre></td>
</tr>
</tbody>
</table>
```

## <span id="Send_the_notification"></span><span id="send_the_notification"></span><span id="SEND_THE_NOTIFICATION"></span>发送通知


尽管本地发送磁贴通知比较简单，但将通知发送到主要磁贴或辅助磁贴略有不同。

**主要磁贴**

若要将通知发送到主要磁贴，请使用 [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622) 为主要磁贴创建磁贴更新程序，并通过调用“更新”发送通知。 无论是否可见，你的应用的主要磁贴始终存在，因此你可以向其发送通知，即使它没有固定。 如果用户稍后固定你的主要磁贴，你发送的通知也将随后显示。

此代码示例将通知发送到主要磁贴。

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
// And send the notification
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**辅助磁贴**

若要将通知发送到辅助磁贴，请首先确保该辅助磁贴存在。 如果你尝试为不存在的辅助磁贴创建磁贴更新程序（例如，如果用户取消固定辅助磁贴），将引发异常。 你可以使用 [**SecondaryTile.Exists**](https://msdn.microsoft.com/library/windows/apps/br242205)(tileId) 来查看你的辅助磁贴是否已固定，然后为辅助磁贴创建磁贴更新程序并发送通知。

此代码示例将通知发送到辅助磁贴。

```
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![默认磁贴和带有通知的磁贴](images/sending-local-tile-01.png)

## <span id="Clear_notifications_on_the_tile__optional_"></span><span id="clear_notifications_on_the_tile__optional_"></span><span id="CLEAR_NOTIFICATIONS_ON_THE_TILE__OPTIONAL_"></span>清除磁贴上的通知（可选）


在大多数情况下，你应当在用户与该内容进行交互后清除通知。 例如，当用户启动你的应用时，你可能需要清除磁贴中的所有通知。 如果你的通知有时间限制，我们建议你对通知设置到期时间，而不是将其显式清除。

此代码示例清除磁贴通知。

```
TileUpdateManager.CreateTileUpdaterForApplication().Clear();</code></pre></td>
</tr>
</tbody>
</table>
```

对于启用通知队列且在队列中有通知的磁贴，调用 Clear 方法可清空队列。 但是，你无法通过应用的服务器清除通知；只有本地应用代码才能清除通知。

定期或推送通知只能添加新的通知或替换现有通知。 本地调用 Clear 方法会清除磁贴，不管它们自身附带的是推送通知、定期通知还是本地通知。 此方法不会清除尚未显示的计划通知。

![带有通知的磁贴和清除后的磁贴](images/sending-local-tile-03.png)

## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>后续步骤


**使用通知队列**

在执行了第一次磁贴更新之后，你可以通过启用[通知队列](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)来扩展磁贴的功能。

**其他通知传送方法**

本文将向你介绍如何将磁贴更新作为一个通知发送。 若要了解其他通知传送方法（包括计划通知、定期通知和推送通知），请参阅[传送通知](tiles-and-notifications-choosing-a-notification-delivery-method.md)。

**XmlEncode 传送方法**

如果你使用的不是 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)，此通知传送方法为备用方法。

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <span id="Code_examples_without_NotificationsExtensions"></span><span id="code_examples_without_notificationsextensions"></span><span id="CODE_EXAMPLES_WITHOUT_NOTIFICATIONSEXTENSIONS"></span>不包含 NotificationsExtensions 的代码示例


如果你希望使用原始 XML，而不是 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) NuGet 程序包，请使用本文提供的前三个示例的备用代码示例。 其余的代码示例可以与 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) 或原始 XML 结合使用。

添加命名空间声明

```
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

创建通知内容

```
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template=&#39;TileMedium&#39;>
            <text>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
        <binding template=&#39;TileWide&#39;>
            <text hint-style=&#39;subtitle&#39;>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

创建通知

```
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <span id="related_topics"></span>相关主题


* [创建自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)
* [自适应磁贴模板：架构和文档](tiles-and-notifications-adaptive-tiles-schema.md)
* [NotificationsExtensions.Win10（NuGet 程序包）](https://www.nuget.org/packages/NotificationsExtensions.Win10/)
* [GitHub 上的 NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Windows.UI.Notifications 命名空间**](https://msdn.microsoft.com/library/windows/apps/br208661)
* [如何使用通知队列 (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [传送通知](tiles-and-notifications-choosing-a-notification-delivery-method.md)
 

 







<!--HONumber=Jun16_HO4-->


