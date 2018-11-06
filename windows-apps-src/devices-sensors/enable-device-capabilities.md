---
author: muhsinking
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: 启用设备功能
description: 本教程介绍如何在 Microsoft Visual Studio 中声明设备功能。 这允许你的应用使用相机、麦克风、位置传感器以及其他设备。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a7250c41795373b089f7a4c76b603c169b1e4dc3
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6042818"
---
# <a name="enable-device-capabilities"></a>启用设备功能



本教程介绍如何在 Microsoft Visual Studio 中声明设备功能。 这允许你的应用使用相机、麦克风、位置传感器以及其他设备。

## <a name="specify-the-device-capabilities-your-app-will-use"></a>指定你的应用将使用的设备功能


当你使用某些类型的设备时，Windows 应用要求你在应用包清单中进行指定。 在 Visual Studio 中，你可以使用[清单设计器](https://msdn.microsoft.com/library/windows/apps/xaml/br230259.aspx)声明大多数功能，也可以按照[如何在程序包清单中指定设备功能（手动）](https://msdn.microsoft.com/library/windows/apps/Dn263092)所述手动添加它们。 本教程假定你使用清单设计器。

**注意**某些类型的设备，例如打印机、 扫描仪和传感器，无需在应用包清单中声明。

-   在 Visual Studio 解决方案资源管理器中，双击程序清单文件 **Package.appxmanifest**。
-   打开“功能”**** 选项卡。
-   选择你的应用使用的设备功能。 如果你在清单设计器中没有看到你需要的功能，请手动添加该功能。 有关详细信息，请参阅[如何在程序包清单中指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)。

| 设备功能 | 清单设计器 | 描述 |
|-------------------|-------------------|-------------|    
| AllJoyn | ![在清单设计器中可用](images/ap-tools.png) | 允许网络上支持 AllJoyn 的应用和设备发现彼此并进行交互。 [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) 命名空间中访问 API 的应用都必须使用此功能。 |
| 阻止的聊天消息 | ![在清单设计器中可用](images/ap-tools.png) | 允许应用读取已由“垃圾邮件筛选器”应用阻止的短信和彩信消息。 |
| 聊天消息访问 | ![在清单设计器中可用](images/ap-tools.png) | 允许应用读取和删除短信。 它还允许应用将聊天消息存储在系统数据存储中。 |
| 代码生成 | ![在清单设计器中可用](images/ap-tools.png) | 允许应用以动态方式生成代码。 |
| 企业身份验证 | ![在清单设计器中可用](images/ap-tools.png) | 此功能遵循 Microsoft Store 策略。 它允许连接至要求提供域凭据的企业 Intranet 资源。 大多数应用通常不需要此功能。 | 
| Internet (客户端) | ![在清单设计器中可用](images/ap-tools.png) | 提供对 Internet 及公共场所（如机场和咖啡厅）网络的出站访问。 例如，用户将网络指定为公共网络的 Intranet 网络。 需要进行 Internet 访问的大多数应用都应使用此功能。 |
| Internet（客户端 &amp; 服务器） | ![在清单设计器中可用](images/ap-tools.png) | 提供对 Internet 及公共场所（如机场和咖啡厅）网络的入站和出站访问。 此功能是“Internet (客户端)”**** 的超集。 如果已启用此功能，则不需要同时启用“Internet (客户端)”****。 对重要端口的入站访问始终会被阻止。 |
| 位置| ![在清单设计器中可用](images/ap-tools.png) | 提供对当前位置的访问。 当前位置是从专用硬件（例如电脑中的 GPS 传感器）或从可用的网络信息中获取的。 | 
| 麦克风 | ![在清单设计器中可用](images/ap-tools.png) | 提供对麦克风音频源的访问。 这允许应用从所连接的麦克风进行录音。 | 
| 音乐库 | ![在清单设计器中可用](images/ap-tools.png) | 能够添加、更改或删除本地电脑和**家庭组**电脑的“音乐库”**** 中的文件。 | 
| 对象 3D | ![在清单设计器中可用](images/ap-tools.png) | 提供对用户 **3D 对象**的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 此功能通常用在需要访问整个 **3D 对象**库的 3D 应用和游戏中。 | 
| 电话呼叫 | ![在清单设计器中可用](images/ap-tools.png) | 允许应用访问设备上的所有电话线路并执行以下功能：在手机上进行拨号并显示系统拨号器而不提示用户；访问与线路相关的元数据；访问与线路相关的触发器。 允许用户选择的垃圾邮件筛选器应用设置并检查阻止列表和呼叫来源信息。 | 
| 图片库 | ![在清单设计器中可用](images/ap-tools.png) | 提供在本地电脑和**家庭组**电脑中添加、更改或删除**图片库**文件的功能。 | 
| 服务点 | ![在清单设计器中可用](images/ap-tools.png) | 提供对服务点外设的访问权限。 若要访问 [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 命名空间中的 API，则需要此功能。 | 
| 专用网络（客户端和服务器） | ![在清单设计器中可用](images/ap-tools.png) | 针对具有经过身份验证的域控制器或用户已指定为家庭或工作网络的 Intranet 网络提供入站和出站访问。 对重要端口的入站访问始终会被阻止。 | 
| 邻近感应 | ![在清单设计器中可用](images/ap-tools.png) | 能够通过近距离通信 (NFC) 与靠近电脑的设备进行连接。 近距离感应可用于向附近设备上的应用发送文件或与其进行通信。 | 
| 可移动存储 | ![在清单设计器中可用](images/ap-tools.png) | 能够添加、更改或删除可移动存储设备上的文件。 应用只能访问可移动存储上使用“文件类型关联”**** 声明在清单中定义的文件类型。 应用无法访问**家庭组**电脑上的可移动存储。 | 
| 共享用户证书 | ![在清单设计器中可用](images/ap-tools.png) | 此功能遵循 Microsoft Store 策略。 它允许访问用于验证用户身份的软件和硬件证书，如智能卡证书。 如果相关 API 在运行时被调用，用户必须执行操作（插卡、选择证书等）。 如果你的应用通过“证书”**** 声明包含专用证书，则不需要此功能。 | 
| 用户帐户信息 | ![在清单设计器中可用](images/ap-tools.png) | 使应用能够访问用户的名称和头像。 若要访问 [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) 命名空间中的某些 API，此功能是必需的。 | 
| 视频库 | ![在清单设计器中可用](images/ap-tools.png) | 能够添加、更改或删除本地电脑和**家庭组**电脑的“视频库”**** 中的文件。 | 
| VOIP 呼叫 | ![在清单设计器中可用](images/ap-tools.png) | 允许应用访问 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空间中的 VOIP 呼叫 API。 | 
| 摄像头 | ![在清单设计器中可用](images/ap-tools.png) | 提供对内置相机或附加摄像头视频源的访问权限。 这允许应用捕获快照和影片。 | 
| USB | | 提供对自定义 USB 设备的访问。 此功能需要子元素。 该功能在 Windows Phone 上不受支持。 | 
| 人体学接口设备 (HID) | | 提供对人体学接口设备 (HID) 的访问。 此功能需要子元素。 有关详细信息，请参阅[如何为 HID 指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263091)。 | 
| 蓝牙 GATT | | 通过主要服务、附属服务、特征和描述符的集合提供对蓝牙 LE 设备的访问。 此功能需要子元素。 有关详细信息，请参阅[如何为蓝牙指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263090)。 | 
| 蓝牙 RFCOMM |  | 提供对支持基本速率/扩展数据速率 (BR/EDR) 传输的 API 的访问，并且允许 UWP 应用访问实现了串行端口配置文件 (SPP) 的设备。 此功能需要子元素。 有关详细信息，请参阅[如何为蓝牙指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263090)。 |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>使用 Windows 运行时 API 与设备进行通信

下表会将某些功能连接到 Windows 运行时 API。

| 设备功能        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) | 
| 阻止的聊天消息    | [**Windows.ApplicationModel.CommunicationBlocking**](https://msdn.microsoft.com/library/windows/apps/Dn974207) | 
| 位置                 | 有关详细信息，请参阅[地图和位置概述](https://msdn.microsoft.com/library/windows/apps/Mt219699)。 | 
| 电话呼叫               | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| 用户帐户信息 | [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) | 
| VOIP 呼叫             | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| USB                      | [**Windows.Devices.Usb**](https://msdn.microsoft.com/library/windows/apps/Dn278466) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://msdn.microsoft.com/library/windows/apps/Dn264174) | 
| Bluetooth GATT           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) | 
| Bluetooth RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) | 
| 服务点         | [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) |

