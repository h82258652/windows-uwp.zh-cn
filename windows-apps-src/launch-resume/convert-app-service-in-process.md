---
author: TylerMSFT
title: 将应用服务转换为与其主机应用在同一个进程中运行
description: 将在单独的后台进程中运行的应用服务代码转换为在与应用服务提供程序相同的进程中运行的代码。
ms.author: twhitney
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10，uwp，应用服务
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: 272102f08b145c0681b0e036be4d41bc7c9ad9ff
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7101191"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>将应用服务转换为与其主机应用在同一个进程中运行

[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) 使其他应用程序可以在后台唤醒你的应用，并建立与它的直接通信。

引入进程内应用服务后，两个正在运行的前台应用程序可通过应用服务连接建立直接通信。 应用服务现在可以在与前台应用程序相同的进程中运行，这简化了应用之间的通信，也无需将服务代码分离到独立项目。

将进程外模型的应用服务转换为进程内模型需要两项更改。 第一项是清单更改。

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

删除`EntryPoint`属性`<Extension>`元素由于现在[onbackgroundactivated （）](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)时调用应用服务将使用的入口点。

第二项更改是将服务逻辑从其单独的后台任务项目移动至可从 **OnBackgroundActivated()** 调用的方法。

此时，应用程序可直接运行应用服务。 例如，在 App.xaml.cs 中：

[!NOTE] 下面的代码是不同于提供示例 1 （进程外服务）。 下面的代码仅用于图提供，并且不在的一部分示例 2 （进程内服务）。  若要继续示例中的文章的过渡 1 （进程外服务） 到示例 2 （进程内服务） 继续使用而不是下面说明的代码示例 1 提供的代码。

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

在上述代码中，`OnBackgroundActivated` 方法处理应用服务激活。 通过 [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) 进行通信所需的所有事件均已注册，并且任务延迟对象已存储，以便在应用程序之间的通信完成时可标记为“完成”。

在应用收到请求并读取所提供的 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) 时，查看是否存在 `Key` 和 `Value` 字符串。 如果存在，则应用服务会将一对 `Response` 和 `True` 字符串值返回到 **AppServiceConnection** 另一侧的应用上。

请在[创建和使用应用服务](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396)中了解关于连接和与其他应用通信的详细信息。
