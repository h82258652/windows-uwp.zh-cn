---
author: joannaleecy
title: "创建零售演示体验应用"
description: "创建零售演示体验 (RDX) 应用，该应用是可在零售演示模式和正常模式下启动的单个应用"
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 零售演示应用"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 843f98782410559d47bdb8dc0b23b50fa96552d6
ms.lasthandoff: 02/07/2017

---
#  <a name="create-a-retail-demo-experience-rdx-app"></a>创建零售演示体验 (RDX) 应用

当客户走进某个零售商店或地点时，他们预期看到最新的电脑和移动电话正在展出，这些展出的设备称为零售演示设备。
零售演示设备和安装在它们上的内容对商店中的客户体验负有很大责任，因为客户经常花费大量时间玩这些设备。

在零售商店中安装在这些电脑和移动电话上的应用必须是零售演示体验 (RDX) 应用。 本文概述了如何设计和开发要在零售商店中的电脑和移动演示设备上安装的应用的零售演示版本。

零售演示体验应用采用可在两种不同模式（_正常_或_零售_）之一下启动的单个版本形式。
从客户的角度来说，只有一个应用，为了帮助客户区分两个版本，建议在零售模式下运行的应用在标题栏或适合位置突出显示“零售”字样。

除了对应用的应用商店要求，RDX 应用还必须与零售演示设备完全兼容，请设置、清理和更新系统以确保客户在零售商店具有一致良好的体验。

## <a name="design-principles"></a>设计原则

### <a name="show-your-best"></a>展示你的精华

使用零售演示体验展示你的应用程序的精彩之处。  这可能是客户首次看到你的应用程序，因此请展示它们最精华的部分！

### <a name="show-it-fast"></a>快速查找

客户可能没有耐心 - 用户越快体验应用的真正价值越好。

### <a name="keep-the-story-simple"></a>使故事保持简单

记住，零售演示体验是对应用价值的电梯游说。

### <a name="focus-on-the-experience"></a>专注于体验

给用户时间来理解你的内容。  尽管使用户快速到达精华部分很重要，但设计合适的暂停可帮助他们完全享受体验。

## <a name="technical-requirements"></a>技术要求

由于零售演示体验应用旨在向零售客户展示应用的精华，因此使它们满足这些技术要求并遵守应用商店针对所有零售演示体验应用的隐私法规很重要。
这还可用作一个清单，帮助你为验证过程做准备，并在测试过程中提供清晰度。 请注意，不仅仅在验证过程中，还必须在零售演示体验应用的整个生存期中保留这些要求；只要应用在零售演示设备上保持运行。

### <a name="critical-level-requirements"></a>关键级别要求

不满足这些关键要求的 RDX 应用将尽快从所有零售演示设备中删除。

* 不要求个人身份信息 (PII)

    不允许应用要求客户输入任何个人身份信息。  这包括所有 Microsoft 帐户信息、联系人详细信息等。

* 无错误体验

    你的应用必须毫无错误地运行。 此外，不应向使用零售演示设备的客户显示任何错误弹出窗口或通知。 这很重要，因为我们希望向客户展示我们的精华，精华应当无错误。
    另一个原因是，错误会对应用本身、你的品牌和运行应用的设备、设备制造商的品牌以及 Microsoft 的品牌产生负面影响。

* 付费应用必须具有试用模式

    为了使应用安装在零售演示设备上，应用需要是免费应用或具有已建立的试用模式。  客户不希望为零售商店中的体验付费。 有关详细信息，请参阅[排除或限制试用版中的功能](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)

### <a name="high-priority-requirements"></a>高优先级要求

不满足这些高优先级要求的 RDX 应用将立即受到调查以获取修复。 如果无法立即找到修复，此应用可能从所有零售演示设备中删除。

* 令人难忘的离线体验

    你的零售演示体验应用需要展现绝佳的离线体验，因为大约 50% 的设备在零售地点处于离线状态。 这是为了确保与离线应用交互的客户仍然能够获得有意义且正面的体验。

* 更新的内容体验

    若要提供绝佳的体验，应用需要始终处于最新状态，并且当应用处于联机状态时永远不应提示客户进行应用程序更新。

* 无匿名通信

    由于使用零售演示设备的客户是匿名用户，他们不应能够从设备发消息或共享内容。

