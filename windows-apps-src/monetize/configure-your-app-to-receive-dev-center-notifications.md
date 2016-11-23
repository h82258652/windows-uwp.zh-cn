---
author: mcleanbyron
Description: "了解如何注册 UWP 应用，以接收从 Windows 开发人员中心发送的推送通知。"
title: "配置应用以接收开发人员中心推送通知"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 0e6ac52f1e76c0f59cc428b2ff26dc524e93cbde

---

# 配置应用以接收开发人员中心推送通知

可以使用 Windows 开发人员中心仪表板中的**推送通知**页面，通过向已安装通用 Windows 平台 (UWP) 应用的设备发送定向推送通知来直接与客户交流。 例如，可以使用定向推送通知鼓励客户采取行动（如为应用评分或试用新功能）。 可以发送多个不同类型的推送通知，包括 Toast 通知、磁贴通知和 XML 原始通知。 还可以跟踪由推送通知导致的应用启动的速度。 有关此功能的详细信息，请参阅[将推送通知发送到应用客户](../publish/send-push-notifications-to-your-apps-customers.md)。

在可以从开发人员中心向客户发送定向推送通知之前，必须使用 Microsoft Store Services SDK 中的 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 类的方法注册应用，才能接收通知。 可以使用此类的其他方法通知开发人员中心，应用为响应定向推送通知已启动（如果想要跟踪由推送通知导致的应用启动的速度），也可以使用此类的其他方法停止接收通知。

## 配置项目

在编写任何代码之前，请遵循以下步骤在你的项目中添加对 Microsoft Store Services SDK 的引用：

