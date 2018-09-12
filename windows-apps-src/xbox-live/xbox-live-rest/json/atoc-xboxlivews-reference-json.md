---
title: JavaScript 对象表示法 (JSON) 对象引用
assetID: 8efcc6f3-d88a-1b15-bcfc-d79a24581b0a
permalink: en-us/docs/xboxlive/rest/atoc-xboxlivews-reference-json.html
author: KevinAsgari
description: " JavaScript 对象表示法 (JSON) 对象引用"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e65936d20923ecbdc2d9cfb0a0ec52bb7504b885
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880877"
---
# <a name="javascript-object-notation-json-object-reference"></a>JavaScript 对象表示法 (JSON) 对象引用
 
JavaScript 对象表示法 (JSON) 是轻量、 基于标准的、 面向对象的表示法封装 web 上的数据。
 
Xbox Live 服务定义的请求和响应从服务中使用的 JSON 对象。 本部分提供有关每个使用 Xbox Live 服务的 JSON 对象的参考信息。
 
<a id="ID4EHB"></a>

 
## <a name="in-this-section"></a>本部分内容

[成就 (JSON)](json-achievementv2.md)

&nbsp;&nbsp;成就对象 （版本 2）。

[ActivityRecord (JSON)](json-activityrecord.md)

&nbsp;&nbsp;有关一个或多个用户的完整状态格式化和本地化字符串。

[ActivityRequest (JSON)](json-activityrequest.md)

&nbsp;&nbsp;有关一个或多个用户的完整状态信息请求。

[AggregateSessionsResponse (JSON)](json-aggregatesessionsresponse.md)

&nbsp;&nbsp;包含聚合的数据的用户的适用性会话。

[BatchRequest (JSON)](json-batchrequest.md)

&nbsp;&nbsp;一个数组，用于筛选状态信息，如用户、 设备和游戏的属性。

[DeviceEndpoint (JSON)](json-deviceendpoint.md)

[DeviceRecord (JSON)](json-devicerecord.md)

&nbsp;&nbsp;有关设备，包括其类型和游戏在其上的信息。

[反馈 (JSON)](json-feedback.md)

&nbsp;&nbsp;包含有关玩家的反馈信息。

[GameClip (JSON)](json-gameclip.md)

[GameClipsServiceErrorResponse (JSON)](json-gameclipsserviceerrorresponse.md)

&nbsp;&nbsp;/Users/ {ownerId} {scid} /scids/ /clips/ {gameClipId} 响应的可选部分/uri/格式 / {gameClipUriType} API。

[GameClipThumbnail (JSON)](json-gameclipthumbnail.md)

&nbsp;&nbsp;包含与单个缩略图相关的信息。 可以有多个大小每个剪辑，并由客户端选择正确显示。

[GameClipUri (JSON)](json-gameclipuri.md)

[GameMessage (JSON)](json-gamemessage.md)

&nbsp;&nbsp;一个游戏会话的消息队列中定义为一条消息的数据的 JSON 对象。

[GameResult (JSON)](json-gameresult.md)

&nbsp;&nbsp;表示数据，以描述游戏会话的结果的 JSON 对象。

[GameSession (JSON)](json-gamesession.md)

&nbsp;&nbsp;多人游戏会话表示游戏数据的 JSON 对象。

[GameSessionSummary (JSON)](json-gamesessionsummary.md)

&nbsp;&nbsp;游戏会话表示摘要数据的 JSON 对象。

[GetClipResponse (JSON)](json-getclipresponse.md)

&nbsp;&nbsp;包装游戏剪辑。

[HopperStatsResults (JSON)](json-hopperstatsresults.md)

&nbsp;&nbsp;表示漏斗的统计信息的 JSON 对象。

[InitialUploadRequest (JSON)](json-initialuploadrequest.md)

&nbsp;&nbsp;POST GameClip 正文上传请求。

[InitialUploadResponse (JSON)](json-initialuploadresponse.md)

[inventoryItem (JSON)](json-inventoryitem.md)

&nbsp;&nbsp;核心清单项表示可授予权利的标准项。

[LastSeenRecord (JSON)](json-lastseenrecord.md)

&nbsp;&nbsp;有关系统上次看到用户，当用户在没有有效 DeviceRecord 可用的信息。

[MatchTicket (JSON)](json-matchticket.md)

&nbsp;&nbsp;表示玩家用于查找其他玩家通过多人游戏会话目录 (MPSD) 的匹配票证的 JSON 对象。

[MediaAsset (JSON)](json-mediaasset.md)

&nbsp;&nbsp;与成就或其奖励媒体资产。

[MediaRecord (JSON)](json-mediarecord.md)

[MediaRequest (JSON)](json-mediarequest.md)

[MultiplayerActivityDetails (JSON)](json-multiplayeractivitydetails.md)

&nbsp;&nbsp;表示**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**的 JSON 对象。

[MultiplayerSessionReference (JSON)](json-multiplayersessionreference.md)

&nbsp;&nbsp;表示**MultiplayerSessionReference**的 JSON 对象。 

[MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md)

&nbsp;&nbsp;请求 JSON 对象传递**MultiplayerSession**对象上的操作。

[MultiplayerSession (JSON)](json-multiplayersession.md)

&nbsp;&nbsp;表示**MultiplayerSession**的 JSON 对象。 

