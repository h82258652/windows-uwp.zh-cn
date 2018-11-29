---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP 应用流式安装
description: 借助通用 Windows 平台 (UWP) 应用流式安装，可指定希望 Microsoft Store 首先下载应用的哪些部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10，uwp，流式处理安装，uwp 应用流式安装
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7982590"
---
# <a name="uwp-app-streaming-install"></a>UWP 应用流式安装
借助通用 Windows 平台 (UWP) 应用流式安装，可指定希望 Microsoft Store 首先下载应用的哪些部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。 

若要使用 UWP 应用流式安装将需要划分为区域的应用的文件。 若要执行此操作，将创建内容组映射，这是与你的应用打包在一起的 XML 文件，允许你设置下载优先级和顺序。 请参阅下面链接的详细信息的主题。

有关添加到你的 UWP 应用的 UWP 应用流式安装的完整指南，请查看此[博客系列](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。

| 主题 | 描述 | 
|-------|-------------|
| [创建和转换源内容组映射](create-cgm.md) | 为让通用 Windows 平台 (UWP) 应用做好添加 UWP 应用流式安装的准备，需要创建内容组映射。 本文介绍有关创建和转换内容组映射的具体信息，同时提供一些相关提示和技巧。 |