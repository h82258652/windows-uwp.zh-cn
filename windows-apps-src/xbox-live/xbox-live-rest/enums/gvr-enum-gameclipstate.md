---
title: GameClipState 枚举
assetID: 97fe5c1e-f7b5-537e-69eb-8284b69cd3e1
permalink: en-us/docs/xboxlive/rest/gvr-enum-gameclipstate.html
description: " GameClipState 枚举"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f7b20224eeab1b98c7c80f0e4b551420b5a15e7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630712"
---
# <a name="gameclipstate-enumeration"></a>GameClipState 枚举
详细介绍 GameClipState 枚举。 
<a id="ID4ET"></a>

 
## <a name="gameclipstate"></a>GameClipState
 
| <b>枚举器</b>| <b>描述</b>| 
| --- | --- | 
| 无 | 游戏剪辑服务状态为未知或未设置。| 
| PendingUpload | 游戏剪辑服务正在等待资产上传。| 
| PendingDelete | 游戏剪辑是为要删除队列中。 （实际上意味着它"已删除"）。| 
| 处理 | 游戏的剪辑已完成所有处理。| 
| 正在处理| 正在处理游戏剪辑 （编码，缩略图，等等。）。| 
| Publishing| 正在发布游戏剪辑资产。| 
| Published| 发布游戏剪辑资产-此状态表明它已准备好查看。| 
| 标记| 游戏的剪辑已被标记为强制。| 
| 已禁止| 已禁止的游戏的剪辑，但尚未删除。| 
| Uploaded| 游戏的剪辑已完成上传。| 
| 已删除| 游戏的剪辑已被删除。| 
| 错误| 游戏剪辑处于错误状态并且不可用。| 
  