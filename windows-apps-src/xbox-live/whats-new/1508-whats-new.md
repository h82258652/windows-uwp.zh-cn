---
title: Xbox Live SDK 的新增功能 - 2015 年 8 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2015 年 8 月
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eca9ac96aa132c395451dfd254fbf2bf274315aa
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6260830"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>Xbox Live SDK 的新增功能 - 2015 年 8 月

请参阅[新增功能 - 2015 年 6 月](1506-whats-new.md)文章了解 2015 年 6 月版本中增加了哪些功能。

Xbox Live SDK 的 8 月版本包括下列更新：

## <a name="os-and-tool-support"></a>操作系统和工具支持
Xbox Live SDK 现在支持 Windows 10 RTM [版本 10.0.10240] 和 Visual Studio 2015 RTM [版本 14.0.23107.0]。

## <a name="multiplayer-manager-winrt-apis"></a>多人游戏管理器 WinRT API
多人游戏管理器（在实验命名空间内）现在支持 WinRT API（除 C++ API 外）

## <a name="submit-batch-feedback-from-a-title"></a>从游戏中提交批量反馈
从游戏中一次性提交大量反馈项目。

## <a name="newupdated-documentation"></a>新/更新文档
Xbox Live SDK 包现在包括“文档”文件夹，包含更新的 API 参考和新“Xbox Live 编程指南”。

Bug 修复：

* 在实时活动服务中删除订阅时崩溃
* 使用来宾帐户登录时崩溃
* 拔出网络电缆时出现访问冲突
* 通道失败现在在 C++ API 中提供错误代码
* TitleStorageService::DownloadBlobAsync 的 ETag 问题
* 修复了示例应用的各个 Bug。
