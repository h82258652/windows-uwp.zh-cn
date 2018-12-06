---
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: 创建 NFC 智能卡应用
description: Windows Phone 8.1 支持的 NFC 卡仿真应用使用基于 SIM 卡的安全元素，但该模型需要安全付款应用与移动网络运营商 (MNO) 进行密切合作。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ed6d9e21f3fed4a5f1d02a3b45fa08917a96117f
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8756761"
---
# <a name="create-an-nfc-smart-card-app"></a>创建 NFC 智能卡应用


**重要提示**本主题仅适用于 windows 10 移动版。

Windows Phone 8.1 支持的 NFC 卡仿真应用使用基于 SIM 卡的安全元素，但该模型需要安全付款应用与移动网络运营商 (MNO) 进行密切合作。 这限制了未与 MNO 密切合作的其他商户或开发人员提供的各种可能的支付解决方案。 在 windows 10 移动版中，我们引入了称为主机卡仿真 (HCE) 的新的卡仿真技术。 HCE 技术使你的应用可以直接与 NFC 读卡器通信。 本主题演示了主机卡仿真 (HCE) 在 windows 10 移动版设备上的工作原理以及如何开发 HCE 应用，以便客户可以无需与 MNO 协作通过他们的手机而非物理卡访问你的服务。

## <a name="what-you-need-to-develop-an-hce-app"></a>开发 HCE 应用需要做哪些准备工作


