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
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938456"
---
# <a name="gameclipstate-enumeration"></a>GameClipState 枚举
详细介绍 GameClipState 枚举。 
<a id="ID4ET"></a>

 
## <a name="gameclipstate"></a>GameClipState
 
| <b>枚举</b>| <b>说明</b>| 
| --- | --- | 
| 无 | 游戏剪辑服务状态为未知或未设置。| 
| PendingUpload | 游戏剪辑服务正在等待资产上传。| 
| PendingDelete | 游戏剪辑是在队列中删除的。 （有效即"删除"）。| 
| 已处理 | 游戏剪辑已完成所有处理。| 
| Processing| 正在处理游戏剪辑 （编码，缩略图等。）。| 
| 发布| 正在发布游戏剪辑资产。| 
| Published| 发布游戏剪辑资产 – 此状态指示它是所有组以查看。| 
| 标记| 被用于强制标记游戏剪辑。| 
| 禁止| 已禁止游戏剪辑，但尚未删除。| 
| Uploaded| 游戏剪辑已完成上传。| 
| 删除| 游戏剪辑已被删除。| 
| 错误| 游戏剪辑处于错误状态和不可用。| 
  