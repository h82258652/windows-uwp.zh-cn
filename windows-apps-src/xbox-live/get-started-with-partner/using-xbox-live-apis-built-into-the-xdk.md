---
title: 使用 XDK 内置的 Xbox Live API
author: KevinAsgari
description: 了解如何在 Xbox 开发人员工具包 (XDK) 项目中使用内置的 Xbox Live API。
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 527a62af7ca31a401da47eb10e59c1593f62e433
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5467982"
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
