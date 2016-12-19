---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "适用于移动设备的 Device Portal"
description: "了解 Windows Device Portal 是如何支持你远程配置和管理你的移动设备。"
translationtype: Human Translation
ms.sourcegitcommit: ea4f755afbf16d4ba5043ebb6be586f19dcc4370
ms.openlocfilehash: c39c1a843c4f466e1999b4e80bf87f5842ad1996

---
# <a name="device-portal-for-mobile"></a>适用于移动设备的 Device Portal

从版本 1511 的 Windows 10 开始，附加的开发人员功能可用于移动设备系列。 这些功能仅在设备上启用了“开发人员模式”时才可用。

有关如何启用“开发人员模式”的信息，请参阅[启用设备进行开发](../get-started/enable-your-device-for-development.md)。

![Device Portal 设置](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>在 Windows Phone 上设置 Device Portal

### <a name="turn-on-device-discovery-and-pairing"></a>打开设备发现和配对

若要连接 Device Portal，必须启用设备发现和 Device Portal。 这允许你将手机与电脑或其他 Windows 10 设备进行配对。 两台设备均必须通过有线或无线的连接方式连接到网络的同一子网，或者它们必须通过 USB 进行连接。

首次连接 Device Portal 时，要求输入区分大小写的 6 个字符的安全代码。 这可确保你对手机拥有访问权限，并且使你的手机免受攻击。 在手机上按“配对”按钮将生成和显示 6 个字符的代码，然后在浏览器的文本框中输入该代码。

![开发人员模式设备发现设置](images/device-portal/mob-dev-mode-pairing.png)

你可以从 3 种方法中选择一种方法连接到 Device Portal：USB、本地主机和通过本地网络（包括 VPN 和网络共享）。

**连接到 Device Portal**

1. 在浏览器中，针对使用的连接类型输入地址，如下所示。

    - USB： `http://127.0.0.1:10080`

    当手机通过 USB 连接连接到电脑时，使用此地址。 两台设备必须具有版本 1511 或更高版本的 Windows 10。
    
    - Localhost： `http://127.0.0.1`

    使用此地址，可通过手机上的 Windows 10 移动版 Microsoft Edge 以本地方式查看 Device Portal。
    
    - 本地网络： `https://<The IP address or hostname of the phone>`

    使用此地址通过本地网络进行连接。

    手机的 IP 地址显示在其 Device Portal 设置中。 身份验证和安全通信要求使用 HTTPS。 主机名\（可在“设置”&gt;“系统”&gt;“关于”中进行编辑\）还可用于访问本地网络\（例如 http://Phone360\）上的 Device Portal，这对于可能频繁更改网络或 IP 地址的设备或者需要共享的设备非常有用。 

2. 在手机上按“配对”按钮以生成和显示所需的安全代码

3. 在浏览器的 Device Portal 密码框中输入 6 个字符的安全代码。

4. （可选）在浏览器中选中“记住我的计算机”复选框以在以后记住此配对。

以下是 Windows Phone 上开发人员设置页面的 Device Portal 部分。

![Device Portal 设置](images/device-portal/mob-dev-mode-portal.png)

如果在信任本地网络上的所有用户、设备上没有任何私人信息并且具有独特要求的受保护环境（例如测试实验室）中使用 Device Portal，你可以禁用身份验证。 这支持未加密的通信，并允许任何拥有手机 IP 地址的用户控制通信。

## <a name="tool-notes"></a>工具说明

## <a name="device-portal-pages"></a>Device Portal 页面
### <a name="processes"></a>进程

Windows 移动版 Device Portal 中并未包含可以终止任意进程的功能。 

移动设备上的 Device Portal 提供了一组标准页面。 有关详细说明，请参阅 [Windows Device Portal 概述](device-portal.md)。

- 应用管理员
- 应用文件资源管理器（独立存储资源管理器）
- 进程
- 性能图表
- Windows 事件跟踪 (ETW)
- 性能跟踪 (WPR) 
- 设备
- 网络


<!--HONumber=Dec16_HO1-->