若要为 windows 10 移动版开发基于 HCE 的卡仿真应用，你将需要准备开发环境。 你可以通过安装 Microsoft Visual Studio2015，其中包括 Windows 开发人员工具和附带 NFC 仿真支持的 windows 10 移动版仿真器进行设置。 有关准备工作的详细信息，请参阅[准备工作](https://msdn.microsoft.com/library/windows/apps/Dn726766)

（可选） 如果你想要使用实际的 windows 10 移动版设备而不是包含的 windows 10 移动版仿真器进行测试，你将需要以下各项。

-   附带 NFC HCE 支持的 windows 10 移动版设备。 目前，Lumia 730、830、640 和 640 XL 具有支持 NFC HCE 应用的硬件。
-   支持协议 ISO/IEC 14443-4 和 ISO/IEC 7816-4 的读卡器终端

Windows 10 移动版将实现提供以下功能的 HCE 服务。

-   应用可注册它们想要模拟的卡的小程序标识符 (AID)。
-   应用程序协议数据单元 (APDU) 命令和响应的冲突解决和路由将基于外部读卡器选择和用户首选项与已注册的其中一个应用进行配对。
-   按照用户操作的结果处理应用的事件和通知。

Windows 10 支持模拟智能卡基于 ISO-DEP (ISO-IEC 14443-4) 并使用 ISO-IEC 7816-4 规范定义的 apdu 进行通信。 Windows 10 支持 ISO/IEC 14443-4 类型 A 技术用于 HCE 应用。 默认情况下，类型 B、类型 F 和非 ISO-DEP（如 MIFARE）技术将路由到 SIM 卡。

仅 windows 10 移动版设备还支持卡仿真功能。 基于 sim 卡和基于 HCE 的卡仿真不可用于其他版本的 windows 10。

在下图中显示了基于 HCE 和 SIM 卡的卡仿真支持的体系结构。

![HCE 和 SIM 卡枚举的体系结构](./images/nfc-architecture.png)

## <a name="app-selection-and-aid-routing"></a>应用选择和 AID 路由

若要开发 HCE 应用，你必须了解因为用户可能会安装多个不同的 HCE 应用，windows 10 移动版设备如何路由到特定应用的 Aid。 每个应用都可以注册多个基于 HCE 和 SIM 卡的卡。 基于 sim 卡的传统 Windows Phone 8.1 应用将继续在 windows 10 移动版上，只要用户在 NFC 设置菜单中选择"SIM 卡"选项作为其默认支付卡。 首次打开设备时，将默认设置该选项。

当用户点击其 windows 10 移动版设备与某一终端时，数据将自动路由到设备上安装的适当应用。 此路由基于小程序 ID (AID)，它们是使用 5 到 16 个字节的标识符。 点击期间，外部终端将传输 SELECT 命令 APDU 以指定后续所有 APDU 命令可能要路由到的 AID。 后续 SELECT 选择命令将再次更改路由。 基于应用注册的 AID 和用户设置，APDU 通信将路由到将发送响应 APDU 的特定应用。 请注意，终端可能想要在同一个点击过程中与多个不同应用通信。 因此，必须确保你的应用在停用后尽快退出其后台任务，从而为其他应用的后台任务提供空间以响应 APDU。 我们将在本主题的后面部分讨论后台任务。

HCE 应用必须使用它们可以处理的特定 AID 自行注册，以便它们可以接收 AID 的 APDU。 应用使用 AID 组来声明 AID。 从概念上来讲，AID 组等同于单个物理卡。 例如，一张信用卡已使用某个 AID 组进行声明，而第二张来自其他银行的信用卡使用不同的第二个 AID 组进行声明，尽管这两张信用卡可能具有相同的 AID。

## <a name="conflict-resolution-for-payment-aid-groups"></a>适用于付款 AID 组的冲突解决

在应用注册了物理卡（AID 组）后，它可以将 AID 组类别声明为“付款”或“其他”。 尽管在任何给定时间可能会注册多个付款 AID 组，但“点击以支付”一次只能启用这些付款 AID 组中的一个，这由用户选择。 存在此行为是因为用户希望有意识地控制选择要使用的单张支付卡、信用卡或借记卡，以便他们在点击其设备以连接到某一终端时，不会使用不想使用的其他卡进行支付。

但是，在未与用户交互的情况下，注册为“其他”的多个 AID 组可能会同时启用。 存在此行为是因为每当他们点击其手机时，诸如会员卡、优惠卡或公交卡等其他类型的卡在没有任何提示的情况下就能很方便地完成支付。

注册为“付款”的所有 AID 组都将显示在 NFC 设置页面的卡列表中，用户可在其中选择其默认支付卡。 在选定了默认支付卡后，已注册此付款 AID 组的应用将成为默认付款应用。 默认付款应用可以启用或禁用任何其 AID 组，而无需与用户进行交互。 当用户拒绝默认付款应用的提示时，当前的默认付款应用（如果有）将继续保留为默认应用。 以下屏幕截图显示了 NFC 设置页面。

![“NFC 设置”页的屏幕截图](./images/nfc-settings.png)

使用上述屏幕截图示例，当用户将其默认支付卡更改为未由“HCE 应用程序 1”注册的其他卡时，系统将创建一个确认提示以要求用户确认。 但是，当用户将其默认支付卡更改为“HCE 应用程序 1”注册的其他卡时，系统不会为用户创建确认提示，因为“HCE 应用程序 1”已是默认付款应用。

## <a name="conflict-resolution-for-non-payment-aid-groups"></a>适用于非付款 AID 组的冲突解决

归类为“其他”的非支付卡不会显示在 NFC 设置页面中。

你的应用可以使用与付款 AID 组相同的方式来创建、注册和启用非付款 AID 组。 主要区别是，对于非付款 AID 组，仿真类别设置为“其他”而不是“付款”。 向系统注册 AID 组后，你需要启用该 AID 组以接收 NFC 通信。 当你尝试启用非付款 AID 组以接收通信时，系统不会提示用户进行确认，除非与系统中其他应用已注册的其中一个 AID 存在冲突。 当存在冲突时，用户将收到有关该卡的信息提示，并且当该用户选择启用新注册的 AID 组时，将禁用其关联的应用。

**与基于 SIM 卡的 NFC 应用程序共存**

在 windows 10 移动版中，系统会 NFC 控制器路由表，可用于在控制器层确认路由决策。 该表包含以下各项的路由信息。

-   单个 AID 路由。
-   基于协议的路由 (ISO-DEP)。
-   基于技术的路由 (NFC-A/B/F)。

当外部读卡器发送“SELECT AID”命令时，NFC 控制器将首先检查路由表中的 AID 路由以进行匹配。 如果不匹配，将为 ISO-DEP (14443-4-A) 通信使用基于协议的路由作为默认路由。 对于其他任何非 ISO-DEP 通信，将使用基于技术的路由。

Windows 10 移动版提供一个菜单选项"SIM 卡"NFC 设置页面来继续使用传统 Windows Phone 8.1 基于 sim 卡的应用，它们不向系统注册其 Aid。 如果用户选择“SIM 卡”作为其默认支付卡，则将 ISO-DEP 路由设置为 UICC，对于下拉菜单中的所有其他选项，ISO-DEP 路由指向主机。

ISO-DEP 路由设置为"SIM 卡"的设备具有支持 se 的 SIM 卡时 windows 10 移动版首次启动设备。 当用户安装支持 HCE 的应用并且该应用支持任何 HCE AID 组注册时，ISO-DEP 路由将指向主机。 新的基于 SIM 卡的应用程序需要在 SIM 卡中注册 AID，以便特定的 AID 路由填充到控制器路由表中。

## <a name="creating-an-hce-based-app"></a>创建基于 HCE 的应用

HCE 应用有两个部分。

-   主要的前台应用用于用户交互。
-   由系统触发的后台任务用于处理给定 AID 的 APDU。

由于加载后台任务以响应 NFC 点击时的性能要求极其严格，我们建议整个后台任务采用 C++/CX 本机代码（包括任何需要依赖的依赖项、引用或库）进行实现，而不是采用 C# 或托管代码进行实现。 尽管 C# 和托管代码通常运行效果较好，但还存在诸如加载 .NET CLR 等开销，这些开销通过采用 C++/CX 进行编写即可避免。
## <a name="create-and-register-your-background-task"></a>创建和注册后台任务

你需要在 HCE 应用中为处理和响应系统路由到它的 APDU 创建后台任务。 首次启动应用期间，前台会注册实现 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/BR224803) 接口的 HCE 后台任务，如以下代码所示。

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

