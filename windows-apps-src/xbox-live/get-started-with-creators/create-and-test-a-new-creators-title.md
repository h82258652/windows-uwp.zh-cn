---
title: Create and test a new Creators title
author: KevinAsgari
description: Learn how to create a new Xbox Live Creators Program title and publish to the test environment.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one, creators, test
ms.openlocfilehash: 0ba9b34ed229b9c7f59ef78711126980540059d7
ms.sourcegitcommit: b185729f0bb73569740d03be67dff9f59a4c4968
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2017
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>Create a new Xbox Live Creators Program title and publish to the test environment

## <a name="introduction"></a>Introduction

Before writing any code, you must setup a new title on your service configuration portal.  You can learn more about service configuration in [Xbox Live Service Configuration](../xbox-live-service-configuration.md)

本文将介绍在 Windows 开发人员中心上配置主题作品、创建新项目以及完成 Xbox Live 测试准备所需的所有内容。 本文作如下假设：

1. 你正在使用 Xbox Live 创意者计划。
2. You are developing a Universal Windows Platform (UWP) title.  UWP titles can run on Xbox One, Windows 10 desktop PCs, and mobile
3. You are configuring your title on Windows Dev Center at [http://dev.windows.com/](http://dev.windows.com).  If in doubt, you should use Windows Dev Center.
4. 开发计算机运行的是 Windows 10。

> [!NOTE]
> 如果你是 Xbox Live 创意者计划的一员，则上述假设适用你，并应遵循本文中的内容

## <a name="dev-center-setup"></a>Dev Center setup

You need an Xbox Live enabled title created on [Windows Dev Center](http://dev.windows.com) as a pre-requisite to any Xbox Live functionality working.

### <a name="create-a-microsoft-account"></a>Create a Microsoft account
如果你没有 Microsoft 帐户（也称为 MSA），则需要先在 [Microsoft 帐户 - 登录](https://go.microsoft.com/fwlink/p/?LinkID=254486) 上创建一个帐户。 If you have an Office 365 account, use Outlook.com, or have an Xbox Live account - you probably already have an MSA.

### <a name="register-as-an-app-developer"></a>Register as an App Developer
你需要先注册成为应用开发人员，然后才可以在开发人员中心创建新主题作品。

要进行注册，请转到[注册为应用开发人员](https://developer.microsoft.com/store/register)，并按照注册流程完成操作。

### <a name="create-a-new-uwp-title"></a>Create a new UWP title
你将需要一个在开发人员中心上定义了的 UWP 主题作品。 为此，先转到 [Windows 开发人员中心仪表板](https://developer.microsoft.com/dashboard/)。

然后，创建一个新主题作品。 You'll need to reserve a name.

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
When you make changes to the Xbox Live configuration for your game, you need to publish the changes to a specific environment before they are picked up by the rest of Xbox Live and can be seen by your game.

When you are still working on your game, you publish to your own development sandbox.  利用开发沙盒，可让你在隔离环境中运行对游戏的更改。

当游戏发布给公众后，Xbox Live 配置将自动发布到 RETAIL 沙盒。

默认情况下，Xbox One 主机和 Windows 10 电脑均位于 RETAIL 沙盒中。

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>Authorize devices and users for the development sandbox

Only authorized devices and users can access the Xbox Live configuration for the game in your development sandbox.

默认情况下，添加到开发人员中心帐户中的所有 Xbox One 开发主机都有权访问你的开发沙盒。  若要添加 Xbox One 主机，请转到[管理 Xbox One 主机](https://developer.microsoft.com/XboxDevices)。 如果你已在开发人员中心帐户中，可以转到**帐户设置** > **帐户设置** > **开发设备** > **Xbox One 开发主机**。

此外，你还可以授权常规 Xbox Live 帐户，使其有权访问你的开发沙盒。  若要授权 Xbox Live 帐户访问你的开发沙盒，请转到[管理帐户](https://developer.microsoft.com/xboxtestaccounts/configurecreators)。

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>将 Xbox Live 配置发布到测试环境

Whenever you enable Xbox Live services and make changes to Xbox Live service configuration, to make the changes effective, you need to publish these changes to your development sandbox.

在 Xbox Live 配置页中，单击**测试**按钮，将当前 Xbox Live 配置发布到开发沙盒。

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

## <a name="next-steps"></a>后续步骤
Now that you have a new title created, you can now setup an Xbox Live enabled title in your Game Engine, Visual Studio or build environment of choice.

See [Step by step guide to integrate Xbox Live](creators-step-by-step-guide.md)
