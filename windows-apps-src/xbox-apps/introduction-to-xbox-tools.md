---
title: Xbox One 工具简介
description: 特定于 Xbox One 且使用 Windows Device Portal 的工具“开发人员主页”。
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp, xbox one, 工具, windows 10, uwp, xbox one, tools
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed106095d83ed0c6e055d22a1a0cf229380cff71
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795004"
---
# <a name="introduction-to-xbox-one-tools"></a>Xbox One 工具简介

本部分介绍如何通过开发人员主页应用访问 Xbox Device Portal。

## <a name="dev-home"></a>开发人员主页

“开发人员主页”是 Xbox One 开发工具包上旨在提高开发人员工作效率的工具体验。 “开发人员主页”提供管理和配置开发人员工具包的功能。

“开发人员主页”是处于开发人员模式的主机启动时打开的默认应用。 也可以通过在主页屏幕上选择**开发人员主页**磁贴来打开“开发人员主页”。 如果不存在任何磁贴，则主机不处于开发人员模式中。

有关开发人员主页的更多信息，请参阅[主机上的开发人员主页（开发人员主页）](dev-home.md)。

## <a name="xbox-device-portal"></a>Xbox Device Portal
Xbox Device Portal 是一种基于浏览器的设备管理工具，便于你添加游戏和应用、添加 Xbox Live 测试帐户、更改沙箱等。

在 Xbox One 主机上启用 Xbox Device Portal：

1. 在主屏幕上选择**开发人员主页**磁贴。

  ![选择“开发人员主页”磁贴](images/introduction-to-xbox-one-tools-1.png)

2. 在开发人员主页中，导航到**主页**选项卡，并在**远程访问**部分中选择**远程访问设置**。

  ![远程管理工具](images/introduction-to-xbox-one-tools-2.png)

3. 选中**启用 Xbox Device Portal** 复选框。

4. 在**身份验证**下，选择**需要身份验证才能从 Web 或电脑工具远程访问此主机**复选框。

5. 输入**用户名**和__密码__，然后选择**保存**。 这些凭据用于验证浏览器对开发人员工具包的访问权限。

6. 选择**关闭**，并在**主页**选项卡上，记下**远程访问**工具上列出的 URL。

7. 在浏览器中输入 URL。 你将收到有关所提供证书的警告（类似于以下屏幕截图），因为 Xbox One 主机签名的安全证书不被视为已知的受信任发布者。 在 Edge 中，单击**详细信息**，然后**转到网页**以访问 Xbox Device Portal。

    ![安全证书警告](images/introduction-to-xbox-one-tools-3.png)

8. 使用你配置的凭据登录。

## <a name="xbox-dev-mode-companion"></a>Xbox 开发人员模式助手
Xbox 开发人员模式助手是一个工具，使你无需离开电脑就可以在你的主机上工作。 该应用使你可以查看主机屏幕以及向主机发送输入。 有关详细信息，请参阅 [Xbox 开发人员模式助手](xbox-dev-mode-companion.md)。

## <a name="see-also"></a>另请参阅
- [在针对 UWP 进行开发时如何将 Fiddler 用于 Xbox One](uwp-fiddler.md)
- [Windows Device Portal 概述](../debug-test-perf/device-portal.md)
- [Xbox One 上的 UWP](index.md)