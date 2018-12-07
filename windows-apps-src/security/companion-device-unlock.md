---
title: 具有 Windows Hello 配套 (IoT) 设备的 Windows 解锁
description: Windows Hello 配套设备是可以与你的 Windows 10 桌面版一起使用来增强用户身份验证体验的设备。 通过使用 Windows Hello 配套设备框架，即使是在生物识别不可用时（例如，在 Windows 10 桌面版缺少相机进行面部身份验证或缺少指纹读取器设备时），配套设备也能提供丰富的 Windows Hello 体验。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全
ms.assetid: 89f3d331-20cd-457b-83e8-1a22aaab2658
ms.localizationpriority: medium
ms.openlocfilehash: b33cf07ef10d0891f2747a06caf098b7d37b62f3
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8790720"
---
# <a name="windows-unlock-with-windows-hello-companion-iot-devices"></a>具有 Windows Hello 配套 (IoT) 设备的 Windows 解锁

Windows Hello 配套设备是可以与你的 Windows 10 桌面版一起使用来增强用户身份验证体验的设备。 通过使用 Windows Hello 配套设备框架，即使是在生物识别不可用时（例如，在 Windows 10 桌面版缺少相机进行面部身份验证或缺少指纹读取器设备时），配套设备也能提供丰富的 Windows Hello 体验。

> **注意** Windows Hello 配套设备框架是不向所有应用开发人员提供的特定功能。 若要使用此框架，应用必须由 Microsoft 专门设置，并且在它的清单中列出受限制的 *secondaryAuthenticationFactor* 功能。 若要获得批准，请联系 [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com)。

## <a name="introduction"></a>简介

