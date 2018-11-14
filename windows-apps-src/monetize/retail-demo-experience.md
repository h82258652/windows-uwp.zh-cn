---
author: joannaleecy
title: 零售演示 (RDX) 功能添加到你的应用
description: 零售演示模式，帮助展示你的应用在零售销售场地准备你的应用。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, 零售演示应用
ms.localizationpriority: medium
ms.openlocfilehash: ee0344d521b4c31449a2291517b20a09280818fc
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6455660"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>零售演示 (RDX) 功能添加到你的应用

在你的 Windows 应用中包括零售演示模式，以便试用电脑和设备销售地板上的客户可以直接跳转中。

当客户在零售商店中，他们希望能够试用的电脑和设备的演示。 他们通常会花费相当大块的将与通过[零售演示体验 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)应用的一起玩游戏的时间。

你可以设置你的应用提供不同的体验，在_正常_或_零售_模式。 例如，如果你的应用启动与安装过程，你可以在零售模式下，跳过它，预填充应用的示例数据和默认设置，以便他们可以直接跳转中。

从客户的角度是仅在一个应用。 若要帮助客户区分两种模式，我们建议在零售模式下你的应用时，它显示单词"零售"突出在标题栏或适合位置。

除了适用于应用的 Microsoft 应用商店要求，RDX 感知应用还必须符合 RDX 设置、 清理和更新过程，以确保客户在零售商店具有一致良好的体验。

## <a name="design-principles"></a>设计原则

* **展示你的精华**。 使用零售演示体验展示你的应用的精彩。 这很可能在首次你的客户将看到你的应用，因此显示它们的最佳部分 ！

* **显示快速**。 客户可能没有耐心 - 用户越快体验应用的真正价值越好。

* **使故事保持简单**。 零售演示体验是价值的电梯为你的应用的值。

* **专注于体验**。 给用户时间来理解你的内容。 尽管使用户快速到达精华部分很重要，但设计合适的暂停可帮助他们完全享受体验。

## <a name="technical-requirements"></a>技术要求

由于 RDX 感知应用旨在展示你的应用向零售客户的精华，它们必须满足技术要求并遵守的 Microsoft 应用商店有针对所有零售演示体验应用的隐私法规。

这可以用作一个清单，帮助你为验证过程做准备，并在测试过程中提供清晰度。 请注意，不仅仅在验证过程中，还必须在零售演示体验应用的整个生存期中保留这些要求；只要应用在零售演示设备上保持运行。

### <a name="critical-requirements"></a>关键要求

不满足这些关键要求的 RDX 感知应用尽快将从所有零售演示设备删除。

* **不要求个人身份信息 (PII)**。 这包括登录信息、 Microsoft 帐户信息或联系人详细信息。

* **无错误体验**。 你的应用必须毫无错误地运行。 此外，不应向使用零售演示设备的客户显示任何错误弹出窗口或通知。 错误品牌产生负面影响应用本身、 你的品牌、 设备的品牌，设备的制造商的品牌以及 Microsoft 的品牌。

* **付费应用必须具有试用模式**。 你的应用需要将免费或包括[试用模式](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)。 客户不希望为零售商店中的体验付费。

### <a name="high-priority-requirements"></a>高优先级要求

不满足这些高优先级要求的 RDX 感知应用需要立即调查获取修复。 如果无法立即找到修复，此应用可能从所有零售演示设备中删除。

* **Memorable 脱机体验**。 你的应用需要展现绝佳的离线体验，因为大约 50%的设备在零售地点处于离线。 这是为了确保与离线应用交互的客户仍然能够获得有意义且正面的体验。

* **更新内容体验**。 你的应用应永远不会提示联机时的更新。 如果需要更新，应以静默方式执行它们。

* **无匿名通信**。 由于使用零售演示设备的客户是匿名用户，他们不应能够从设备消息或共享内容。

* **提供一致的体验，使用清理过程**。 当客户走到零售演示设备前时，每个客户都应具有相同的体验。 你的应用应使用[清理过程](#clean-up-process)以在每次使用后返回到相同的默认状态。 我们不希望查看在最后一个客户留下的下一个客户。 这包括计分牌、成就和解锁。

* **适合年龄的内容**。 所有应用内容都需要分配为青少年或更低的分级类别。 若要了解详细信息，请参阅[获取你的应用由 IARC 分级](https://www.globalratings.com/for-developers.aspx)和[ESRB 分级](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中等优先级要求

Windows 零售商店团队可能直接联系开发人员，以设置有关如何解决这些问题的讨论。

* **能够在广泛的设备上成功运行**。 应用必须在所有设备，包括带有低端规范的设备上正常运行。 如果不满足最低要求的设备上安装该应用，则应用将需要清楚地告知用户这。 必须公布最低设备要求，以便应用可以始终高性能地运行。

* **满足零售商店应用大小要求**。 应用必须小于 800MB。 如果你 RDX 感知应用不满足大小要求，请联系 Windows 零售商店团队直接进行进一步讨论。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API： 准备你的代码演示模式

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
在[**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo)实用程序类中，这是 Windows 10 SDK 中的[Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile)命名空间的一部分， [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)属性用作的布尔值指示器用来指定你的应用运行-在哪个代码路径_法线_模式或_零售_模式。

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>RetailInfo.Properties

当 [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 返回 true 时，可使用 [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) 查询一组关于设备的属性，以生成更加自定义的零售演示体验。 这些属性包括 [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername)、[**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize)、[**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 等。

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>清理过程

清理开始购物者停止与设备交互后两分钟。 零售演示播放，并且 Windows 开始重置联系人、 照片和其他应用中的任何示例数据。 根据设备，这可能需要之间 1-5 分钟才能完全所有内容都将重置为正常。 这将确保零售商店中的每个客户可以走到设备，与设备交互时获得相同的体验。

步骤 1： 清理
* 关闭所有 Win32 和应用商店应用
* 删除已知文件夹（如 __Pictures__、__Videos__、__Music__、__Documents__、__SavedPictures__、__CameraRoll__、__Desktop__ 和 __Downloads__ 文件）中的所有文件夹。
* 删除非结构化和结构化漫游状态
* 删除结构化本地状态

步骤 2： 设置
* 对于离线设备：文件夹保持空白
* 对于联机设备：零售演示资源可从 Microsoft Store 推送到设备

### <a name="store-data-across-user-sessions"></a>跨用户会话存储数据

若要跨用户会话存储数据，可以存储信息__ApplicationData.Current.TemporaryFolder__中，因为默认清理过程不会自动删除此文件夹中的数据。 请注意，清理过程期间删除使用*LocalState*存储的信息。

### <a name="customize-the-cleanup-process"></a>自定义清理过程

若要自定义清理过程，实现`Microsoft-RetailDemo-Cleanup`应用服务转换你的应用。

需要自定义清理逻辑的方案包括运行广泛的设置、 下载和缓存数据，或者不希望*LocalState*要删除数据。

步骤 1： 声明你的应用清单中的_Microsoft RetailDemo 清理_服务。
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

步骤 2： 实现你在使用下面的示例模板_AppdataCleanup_案例函数下的自定义清理逻辑。
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
* [零售演示体验 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
