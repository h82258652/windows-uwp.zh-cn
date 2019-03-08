---
title: 在 Visual Studio 中测试的 Unity 游戏
description: 在 Visual Studio 中生成的测试成功的 Unity 清单。
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589872"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>在 Visual Studio 中测试你的 Unity 游戏生成

若要测试你的 Unity 游戏使用实际数据的 Xbox Live 功能，必须构建游戏，如中所述**生成和测试项目**一部分[配置 Unity 项目中的 Xbox Live](configure-xbox-live-in-unity.md)。 以下文章将为您提供的各项以帮助确保测试成功的 Unity 游戏在 Visual Studio 中的清单。

1. **再次确认已正确配置的标题与您的 Unity 游戏相关联。**
    如果您启用了 Xbox Live 通过 Xbox Live 关联向导，您将想要自己应熟悉[合作伙伴中心](https://partner.microsoft.com/dashboard)。 合作伙伴中心，可以配置在标题上的 Xbox Live 设置，并且必须在您的标题以与 Xbox Live 通信顺序中正确的安装程序。 文章[创建一个新的 Xbox Live Creators 计划标题，并将发布到测试环境](create-and-test-a-new-creators-title.md)指导你完成合作伙伴中心安装过程。 如果您已安装程序通过您的游戏**Xbox 配置向导**在 Unity 中，则可以跳过的部分[测试 Xbox Live 游戏服务配置](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game)。 在合作伙伴中心，请务必检查以查看你的 Unity 游戏在 Xbox Live 配置中的信息与您的游戏的合作伙伴中心配置相匹配。

2. **请确保你的标题已授权的 Microsoft Account(with gamertag)，可以登录到你的标题。**
    授权帐户的情况下你将无法完成登录中测试你的标题时也能够使用 Xbox Live 的其他功能。 若要确保你已授权的 Microsoft 帐户和玩家代号读取[授权 Xbox Live 帐户为你的环境中测试](authorize-xbox-live-accounts.md)。

3. **请确保你的标题，已发布的测试。**
    进行更改时向你的标题这些更改必须发布到沙盒中才可以使用。 请确保你推送**测试**按钮以将所做的更改发布到你的配置。

    ![发布适用于测试映像](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    **测试**中找到按钮[合作伙伴中心](https://partner.microsoft.com/dashboard)下标题的 Xbox Live 服务设置。 如果你的配置如添加新的授权的测试帐户，想要推送到进行任何更改**测试**按钮以发布到 Xbox Live 服务的新的配置设置。

4. **检查以确保您的 PC 处于开发人员模式，您将使用适当的 Xbox Live 沙盒。**
    当为发布在标题上时对其进行测试发布到特定的环境，名为沙盒。 如果您的开发 PC 未设置为使用该沙盒将无法以测试 Xbox Live 功能。 了解如何检查和更改与你的电脑的沙盒[Xbox Live 沙盒简介](xbox-live-sandboxes-creators.md)。

5. **请确保生成为 x64 项目生成以在您的 PC 上进行构建在本地计算机为目标**

    ![生成设置](../images/unity/get-started-with-creators/vsBuildSettings.JPG)