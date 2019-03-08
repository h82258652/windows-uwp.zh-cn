---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP 应用流式安装
description: 借助通用 Windows 平台 (UWP) 应用流式安装，可指定希望 Microsoft Store 首先下载应用的哪些部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, UWP, 流式安装, UWP 应用流式安装
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608652"
---
# <a name="uwp-app-streaming-install"></a>UWP 应用流式安装
借助通用 Windows 平台 (UWP) 应用流式安装，可指定希望 Microsoft Store 首先下载应用的哪些部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。 

要使用 UWP 应用流式安装，需将应用的文件分成几个部分。 为此，需要创建一个内容组映射，该映射是一个与应用打包在一起的 XML 文件，使用它可设置下载优先级和顺序。 有关详细信息，请参见以下链接的主题。

有关将 UWP 应用流式安装添加到 UWP 应用的完整指南，请查看此[博客系列](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。

| 主题 | 描述 | 
|-------|-------------|
| [创建并将转换源内容组映射](create-cgm.md) | 为让通用 Windows 平台 (UWP) 应用做好添加 UWP 应用流式安装的准备，需要创建内容组映射。 本文介绍有关创建和转换内容组映射的具体信息，同时提供一些相关提示和技巧。 |