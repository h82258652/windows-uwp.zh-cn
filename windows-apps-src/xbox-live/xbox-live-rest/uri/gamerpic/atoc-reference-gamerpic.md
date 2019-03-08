---
title: 玩家头像 URI
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " 玩家头像 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618382"
---
# <a name="gamerpic-uris"></a>玩家头像 URI
 
本部分提供有关 Gamerpic 通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务中针对*gamerpics*。
 
这些 Uri 的域是`gamerpics.xboxlive.com`。
 
经过精心设计 Gamerpic 服务为用户提供更多个性化设置选项，通过授予能够允许用户生成的游戏字符 gamerpic 的标题 （在此方案中的游戏字符是指在游戏 protagonist; 它可能是一个人一辆汽车、 一个活动的太空船或用户控件的标题中的任何其他实体)。
 
生成标题 gamerpic 的基本流程如下所示：
 
   * 标题为用户提供创建游戏中字符的映像的功能。 
     * 如果没有，标题然后消息，它们没有相应权限的用户。
     * 如果用户有权限，用户可以继续创建其字符 gamerpic。
  
   * 用户创建图像和标题将 1080 x 1080.png 文件发送到 gamerpic 服务。
   * 该服务中存储图像，并为用户的新 gamerpic 设置的图像。
   * 调用用户的 gamerpic 任何体验将获得更新的映像。
  
设置标题 gamerpic 的能力由仅限强制的特权 (211) 控制。 如果强制撤消权限，用户将无法从保存标题 gamerpic，并且服务将返回 403。 标题应调用 CheckPrivilege 来验证允许用户共享内容 (priv 211)。
 
目前，若要使用此服务，你的标题必须是已列入允许列表。 若要请求审批，电子邮件`slsgamerpics@microsoft.com`。
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;访问 1080 x 1080 gamerpic。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   