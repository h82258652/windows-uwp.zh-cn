---
author: aablackm
title: 在 Xbox One 主机上进行测试
description: 了解如何测试 Xbox Live 主机上的 Xbox Live 服务
ms.author: aablackm
ms.date: 08/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，游戏，xbox，xbox live，xbox one
ms.localizationpriority: low
ms.openlocfilehash: 5500f6f396d6dae179e434283097c34274d9b829
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4352414"
---
# <a name="testing-on-the-xbox-one-console"></a>在 Xbox One 主机上进行测试

开发你的游戏为主机的 Xbox One 系列时才自然想要能够测试你的游戏与实际的主机上的 Xbox Live 功能。 有几种选项测试你的硬件上的作品。 可以使用任何零售 Xbox One 主机激活控制台的开发人员模式来测试通用 Windows 平台 (UWP) 游戏或应用。 此选项可供所有开发人员，并且是 Xbox Live 创意者计划开发人员的唯一选项。 ID@Xbox和托管的合作伙伴可以排序和使用 Xbox 开发工具包的选项。

## <a name="retail-console-testing-xbox-live-creators"></a>零售控制台测试： Xbox Live 创意者

在零售 Xbox One 主机上的激活开发人员模式将允许你通过使用 Visual Studio 生成配对，则为 Xbox One 主机部署 UWP 游戏和应用。 这是测试选项适用于 Xbox Live 创意者计划开发人员控制台。 你将无法测试在零售 Xbox One 主机上的 XDK 游戏。

* 按照的[开发人员模式激活说明](../xbox-apps/devkit-activation.md)以允许开发在零售控制台上进行测试。  
* 按照[设置 Xbox One 说明](../xbox-apps/development-environment-setup.md#setting-up-your-xbox-one)加载到 Xbox One 游戏。  
* 按照使你的主机返回到零售模式或卸载零售控制台的开发环境的[开发人员模式停用说明](../xbox-apps/devkit-deactivation.md)。  
* 你的主机处于开发人员模式时你可以通过远程访问它通过你的电脑使用[适用于 Xbox 的 Windows 设备门户](../debug-test-perf/device-portal-xbox.md)。  

## <a name="xbox-development-kit-testing-idxbox-and-managed-partners"></a>Xbox 开发工具包测试：ID@Xbox和托管合作伙伴

托管的合作伙伴和ID@Xbox开发人员可以即仅限于托管开发人员帐户可以访问[Xbox 开发人员应用商店](https://gamedevstore.partners.extranet.microsoft.com/)中购买的 Xbox 开发人员工具包的选项。 Xbox 开发工具包将允许你加载 XDK 游戏到控制台进行测试，UWP 游戏还可以使用开发人员工具包进行测试。 开发人员工具包附带硬件选项和允许更深入的性能测试和控制台管理的测试功能。

若要使用 Xbox 开发人员的旅程工具包读取[开始使用 Xbox One 开发文章](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-getting-started)托管的合作伙伴文档站点上。 本文档仅授权开发人员可以访问ID@Xbox计划和托管的合作伙伴。