1. 如果先前尚未这样做，请在开发计算机上[安装 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了用于注册应用接收通知的 API 外，此 SDK 还提供了用于其他功能的 API，例如在应用中通过 A/B 测试运行实验和显示广告。 
2. 在 Visual Studio 中打开你的项目。
3. 在“解决方案资源管理器”中，右键单击你的项目的“引用”节点，然后单击“添加引用”。
4. 在“引用管理器”中，展开“通用 Windows”并单击“扩展”。
5. 在 SDK 列表中，单击“Microsoft 协议框架”旁边的复选框，然后单击“确定”。

## 注册推送通知

若要注册应用以从开发人员中心接收定向推送通知，请执行以下操作：

1. 在项目中，找到启动过程中运行的代码部分，可以在其中注册用于接收开发人员中心通知的应用。
2. 将以下语句添加到代码文件顶部。

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```

3. 在之前确定的启动代码中，获取 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 对象，并调用其中一个 [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync.aspx) 重载。 每次启动应用时，都应该调用此方法。

  * 如果要开发人员中心为通知创建其自己的通道 URI，请调用 [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 重载。

    ```csharp
    StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
    await engagementManager.RegisterNotificationChannelAsync();
    ```

    >**重要信息**
            &nbsp;&nbsp;如果应用还会调用 [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) 为 WNS 创建通知通道，请确保代码不会同时调用 [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) 和 [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 重载。 如果需要调用这两个方法，请确保按序调用它们，即等待一个方法返回，然后再调用另一个方法。

  * 如果想要从开发人员中心指定用于定向推送通知的通道 URI，请调用 [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://msdn.microsoft.com/library/windows/apps/mt771191.aspx) 重载。 例如，在应用已使用 Windows 推送通知服务 (WNS) 并且想要使用同一通道 URI 时，可能希望这样做。 必须先创建 [StoreServicesNotificationChannelParameters](https://msdns.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.aspx) 对象，并将 [CustomNotificationChannelUri](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri.aspx) 属性分配给你的通道 URI。

    ```csharp
    StoreServicesNotificationChannelParameters parameters =
        new StoreServicesNotificationChannelParameters();
    parameters.CustomNotificationChannelUri = "Assign your channel URI here";

    StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
    await engagementManager.RegisterNotificationChannelAsync(parameters);
    ```

  >#### 了解应用在用户启动应用时如何响应

  >完成用于接收通知的应用注册并且[从开发人员中心向应用客户发送推送通知](../publish/send-push-notifications-to-your-apps-customers.md)之后，当用户为响应推送通知启动应用时，将调用应用中的以下入口点之一。 如果有一些代码想要在用户启动应用时运行，可以将该代码添加到应用中的这些入口点之一。

  >* 如果推送通知中含有前台激活类型，请覆盖项目中 **App** 类的 [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) 方法，然后向该方法中添加你的代码。

  >* 如果推送通知中含有后台激活类型，请将你的代码添加到[后台任务](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中。

  >例如，你可能想要通过以下方式对在你的应用中购买过任何付费加载项的用户进行奖励：给予这些用户免费的加载项。 在此情况下，可以将推送通知发送给面向这些用户的[客户群](../publish/create-customer-segments.md)。 然后，可以在上述所列的其中一个入口点中添加用于给予他们免费[应用内购买](in-app-purchases-and-trials.md)的代码。

## 通知开发人员中心应用启动

如果选择开发人员中心推送通知的“跟踪应用启动速率”选项，请从应用的相应入口点中调用 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 方法，以通知开发人员中心，应用为响应推送通知已启动。

该方法还会返回应用的原始启动参数。 在选择跟踪开发人员中心推送通知的应用启动速率后，会将不透明跟踪 ID 添加到启动参数，以帮助在开发人员中心跟踪应用启动。 必须将应用的启动参数传递到 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 方法，该方法将向开发人员中心发送跟踪 ID、从启动参数中删除跟踪 ID，然后将原始启动参数返回到你的代码。

调用此方法的方式取决于定向推送通知的激活类型：

* 如果推送通知中含有前台激活类型，请从应用的 [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) 方法重写中调用此方法，然后传递在向此方法传递的 [ToastNotificationActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.toastnotificationactivatedeventargs.aspx) 对象中可用的参数。 以下代码示例假设你的代码文件中已有 **Microsoft.Services.Store.Engagement** 和 **Windows.ApplicationModel.Activation** 命名空间的 **using** 语句。

  ```csharp
  protected override void OnActivated(IActivatedEventArgs args)
  {
       base.OnActivated(args);   

       if (args is ToastNotificationActivatedEventArgs)
       {
             var toastActivationArgs = args as ToastNotificationActivatedEventArgs;

             StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
             string originalArgs = engagementManager.ParseArgumentsAndTrackAppLaunch(
                 toastActivationArgs.Argument);

             // Use the originalArgs variable to access the original arguments
             // that were passed to the app.
       }
  }
  ```

* 如果推送通知中含有后台激活类型，请从[后台任务](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中调用此方法，然后传递在向此方法传递的 [ToastNotificationActionTriggerDetail](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.toastnotificationactiontriggerdetail.aspx) 对象中可用的参数。 以下代码示例假设你的代码文件中已有 **Microsoft.Services.Store.Engagement**、**Windows.ApplicationModel.Background** 和 **Windows.UI.Notifications** 命名空间的 **using** 语句。

  ```csharp
  public void Run(IBackgroundTaskInstance taskInstance)
  {
       var details = taskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;

       if (details != null)
       {
            StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
            string originalArgs = engagementManager.ParseArgumentsAndTrackAppLaunch(details.Argument);

            // Use the originalArgs variable to access the original arguments
            // that were passed to the app.
       }
  }
  ```

## 注销推送通知

如果想要应用停止接收定向 Windows 开发人员中心推送通知，请调用 [UnregisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 方法。

```csharp
StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
await engagementManager.UnregisterNotificationChannelAsync();
```

请注意，此方法会使用于通知的通道失效，因此该应用不再从*任何*服务接收推送通知。 通道已关闭后，该通道再也不能用于任何服务，其中包括定向 Windows 开发人员中心推送通知和使用 WNS 的其他通知。 若要恢复向此应用发送推送通知，该应用必须请求新的通道。

## 相关主题

* [向应用客户发送推送通知](../publish/send-push-notifications-to-your-apps-customers.md)
* [Windows 推送通知服务 (WNS) 概述](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [如何请求、创建和保存通知通道](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868221)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->


