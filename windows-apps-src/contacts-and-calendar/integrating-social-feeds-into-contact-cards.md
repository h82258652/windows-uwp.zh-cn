---
author: normesta
description: "显示如何将社交源集成到“人脉”应用"
MSHAttr: PreferredLib:/library/windows/apps
title: "向“人脉”应用提供社交源"
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# 向“人脉”应用提供社交源

将社交源数据从数据库集成到“人脉”应用。

你的源数据将显示在“人脉”应用的“最近更新”****或联系人的“个人资料”****页面中。

用户可以点击某个源项来打开你的应用。

![“人脉”应用中的社交源](images/social-feeds.png)

若要开始，请创建一个标记社交源联系人的前台应用和一个向“人脉”应用发送源数据的后台代理。

有关更完整的示例，请参阅[社交信息示例](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)。

## 创建前台应用

首先，创建一个通用 Windows 平台 (UWP) 项目，然后向它添加**适用于 UWP 的 Windows 移动扩展**。

![移动扩展](images/mobile-extensions.png)

### 查找或创建联系人

你可以使用姓名、电子邮件地址或电话号码来查找联系人。

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
你还可以创建联系人，然后将它们添加到联系人列表。

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### 使用批注标记每个联系人

此*批注*导致“人脉”应用从后台代理请求联系人的源数据。

作为批注的一部分，将联系人的 ID 与你的应用在内部用于标识该联系人的 ID 关联。

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### 预配后台代理

请确保 [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) API 合约在将运行你的应用的设备上可用。

如果可用，则预配后台代理。

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## 创建后台代理

后台代理是一个 Windows 运行时组件，可响应来自“人脉”应用的源请求。

在代理中，你将通过提供来自数据库的“人脉”应用源数据来响应这些请求。

### 创建 Windows 运行时组件

将“Windows 运行时组件(通用 Windows)”****项目添加到你的解决方案。

![Windows 运行时组件](images/windows-runtime-component.png)

### 将后台代理注册为应用服务

通过将协议处理程序添加到清单的 ``Extensions`` 元素来注册。

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
你还可以在 Visual Studio 中的清单设计器的“声明”****选项卡中添加这些协议处理程序。

![清单设计器中的应用服务](images/manifest-designer-app-service.png)

### 从“人脉”应用请求操作

询问“人脉”应用接下来它需要哪种类型的数据。 “人脉”应用将通过用于指示它需要哪个源的数据的代码来响应你的请求。

此表介绍每个源：

| 源 | 说明 |
|-------|-------------|
| 主页 | 显示在“人脉”应用的“最近更新”页面中的源。 |
| 联系人 | 显示在联系人的“最近更新”页面中的源。 |
| 仪表板 | 显示在个人资料图片旁边的联系人卡片中的源。 |
<br>
你将通过请求 *operation* 来询问“人脉”应用。 实现 [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx) 接口并替代 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法。

在 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中，向“人脉”应用发送两个键值对。 其中一个包含协议的版本，另一个包含操作的类型。

然后，侦听来自“人脉”应用的响应。 该响应将包含代码。

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

请参考 [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) 属性的 ``Type`` 元素来获取该代码。 下面是代码的完整列表。

| 类型| 说明 |
|-----|-------------|
| 0x10 | 针对下一次操作向“人脉”应用发出的请求。 |
| 0x11 | 来自“人脉”应用的为主要用户提供主页源的请求。 |
| 0x13 | 来自“人脉”应用的获取选定联系人的联系人源的请求。 |
| 0x15 | 来自“人脉”应用的获取选定联系人仪表板项的请求。 |
| 0x80 | 指示操作已完成。 这将通知“人脉”应用数据现在可用。 |
| 0xF1 | 来自“人脉”应用的消息，指示它不需要任何其他操作。 后台代理现在可以关闭。 |
<br>
[AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) 属性还返回描述该响应的其他键值对的集合。 下面是它们的列表。

| 密钥 | 类型 | 说明 |
|-----|------|-------------|
| 版本 | UINT32 | （必需）表示消息协议的版本。 高 16 位是主要版本，低 16 位是次要版本。 |
| 类型 | UINT32 | （必需）要执行的操作类型。 上一个示例使用 Type 键确定“人脉”应用正在请求哪种操作。
| OperationId | UINT32 | 操作的 ID。 |
| OwnerRemoteId | 字符串 | 你的应用在内部用于标识该联系人的 ID。 |
| LastFeedItemTimeStamp | 字符串 | 最后一个检索到的源项的 ID。 |
| LastFeedItemTimeStamp | 日期/时间 | 最后一个检索到的源项的时间戳。 |
| ItemCount | UINT32 | “人脉”应用要求的项目数。 |
| IsFetchMore | 布尔 | 确定何时更新内部缓存。 |
| ErrorCode | UINT32 | 与后台代理操作关联的错误代码。 |
<br>
### 向“人脉”应用提供数据源

``0x11``、``0x13`` 或 ``0x15`` 的 **Type** 值是来自“人脉”应用对源数据的请求。  

接下来的一些代码段显示向“人脉”应用提供该数据的方法。

> [!NOTE]
> 这些代码段来自[社交信息示例](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)。 它们包含对在示例中的其他位置定义的接口、类和成员的引用。 使用这些代码段以及本主题中的其他示例来了解任务的流程，如果你有兴趣进一步了解接口、类和类型的堆栈，请参考示例。

**获取联系人源项**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**获取仪表板项**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**获取主页源项**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### 将成功或失败通知发送回“人脉”应用

将你的调用封装在 try catch 块中，然后在提供源数据后将成功或失败消息传递回“人脉”应用。

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


