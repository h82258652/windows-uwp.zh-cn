---
title: 配置 Xbox 开发主机
author: KevinAsgari
description: 了解如何配置 Xbox 开发主机以支持 Xbox Live 开发。
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef85dfa85f6beaabc038a1bec1c9a66cf6ea4148
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6194818"
---
# <a name="configure-your-xbox-development-console"></a>配置 Xbox 开发主机

要配置你的开发主机：
- 获取你的 ID
- 在开发工具包上设置沙盒
- 使用开发帐户登录

## <a name="get-your-ids"></a>获取你的 ID
要启用沙盒和 Xbox Live 服务，你需要获取多个 ID 以配置开发工具包和主题作品。 可以使用相同的过程完成这些操作。

按照 [Xbox Live 服务配置](../xbox-live-service-configuration.md)获取你的 ID

## <a name="set-your-sandbox-on-your-development-kits"></a>在开发工具包上设置沙盒
不设置沙盒 ID 将无法启动开发工具包。 要执行此操作，你可以使用由 XDK 安装在电脑上的“Xbox One 管理器”，或者，可以打开 XDK 命令窗口，并使用“配置”(xbconfig.exe) 命令，如下所示：

查看当前沙盒。 在命令提示符中键入 xbconfig sandboxid。

如果不是需要的内容，请更改沙盒 ID。在命令提示符中键入“xbconfig sandboxid=<your sandbox id>”。

在命令提示符中使用 Reboot (xbreboot.exe) 重新启动主机。

验证沙盒是否已正确重置。 在命令提示符中键入 xbconfig sandboxid。

## <a name="sign-in-with-a-development-account"></a>使用开发帐户登录

你可以创建用于登录[Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts)或[合作伙伴中心](https://partner.microsoft.com/dashboard)上的开发帐户