请注意，任务触发器将设置为 [**SmartCardTriggerType**](https://msdn.microsoft.com/library/windows/apps/Dn608017)。 **EmulatorHostApplicationActivated**。 这意味着只要操作系统接收到解析为你的应用的 SELECT AID 命令 APDU，就会启动你的后台任务。

## <a name="receive-and-respond-to-apdus"></a>接收和响应 APDU

当存在面向你的应用的 APDU 时，系统将启动后台任务。 你的后台任务将收到通过 [**SmartCardEmulatorApduReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894640) 对象的 [**CommandApdu**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu.aspx) 属性传递的 APDU，并使用同一对象的 [**TryRespondAsync**](https://msdn.microsoft.com/library/windows/apps/mt634299.aspx) 方法响应 APDU。 为了提供性能，请考虑使你的后台任务保持轻量运行。 例如，立即响应 APDU 并在完成所有处理后退出后台任务。 由于 NFC 交易的性质，用户将其设备贴靠读卡器的时间通常很短。 后台任务将继续从该读卡器接收通信，直到连接被停用，在这种情况下，将收到 [**SmartCardEmulatorConnectionDeactivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894644) 对象。 连接可能会因以下原因而被停用，如 [**SmartCardEmulatorConnectionDeactivatedEventArgs.Reason**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason) 属性中所示。

-   如果连接被停用时附带 **ConnectionLost** 值，这意味着用户已将其设备远离读卡器。 如果你的应用需要用户点击终端更长时间，你可以提示用户提供反馈。 应快速（通过完成延迟）终止你的后台任务，以确保当他们再次点击它时，不会延迟以等待上一后台任务退出。
-   如果连接被停用时附带 **ConnectionRedirected**，这意味着终端已发送一个新的定向到其他 AID 的 SELECT AID 命令 APDU。 在此情况下，你的应用应立即退出后台任务（通过完成延迟）以允许运行其他后台任务。

此外，后台任务应在 [**IBackgroundTaskInstance interface**](https://msdn.microsoft.com/library/windows/apps/BR224798) 上注册 [**Canceled event**](https://msdn.microsoft.com/library/windows/apps/BR224797) 并且还应快速退出后台任务（通过完成延迟），因为当后台任务完成此事件时，系统会触发它。 以下代码演示了 HCE 应用后台任务。

```csharp
void BgTask::Run(
    IBackgroundTaskInstance^ taskInstance)
{
    m_triggerDetails = static_cast<SmartCardTriggerDetails^>(taskInstance->TriggerDetails);
    if (m_triggerDetails == nullptr)
    {
        // May be not a smart card event that triggered us
        return;
    }

    m_emulator = m_triggerDetails->Emulator;
    m_taskInstance = taskInstance;

    switch (m_triggerDetails->TriggerType)
    {
    case SmartCardTriggerType::EmulatorHostApplicationActivated:
        HandleHceActivation();
        break;

    case SmartCardTriggerType::EmulatorAppletIdGroupRegistrationChanged:
        HandleRegistrationChange();
        break;

    default:
        break;
    }
}

void BgTask::HandleHceActivation()
{
 try
 {
        auto lock = m_srwLock.LockShared();
        // Take a deferral to keep this background task alive even after this "Run" method returns
        // You must complete this deferal immediately after you have done processing the current transaction
        m_deferral = m_taskInstance->GetDeferral();

        DebugLog(L"*** HCE Activation Background Task Started ***");

        // Set up a handler for if the background task is cancelled, we must immediately complete our deferral
        m_taskInstance->Canceled += ref new Windows::ApplicationModel::Background::BackgroundTaskCanceledEventHandler(
            [this](
            IBackgroundTaskInstance^ sender,
            BackgroundTaskCancellationReason reason)
        {
            DebugLog(L"Cancelled");
            DebugLog(reason.ToString()->Data());
            EndTask();
        });

        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            auto denyIfLocked = Windows::Storage::ApplicationData::Current->RoamingSettings->Values->Lookup("DenyIfPhoneLocked");
            if (denyIfLocked != nullptr && (bool)denyIfLocked == true)
            {
                // The phone is locked, and our current user setting is to deny transactions while locked so let the user know
                // Denied
                DoLaunch(Denied, L"Phone was locked at the time of tap");

                // We still need to respond to APDUs in a timely manner, even though we will just return failure
                m_fDenyTransactions = true;
            }
        }
        else
        {
            m_fDenyTransactions = false;
        }

        m_emulator->ApduReceived += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorApduReceivedEventArgs^>(
            this, &BgTask::ApduReceived);

        m_emulator->ConnectionDeactivated += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorConnectionDeactivatedEventArgs^>(
                [this](
                SmartCardEmulator^ emulator,
                SmartCardEmulatorConnectionDeactivatedEventArgs^ eventArgs)
            {
                DebugLog(L"Connection deactivated");
                EndTask();
            });

  m_emulator->Start();
        DebugLog(L"Emulator started");
 }
 catch (Exception^ e)
 {
        DebugLog(("Exception in Run: " + e->ToString())->Data());
        EndTask();
 }
}
```
## <a name="create-and-register-aid-groups"></a>创建和注册 AID 组

在首次启动你的应用程序期间，将在预配完卡后，通过系统创建和注册 AID 组。 系统将确定外部读卡器可与其进行通信的应用，并根据注册的 AID 和用户设置相应地路由 APDU。

大部分支付卡都注册相同的 AID（即 PPSE AID）以及其他支付网络卡特定的 AID。 每个 AID 组代表一张卡，并且当用户启用该卡时，会启用组内的所有 AID。 同样，当用户停用该卡时，将禁用组内的所有 AID。

若要注册 AID 组，你需要创建 [**SmartCardAppletIdGroup**](https://msdn.microsoft.com/library/windows/apps/Dn910955) 对象并设置其属性，以反映这是一张基于 HCE 的支付卡。 显示名称对于用户应具有描述性，因为它将显示在 NFC 设置菜单以及用户提示中。 对于 HCE 支付卡，[**SmartCardEmulationCategory**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx) 属性应设置为 **Payment**，而 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 属性应设置为 **Host**。

```csharp
public static byte[] AID_PPSE =
        {
            // File name "2PAY.SYS.DDF01" (14 bytes)
            (byte)'2', (byte)'P', (byte)'A', (byte)'Y',
            (byte)'.', (byte)'S', (byte)'Y', (byte)'S',
            (byte)'.', (byte)'D', (byte)'D', (byte)'F', (byte)'0', (byte)'1'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Host);
```

对于非 HCE 支付卡，[**SmartCardEmulationCategory**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx) 属性应设置为 **Other**，而 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 属性应设置为 **Host**。

```csharp
public static byte[] AID_OTHER =
        {
            (byte)'1', (byte)'2', (byte)'3', (byte)'4',
            (byte)'5', (byte)'6', (byte)'7', (byte)'8',
            (byte)'O', (byte)'T', (byte)'H', (byte)'E', (byte)'R'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_OTHER.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
```

每个 AID 组最多可以包含 9 个 AID（每一个的长度为 5 到 16 个字节）。

使用 [**RegisterAppletIdGroupAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894656) 方法向系统注册 AID 组，这将返回 [**SmartCardAppletIdGroupRegistration**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 对象。 默认情况下，注册对象的 [**ActivationPolicy**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 属性会设置为 **Disabled**。 这意味着即使在系统中注册了 AID，但它们尚未启用，它们也无法接收通信。

```csharp
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

你可以通过使用 [**SmartCardAppletIdGroupRegistration**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 类的 [**RequestActivationPolicyChangeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 方法来启用注册的卡（AID 组），如下所示。 由于一次只能在系统上启用一张支付卡，因此将付款 AID 组的 [**ActivationPolicy**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 设置为 **Enabled** 等同于设置默认支付卡。 系统将提示用户是否允许设置此卡作为默认支付卡，而不考虑是否已经选择了默认支付卡。 如果你的应用已经是默认的付款应用程序并且仅在其自己的 AID 组之间进行更改，则此语句不为 true。 每个应用最多可以注册 10 个 AID 组。

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

你可以通过操作系统查询你应用已注册的 AID 组，并使用 [**GetAppletIdGroupRegistrationsAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894654) 方法来查看其激活策略。

当用户将支付卡的激活策略从 **Disabled** 更改为 **Enabled** 时（仅当该应用已不是默认的付款应用时），用户将收到提示信息。 仅当用户将非支付卡的激活策略从 **Disabled** 更改为 **Enabled** 时（如果存在 AID 冲突），用户才会收到提示信息。

```csharp
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

**激活策略更改时的事件通知**

在后台任务中，可以针对以下情况进行注册以接收事件：在你的应用之外更改了 AID 组注册之一的激活策略时。 例如，用户可能会通过 NFC 设置菜单将默认付款应用从你的其中一张卡更改为其他应用托管的其他卡。 如果你的应用需要了解有关此内部设置的更改（例如，更新动态磁贴），你可以接收此更改的事件通知并在应用中相应地执行操作。

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## <a name="foreground-override-behavior"></a>前台重写行为

当你的应用在前台运行时，可以在不提示用户的情况下将任何 AID 组注册的 [**ActivationPolicy**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 更改为 **ForegroundOverride**。 如果用户点击其设备以连接到终端，同时你的应用在前台运行，那么即使用户未选择任何支付卡作为其默认支付卡，该通信也会路由到你的应用。 当你将卡的激活策略更改为 **ForegroundOverride** 时，此更改仅临时有效，直至应用退出前台运行，并且它不会更改由用户设置的当前默认支付卡。 你可以按照以下方法从你的前台应用更改支付卡或非支付卡的 **ActivationPolicy**。 请注意，[**RequestActivationPolicyChangeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 方法只能从前台应用调用，不能从后台任务调用。

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

此外，你可以注册由单个 0 长度 AID 组成的 AID 组，这将导致系统路由所有 APDU（而不考虑 AID），包括在接收 SELECT AID 命令前已发送的任何命令 APDU。 但是，此类 AID 组仅在你的应用在前台运行时才有效，因为它只能设置为 **ForegroundOverride** 且无法永久启用。 此外，此机制适用于 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639) 枚举的 **Host** 和 **UICC** 值以将所有通信路由到你的 HCE 后台任务，或路由到 SIM 卡。

```csharp
public static byte[] AID_Foreground =
        {};

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_Foreground.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

## <a name="check-for-nfc-and-hce-support"></a>检查 NFC 和 HCE 支持

你的应用应先检查设备是否具有 NFC 硬件、支持卡仿真功能以及支持主机卡仿真，然后再向用户提供此类功能。

NFC 智能卡仿真功能仅在 windows 10 移动版中，因此尝试使用智能卡仿真程序 Api 在任何其他版本的 windows 10 中受支持，将导致错误。 你可以使用以下代码段来检查是否支持智能卡 API。

```csharp
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

此外，你可以检查设备是否具有支持某种形式的卡仿真的 NFC 硬件，方法是检查 [**SmartCardEmulator.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/Dn608008) 方法是否返回 null。 如果返回 null，则 NFC 卡仿真在设备上不受支持。

```csharp
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

对基于 HCE 和 AID 的 UICC 路由的支持仅适用于最近发布的设备，例如 Lumia 730、830、640 和 640 XL。 任何运行 windows 10 移动版的新 NFC 能够设备应支持 HCE 和。 你的应用可以检查是否支持 HCE，如下所示。

```csharp
Smartcardemulator.IsHostCardEmulationSupported();
```

## <a name="lock-screen-and-screen-off-behavior"></a>锁定屏幕和屏幕关闭行为

Windows 10 移动版具有设备级卡仿真设置，可以通过移动运营商或设备的制造商设置。 默认情况下，“点击以支付”切换处于禁用状态，并且“设备级的启用策略”设置为“始终”，除非 MO 或 OEM 重写这些值。

你的应用程序可以查询设备级的 [**EnablementPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn608006) 的值，并针对每种情况执行操作，具体取决于每个状态中的应用的所需行为。

```csharp
SmartCardEmulator emulator = await SmartCardEmulator.GetDefaultAsync();

switch (emulator.EnablementPolicy)
{
case Never:
// you can take the user to the NFC settings to turn "tap and pay" on
await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings-nfctransactions:"));
break;

 case Always:
return "Card emulation always on";

 case ScreenOn:
 return "Card emulation on only when screen is on";

 case ScreenUnlocked:
 return "Card emulation on only when screen unlocked";
}
```

仅当外部读卡器选择可解析为你的应用的 AID 时，才会启动你的应用的后台任务，即使手机锁定和/或屏幕处于关闭状态也是如此。 可以在你的后台任务中响应来自读卡器的命令，但如果你需要来自用户的任何输入，或者想要向用户显示信息，可以通过一些参数启动你的前台应用。 你的后台任务可以启动具有以下行为的前台应用。

-   在设备锁屏界面下（仅在用户解锁设备后她才可以看到你的前台应用）
-   在设备锁屏界面上（用户关闭你的应用后，设备仍处于锁定状态）

```csharp
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        }
```

## <a name="aid-registration-and-other-updates-for-sim-based-apps"></a>适用于基于 SIM 卡应用的 AID 注册和其他更新

使用 SIM 卡作为安全元素的卡仿真应用可以向 Windows 服务注册以声明 AID 在 SIM 卡上受支持。 此注册非常类似于基于 HCE 的应用注册。 唯一的区别是 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639)，它应设置为基于 SIM 卡应用的 UICC。 支付卡注册后，卡的显示名称还将填充在 NFC 设置菜单中。

```csharp
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

* * 重要 * * Windows Phone 8.1 支持传统二进制短信拦截已删除并替换为新 windows 10 移动版中更广泛的短信支持，但依赖的任何传统 Windows Phone 8.1 应用必须更新为使用新的 windows 10 移动版短信Api。
