---
title: “我的人脉”通知
description: 介绍如何创建和使用“我的人脉”通知，这类通知是一种新的 Toast。
author: muhsinking
ms.author: mukin
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 943d236699ccab6d61e5394426077a32d7249592
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7305633"
---
# <a name="my-people-notifications"></a>“我的人脉”通知

“我的人脉”通知提供了一种新的方式，可让用户通过快速的表意手势与他们所关心的人员进行联系。 本文介绍如何在应用程序中设计和实现“我的人脉”通知。 有关完整的实现，请参阅[“我的人脉”通知示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)。

![心形表情符号通知](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>要求

+ Windows 10 和 Microsoft Visual Studio 2017。 有关安装详细信息，请参阅[设置 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ C# 或类似面向对象的编程语言的基础知识。 若要开始使用 C#，请参阅[创建“Hello, world”应用](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="how-it-works"></a>工作原理

作为常规 Toast 通知的替代方法，你现在可以通过“我的人脉”功能发送通知，以向用户提供更加个性化的体验。 这是从固定到具有“我的人脉”功能的用户任务栏的联系人发送的一种新型 Toast。 收到通知时，将在任务栏中动态显示发件人的联系人图片并且将播放声音，这表示通知正在启动。 负载中指定的动画或图像将显示 5 秒钟（或者，如果负载是持续时间少于 5 秒的动画，则将循环显示，直到 5 秒钟过后为止）。

## <a name="supported-image-types"></a>支持的图像类型

+ GIF
+ 静态图像（JPEG、PNG）
+ Spritesheet（仅限垂直）

> [!NOTE]
> Spritesheet 是源于静态图像（JPEG 或 PNG）的动画。 单个帧会进行垂直排列，使得第一帧位于顶部（但你可以在 Toast 负载中指定不同的起始帧）。 每个帧都必须具有相同的高度，这样，程序会进行循环以创建动态序列（例如页面采用垂直布局的翻页书）。 下面显示了 SpriteSheet 的一个示例。

![彩虹 Spritesheet](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>通知参数
“我的人脉”通知使用 [Toast 通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)框架，但在 Toast 负载中需要一个附加的绑定节点。 此第二个绑定必须包含以下参数：

```xml
experienceType=”shoulderTap”
```

这表示 Toast 应被视为“我的人脉”通知。

绑定内部的图像节点应包含以下参数：

+ **src**
    + 资源的 URI。 这可以是 HTTP/HTTPS Web URI、msappx URI 或本地文件的路径。
+ **spritesheet-src**
    + 资源的 URI。 这可以是 HTTP/HTTPS Web URI、msappx URI 或本地文件的路径。 只有 Spritesheet 动画才需要此参数。
+ **spritesheet-height**
    + 帧高度（以像素为单位）。 只有 Spritesheet 动画才需要此参数。
+ **spritesheet-fps**
    + 每秒帧数 (FPS)。 只有 Spritesheet 动画才需要此参数。 仅支持值 1-120。
+ **spritesheet-startingFrame**
    + 开始播放动画的帧编号。 此参数仅用于 Spritesheet 动画，如果未提供，则默认为 0。
+ **alt**
    + 用于屏幕阅读器旁白的文本字符串。

> [!NOTE]
> 当发出自动通知时，你仍应该在“src”参数中指定静态图像。 如果动画无法显示，该图像将用作回退。

此外，顶级 Toast 节点必须包含 **hint-people** 参数以指定发送方联系人。 此参数可能具有以下任何值：

+ **电子邮件地址** 
    + 例如 mailto:johndoe@mydomain.com
+ **电话号码** 
    + 例如 tel:888-888-8888
+ **远程 ID** 
    + 例如 remoteid:1234

> [!NOTE]
> 如果应用使用 [ContactStore API](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore)，并且使用 [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) 属性将存储在电脑上的联系人链接至远程存储的联系人，则 RemoteId 属性的值必须稳定并且是唯一的。 这意味着远程 ID 必须一致地标识单个用户帐户，并且应包含唯一标记，以保证它不与电脑上其他联系人（包括其他应用所拥有的联系人）的远程 ID 冲突。
> 如果无法保证应用所使用的远程 ID 的稳定性和唯一性，则可以使用 [RemoteIdHelper 类](https://msdn.microsoft.com/en-us/library/windows/apps/jj207024(v=vs.105).aspx#BKMK_UsingtheRemoteIdHelperclass)，以便在将所有远程 ID 添加到系统之前，先将唯一标记添加到这些 ID 中。 或者，你也可以选择彻底不使用 RemoteId 属性，而是创建一个自定义扩展属性来存储联系人的远程 ID。

除了第二个绑定和负载，你必须在回退 Toast 的第一个绑定中包含另一个负载。 如果回退 Toast 被强制恢复为普通 Toast，通知将使用该负载（[本文末尾](https://review.docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications#falling-back-to-toast)进行了进一步说明）。

## <a name="creating-the-notification"></a>创建通知
你可以创建“我的人脉”通知模板，就像创建 [Toast 通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)一样。

下面是如何使用静态图像负载创建“我的人脉”通知的一个示例：

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

当你启动通知时，通知应该如下所示：

![静态图像通知](images/static-image-notification-small.gif)

下面是如何使用动画 SpriteSheet 负载创建通知的一个示例。 此 Spritesheet 的帧高度为 80 个像素，我们将以每秒 25 帧的速度呈现动画。 我们将起始帧设置为 15，并在“src”参数中为其提供一个静态回退图像。 如果无法显示 Spritesheet 动画，则使用回退图像。

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

当你启动通知时，通知应该如下所示：

![Spritesheet 通知](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>启动通知
为了启动“我的人脉”通知，我们需要将 Toast 模板转换为 [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx) 对象。 当你已在 XML 文件（此处名为“content.xml”）中定义 Toast 时，可以使用此 C# 代码启动它：

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

然后，你可以使用此代码来创建和发送 Toast：

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>回退到 Toast
在某些情况下，“我的人脉”通知将改为显示为常规 Toast 通知。 在以下情况下，“我的人脉”通知将回退到 Toast：

+ 通知无法显示
+ 收件人未启用“我的人脉”通知
+ 发件人的联系人未固定到收件人的任务栏

如果“我的人脉”通知回退到 Toast，则将忽略第二个特定于“我的人脉”的绑定，并且仅使用第一个绑定来显示 Toast。 这正是为何在第一个 Toast 绑定中提供回退负载很重要。

## <a name="see-also"></a>另请参阅
+ [“我的人脉”通知示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [添加“我的人脉”支持](my-people-support.md)
+ [自适应 Toast 通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [ToastNotification 类](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)