[PagingInfo (JSON)](json-paginginfo.md)

&nbsp;&nbsp;包含数据页中返回的结果的页面信息。

[PeopleList (JSON)](json-peoplelist.md)

&nbsp;&nbsp;[用户](json-person.md)对象的集合。

[PermissionCheckBatchRequest (JSON)](json-permissioncheckbatchrequest.md)

&nbsp;&nbsp;PermissionCheckBatchRequest 对象的集合。

[PermissionCheckBatchResponse (JSON)](json-permissioncheckbatchresponse.md)

&nbsp;&nbsp;批处理权限的结果检查的多个用户的权限值列表。

[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)

&nbsp;&nbsp;批处理权限的原因检查的一个目标用户的权限值的列表。

[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)

&nbsp;&nbsp;从单个权限设置针对单个目标用户的单个用户检查的结果。

[PermissionCheckResult (JSON)](json-permissioncheckresult.md)

&nbsp;&nbsp;从单个权限设置针对单个目标用户的单个用户检查的结果。

[人 (JSON)](json-person.md)

&nbsp;&nbsp;人脉系统中的单个用户相关的元数据。

[PersonSummary (JSON)](json-personsummary.md)

&nbsp;&nbsp;[人 (JSON)](json-person.md)对象的集合。

[玩家 (JSON)](json-player.md)

&nbsp;&nbsp;在游戏会话包含玩家数据。

[Presencerecord，他 (JSON)](json-presencerecord.md)

&nbsp;&nbsp;联机状态相关的单个用户的数据。

[配置文件 (JSON)](json-profile.md)

&nbsp;&nbsp;用户的个人配置文件设置。

[进度 (JSON)](json-progression.md)

&nbsp;&nbsp;用户的距离解锁成就的进度。

[属性 (JSON)](json-property.md)

&nbsp;&nbsp;包含匹配请求条件为提供的客户端的属性数据。

[QueryClipsResponse (JSON)](json-queryclipsresponse.md)

&nbsp;&nbsp;包装返回的游戏剪辑，以及列表的页面信息的列表。

[quotaInfo (JSON)](json-quota.md)

&nbsp;&nbsp;包含有关标题组的配额信息。

[要求 (JSON)](json-requirement.md)

&nbsp;&nbsp;成就且距离用户向会议它们的解锁条件。

[ResetReputation (JSON)](json-resetreputation.md)

&nbsp;&nbsp;包含新的基本信誉评分应更改用户的现有评分。

[奖励 (JSON)](json-reward.md)

&nbsp;&nbsp;与成就关联的奖励。

[RichPresenceRequest (JSON)](json-richpresencerequest.md)

&nbsp;&nbsp;完整状态的信息应使用哪些信息请求。

[ServiceError (JSON)](json-serviceerror.md)

&nbsp;&nbsp;包含有关错误对服务调用失败时返回的信息。

[ServiceErrorResponse (JSON)](json-serviceerrorresponse.md)

&nbsp;&nbsp;当遇到服务错误时，将返回一个相应的 HTTP 错误代码。 （可选） 服务还可能包括 ServiceErrorResponse 对象，如下面定义。 在生产环境中，较少的数据可能会包含。

[SessionEntry (JSON)](json-sessionentry.md)

&nbsp;&nbsp;用于健身会话中包含的数据。

[TitleAssociation (JSON)](json-titleassociation.md)

&nbsp;&nbsp;与成就关联的标题。

[TitleBlob (JSON)](json-titleblob.md)

&nbsp;&nbsp;包含有关标题存储中的信息。

[TitleRecord (JSON)](json-titlerecord.md)

&nbsp;&nbsp;有关游戏，包括其名称和上次修改时间戳的信息。

[TitleRequest (JSON)](json-titlerequest.md)

&nbsp;&nbsp;有关游戏的请求。

[UpdateMetadataRequest (JSON)](json-updatemetadatarequest.md)

&nbsp;&nbsp;应更新剪辑元数据。

[用户 (JSON)](json-user.md)

&nbsp;&nbsp;包含用户排行榜数据。

[UserClaims (JSON)](json-userclaims.md)

&nbsp;&nbsp;返回与当前身份验证的用户相关信息。

[用户列表 (JSON)](json-userlist.md)

&nbsp;&nbsp;[用户](json-user.md)对象的集合。

[用户设置 (JSON)](json-usersettings.md)

&nbsp;&nbsp;返回当前身份验证的用户的设置。

[UserTitle (JSON)](json-usertitlev2.md)

&nbsp;&nbsp;包含用户的游戏数据。

[VerifyStringResult (JSON)](json-verifystringresult.md)

&nbsp;&nbsp;结果代码对应于提交到[/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)每个字符串。

[XuidList (JSON)](json-xuidlist.md)

&nbsp;&nbsp;要对其执行操作的 Xuid 列表。
 
<a id="ID4ENH"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EPH"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Xbox Live 服务 RESTful 参考](../atoc-xboxlivews-reference.md)

  
<a id="ID4EZH"></a>

 
##### <a name="external-links-ecma-international-standard-262-ecmascript-language-specificationhttpwwwecma-internationalorgpublicationsfilesecma-stecma-262pdf"></a>外部链接[ECMA 国际标准 262: ECMAScript 语言规范](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-262.pdf)

   