* 利用清理过程提供一致的体验

    当客户走到零售演示设备前时，每个客户都应具有相同的体验。 你应利用[清理进程](#clean-up-process) 在每次使用后返回到相同的默认状态，因为我们不希望下一位客户看到上一位客户留下的内容。  这包括计分牌、成就和解锁。

* 适合年龄的内容

    所有零售演示体验应用内容需要分配为“青少年”或更低的分级类别。 有关详细信息，请参阅[使应用由 IARC 分级](https://www.globalratings.com/for-developers.aspx)和 [ESRB 分级](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中等优先级要求

Windows 零售商店团队可能直接联系开发人员，以设置有关如何解决这些问题的讨论。

* 能够在广泛的设备上成功运行

    零售演示体验应用必须在所有设备上运行良好，包括带有低端规范的设备。 如果零售演示体验应用安装在不满足运行应用的最低规范的设备上，则应用需要向用户明确通知这一点。 必须公布最低设备要求，以便应用可以始终高性能地运行。

* 满足零售商店应用大小要求

    应用必须小于 800MB。 如果你的零售体验应用不满足大小要求，请直接联系 Windows 零售商店团队进行进一步讨论。

## <a name="preparing-codebase-for-retail-demo-mode-development"></a>准备代码库用于零售演示模式开发

[
    **RetailInfo**
  ](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.isdemomodeenabled.aspx) 实用程序类中的 [**IsDemoModeEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.aspx) 属性（作为 Windows 10 SDK 中的 [Windows.System.Profile](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.aspx) 命名空间的一部分）用作指定应用程序在哪个代码路径上运行的布尔值指示器 - _正常_模式或_零售_模式。

当 [**RetailInfo.IsDemoModeEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.isdemomodeenabled.aspx) 返回 true 时，可使用 [**RetailInfo.Properties**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.properties.aspx) 查询一组关于设备的属性，以生成更加自定义的零售演示体验。 这些属性包括 [**ManufacturerName**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.manufacturername.aspx)、[**Screensize**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.screensize.aspx)、[**Memory**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.memory.aspx) 等。


## <a name="clean-up-process"></a>清理过程

当在固定持续时间内与设备没有交互时，使用清理过程将零售演示设备重置回原始默认设置。 这是为了确保零售商店中的每个用户都可以走到设备前，在与设备交互时获得准确的默认所需体验。 开发零售演示体验应用时，了解何时及如何触发清理过程、默认清理过程期间会发生什么并了解如何根据所需零售演示体验的要求自定义此清理过程很重要。

### <a name="when-does-clean-up-begin"></a>清理何时开始？

清理序列在特定的设备空闲时间后开始。 当设备上没有来自触摸、鼠标和键盘的输入时，空闲时间开始计数。

#### <a name="desktoppc"></a>台式机/电脑

120 秒空闲时间后，空闲吸引应用视频将开始在设备上播放。 5 秒后，清理过程开始。

#### <a name="phone"></a>手机

60 秒空闲时间后，空闲吸引应用视频将开始在设备上播放，并且清理过程立即开始。

### <a name="what-happens-during-a-default-clean-up-process"></a>默认的清理过程期间会发生什么？

#### <a name="step-1-clean-up"></a>步骤 1：清理
* 关闭所有 Win32 和应用商店应用
* 删除已知文件夹（如 __Pictures__、__Videos__、__Music__、__Documents__、__SavedPictures__、__CameraRoll__、__Desktop__ 和 __Downloads__ 文件）中的所有文件夹。
* 删除非结构化和结构化漫游状态
* 删除结构化本地状态

#### <a name="step-2-set-up"></a>步骤 2：设置
* 对于离线设备：文件夹保持空白
* 对于联机设备：零售演示资源可从 Windows 应用商店推送到设备

### <a name="how-to-store-data-across-user-sessions"></a>如何跨用户会话存储数据？

如果要跨用户会话存储数据，可将信息存储在 __ApplicationData.Current.TemporaryFolder__ 中，因为默认清理过程不会自动删除此文件夹中的数据。 请注意，将在清理过程期间删除使用 *LocalState* 存储的信息。

### <a name="how-to-customize-the-clean-up-process"></a>如何自定义清理过程？

如果你希望自定义清理过程，你需要在应用中实现 `Microsoft-RetailDemo-Cleanup` 应用服务。

需要自定义清理逻辑的方案包括运行耗费资源的设置、下载和缓存数据或者不希望删除 *LocalState* 数据。

步骤 1：在应用程序清单中声明 _Microsoft-RetailDemo-Cleanup_ 服务。
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

步骤 2：使用下面的示例模板在 _AppdataCleanup_ 案例函数下实现自定义清理逻辑。
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>相关链接

* [存储和检索应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [如何创建和使用应用服务](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [本地化应用内容](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)


 

 

