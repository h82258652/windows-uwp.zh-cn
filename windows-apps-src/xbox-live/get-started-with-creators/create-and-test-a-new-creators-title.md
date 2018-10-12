---
title: 创建和测试新的创意者主题作品
author: KevinAsgari
description: 了解如何创建新的 Xbox Live 创意者计划主题作品并发布到测试环境。
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 创意者, 测试
ms.localizationpriority: medium
ms.openlocfilehash: fe31a014c3076fa306249aeea006f817956013e2
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563179"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>创建新的 Xbox Live 创意者计划主题作品并发布到测试环境

## <a name="introduction"></a>简介

编写任意 Xbox Live 代码之前，必须在你的服务配置门户上设置新主题作品。  你可以在 [Xbox Live 服务配置](../xbox-live-service-configuration.md)中了解有关服务配置的详细信息。

本文将介绍在 Windows 开发人员中心上配置主题作品、创建新项目以及完成 Xbox Live 测试准备所需的所有内容。 本文作如下假设：

1. 你正在使用 Xbox Live 创意者计划。
2. 你正在开发通用 Windows 平台 (UWP) 主题作品。  UWP 主题作品可以在 Xbox One、Windows 10 台式电脑和移动设备上运行
3. 要在 Windows 开发人员中心上配置你的游戏[http://dev.windows.com/](http://dev.windows.com)。  如有疑问，应使用 Windows 开发人员中心。
4. 开发计算机运行的是 Windows 10。

> [!NOTE]
> 如果你是 Xbox Live 创意者计划的一员，则上述假设适用你，并应遵循本文中的内容

## <a name="dev-center-setup"></a>开发人员中心设置

你需要在 [Windows 开发人员中心](http://dev.windows.com)上创建一个支持 Xbox Live 的主题作品，作为使用所有 Xbox Live 功能的先决条件。

### <a name="create-a-microsoft-account"></a>创建 Microsoft 帐户
如果你没有 Microsoft 帐户（也称为 MSA），则需要先在 [Microsoft 帐户 - 登录](https://go.microsoft.com/fwlink/p/?LinkID=254486) 上创建一个帐户。 如果你有 Office 365 帐户、使用 Outlook.com 或拥有 Xbox Live 帐户，则你可能已经有 MSA 了。

### <a name="register-as-an-app-developer"></a>注册成为应用开发人员。
你需要先注册成为应用开发人员，然后才可以在开发人员中心创建新主题作品。

要进行注册，请转到[注册为应用开发人员](https://developer.microsoft.com/store/register)，并按照注册流程完成操作。

### <a name="create-a-new-uwp-title"></a>创建新的 UWP 主题作品
你将需要一个在开发人员中心上定义了的 UWP 主题作品。 为此，先转到 [Windows 开发人员中心仪表板](https://developer.microsoft.com/dashboard/)。

然后，创建一个新主题作品。 你需要保留一个名称。

![](../images/getting_started/first_xbltitle_newapp.png)

屏幕截图显示了进行 Xbox Live 配置的主要页面，可通过**服务** > **Xbox Live** 菜单访问该页面。

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>启用 Xbox Live 服务
为某个产品首次单击**服务**下的 **Xbox Live** 链接时，你将会转到“启用 Xbox Live 创意者计划”页。  然后单击**启用**按钮以显示“Xbox Live 设置”对话框。

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

在设置对话框中，选择要启用 Xbox Live 服务的平台（默认情况下，选择 Xbox One 和 Windows 10 电脑）。  单击**确认**按钮以为游戏启用 Xbox Live 创意者计划。

> [!IMPORTANT]
> Xbox Live 仅支持游戏。 为了通过 Xbox Live 成功发布游戏，你必须在提交的属性部分将产品类型设置为“游戏”。

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>测试游戏中的 Xbox Live 服务配置
在对游戏的 Xbox Live 配置进行更改时，你需要先将更改发布到一个特定环境，然后再由其余 Xbox Live 进行选择，并在游戏中看到。

如果你的游戏还处于调试阶段，则可以将其发布到你自己的开发沙盒。  利用开发沙盒，可让你在隔离环境中运行对游戏的更改。

当游戏发布给公众后，Xbox Live 配置将自动发布到 RETAIL 沙盒。

默认情况下，Xbox One 主机和 Windows 10 电脑均位于 RETAIL 沙盒中。

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>将 Xbox Live 配置发布到测试环境

每当启用 Xbox Live 服务并对 Xbox Live 服务配置进行更改时，为使更改生效，你需要将这些更改发布到开发沙盒。

在 Xbox Live 配置页中，单击**测试**按钮，将当前 Xbox Live 配置发布到开发沙盒。

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>授权设备和用户访问开发沙盒

仅授权的设备和用户可以访问开发沙盒中游戏的 Xbox Live 配置。

默认情况下，添加到开发人员中心帐户中的所有 Xbox One 开发主机都有权访问你的开发沙盒。  若要添加 Xbox One 主机，请转到[管理 Xbox One 主机](https://partner.microsoft.com/XboxDevices)。 如果你已在开发人员中心帐户中，可以转到**帐户设置** > **帐户设置** > **开发设备** > **Xbox One 开发主机**。

此外，你还可以授权常规 Xbox Live 帐户，使其有权访问你的开发沙盒。  若要授权 Xbox Live 帐户访问你的开发沙盒，请转到[管理帐户](https://developer.microsoft.com/xboxtestaccounts/configurecreators)。

## <a name="next-steps"></a>后续步骤
至此，你已经创建了一个新主题作品，接下来可以在游戏引擎、Visual Studio 或选择的生成环境中设置支持 Xbox Live 的主题作品。

请参阅[集成 Xbox Live 的分步指南](creators-step-by-step-guide.md)
