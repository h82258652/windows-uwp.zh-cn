---
title: 使用 XDK 内置的 Xbox Live API
description: 了解如何在 Xbox 开发人员工具包 (XDK) 项目中使用内置的 Xbox Live API。
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8324156"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>使用 XDK 内置的 Xbox Live API

1. 在 Visual Studio 中右键单击项目，然后选择“引用”。
1. 选择“添加新引用”
1. 单击“Durango.<build number>” 和左侧面板上的“扩展”
1. 在中间选择两者之一：
- 如果想使用 WinRT XSAPI API，则选择“Xbox Services API”
- 如果想使用 C++ XSAPI API，则选择“Xbox Services API Cpp”
1. 单击“确定”

注意：如果你的版本系统不支持属性文件，则必须手动添加预处理器定义和库，如
`%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`
