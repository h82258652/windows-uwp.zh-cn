---
author: manoskow
Description: Learn how to group notifications in Action Center using collections.
title: Toast 集合
label: Toast Collections
template: detail.hbs
ms.author: mijacobs
ms.date: 05/16/2018
ms.topic: article
keywords: windows 10, uwp, 通知, 集合, 通知分组, 分组, 组织, 操作中心, toast
ms.localizationpriority: medium
ms.openlocfilehash: be7c4ec2e9a47eeeb00663ae94f89e44c6751352
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6977851"
---
# <a name="grouping-toast-notifications-with-collections"></a>通过集合对 Toast 通知分组
在操作中心使用集合来组织应用的 Toast。 集合能帮助用户更轻松地在操作中心找到信息，并帮助开发人员更好地管理他们的通知。  以下 API 可用于删除、创建和更新通知集合。

> [!IMPORTANT]
> **需要创意者更新**：目标必须为 SDK 15063，并且必须运行版本 15063 或更高版本才能使用 Toast 集合。 相关 API 包括 [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection) 和 [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager)

下面的示例包含一个消息传送应用，该应用根据聊天群组将通知分开，每个标题（Comp Sci 160A Project Chat、Direct Messages、Lacrosse Team Chat）都代表一个单独的集合。  可以看到，虽然所有通知都来自同一个应用，但它们分成了各不相关的组，好像来自不同的应用一样。  如果希望使用更巧妙的方式来组织通知，请参阅 [Toast 标题](toast-headers.md)。  
![包含两个不同通知组的集合示例](images/toast-collection-example.png)

## <a name="creating-collections"></a>创建集合
创建每个集合时，须提供显示名称和图标，二者作为集合标题的一部分显示在操作中心，如上图所示。 集合需要一个启动参数，以在用户单击集合的标题时，帮助应用导航到正确的位置。  

### <a name="create-a-collection"></a>创建集合

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>向集合发送通知
我们将介绍从三种不同的 Toast 管道发送通知的方式：本地、计划和推送。  对于其中每个示例，我们将创建一个示例 Toast 以通过其下方的代码发送，然后重点看一下如何通过每个管道将 Toast 发送到集合。

构造通知有效负载：

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>将 Toast 发送到集合

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>将计划 Toast 发送到集合

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>将推送 Toast 发送到集合
对于推送 Toast，需要将 X-WNS-CollectionId 标题添加到 POST 消息。
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>管理集合
#### <a name="create-the-toast-collection-manager"></a>创建 Toast 集合管理器
在此“管理集合”部分的其余代码段中，我们将使用下面的 collectionManager。
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>获取所有集合

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>获取所建集合数目

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>删除集合

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>更新集合
可以通过以下方式来更新集合：创建具有相同 ID 的新集合并保存该集合的新实例。
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>管理集合中的 Toast
#### <a name="group-and-tag-properties"></a>分组和标记属性
分组和标记属性共同唯一标识集合中的通知。  分组（和标记）充当一个符合主键（多个标识符）来标识通知。 例如，如果要删除或替换某个通知，必须能够指定要删除/替换*哪个通知*，你通过指定标记和分组来实现此目的。 以消息传送应用为例。  开发人员应使用对话 ID 作为分组，使用消息 ID 作为标记。

#### <a name="remove-a-toast-from-a-collection"></a>从集合中删除 Toast
可以使用标记和分组 ID 删除各个 Toast，或者清除集合中的所有 Toast。
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>清除集合中的所有 Toast
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>通知可视化工具中的集合
可以使用[通知可视化工具](notifications-visualizer.md)帮助设计集合。 按以下步骤操作：

* 单击右下角的齿轮图标。 
* 选择“Toast 集合”。
* 在 Toast 预览上方，有一个“Toast 集合”下拉菜单。 选择管理集合。
* 单击“添加集合”，填写集合的详细信息，然后保存。
* 可以添加更多集合，或者点击关掉管理集合框以返回到主屏幕。
* 从“Toast 集合”下拉菜单中，选择要将 Toast 添加到的集合。
* 当激发 Toast 时，它将添加到操作中心的相应集合中。


## <a name="other-details"></a>其他详细信息
所创建的 Toast 集合还会反映在用户的通知设置中。  用户可以切换每个集合的设置以打开或关闭这些子组。  如果在应用顶层关闭通知，则会关闭所有集合通知。  此外，每个集合还在操作中心默认显示 3 个通知，用户可以扩展此设置以最多显示 20 个通知。

## <a name="related-topics"></a>相关主题

* [Toast 内容](adaptive-interactive-toasts.md)
* [Toast 标题](toast-headers.md)
* [GitHub 上的通知库（Windows 社区工具包的一部分）](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)