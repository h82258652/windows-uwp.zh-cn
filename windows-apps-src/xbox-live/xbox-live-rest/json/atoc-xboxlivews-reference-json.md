---
title: JavaScript 对象表示法 (JSON) 对象参考
assetID: 8efcc6f3-d88a-1b15-bcfc-d79a24581b0a
permalink: en-us/docs/xboxlive/rest/atoc-xboxlivews-reference-json.html
description: " JavaScript 对象表示法 (JSON) 对象参考"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c46557e3fb837bebccbb1039fb416f3e9787af2a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626552"
---
# <a name="javascript-object-notation-json-object-reference"></a>JavaScript 对象表示法 (JSON) 对象参考
 
JavaScript 对象表示法 (JSON) 是轻量、 基于标准的、 面向对象的表示法中，用于封装在 web 上找到的数据。
 
Xbox Live 服务定义，请求和响应从服务中使用的 JSON 对象。 本部分提供有关与 Xbox Live 服务一起使用的每个 JSON 对象的参考信息。
 
<a id="ID4EHB"></a>

 
## <a name="in-this-section"></a>本部分内容

[成就 (JSON)](json-achievementv2.md)

&nbsp;&nbsp;成就对象 （第 2 版）。

[ActivityRecord (JSON)](json-activityrecord.md)

&nbsp;&nbsp;有关一个或多个用户的丰富的状态显示一个格式和本地化的字符串。

[ActivityRequest (JSON)](json-activityrequest.md)

&nbsp;&nbsp;有关一个或多个用户的丰富的状态显示信息请求。

[AggregateSessionsResponse (JSON)](json-aggregatesessionsresponse.md)

&nbsp;&nbsp;包含聚合的数据的用户的适用性会话。

[BatchRequest (JSON)](json-batchrequest.md)

&nbsp;&nbsp;要用于筛选状态显示信息，例如用户、 设备和标题的属性的数组。

[DeviceEndpoint (JSON)](json-deviceendpoint.md)

[DeviceRecord (JSON)](json-devicerecord.md)

&nbsp;&nbsp;有关设备，包括其类型和在其上的标题的信息。

[Feedback (JSON)](json-feedback.md)

&nbsp;&nbsp;包含有关播放器的反馈信息。

[GameClip (JSON)](json-gameclip.md)

[GameClipsServiceErrorResponse (JSON)](json-gameclipsserviceerrorresponse.md)

&nbsp;&nbsp;对 /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} 的响应的可选部分/uri/格式 / {gameClipUriType} API。

[GameClipThumbnail (JSON)](json-gameclipthumbnail.md)

&nbsp;&nbsp;包含与单个缩略图相关的信息。 可以有多种大小，每个剪辑，并且由客户端选择了正确的租户进行显示。

[GameClipUri (JSON)](json-gameclipuri.md)

[GameMessage (JSON)](json-gamemessage.md)

&nbsp;&nbsp;JSON 对象，游戏会话的消息队列中定义的消息的数据。

[GameResult (JSON)](json-gameresult.md)

&nbsp;&nbsp;表示描述游戏会话结果的数据的 JSON 对象。

[GameSession (JSON)](json-gamesession.md)

&nbsp;&nbsp;多玩家会话表示游戏数据的 JSON 对象。

[GameSessionSummary (JSON)](json-gamesessionsummary.md)

&nbsp;&nbsp;表示为游戏会话的摘要数据的 JSON 对象。

[GetClipResponse (JSON)](json-getclipresponse.md)

&nbsp;&nbsp;包装的游戏的剪辑。

[HopperStatsResults (JSON)](json-hopperstatsresults.md)

&nbsp;&nbsp;表示 hopper 的统计信息的 JSON 对象。

[InitialUploadRequest (JSON)](json-initialuploadrequest.md)

&nbsp;&nbsp;开机自检 GameClip 正文上载请求。

[InitialUploadResponse (JSON)](json-initialuploadresponse.md)

[inventoryItem (JSON)](json-inventoryitem.md)

&nbsp;&nbsp;核心清单项表示了授权可以授予标准项。

[LastSeenRecord (JSON)](json-lastseenrecord.md)

&nbsp;&nbsp;有关系统上次看到用户，在用户有无有效 DeviceRecord 时可用的信息。

[MatchTicket (JSON)](json-matchticket.md)

&nbsp;&nbsp;表示匹配票证，播放机用来定位其他玩家通过多人游戏会话目录 (MPSD) 的 JSON 对象。

[MediaAsset (JSON)](json-mediaasset.md)

&nbsp;&nbsp;与成就或其奖励相关联的媒体资产。

[MediaRecord (JSON)](json-mediarecord.md)

[MediaRequest (JSON)](json-mediarequest.md)

[MultiplayerActivityDetails (JSON)](json-multiplayeractivitydetails.md)

&nbsp;&nbsp;一个 JSON 对象，表示**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**。

[MultiplayerSessionReference (JSON)](json-multiplayersessionreference.md)

&nbsp;&nbsp;一个 JSON 对象，表示**MultiplayerSessionReference**。 

[MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md)

&nbsp;&nbsp;上的操作请求 JSON 对象传递**MultiplayerSession**对象。

[MultiplayerSession (JSON)](json-multiplayersession.md)

&nbsp;&nbsp;一个 JSON 对象，表示**MultiplayerSession**。 

