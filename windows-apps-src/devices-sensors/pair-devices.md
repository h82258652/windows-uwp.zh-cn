---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: 设备配对
description: 某些设备需要先进行配对，然后才能使用。 Windows.Devices.Enumeration 命名空间支持使用三种不同方式的配对设备。
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6f59a23f08abbd5edeb9d68be476fb29bd8aab2e
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63813618"
---
# <a name="pair-devices"></a>设备配对



**重要的 Api**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

某些设备需要先进行配对，然后才能使用。 [  **Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 命名空间支持三种配对设备的不同方式。

-   自动配对
-   基本配对
-   自定义配对

**提示**  某些设备不需要为可进行配对。 这将在自动配对的部分中进行介绍。

 

## <a name="automatic-pairing"></a>自动配对


有时你想要在你的应用程序中使用设备，但不关心设备是否已配对。 只想要能够使用与设备关联的功能。 例如，你的应用只想要捕获来自摄像头的图像，对设备本身不一定感兴趣，只关注图像捕获。 如果有适用于你感兴趣的设备的设备 API，这种情况可归入自动配对。

在此情况下，你只需使用与设备关联的 API，然后按需调用并信任系统处理任何可能需要的配对。 某些设备无需进行配对即可供你使用其功能。 当设备并不需要进行配对时，该设备 API 将在后台处理配对操作，这样一来你便无需将该功能集成到你的应用中。 你的应用无需知道给定设备是否已配对或者是否需要配对，不过你仍然可以访问该设备并使用其功能。

## <a name="basic-pairing"></a>基本配对


基本配对是应用程序使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 来尝试配对设备的过程。 在此情况下，你将允许 Windows 尝试配对过程并处理它。 如果需要任何用户交互，将由 Windows 来处理。 如果你需要与设备配对而且不存在将尝试自动配对的相关设备 API，你将使用基本配对。 你只是想要能够使用该设备并且需要先与之配对。

若要尝试基本配对，首先需要获取感兴趣的设备的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象。 收到该对象后，将与属于 [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 对象的 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 属性进行交互。 若要尝试与之配对，只需调用 [**DeviceInformationPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/mt608800) 即可。 你将需要 **await** 结果，以便让应用可以有时间来尝试完成配对操作。 将返回配对操作的结果，并且只要未返回任何错误，就将配对设备。

如果你使用的是基本配对，还可以访问有关设备配对状态的其他信息。 例如，你可以了解配对状态 ([**IsPaired**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) 以及设备是否可以配对 ([**CanPair**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair))。 这两者都是 [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 对象的属性。 如果你使用的是自动配对，你可能无法访问此信息，除非你获得相关的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象。

## <a name="custom-pairing"></a>自定义配对


自定义配对使你的应用能够参与配对过程。 这使你的应用可以指定配对过程所支持的 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808)。 你还将负责创建你自己的用户界面，以便根据需要与用户进行交互。 如果你希望你的应用对配对过程的处理方式所起到的作用更大些，或者希望显示自己的配对用户界面，可以使用自定义配对。

为了实现自定义配对，你将需要获取感兴趣的设备的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象，就像使用基本配对那样。 不过，你感兴趣的特定属性是 [**DeviceInformation.Pairing.Custom**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.custom.aspx)。 这将为你提供 [**DeviceInformationCustomPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.aspx) 对象。 所有的 [**DeviceInformationCustomPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairasync.aspx) 方法都要求你包含 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) 参数。 这表示用户为尝试配对设备需要执行的操作。 有关其他配对类型以及用户需要执行的操作的详细信息，请参阅 **DevicePairingKinds** 参考页面。 就像使用基本配对一样，你将需要 **await** 结果，以便让应用有时间来尝试完成配对操作。 将返回配对操作的结果，并且只要未返回任何错误，就将配对设备。

为了支持自定义配对，将需要为 [**PairingRequested**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested.aspx) 事件创建一个处理程序。 此处理程序需要确保可处理可能会在自定义配对方案中使用的所有各种 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808)。 要执行何种操作具体取决于作为事件参数的一部分提供的 **DevicePairingKinds**。

请务必注意，自定义配对始终是系统级操作。 因此，如果你在桌面或 Windows Phone 上操作，将始终在配对即将发生时向用户显示系统对话框。 这是因为这两个平台所拥有的用户体验都要求征得用户同意。 由于该对话框是自动生成的，因此如果你在这两个平台上操作时为 **ConfirmOnly** 选择了 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808)，将无需创建你自己的对话框。 对于其他 **DevicePairingKinds**，你将需要执行一些特殊处理，具体取决于特定 **DevicePairingKinds** 值。 有关如何针对不同的 **DevicePairingKinds** 值处理自定义配对的示例，请参阅相关示例。

从 Windows 10，版本 1903年，一个新**DevicePairingKinds**支持，则**ProvidePasswordCredential**。 此值表示，应用必须请求用户名和密码从用户为了使用配对的设备进行身份验证。 若要处理这种情况下，调用[ **AcceptWithPasswordCredential** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs.acceptwithpasswordcredential?branch=release-19h1#Windows_Devices_Enumeration_DevicePairingRequestedEventArgs_AcceptWithPasswordCredential_Windows_Security_Credentials_PasswordCredential_)的事件参数的方法**PairingRequested**事件处理程序以接受配对。 传入[ **PasswordCredential** ](https://docs.microsoft.com/uwp/api/windows.security.credentials.passwordcredential)封装的用户名和密码作为参数的对象。 请注意，用户名和密码远程设备是不同于和通常不完全相同的本地登录的用户的凭据。

## <a name="unpairing"></a>取消配对


取消配对设备仅适用于上面所述的基本配对或自定义配对方案。 如果你使用的是自动配对，你的应用仍无需关注设备的配对状态且无需进行取消配对。 如果你选择取消配对设备，则该过程与实现基本配对或自定义配对之中的任一过程相同。 这是因为不需要提供其他信息或在取消配对过程中进行交互。

取消设备配对的第一步是获取你想要取消配对的设备的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象。 然后，你需要检索 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 属性并调用 [**DeviceInformationPairing.UnpairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.unpairasync)。 与使用配对类似，将需要 **await** 结果。 将返回取消配对操作的结果，并且只要未返回任何错误，就将取消设备配对。

## <a name="sample"></a>示例


若要下载展示如何使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 的示例，请单击[此处](https://go.microsoft.com/fwlink/?LinkID=620536)。

 

 
