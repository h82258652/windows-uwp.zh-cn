---
title: /handles/ {handleId} / 会话
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
author: KevinAsgari
description: " /handles/ {handleId} / 会话"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eb7ca17500f571ed72cf0bcd6ececbcde17ce717
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3935265"
---
# <a name="handleshandleidsession"></a>/handles/ {handleId} / 会话
支持 PUT 和获取对会话的操作，使用句柄取消引用。 

> [!NOTE] 
> 此 URI 由 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在使用模板合约 104/105 或更高版本。  

 

> [!NOTE] 
> 此 URI 目前仅从外部访问 Xbox One 主机和使用服务标识符的服务器。  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| 会话句柄的唯一 ID。| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>有效的方法

[获取 (/handles/ {handleId} / 会话)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;获取指定的句柄标识符会话对象。 

[放入 (/handles/ {句柄 id} / 会话)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;创建或更新会话由取消引用句柄。
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[会话目录 Uri](atoc-reference-sessiondirectory.md)

   