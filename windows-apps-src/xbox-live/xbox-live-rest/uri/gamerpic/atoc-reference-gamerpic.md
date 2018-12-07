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
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794347"
---
# <a name="gamerpic-uris"></a>玩家头像 URI
 
本部分提供有关的玩家图片通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务的*图片*。
 
这些 Uri 的域是`gamerpics.xboxlive.com`。
 
玩家图片服务旨在通过授予的能力，以允许用户生成的其游戏的字符的玩家图片的标题为用户提供更多个性化选项，（在此方案中的游戏字符指的是游戏内 protagonist; 它可以是一个人汽车、 宇宙飞船或用户控件在游戏中的任何其他实体)。
 
生成的游戏玩家图片的基本流程如下所示：
 
   * 标题为用户提供与能够创建其游戏内字符的图像。 
     * 如果没有，游戏可以然后消息它们没有相应的权限的用户。
     * 如果用户具有权限，用户可以继续创建其字符玩家图片。
  
   * 用户创建图像和标题向玩家图片服务发送 1080 x 1080.png 文件。
   * 该服务将存储该图像并设置为用户的新玩家图片的图像。
   * 为用户的玩家图片调用任何体验将获取更新的图像。
  
设置游戏玩家图片的功能仅强制执行特权 (211) 由控制。 如果强制执行吊销权限，将阻止用户保存的游戏的玩家图片，并且该服务将返回 403。 游戏应调用 CheckPrivilege 以验证允许用户要共享的内容 （专用 211）。
 
目前，才能使用此服务，你的游戏必须列入白名单。 若要请求审批，发送电子邮件至`slsgamerpics@microsoft.com`。
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;访问 1080 x 1080 玩家图片。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   