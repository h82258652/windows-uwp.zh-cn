---
title: 将零售演示 (RDX) 功能添加到您的应用程序
description: 对于零售演示模式，帮助展示你的应用上零售销售场地准备您的应用程序。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, 零售演示应用
ms.localizationpriority: medium
ms.openlocfilehash: 39b1323f048c1b420a2cf0b239cd9f1a9fb63ff7
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334875"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>将零售演示 (RDX) 功能添加到您的应用程序

在你的 Windows 应用中包括零售演示模式，以便在销售场所试用电脑和设备的客户可以直接进入。

零售商店客户时，他们希望能够试用的 Pc 和设备的演示。 他们通常会花费相当大块的时间都较为深刻地应用的整个[零售演示体验 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)。

可以设置您的应用程序提供不同的体验，同时可以在_正常_或_零售_模式。 例如，如果您的应用程序以安装过程，可能在零售模式下，跳过它，并预先填充应用程序与示例数据和默认设置，以便它们可以直接跳转中。

从我们客户的角度来看，是只能有一个应用。 为帮助区分两种模式的客户，我们建议，零售模式在您的应用程序时，它显示单词"零售"突出的标题栏中或在合适的位置。

应用程序的 Microsoft Store 要求，除了 RDX 感知应用，还必须符合 RDX 安装程序、 清理和更新过程，以确保客户在零售商店具有一致的正面体验。

## <a name="design-principles"></a>设计原则

* **显示最佳**。 使用您的应用程序为什么 rocks 的展示零售演示体验。 这是可能的第一个时间客户将看到您的应用程序，因此显示它们的最佳部分 ！

* **显示快速**。 客户可能没有耐心 - 用户越快体验应用的真正价值越好。

* **使文章简单**。 零售演示体验是您的应用程序值电梯游说。

* **重点体验**。 给用户时间来理解你的内容。 尽管使用户快速到达精华部分很重要，但设计合适的暂停可帮助他们完全享受体验。

## <a name="technical-requirements"></a>技术要求

RDX 感知应用旨在展示你的应用到零售客户的最佳，因为它们必须符合技术要求，并遵守隐私法规的 Microsoft Store 已为所有零售演示体验应用。

这可以作为一份清单，以帮助您准备验证过程，并为清楚起见，在测试过程中的使用。 请注意，不仅仅在验证过程中，还必须在零售演示体验应用的整个生存期中保留这些要求；只要应用在零售演示设备上保持运行。

### <a name="critical-requirements"></a>关键的要求

RDX 识别不符合这些关键的要求的应用程序尽可能快地将从所有零售演示设备删除。

* **不要询问有关个人身份信息 (PII)**。 这包括登录信息、 Microsoft 帐户信息或联系人详细信息。

* **无错误的体验**。 你的应用必须毫无错误地运行。 此外，不应向使用零售演示设备的客户显示任何错误弹出窗口或通知。 错误的应用上反映产生负面，本身、 自己的品牌、 设备的品牌、 设备的制造商的品牌和 Microsoft 的品牌。

* **付费应用程序必须具有试用模式**。 您的应用程序，或者需要以一种免费或包括[试用模式](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)。 客户不希望为零售商店中的体验付费。

### <a name="high-priority-requirements"></a>优先级较高的要求

RDX 识别不符合这些高优先级要求的应用程序需要立即调查会提供修补程序。 如果无法立即找到修复，此应用可能从所有零售演示设备中删除。

* **令人难忘的脱机体验**。 您的应用程序需要演示更完美的脱机体验，如在零售点约 50%的设备处于脱机状态。 这是为了确保与离线应用交互的客户仍然能够获得有意义且正面的体验。

* **更新内容体验**。 您的应用程序应永远不会提示更新时联机。 如果需要更新，它们应以无提示方式执行。

* **没有匿名通信**。 使用零售演示设备的客户是匿名用户，因为它们不应能够将消息或共享内容从该设备。

* **通过使用在清理过程中提供一致的体验**。 当客户走到零售演示设备前时，每个客户都应具有相同的体验。 您的应用程序应使用[清理过程](#cleanup-process)每次使用后返回到相同的默认状态。 我们不希望下一步的客户，若要查看哪些留在原地的最后一个客户。 这包括计分牌、成就和解锁。

* **Age 合适的内容**。 所有应用程序内容必须是分配的青少年或较低的评级类别。 若要了解详细信息，请参阅[IARC 让应用程序评级](https://www.globalratings.com/for-developers.aspx)并[ESRB 分级](https://www.esrb.org/ratings/ratings_guide.aspx)。

### <a name="medium-priority-requirements"></a>中等优先级要求

Windows 零售商店团队可能直接联系开发人员，以设置有关如何解决这些问题的讨论。

* **设备在范围内成功运行的能力**。 应用必须也在所有设备，包括与低端硬件规范的设备上运行。 如果未满足的最低规格的设备上安装应用程序，则应用需要清楚地向用户通知相关信息。 必须公布最低设备要求，以便应用可以始终高性能地运行。

* **满足零售应用商店应用程序大小要求**。 应用必须小于 800MB。 如果您 RDX 感知的应用程序不符合大小要求，请联系 Windows 零售商店团队直接获取进一步讨论。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API:准备你的代码演示模式

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
[ **IsDemoModeEnabled** ](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)中的属性[ **RetailInfo** ](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo)实用程序类，它是一部分的[Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile)在 Windows 10 SDK 中，命名空间用作布尔指示器来指定你的应用运行的代码路径_正常_模式下或_零售_模式。

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

## <a name="cleanup-process"></a>清除进程

清理开始两分钟后购物者停止与设备交互。 零售演示播放时，Windows 开始重置联系人、 照片和其他应用中的任何示例数据。 具体取决于该设备，这可能需要将完全将一切重置回正常的 1-5 分钟之间。 这可确保在零售商店中的每个客户可以走到设备并与设备交互时获得相同的体验。

第 1 步：Cleanup
* 关闭所有 Win32 和应用商店应用
* 删除已知文件夹（如 __Pictures__、__Videos__、__Music__、__Documents__、__SavedPictures__、__CameraRoll__、__Desktop__ 和 __Downloads__ 文件）中的所有文件夹。
* 删除非结构化和结构化漫游状态
* 删除结构化本地状态

步骤 2：安装
* 对于脱机设备：文件夹保留为空
* 对于联机设备：零售演示资产可以从 Microsoft Store 推送到设备

### <a name="store-data-across-user-sessions"></a>在用户会话之间存储数据

若要在用户会话之间存储数据，您可以将信息存储在__ApplicationData.Current.TemporaryFolder__作为默认清除进程不会自动删除此文件夹中的数据。 请注意使用存储该信息*LocalState*在清理过程中删除。

### <a name="customize-the-cleanup-process"></a>自定义清除过程

若要自定义清除过程，实现`Microsoft-RetailDemo-Cleanup`到你的应用的应用服务。

需要自定义清理逻辑的位置的方案包括运行大量安装程序，下载和缓存数据，或不想*LocalState*要删除数据。

第 1 步：声明_Microsoft RetailDemo 清理_服务应用程序清单中。
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

步骤 2：实现自定义清理逻辑下的_AppdataCleanup_ case 函数使用以下示例模板。
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

* [存储和检索应用程序数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [如何创建和使用应用服务](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [本地化应用内容](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [零售演示体验 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