[PagingInfo (JSON)](json-paginginfo.md)

&nbsp;&nbsp;包含分页信息的数据的页面中返回的结果。

[PeopleList (JSON)](json-peoplelist.md)

&nbsp;&nbsp;集合[人员](json-person.md)对象。

[PermissionCheckBatchRequest (JSON)](json-permissioncheckbatchrequest.md)

&nbsp;&nbsp;PermissionCheckBatchRequest 对象的集合。

[PermissionCheckBatchResponse (JSON)](json-permissioncheckbatchresponse.md)

&nbsp;&nbsp;结果的批处理权限检查的多个用户的权限值的列表。

[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)

&nbsp;&nbsp;批处理权限的原因检查单个目标用户的权限值的列表。

[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)

&nbsp;&nbsp;从单个用户针对单个目标用户的单个权限设置的检查结果。

[PermissionCheckResult (JSON)](json-permissioncheckresult.md)

&nbsp;&nbsp;从单个用户针对单个目标用户的单个权限设置的检查结果。

[人员 (JSON)](json-person.md)

&nbsp;&nbsp;用户系统中的单个用户相关的元数据。

[PersonSummary (JSON)](json-personsummary.md)

&nbsp;&nbsp;集合[Person (JSON)](json-person.md)对象。

[播放机 (JSON)](json-player.md)

&nbsp;&nbsp;包含玩家在游戏会话中数据。

[PresenceRecord (JSON)](json-presencerecord.md)

&nbsp;&nbsp;有关单个用户的联机状态的数据。

[配置文件 (JSON)](json-profile.md)

&nbsp;&nbsp;用户个人配置文件设置。

[进度 (JSON)](json-progression.md)

&nbsp;&nbsp;用户的解锁成就的进度。

[属性 (JSON)](json-property.md)

&nbsp;&nbsp;包含客户端提供的匹配请求条件的属性数据。

[QueryClipsResponse (JSON)](json-queryclipsresponse.md)

&nbsp;&nbsp;包装返回的游戏剪辑，以及分页信息的列表的列表。

[quotaInfo (JSON)](json-quota.md)

&nbsp;&nbsp;包含有关标题组的配额信息。

[要求 (JSON)](json-requirement.md)

&nbsp;&nbsp;解锁成就和距离用户是朝向满足这些条件。

[ResetReputation (JSON)](json-resetreputation.md)

&nbsp;&nbsp;包含用户的现有分数应更改到新的基本信誉分数。

[奖励 (JSON)](json-reward.md)

&nbsp;&nbsp;这种成就与相关联的回报。

[RichPresenceRequest (JSON)](json-richpresencerequest.md)

&nbsp;&nbsp;丰富的状态显示信息应使用其中的相关信息的请求。

[ServiceError (JSON)](json-serviceerror.md)

&nbsp;&nbsp;包含有关对服务调用失败时返回错误的信息。

[ServiceErrorResponse (JSON)](json-serviceerrorresponse.md)

&nbsp;&nbsp;当遇到了服务错误时，将返回相应的 HTTP 错误代码。 此外，该服务可能还包括 ServiceErrorResponse 对象，定义如下。 在生产环境中，可以包含较少的数据。

[SessionEntry (JSON)](json-sessionentry.md)

&nbsp;&nbsp;包含适用性会话数据。

[TitleAssociation (JSON)](json-titleassociation.md)

&nbsp;&nbsp;一个与实现相关联的标题。

[TitleBlob (JSON)](json-titleblob.md)

&nbsp;&nbsp;包含有关从存储的标题信息。

[TitleRecord (JSON)](json-titlerecord.md)

&nbsp;&nbsp;有关标题，包括其名称和上次修改时间戳信息。

[TitleRequest (JSON)](json-titlerequest.md)

&nbsp;&nbsp;有关标题信息的请求。

[UpdateMetadataRequest (JSON)](json-updatemetadatarequest.md)

&nbsp;&nbsp;应为一个剪辑更新元数据。

[用户 (JSON)](json-user.md)

&nbsp;&nbsp;包含用户排行榜数据。

[UserClaims (JSON)](json-userclaims.md)

&nbsp;&nbsp;返回有关当前经过身份验证用户的信息。

[UserList (JSON)](json-userlist.md)

&nbsp;&nbsp;一系列[用户](json-user.md)对象。

[UserSettings (JSON)](json-usersettings.md)

&nbsp;&nbsp;返回当前经过身份验证的用户的设置。

[UserTitle (JSON)](json-usertitlev2.md)

&nbsp;&nbsp;包含用户标题数据。

[VerifyStringResult (JSON)](json-verifystringresult.md)

&nbsp;&nbsp;导致代码与提交到每个字符串对应[/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)。

[XuidList (JSON)](json-xuidlist.md)

&nbsp;&nbsp;要对其执行操作的 XUIDs 列表。
 
<a id="ID4ENH"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EPH"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Xbox Live 服务 RESTful 参考](../atoc-xboxlivews-reference.md)

  
<a id="ID4EZH"></a>

 
##### <a name="external-links-ecma-international-standard-262-ecmascript-language-specificationhttpswwwecma-internationalorgpublicationsfilesecma-stecma-262pdf"></a>外部链接[ECMA 国际标准 262:ECMAScript 语言规范](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-262.pdf)

   