> 有关视频概述，请参阅第 9 频道上来自内部版本 2016 的[具有 IoT 设备的 Windows 解锁](https://channel9.msdn.com/Events/Build/2016/P491)会话。

> 有关代码示例，请参阅 [Windows Hello 配套设备框架 Github 存储库](https://github.com/Microsoft/companion-device-framework)。

### <a name="use-cases"></a>用例

有很多方法可以将 Windows Hello 配套设备框架与配套设备结合使用，从而生成出色的 Windows 解锁体验。 例如，用户可以：

- 通过 USB 将配套设备连接到电脑、触摸配套设备上的按钮，并自动解锁电脑。
- 在口袋中携带已通过蓝牙与电脑配对的手机。 当在电脑上点击空格键时，手机会收到通知。 同意该通知，电脑即会解锁。
- 点击 NFC 读卡器的配套设备，快速解锁电脑。
- 戴上已验证穿戴者身份的健身环。 接近电脑，通过执行特殊手势（例如鼓掌），解锁电脑。

### <a name="biometric-enabled-windows-hello-companion-devices"></a>启用生物识别的 Windows Hello 配套设备

如果配套设备支持生物识别，在某些情况下，[Windows 生物识别框架](https://msdn.microsoft.com/library/windows/hardware/mt608302(v=vs.85).aspx)可能是比 Windows Hello 配套设备框架更好的解决方案。 请联系 [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com)，我们将帮助你选取正确的方法。

### <a name="components-of-the-solution"></a>解决方案的构成

下图描述了解决方案的构成以及构建它们的责任方。

![框架概述](images/companion-device-1.png)

Windows Hello 配套设备框架实现为在 Windows 上运行的服务（在本文中称为配套身份验证服务）。 此服务负责生成需要由存储在 Windows Hello 配套设备上的 HMAC 密钥保护的解锁令牌。 这可保证访问解锁令牌需要有 Windows Hello 配套设备。 每个（电脑，Windows 用户）元组都会有唯一的解锁令牌。

与 Windows Hello 配套设备框架集成需要：

- 从 Windows 应用商店下载的配套设备的[通用 Windows 平台 (UWP)](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) Windows Hello 配套设备应用。 
- 能够在 Windows Hello 配套设备上创建两个 256 位的 HMAC 密钥，并通过它生成 HMAC（使用 SHA-256）。
- 正确配置 Windows 10 桌面版的安全设置。 配套身份验证服务将要求在任何 Windows Hello 配套设备插入此 PIN 前先设置它。 用户必须通过“设置”&gt;“帐户”&gt;“登录”选项来设置 PIN。

除上述要求外，Windows Hello 配套设备应用还负责：

- 用户体验和初始注册的品牌，以及在以后取消 Windows Hello 配套设备注册。
- 在后台运行、发现 Windows Hello 配套设备，与 Windows Hello 配套设备和配套身份验证服务通信。
- 错误处理

正常情况下，配套设备随附在用于初始设置的应用上，例如首次设置健身环。 本文档介绍的功能可能是该应用的一部分，而且也不需要单独的应用。  

### <a name="user-signals"></a>用户信号

每个 Windows Hello 配套设备均应与支持三种用户信号的应用结合。 这些信号可以采用操作或手势的形式。

- **意图信号**：通过诸如点击 Windows Hello 配套设备上的按钮等方式，允许用户显示解锁意图。 意图信号必须在 **Windows Hello 配套设备**端收集。
- **用户存在信号**：证明存在用户。 例如，Windows Hello 配套设备可能在可用于解锁电脑前需要 PIN（不要与电脑 PIN 混淆），或者它可能需要按某个按钮。
- **消除歧义信号**：当向 Windows Hello 配套设备提供多个选项时，消除用户想要解锁哪台 Windows 10 桌面版的疑虑。

上述任意数量的用户信号都可以组合成一个信号。 每次使用时都必须要有用户存在信号和意图信号。

### <a name="registration-and-future-communication-between-a-pc-and-windows-hello-companion-devices"></a>注册以及电脑和 Windows Hello 配套设备之间的未来通信

在 Windows Hello 配套设备可插入 Windows Hello 配套设备框架前，它需要先向该框架注册。 注册体验完全归 Windows Hello 配套设备应用所有。

Windows Hello 配套设备和 Windows 10 桌面版设备的关系可以是一对多的关系（即，一台配套设备可用于多台 Windows 10 桌面版设备）。 但是，每台 Windows Hello 配套设备在每台 Windows 10 桌面版设备上仅可用于一个用户。   

在 Windows Hello 配套设备能与电脑通信前，它们需要同意使用某种传输。 此类选择留待 Windows Hello 配套设备应用做出；Windows Hello 配套设备框架不会对传输类型（USB、NFC、WLAN、BT、BLE 等）或在 Windows Hello 配套设备和 Windows 10 桌面版设备端的 Windows Hello 配套设备应用之间使用的协议施加任何限制。 但是，就像在本文档的“安全要求”部分中所示那样，它会提示传输层的某些安全注意事项。 提供这些要求是设备提供商的责任。 框架不会为你提供它们。


## <a name="user-interaction-model"></a>用户交互模型

### <a name="windows-hello-companion-device-app-discovery-installation-and-first-time-registration"></a>Windows Hello 配套设备应用发现、安装和首次注册

典型的用户工作流如下所示：

- 用户在想要通过该 Windows Hello 配套设备解锁的每台目标 Windows 10 桌面版设备上设置 PIN。
- 用户在 Windows 10 桌面版设备上运行 Windows Hello 配套设备应用，以将 Windows Hello 配套设备注册到 Windows 10 桌面版。

注释：

- 我们建议简化和自动化（如果可能）Windows Hello 配套设备应用的发现、下载和启动（例如当点击 Windows 10 桌面版设备端的 NFC 读卡器上的 Windows Hello 配套设备时，即可下载应用）。 但是，这是 Windows Hello 配套设备和 Windows Hello 配套设备应用的责任。
- 在企业环境中，Windows Hello 配套设备应用可通过 MDM 部署。
- Windows Hello 配套设备应用负责向用户显示任何在注册时发生的错误消息。

### <a name="registration-and-de-registration-protocol"></a>注册和注销协议

下图说明了在注册期间 Windows Hello 配套设备如何与配套身份验证服务交互。  

![注册流程](images/companion-device-2.png)

在协议中要用到两个密钥：

- 设备密钥 (**devicekey**)：用于保护电脑解锁 Windows 所需的解锁令牌。
- 身份验证密钥 (**authkey**)：用于共同验证 Windows Hello 配套设备和配套身份验证服务。

设备密钥和身份验证密钥在注册时会在 Windows Hello 配套设备应用和 Windows Hello 配套设备之间交换。 因此，Windows Hello 配套设备应用和 Windows Hello 配套设备必须使用安全传输来保护密钥。

此外请注意，虽然上图显示了两个在 Windows Hello 配套设备上生成的 HMAC 密钥，但应用也可以生成它们并将它们发送到 Windows Hello 配套设备以进行存储。

### <a name="starting-authentication-flows"></a>启动身份验证流程

通过使用 Windows Hello 配套设备框架（即提供意图信号），用户有两种方法可以启动 Windows 10 桌面版的登录流程：

- 打开笔记本电脑的盖子，或者点击电脑空格键或轻扫。
- 在 Windows Hello 配套设备端执行手势或操作。

由 Windows Hello 配套设备选择哪一个是起始点。 当选项一发生时，Windows Hello 配套设备框架会通知配套设备应用。 对于选项二，Windows Hello 配套设备应用应查询配套设备以查看是否已捕获该事件。 这确保了在解锁成功前 Windows Hello 配套设备能收集意图信号。

### <a name="windows-hello-companion-device-credential-provider"></a>Windows Hello 配套设备凭据提供程序

Windows 10 拥有处理所有 Windows Hello 配套设备的全新凭据提供程序。

Windows Hello 配套设备凭据提供程序负责通过激活某个触发器来启动配套设备后台任务。 该触发器在唤醒电脑并且显示锁屏界面时进行首次设置。 第二次是在电脑进入登录 UI 并且 Windows Hello 配套设备凭据提供程序已是选定磁贴时。

Windows Hello 配套设备应用的帮助程序库会侦听锁屏界面状态更改，并发送对应 Windows Hello 配套设备后台任务的事件。

如果有多个 Windows Hello 配套设备后台任务，则第一个完成身份验证过程的后台任务将解锁电脑。 配套设备身份验证服务将忽略任何其余的身份验证调用。

Windows Hello 配套设备端的体验归 Windows Hello 配套设备应用所有并由其管理。 Windows Hello 配套设备框架无法控制此部分的用户体验。 更具体地说，配套身份验证提供程序通知 Windows Hello 配套设备应用（通过后台应用）有关登录 UI 的状态更改情况（例如锁屏界面刚应用，或者用户刚点击了空格键而消除了锁屏界面），而围绕状态更改来生成体验（例如当用户点击空格键和消除锁屏界面时，开始通过 USB 寻找设备）则是 Windows Hello 配套设备应用的责任。

Windows Hello 配套设备框架会提供大量（本地化的）文本和错误消息以供 Windows Hello 配套设备应用进行选择。 这些都将在锁屏界面顶部（或登录 UI 中）显示。 有关更多详细信息，请参阅“处理消息和错误”部分。

### <a name="authentication-protocol"></a>身份验证协议

在与 Windows Hello 配套设备应用关联的后台任务的触发器启动后，它有责任请求 Windows Hello 配套设备验证配套身份验证服务计算的某个 HMAC 值，来帮助计算两个 HMAC 值：
- 验证服务 HMAC = HMAC(authentication key, service nonce || device nonce || session nonce)。
- 计算具有 nonce 的设备密钥的 HMAC。
- 计算第一个 HMAC 值与配套身份验证服务生成的 nonce 串联的身份验证密钥的 HMAC。

第二个计算值由服务用于验证设备身份，并且还阻止传输通道中的重播攻击。

![注册流程](images/companion-device-3.png)

## <a name="lifecycle-management"></a>生命周期管理

### <a name="register-once-use-everywhere"></a>一次注册，随处使用

如果没有后端服务器，用户必须将他们的 Windows Hello 配套设备分别注册到每台 Windows 10 桌面版设备上。

配套设备供应商或 OEM 可以实现 Web 服务以在用户的 Windows 10 桌面版或移动设备上漫游注册状态。 有关更多详细信息，请参阅“漫游、吊销和筛选器服务”部分。

### <a name="pin-management"></a>PIN 管理

在配套设备可以使用前，需要在 Windows 10 桌面设备上设置 PIN。 这可确保用户拥有一份备份，以防 Windows Hello 配套设备无法运行。 PIN 受到 Windows 管理，但应用永远无法看到。 若要更改它，用户应导航到“设置”&gt;“帐户”&gt;“登录”选项。

### <a name="management-and-policy"></a>管理和策略

通过在 Windows 10 桌面版上运行 Windows Hello 配套设备应用，用户可从该桌面版删除 Windows Hello 配套设备。

企业有两个选择可以控制 Windows Hello 配套设备框架：

- 打开或关闭功能
- 使用 Windows 应用保险箱定义允许的 Windows Hello 配套设备白名单

Windows Hello 配套设备框架不支持任何保留可用配套设备清单的集中式方法，或者是进一步筛选允许哪一个 Windows Hello 配套设备类型的实例的方法（例如仅允许序列号在 X 和 Y 之间的配套设备）。 但是，应用开发人员可以生成提供此类功能的服务。 有关更多详细信息，请参阅“漫游、吊销和筛选器服务”部分。

### <a name="revocation"></a>吊销

Windows Hello 配套设备框架不支持远程删除特定 Windows 10 桌面版设备的配套设备。 用户可以转而通过在该 Windows 10 桌面版上运行的 Windows Hello 配套设备应用来删除 Windows Hello 配套设备。

但是，配套设备供应商可以生成能提供远程吊销功能的服务。 有关更多详细信息，请参阅“漫游、吊销和筛选器服务”部分。

### <a name="roaming-and-filter-services"></a>漫游和筛选器服务

配套设备供应商可以实现可用于以下方案的 Web 服务：

- 企业的筛选器服务：企业可以将能够在它们的环境中运行的 Windows Hello 配套设备集限制为特定供应商的少数精选设备。 例如，公司 Contoso 可以从供应商 X 订购 10000 台 Y 型号配套设备，并确保仅这些设备能够在 Contoso 域中运行（并且供应商 X 的任何其他设备型号都不行）。
- 清单：企业可以确定在企业环境中使用的现有配套设备的列表。
- 实时吊销：如果员工报告配套设备丢失或失窃，可使用 Web 服务吊销该设备。
- 漫游：用户仅需注册配套设备一次，它就可以在所有 Windows 10 桌面版和移动版上运行。

实现这些功能要求 Windows Hello 配套设备应用在注册和使用时检查 Web 服务。 Windows Hello 配套设备应用可以为要求一天仅检查一次 Web 服务之类的缓存登录方案进行优化（代价是将吊销事件延长至最多一天）。  

## <a name="windows-hello-companion-device-framework-api-model"></a>Windows Hello 配套设备框架 API 模型

### <a name="overview"></a>概述

Windows Hello 配套设备应用应包含两个组件：UI 负责注册和注销设备的前台应用，以及处理身份验证的后台任务。

整体 API 流程如下所示：

1. 注册 Windows Hello 配套设备
    * 确保设备在附近并查询其功能（如果需要）
    * 生成两个 HMAC 密钥（在配套设备端或在应用端）
    * 调用 RequestStartRegisteringDeviceAsync
    * 调用 FinishRegisteringDeviceAsync
    * 确保 Windows Hello 配套设备应用存储了 HMAC 密钥（如果支持），并且 Windows Hello 配套设备应用放弃了其副本
2. 注册后台任务
3. 等待后台任务的正确事件
    * WaitingForUserConfirmation：如果开始身份验证流程需要 Windows Hello 配套设备端上的用户操作/手势，请等待此事件
    * CollectingCredential：如果电脑端的 Windows Hello 配套设备依靠用户操作/手势启动身份验证流程（例如通过点击空格键），请等待此事件
    * 其他触发器（例如智能卡）：确保查询当前身份验证状态，以调用正确的 API。
4. 通过调用 ShowNotificationMessageAsync，随时通知用户错误消息或要求的后续步骤。 仅在收集了意图信号后调用此 API
5. 解除锁定
    * 确保已收集意图和用户存在信号
    * 调用 StartAuthenticationAsync
    * 与配套设备通信，执行所需的 HMAC 操作
    * 调用 FinishAuthenticationAsync
6. 在用户提出相关请求时（例如用户丢失了配套设备），取消 Windows Hello 配套设备注册
    * 通过 FindAllRegisteredDeviceInfoAsync 为登录的用户枚举 Windows Hello 配套设备
    * 使用 UnregisterDeviceAsync 取消注册

### <a name="registration-and-de-registration"></a>注册和注销

注册需要配套身份验证服务的两个 API 调用：RequestStartRegisteringDeviceAsync 和 FinishRegisteringDeviceAsync。

在执行任何这些调用前，Windows Hello 配套设备应用必须确保 Windows Hello 配套设备可用。 如果 Windows Hello 配套设备负责生成 HMAC 密钥（身份验证和设备密钥），Windows Hello 配套设备应用还应要求配套设备在执行任何上述两个调用前生成这些密钥。 如果 Windows Hello 配套设备应用负责生成 HMAC 密钥，则它在调用上述两个调用前也应这样做。

此外，作为第一个 API 调用 (RequestStartRegisteringDeviceAsync) 的一部分，Windows Hello 配套设备应用必须确定设备功能，并准备将其作为 API 调用的一部分进行传递（例如 Windows Hello 配套设备是否支持安全存储 HMAC 密钥）。 如果相同 Windows Hello 配套设备应用用于管理多个版本的相同配套设备，并且这些功能会更改（还要求进行设备查询才能做出决定），我们建议此查询要在执行第一个 API 调用前发生。   

第一个 API (RequestStartRegisteringDeviceAsync) 将返回第二个 API (FinishRegisteringDeviceAsync) 使用的句柄。 首次调用注册将启动 PIN 提示符以确保用户已存在。 如果没有设置任何 PIN，此调用将失败。 Windows Hello 配套设备应用还可通过 KeyCredentialManager.IsSupportedAsync 调用查询 PIN 是否已设置。 如果策略已禁用 Windows Hello 配套设备，RequestStartRegisteringDeviceAsync 调用也可能会失败。

第一个调用的结果通过 SecondaryAuthenticationFactorRegistrationStatus 枚举返回：

```C#
{
    Failed = 0,         // Something went wrong in the underlying components
    Started,            // First call succeeded
    CanceledByUser,     // User cancelled PIN prompt
    PinSetupRequired,   // PIN is not set up
    DisabledByPolicy,   // Companion device framework or this app is disabled
}
```

第二个调用 (FinishRegisteringDeviceAsync) 完成注册。 作为注册过程的一部分，Windows Hello 配套设备应用可以使用配套身份验证服务来存储配套设备配置数据。 此数据的大小限制为 4K。 在进行身份验证时，此数据将提供给 Windows Hello 配套设备应用。 例如，此数据可用于连接 Windows Hello 配套设备（例如 MAC 地址），或者如果 Windows Hello 配套设备没有存储，但配套设备想要将电脑用于存储，则可使用配置数据。 注意，任何作为配置数据的一部分存储的敏感数据都必须使用仅 Windows Hello 配套设备知道的密钥加密。 另外，考虑到配置数据由 Windows 服务存储，它在用户配置文件上可用于 Windows Hello 配套设备应用。

Windows Hello 配套设备应用可调用 AbortRegisteringDeviceAsync 以取消注册，并传入错误代码。 配套身份验证服务将在遥测数据中记录错误。 执行此调用很好的示例是 Windows Hello 配套设备出错并且无法完成注册（例如无法存储 HMAC 密钥或者丢失了 BT 连接）。

Windows Hello 配套设备应用必须让用户可以选择从 Windows 10 桌面版注销 Windows Hello 配套设备（例如，在他们丢失了配套设备或购买了较新的版本时）。 当用户选择该选项时，Windows Hello 配套设备应用必须调用 UnregisterDeviceAsync。 此 Windows Hello 配套设备应用的调用将触发配套设备身份验证服务从电脑端删除与调用方应用的特定设备 Id 和 AppId 对应的所有数据（包括 HMAC 密钥）。 此 API 调用不会尝试从 Windows Hello 配套设备应用或配套设备端删除 HMAC 密钥。 这留待 Windows Hello 配套设备应用来实现。

Windows Hello 配套设备应用负责显示在注册和注销阶段发生的任何错误消息。

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.UI.Popups;

namespace SecondaryAuthFactorSample
{
    public class DeviceRegistration
    {

        public void async OnRegisterButtonClick()
        {
            //
            // Pseudo function, the deviceId should be retrieved by the application from the device
            //
            string deviceId = await ReadSerialNumberFromDevice();

            IBuffer deviceKey = CryptographicBuffer.GenerateRandom(256/8);
            IBuffer mutualAuthenticationKey = CryptographicBuffer.GenerateRandom(256/8);

            SecondaryAuthenticationFactorRegistration registrationResult =
                await SecondaryAuthenticationFactorRegistration.RequestStartRegisteringDeviceAsync(
                    deviceId,  // deviceId: max 40 wide characters. For example, serial number of the device
                    SecondaryAuthenticationFactorDeviceCapabilities.SecureStorage |
                        SecondaryAuthenticationFactorDeviceCapabilities.HMacSha256 |
                        SecondaryAuthenticationFactorDeviceCapabilities.StoreKeys,
                    "My test device 1", // deviceFriendlyName: max 64 wide characters. For example: John's card
                    "SAMPLE-001", // deviceModelNumber: max 32 wide characters. The app should read the model number from device.
                    deviceKey,
                    mutualAuthenticationKey);

            switch(registerResult.Status)
            {
            case SecondaryAuthenticationFactorRegistrationStatus.Started:
                //
                // Pseudo function:
                // The app needs to retrieve the value from device and set into opaqueBlob
                //
                IBuffer deviceConfigData = ReadConfigurationDataFromDevice();

                if (deviceConfigData != null)
                {
                    await registrationResult.Registration.FinishRegisteringDeviceAsync(deviceConfigData); //config data limited to 4096 bytes
                    MessageDialog dialog = new MessageDialog("The device is registered correctly.");
                    await dialog.ShowAsync();
                }
                else
                {
                    await registrationResult.Registration.AbortRegisteringDeviceAsync("Failed to connect to the device");
                    MessageDialog dialog = new MessageDialog("Failed to connect to the device.");
                    await dialog.ShowAsync();
                }
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.CanceledByUser:
                MessageDialog dialog = new MessageDialog("You didn't enter your PIN.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.PinSetupRequired:
                MessageDialog dialog = new MessageDialog("Please setup PIN in settings.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.DisabledByPolicy:
                MessageDialog dialog = new MessageDialog("Your enterprise prevents using this device to sign in.");
                await dialog.ShowAsync();
                break;
            }
        }

        public void async UpdateDeviceList()
        {
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.User);

            if (deviceInfoList.Count > 0)
            {
                foreach (SecondaryAuthenticationFactorInfo deviceInfo in deviceInfoList)
                {
                    //
                    // Add deviceInfo.FriendlyName and deviceInfo.DeviceId into a combo box
                    //
                }
            }
        }

        public void async OnUnregisterButtonClick()
        {
            string deviceId;
            //
            // Read the deviceId from the selected item in the combo box
            //
            await SecondaryAuthenticationFactorRegistration.UnregisterDeviceAsync(deviceId);
        }
    }
}
```

### <a name="authentication"></a>身份验证

身份验证需要配套身份验证服务的两个 API 调用：StartAuthenticationAsync 和 FinishAuthencationAsync。

第一个启动 API 将返回第二个 API 使用的句柄。  在其他事项中，第一个调用返回需要通过存储在 Windows Hello 配套设备上的设备密钥进行 HMAC 处理的 nonce（在与其他内容串联后）。 第二个调用返回 HMAC 的结果和设备密钥，并且最后可能验证身份成功（即用户将看到桌面）。

如果在初始注册后策略禁用了该 Windows Hello 配套设备，则第一个启动 API (StartAuthenticationAsync) 可能失败。 如果在 WaitingForUserConfirmation 或 CollectingCredential 状态之外执行了 API 调用，它也可能失败（稍后在本部分还会有详细介绍）。 如果未注册的配套设备应用进行调用，它也可能失败。 SecondaryAuthenticationFactorAuthenticationStatus 枚举总结了以下可能的结果：

```C#
{
    Failed = 0,                     // Something went wrong in the underlying components
    Started,
    UnknownDevice,                  // Companion device app is not registered with framework
    DisabledByPolicy,               // Policy disabled this device after registration
    InvalidAuthenticationStage,     // Companion device framework is not currently accepting
                                    // incoming authentication requests
}
```

如果第一个 API 调用提供的 nonce 到期（20 秒），第二个调用 (FinishAuthencationAsync) 可能会失败。 SecondaryAuthenticationFactorFinishAuthenticationStatus 枚举捕获可能的结果。

```C#
{
    Failed = 0,     // Something went wrong in the underlying components
    Completed,      // Success
    NonceExpired,   // Nonce is expired
}
```

两个 API 调用（StartAuthenticationAsync 和 FinishAuthencationAsync）的计时需要符合 Windows Hello 配套设备收集意图、用户存在和消除歧义信号的方式（有关详细信息，请参阅“用户信号”）。 例如，禁止在意图信号可用前提交第二个调用。 换句话说，在用户没有表达意图前，电脑不应解锁。 为了使表达更加清楚，假定蓝牙邻近感应用于电脑解锁，则必须收集显式意图信号；否则，只要用户走向厨房路过电脑旁边，电脑就会解锁。 另外，从第一个调用返回的 nonce 是有时间限制的（20 秒），并且在某段时间后将到期。 因此，仅应在 Windows Hello 配套设备应用可以清楚指示配套设备存在（例如，配套设备已插入 USB 端口或者已点击 NFC 读卡器）时，才能执行第一个调用。 使用蓝牙时，必须小心避免影响电脑端的电池或避免在检查 Windows Hello 配套设备存在时影响正在进行的其他蓝牙活动。 另外，如果需要提供用户存在信号（例如通过键入 PIN），建议在收集完该信号后执行第一个身份验证调用。

通过提供用户所处的身份验证流程的全貌，Windows Hello 配套设备框架可帮助 Windows Hello 配套设备应用明智地作出何时执行上述两个调用的决定。 通过向应用后台任务提供锁定状态更改通知，Windows Hello 配套设备框架可提供此功能。

![配套设备流](images/companion-device-4.png)

这些状态的详细信息如下所示：

| 状态                         | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| WaitingForUserConfirmation    | 当锁屏界面应用（例如用户已按下 Windows + L）时，将触发此状态更改通知事件。 我们不建议请求与在此状态中难以查找设备相关的任何错误消息。 一般情况下，我们建议仅在提供意图信号时显示消息。 如果配套设备收集意图信号（例如点击 NFC 读卡器、在配套设备上按某个按钮，或者是诸如鼓掌等特定手势），Windows Hello 配套设备应用应在此状态中将第一个 API 调用用于身份验证，并且 Windows Hello 配套设备应用后台任务会从配套设备接收已检测到意图信号的指示。 否则，如果 Windows Hello 配套设备应用依赖电脑启动身份验证流程（通过让用户轻扫解锁屏幕或点击空格键），则 Windows Hello 配套设备应用需要等待下一个状态 (CollectingCredential)。     |
| CollectingCredential          | 当用户打开笔记本电脑盖子、点击任意键盘按键或轻扫解锁屏幕时，将触发此状态更改通知事件。 如果 Windows Hello 配套设备依赖上述操作来开始收集意图信号，则 Windows Hello 配套设备应用应开始收集该信号（例如，通过配套设备上询问用户是否想要解锁电脑的弹出窗口）。 如果 Windows Hello 配套设备应用需要用户在配套设备上提供用户存在信号（例如在 Windows Hello 配套设备上键入 PIN），这将是提供错误情况的良好时机。                                                                                                                                                                                                                                                                                                                                            |
| SuspendingAuthentication      | 当 Windows Hello 配套设备应用接收此状态时，这意味着配套身份验证服务已停止接收身份验证请求。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| CredentialCollected           | 这表示另一个 Windows Hello 配套设备应用已调用了第二个 API，并且配套身份验证服务正在验证提交内容。 此时，除非当前提交的身份验证请求没有通过验证，否则配套身份验证服务不会接受任何其他身份验证请求。 Windows Hello 配套设备应用应继续关注，直到到达下一个状态。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CredentialAuthenticated       | 这表示提交的凭据有效。 CredentialAuthenticated 拥有已成功的 Windows Hello 配套设备的设备 ID。 Windows Hello 配套设备应用应确保查看它的关联设备是否是入选方。 如果不是，则 Windows Hello 配套设备应用应避免显示任何后身份验证流程（例如配套设备上的成功消息或者可能是该设备的振动）。 请注意，如果提交的凭据无效，则状态将更改为 CollectingCredential 状态。                                                                                                                                                                                                                                                                                                                                                                                       |
| StoppingAuthentication        | 身份验证已成功，并且用户已看到桌面。 现在应终止后台任务。 在退出后台任务之前，显式注销 StageEvent 处理程序。 这将有助于快速退出后台任务。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |



Windows Hello 配套设备应用在前两种状态中应当仅调用两个身份验证 API。 Windows Hello 配套设备应用应查看触发此事件的方案。 存在两种可能性：解锁或后解锁。 当前仅支持解锁。 在即将推出的版本中，后解锁方案可能会受支持。 SecondaryAuthenticationFactorAuthenticationScenario 枚举捕获以下两个选项：

```C#
{
    SignIn = 0,         // Running under lock screen mode
    CredentialPrompt,   // Running post unlock
}
```

完整代码示例：

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using System.Threading;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public sealed class AuthenticationTask : IBackgroundTask
    {
        private string _deviceId;
        private static AutoResetEvent _exitTaskEvent = new AutoResetEvent(false);
        private static IBackgroundTaskInstance _taskInstance;
        private BackgroundTaskDeferral _deferral;

        private void Authenticate()
        {
            int retryCount = 0;

            while (retryCount < 3)
            {
                //
                // Pseudo code, the svcAuthNonce should be passed to device or generated from device
                //
                IBuffer svcAuthNonce = CryptographicBuffer.GenerateRandom(256/8);

                SecondaryAuthenticationFactorAuthenticationResult authResult = await
                    SecondaryAuthenticationFactorAuthentication.StartAuthenticationAsync(
                        _deviceId,
                        svcAuthNonce);
                if (authResult.Status != SecondaryAuthenticationFactorAuthenticationStatus.Started)
                {
                    SecondaryAuthenticationFactorAuthenticationMessage message;
                    switch (authResult.Status)
                    {
                        case SecondaryAuthenticationFactorAuthenticationStatus.DisabledByPolicy:
                            message = SecondaryAuthenticationFactorAuthenticationMessage.DisabledByPolicy;
                            break;
                        case SecondaryAuthenticationFactorAuthenticationStatus.InvalidAuthenticationStage:
                            // The task might need to wait for a SecondaryAuthenticationFactorAuthenticationStageChangedEvent
                            break;
                        default:
                            return;
                    }

                    // Show error message. Limited to 512 characters wide
                    await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(null, message);
                    return;
                }

                //
                // Pseudo function:
                // The device calculates and returns sessionHmac and deviceHmac
                //
                await GetHmacsFromDevice(
                    authResult.Authentication.ServiceAuthenticationHmac,
                    authResult.Authentication.DeviceNonce,
                    authResult.Authentication.SessionNonce,
                    out deviceHmac,
                    out sessionHmac);
                if (sessionHmac == null ||
                    deviceHmac == null)
                {
                    await authResult.Authentication.AbortAuthenticationAsync(
                        "Failed to read data from device");
                    return;
                }

                SecondaryAuthenticationFactorFinishAuthenticationStatus status =
                    await authResult.Authentication.FinishAuthencationAsync(deviceHmac, sessionHmac);
                if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.NonceExpired)
                {
                    retryCount++;
                    continue;
                }
                else if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.Completed)
                {
                    // The credential data is collected and ready for unlock
                    return;
                }
            }
        }

        public void OnAuthenticationStageChanged(
            object sender,
            SecondaryAuthenticationFactorAuthenticationStageChangedEventArgs args)
        {
            // The application should check the args.StageInfo.Stage to determine what to do in next. Note that args.StageInfo.Scenario will have the scenario information (SignIn vs CredentialPrompt).

            switch(args.StageInfo.Stage)
            {
            case SecondaryAuthenticationFactorAuthenticationStage.WaitingForUserConfirmation:
                // Show welcome message
                await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(
                    null,
                    SecondaryAuthenticationFactorAuthenticationMessage.WelcomeMessageSwipeUp);
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CollectingCredential:
                // Authenticate device
                Authenticate();
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CredentialAuthenticated:
                if (args.StageInfo.DeviceId = _deviceId)
                {
                    // Show notification on device about PC unlock
                }
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.StoppingAuthentication:
                // Quit from background task
                _exitTaskEvent.Set();
                break;
            }

            Debug.WriteLine("Authentication Stage = " + args.StageInfo.AuthenticationStage.ToString());
        }

        //
        // The Run method is the entry point of a background task.
        //
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            _taskInstance = taskInstance;
            _deferral = taskInstance.GetDeferral();

            // Register canceled event for this task
            taskInstance.Canceled += TaskInstanceCanceled;

            // Find all device registred by this application
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.AllUsers);

            if (deviceInfoList.Count == 0)
            {
                // Quit the task silently
                return;
            }
            _deviceId = deviceInfoList[0].DeviceId;
            Debug.WriteLine("Use first device '" + _deviceId + "' in the list to signin");

            // Register AuthenticationStageChanged event
            SecondaryAuthenticationFactorRegistration.AuthenticationStageChanged += OnAuthenticationStageChanged;

            // Wait the task exit event
            _exitTaskEvent.WaitOne();

            _deferral.Complete();
        }

        void TaskInstanceCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _exitTaskEvent.Set();
        }
    }
}
```

### <a name="register-a-background-task"></a>注册后台任务

当 Windows Hello 配套设备应用注册第一台配套设备时，它还应注册该配套设备能够在设备和配套设备身份验证服务之间传递身份验证信息的后台任务组件。

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public class BackgroundTaskManager
    {
        // Register background task
        public static async Task<IBackgroundTaskRegistration> GetOrRegisterBackgroundTaskAsync(
            string bgTaskName,
            string taskEntryPoint)
        {
            // Check if there's an existing background task already registered
            var bgTask = (from t in BackgroundTaskRegistration.AllTasks
                          where t.Value.Name.Equals(bgTaskName)
                          select t.Value).SingleOrDefault();
            if (bgTask == null)
            {
                BackgroundAccessStatus status =
                    BackgroundExecutionManager.RequestAccessAsync().AsTask().GetAwaiter().GetResult();

                if (status == BackgroundAccessStatus.Denied)
                {
                    Debug.WriteLine("Background Execution is denied.");
                    return null;
                }

                var taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = bgTaskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger(new SecondaryAuthenticationFactorAuthenticationTrigger());
                bgTask = taskBuilder.Register();
                // Background task is registered
            }

            bgTask.Completed += BgTask_Completed;
            bgTask.Progress += BgTask_Progress;

            return bgTask;
        }
    }
}
```

### <a name="errors-and-messages"></a>错误和消息

Windows Hello 配套设备框架负责向用户提供有关登录成功或失败的反馈。 Windows Hello 配套设备框架会提供大量（本地化的）文本和错误消息以供 Windows Hello 配套设备应用进行选择。 这些内容将显示在登录 UI 中。

![配套设备错误](images/companion-device-5.png)

Windows Hello 配套设备应用可使用 ShowNotificationMessageAsync 向用户显示作为登录 UI 一部分的消息。 在意图信号可用时，调用此 API。 请注意，意图信号必须始终在 Windows Hello 配套设备端上收集。

有两种消息类型：指导和错误消息。

指导消息旨在向用户显示如何启动解锁过程。 这些消息只会在设备第一次注册时在锁屏界面上向用户显示一次，并且不会再显示。 这些消息将继续显示在锁屏界面下。

错误消息将始终显示并且会在提供意图信号后进行显示。 考虑到必须先收集意图信号再向用户显示消息，并且用户将仅使用其中一台 Windows Hello 配套设备提供该意图，所以禁止出现多台 Windows Hello 配套设备争相显示错误消息的情况。 因此，Windows Hello 配套设备框架不会保持任何队列。 当调用方请求错误消息时，这会显示 5 秒钟，并且在这 5 秒钟时间内其他所有显示错误消息的请求都会被弃用。 5 秒钟过后，其他调用方就有机会显示错误消息。 我们禁止任何调用方堵塞错误通道。

指导和错误消息如下所示。 设备名称是配套设备应用作为 ShowNotificationMessageAsync 的一部分传递的参数。

**指导**

- “轻扫或按空格键以使用*设备名称*登录。”
- “正在设置配套设备。 请等待或使用其他登录选项。”
- “点击 NFC 读卡器的*设备名称*以登录。”
- “查找*设备名称*...”
- “将*设备名称*插入 USB 端口以登录。”

**错误**

- “有关登录说明，请参阅*设备名称*。”
- “打开蓝牙以使用*设备名称*登录。”
- “打开 NFC 以使用*设备名称*登录。”
- “连接到 WLAN 网络以使用*设备名称*登录。”
- “再次点击*设备名称*。”
- “你的企业阻止使用*设备名称*登录。 请使用其他登录选项。”
- “点击*设备名称*以登录。”
- “将你的手指放在*设备名称*上以登录。”
- “用手指在*设备名称*上轻扫以登录。”
- “无法使用*设备名称*登录。 请使用其他登录选项。”
- “出现了一些问题。 请使用其他登录选项，然后再次设置*设备名称*。”
- “再试一次。”
- “在*设备名称*中说出你的口令密码。”
- “准备好使用*设备名称*登录。”
- “请先使用其他登录选项，然后再使用*设备名称*登录。”

### <a name="enumerating-registered-devices"></a>枚举已注册的设备

Windows Hello 配套设备应用可通过 FindAllRegisteredDeviceInfoAsync 调用来枚举注册的配套设备列表。 此 API 支持两种由枚举 SecondaryAuthenticationFactorDeviceFindScope 定义的查询类型：

```C#
{
    User = 0,
    AllUsers,
}
```

第一个作用域为登录的用户返回设备列表。 第二个作用域在该电脑上为所有用户返回该列表。 第一个作用域必须在注销时使用，以避免注销其他用户的 Windows Hello 配套设备。 第二个作用域必须在身份验证或注册时使用：在注册时，此枚举可帮助应用避免尝试注册相同的 Windows Hello 配套设备两次。

请注意，即使应用不会执行此检查，电脑也会这样做，将拒绝多次注册相同的 Windows Hello 配套设备。 在进行身份验证时，使用 AllUsers 作用域可帮助 Windows Hello 配套设备应用支持切换用户流：当用户 B 登录时用户 A 也登录（这要求两个用户都安装了 Windows Hello 配套设备应用，并且用户 A 已将配套设备注册到了电脑且电脑正显示锁屏界面（或登录屏幕））。

## <a name="security-requirements"></a>安全要求

配套身份验证服务提供以下安全保护。

- 在 Windows 10 桌面版设备上作为媒介用户或应用容器运行的恶意软件无法在电脑上以静默方式使用 Windows Hello 配套设备访问用户凭据密钥（存储为 Windows Hello 的一部分）。
- 使用 Windows 10 桌面版设备的恶意用户无法使用属于该 Windows 10 桌面版设备上的其他用户的 Windows Hello 配套设备静默访问其用户凭据密钥（在相同的 Windows 10 桌面版设备上）。
- Windows Hello 配套设备上的恶意软件无法在 Windows 10 桌面版设备上静默访问用户凭据密钥，包括利用专为 Windows Hello 配套设备框架开发的功能或代码。
- 恶意用户无法通过捕获 Windows Hello 配套设备和 Windows 10 桌面版设备之间的通信并在以后重播它，来解锁 Windows 10 桌面版设备。 在协议中使用 nonce、authkey 和 HMAC 可保证防止重播攻击。
- 恶意电脑上的恶意软件或恶意用户无法使用 Windows Hello 配套设备访问真实的用户电脑。 这通过在协议中使用 authkey 和 HMAC 相互验证配套身份验证服务和 Windows Hello 配套设备来实现。

实现上述枚举的安全保护的关键是保护 HMAC 密钥不受未经授权的访问，并验证用户存在。 更具体地说，它必须满足以下要求：

- 防止克隆 Windows Hello 配套设备
- 防止在注册期间将 HMAC 密钥发送到电脑时窃听
- 确保用户存在信号可用
