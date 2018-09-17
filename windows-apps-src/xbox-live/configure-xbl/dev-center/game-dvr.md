---
title: 游戏 DVR
author: shrutimundra
description: 了解如何在 Windows 开发人员中心 2017 上配置 Xbox Live 游戏 DVR 字符串
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 游戏, uwp, windows 10, Xbox one, 游戏 DVR, Windows 开发人员中心
ms.openlocfilehash: d3d50dff4f8fc09f4c9303fa68172bd4a46eb2fa
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3989219"
---
# <a name="configuring-game-dvr-on-windows-dev-center"></a>在 Windows 开发人员中心上配置游戏 DVR

在 Xbox One 上，最受欢迎的功能之一是游戏 DVR，玩家借助它可以轻松录制、编辑和分享最精彩的游戏时刻。 游戏 DVR 字符串将显示为你的作品中所有开发人员创建的作品 DVR 剪辑的标题。 在服务中配置字符串将确保在该剪辑精选到的任何应用中显示该字符串的正确本地化版本。 例如，如果你想要在用户打败你作品中的最终 Boss 时创建一个剪辑，则需要先配置称为“Boss Battle”的字符串。 当在你的作品代码中调用它以创建该剪辑时，将引用该 ID。

你可以使用 [Windows 开发人员中心](https://developer.microsoft.com/dashboard)来配置与你的游戏关联的游戏 DVR 字符串。 通过执行以下操作添加配置：

1. 导航至作品的**游戏 DVR** 部分（位于**服务** > **Xbox Live** > **游戏 DVR** 下）。
2. 单击**创建新字符串**按钮。
3. 在弹出的模式中，输入游戏 DVR 字符串。 完成后，单击**确认**。

![新游戏 DVR 字符串对话框的图像](../../images/dev-center/game-dvr/game-dvr-1.png)
