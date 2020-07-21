---
title: 向应用程序添加零售演示（RDX）功能
description: 为零售演示模式准备你的应用，帮助在零售楼层上展示你的应用。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, 零售演示应用
ms.localizationpriority: medium
ms.openlocfilehash: 5be39760ee2b8837cfb9b0809a354262e790970b
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73051993"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>向应用程序添加零售演示（RDX）功能

在你的 Windows 应用中包括零售演示模式，以便在销售场所试用电脑和设备的客户可以直接进入。

当客户在零售商店中时，他们希望能够试用 Pc 和设备的演示。 他们通常会花费相当多的时间块，通过[零售演示体验（RDX）](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)与应用程序进行玩。

你可以将应用设置为在_正常_或_零售_模式下提供不同的体验。 例如，如果你的应用程序是在安装过程中启动的，则你可能会在 "零售" 模式下跳过它，并使用示例数据和默认设置预填充应用，以便他们可以直接进入。

从客户的角度来看，只有一个应用。 为了帮助客户区分这两种模式，我们建议，在你的应用处于零售模式时，它会在标题栏或适当位置显示 "零售" 一词。

除了适用于应用的 Microsoft Store 要求外，RDX 感知的应用还必须与 RDX 安装、清理和更新过程兼容，以确保客户在零售商店具有积极的体验。

## <a name="design-principles"></a>设计原则

* **显示您的最佳**方案。 使用零售演示体验展示你的应用 rocks 的原因。 这很可能是您首次看到您的应用程序，因此显示最好的棋子！

* **快速显示**。 客户可能没有耐心 - 用户越快体验应用的真正价值越好。

* **使故事保持简单**。 零售演示体验是适用于应用价值的电梯间推介。

* **专注于经验**。 给用户时间来理解你的内容。 尽管使用户快速到达精华部分很重要，但设计合适的暂停可帮助他们完全享受体验。

## <a name="technical-requirements"></a>技术要求

由于 RDX 识别的应用程序旨在为零售客户展示您的应用程序，因此他们必须满足技术要求，并遵守 Microsoft Store 对所有零售演示体验应用程序的隐私条例。

这可用作检查表，以帮助你为验证过程做好准备并在测试过程中提供清晰性。 请注意，不仅仅在验证过程中，还必须在零售演示体验应用的整个生存期中保留这些要求；只要应用在零售演示设备上保持运行。

### <a name="critical-requirements"></a>关键要求

不符合这些关键要求的 RDX 感知应用将尽快从所有零售演示设备中删除。

* **请勿要求提供个人身份信息（PII）** 。 这包括登录信息、Microsoft 帐户信息或联系人详细信息。

* **无错误体验**。 你的应用必须毫无错误地运行。 此外，不应向使用零售演示设备的客户显示任何错误弹出窗口或通知。 错误会对应用本身、品牌、设备品牌、设备的 manufacturer's 品牌和 Microsoft 品牌产生不良反应。

* **付费应用必须具有试用模式**。 您的应用程序需要是免费的或包含[试用模式](https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)。 客户不希望为零售商店中的体验付费。

### <a name="high-priority-requirements"></a>高优先级要求

对于不满足这些高优先级要求的 RDX 感知应用，需要立即调查是否有修补程序。 如果无法立即找到修复，此应用可能从所有零售演示设备中删除。

* 令人**印象的脱机体验**。 应用需要演示丰富的脱机体验，因为大约50% 的设备在零售位置处于脱机状态。 这是为了确保与离线应用交互的客户仍然能够获得有意义且正面的体验。

* **更新的内容体验**。 在联机时，应用程序不应提示进行更新。 如果需要更新，则应以无提示方式执行更新。

* **无匿名通信**。 由于使用零售演示设备的客户是匿名用户，因此他们不应该能够通过该设备发送或共享内容。

* **使用清除过程提供一致的体验**。 当客户走到零售演示设备前时，每个客户都应具有相同的体验。 应用应在每次使用后使用 "[清除进程](#cleanup-process)" 返回到相同的默认状态。 我们不希望下一个客户查看最后一个客户留下的内容。 这包括计分牌、成就和解锁。

* **Age 相应内容**。 需要为所有应用程序内容分配 "一个或更低的评分" 类别。 若要了解详细信息，请参阅使用 IARC 和[ESRB 评级](https://www.esrb.org/ratings/ratings_guide.aspx)[获取应用](https://www.globalratings.com/for-developers.aspx)。

### <a name="medium-priority-requirements"></a>中等优先级要求

Windows 零售商店团队可能直接联系开发人员，以设置有关如何解决这些问题的讨论。

* **能够在一系列设备上成功运行**。 应用必须在所有设备上正常运行，包括具有低端规范的设备。 如果在未满足最低规范的设备上安装应用，则应用需要明确通知用户有关此操作的信息。 必须公布最低设备要求，以便应用可以始终高性能地运行。

* **满足零售应用商店应用大小要求**。 应用必须小于 800MB。 如果你的 RDX 感知应用不满足大小要求，请直接联系 Windows 零售商店团队，进一步讨论。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API：在演示模式下准备你的代码

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
[**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo)实用工具类中的[**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)属性，该类是 windows 10 SDK 中的[windows 配置文件](https://docs.microsoft.com/uwp/api/windows.system.profile)命名空间的一部分，用作布尔指示符，用于指定应用运行的代码路径（_正常_模式或_零售_模式。

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo

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

#### <a name="idl"></a>.IDL

```cpp
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

当购物者停止与设备进行交互后，就会开始两分钟。 零售演示重头戏，Windows 开始重置联系人、照片和其他应用中的任何示例数据。 根据设备，这可能需要1-5 分钟才能完全将一切恢复到正常。 这可确保零售商店中的每个客户都可以遍历设备，并在与设备交互时具有相同的体验。

步骤1：清理
* 关闭所有 Win32 和应用商店应用
* 删除已知文件夹（如 __Pictures__、__Videos__、__Music__、__Documents__、__SavedPictures__、__CameraRoll__、__Desktop__ 和 __Downloads__ 文件）中的所有文件夹。
* 删除非结构化和结构化漫游状态
* 删除结构化本地状态

步骤2：设置
* 对于离线设备：文件夹保持空白
* 对于联机设备：零售演示资源可从 Microsoft Store 推送到设备

### <a name="store-data-across-user-sessions"></a>跨用户会话存储数据

若要跨用户会话存储数据，可以将信息存储在__ApplicationData__中，因为默认的清理过程不会自动删除此文件夹中的数据。 请注意，在清理过程中，将删除使用*LocalState*存储的信息。

### <a name="customize-the-cleanup-process"></a>自定义清除过程

若要自定义清理过程，请在应用中实现 `Microsoft-RetailDemo-Cleanup` 应用服务。

需要自定义清理逻辑的情况包括运行广泛的设置、下载和缓存数据，或者不希望删除*LocalState*数据。

步骤1：在应用程序清单中声明_RetailDemo-清理_服务。
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

步骤2：使用下面的示例模板在_AppdataCleanup_ case 函数下实现自定义清理逻辑。
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

* [存储和检索应用数据](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [如何创建和使用应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [本地化应用程序内容](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [零售演示体验（RDX）](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
