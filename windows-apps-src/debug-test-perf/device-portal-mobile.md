---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: 适用于移动设备的 Device Portal
description: 了解 Windows 设备门户是如何支持你远程配置和管理你的移动设备。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 0531cbefef509f7bc323829031b366bec3c798d8
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959975"
---
# <a name="device-portal-for-mobile"></a>适用于移动设备的 Device Portal

从版本 1511 的 Windows 10 开始，附加的开发人员功能可用于移动设备系列。 这些功能仅在设备上启用了“开发人员模式”时才可用。

有关如何启用“开发人员模式”的信息，请参阅[启用设备进行开发](../get-started/enable-your-device-for-development.md)。

![设备门户设置](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>在 Windows Phone 上设置设备门户

### <a name="turn-on-device-discovery-and-pairing"></a>打开设备发现和配对

若要连接到设备门户，必须在手机设置中启用设备发现和设备门户。 这允许你将手机与电脑或其他 Windows 10 设备进行配对。 两台设备均必须通过有线或无线的连接方式连接到网络的同一子网，或者它们必须通过 USB 进行连接。

首次连接设备门户时，要求输入区分大小写的 6 个字符的安全代码。 这可确保你对手机拥有访问权限，并且使你的手机免受攻击。 在手机上按“配对”按钮将生成和显示 6 个字符的代码，然后在浏览器的文本框中输入该代码。

![开发人员模式设备发现设置](images/device-portal/mob-dev-mode-pairing.png)

你可以从 3 种方法中选择一种方法连接到设备门户：USB、本地主机和通过本地网络（包括 VPN 和网络共享）。

**连接到设备门户**

1. 在浏览器中，针对使用的连接类型输入地址，如下所示。

    - USB： `http://127.0.0.1:10080`

    当手机通过 USB 连接连接到电脑时，使用此地址。 两台设备必须具有版本 1511 或更高版本的 Windows 10。
    
    - Localhost： `http://127.0.0.1`

    使用此地址，可通过手机上的 Windows 10 移动版 Microsoft Edge 以本地方式查看设备门户。
    
    - 本地网络： `https://<The IP address or hostname of the phone>`

    使用此地址通过本地网络进行连接。

    手机的 IP 地址显示在其设备门户设置中。 身份验证和安全通信要求使用 HTTPS。 主机名（可在“设置”>“系统”>“关于”中进行编辑）还可用于访问本地网络（例如 http://Phone360)）上的设备门户，这对于可能频繁更改网络或 IP 地址的设备或者需要共享的设备非常有用。 

2. 在手机上按“配对”按钮以生成和显示所需的安全代码

3. 在浏览器的设备门户密码框中输入 6 个字符的安全代码。

4. （可选）在浏览器中选中“记住我的计算机”复选框以在以后记住此配对。

以下是 Windows Phone 上开发人员设置页面的设备门户部分。

![设备门户设置](images/device-portal/mob-dev-mode-portal.png)

如果在信任本地网络上的所有用户、设备上没有任何私人信息并且具有独特要求的受保护环境（例如测试实验室）中使用设备门户，你可以禁用身份验证。 这支持未加密的通信，并允许任何拥有手机 IP 地址的用户控制通信。

## <a name="tool-notes"></a>工具说明

## <a name="device-portal-pages"></a>设备门户页面
### <a name="processes"></a>进程

Windows 移动版设备门户中并未包含可以终止任意进程的功能。 

移动设备上的设备门户提供了一组标准页面。 有关详细说明，请参阅 [Windows 设备门户概述](device-portal.md)。

- 应用管理员
- 应用文件资源管理器（独立存储资源管理器）
- 进程
- 性能图表
- Windows 事件跟踪 (ETW)
- 性能跟踪 (WPR) 
- 设备
- 网络

## <a name="see-also"></a>另请参阅

* [Windows 设备门户概述](device-portal.md)
* [设备门户核心 API 参考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)