---
title: 多人游戏会话浏览
author: KevinAsgari
description: 了解如何通过使用 Xbox Live 多人游戏实现多人游戏会话浏览。
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.author: kevinasg
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea47c73cc882db4031f9c43ccbc64030ee039b3f
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4123583"
---
# <a name="multiplayer-session-browse"></a>多人游戏会话浏览

多人游戏会话浏览是在 2016 年 11 月引入的一项新功能，使作品能够查询满足指定条件的打开多人游戏会话列表。

## <a name="what-is-session-browse"></a>什么是会话浏览？

在会话浏览方案中，游戏玩家可以检索可加入游戏会话的列表。 此列表中的每个会话条目都包含有关游戏的一些其他元数据，玩家可以使用这些元数据帮助他们选择要加入的会话。  他们还可以根据元数据筛选会话列表。 玩家一旦看见吸引他们的游戏会话，就会加入其中。

玩家还可以创建新的游戏会话，并使用会话浏览招募其他玩家，而不是依赖于匹配。

会话浏览不同于传统匹配方案。对于前者，玩家自行选择要加入的游戏会话。而在匹配方案中，玩家通常要单击“查找游戏”按钮，尝试自动将玩家放置在相应的游戏会话中。 虽然会话浏览是一个手动过程，速度相对较慢，可能无法始终选择客观上最好的游戏，但它让玩家拥有更大的控制权，可以说是更符合主观意愿的游戏选择。

游戏中既包括会话浏览，又包括匹配方案，这是很常见的。 通常，匹配用于常玩的游戏模式，而会话浏览用于自定义游戏。

**示例：** John 可能喜欢玩角色竞技场风格的多人游戏，但又希望玩所有玩家都能随机选择其角色的游戏。 他可以检索打开的游戏会话列表，并查找描述中包括“随机角色”的游戏会话，或者，如果游戏 UI 允许的话，他可以选择“随机角色”游戏模式，并仅检索标记为“RandomHero”游戏的会话。

找到自己喜欢的游戏后，他可以立即加入其中。 当有足够多的玩家加入会话时，该游戏会话的主机将会启动游戏。

### <a name="roles"></a>角色

会话浏览中的游戏可能需要招募玩家来扮演特定角色。 例如，一个玩家可能想要创建一个游戏会话，指定该会话最多包含 5 个袭击类别，但必须至少包含 2 个治愈者角色和 1 个坦克人角色。

当另一个玩家申请加入会话时，他们可以预先选择自己的角色，如果他们所选的角色没有空位，则服务将不允许他们加入该会话。

另一个示例是，如果玩家想要保留 2 个空位供他们的好友加入，则游戏可以指定“好友”角色，而且只有会话主机上作为好友的玩家，才能填充专门为“好友”角色预留的 2 个位置。

有关角色的更多信息，请参阅[多人游戏角色](multiplayer-roles.md)。



## <a name="how-does-session-browse-work"></a>会话浏览的工作原理

会话浏览主要在使用搜索句柄时工作。 搜索句柄是数据包，其中包含对会话的引用，以及与会话相关的其他元数据，即搜索属性。

在创建符合会话浏览条件的新游戏会话后，作品将会为该会话创建搜索句柄。 搜索句柄存储在多人游戏服务目录 (MPSD) 中，该目录用于维护作品的搜索句柄。

需要检索会话列表时，作品可以将搜索查询发送到 MPSD，以返回符合搜索条件的搜索句柄列表。 然后，作品可以使用会话列表向玩家显示可加入的游戏列表。

当会话已满，或以其他方式不能加入时，作品可从 MPSD 中删除搜索句柄，以便该会话将不再显示在会话浏览查询中。

>[!NOTE]
> 搜索句柄在显示要呈现给用户的会话列表时使用。 如果可能，应尽量避免使用搜索句柄进行背景匹配，应考虑使用 [SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>为会话浏览设置会话

若要将搜索句柄用于会话，该会话必须将以下功能设置为 true：

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> 只有 UWP 游戏需要 `userAuthorizationStyle` 功能，但我们建议为所有 Xbox Live 游戏（包括 XDK 游戏）应用这些功能，以确保将来的可移植性。

>[!NOTE]
> 设置 `userAuthorizationStyle` 功能会默认将会话的 `readRestriction` 和 `joinRestriction` 设置为 `local` 而不是 `none`。 这意味着游戏必须使用搜索句柄或转移句柄来加入游戏会话。

配置 Xbox Live 服务时，可在会话模板中设置这些功能。

对于会话浏览，只能在会话中创建要用于实际游戏玩法而非大厅会话的搜索句柄。

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>对于会话的所有者，这意味着什么？

虽然很多游戏会话类型（例如 SmartMatch 或仅限好友的游戏）不需要所有者，但最好为会话浏览会话设定所有者。 

拥有所有者管理的会话对所有者有益。 所有者可从会话中删除其他成员，或更改其他成员的所有权状态。

若要将所有者用于会话，该会话必须将以下功能设置为 true：

* `hasOwners`

如果会话所有者已阻止 Xbox Live 成员，则该成员不能加入会话。

使用[多人游戏角色](multiplayer-roles.md)时，可对其进行设置，这样只有所有者才能将角色分配给用户。

如果所有的所有者都离开会话，则服务将根据为该会话定义的 `ownershipPolicy.migration` 策略对该会话执行相应的操作。 如果策略为“oldest”，则加入会话时间最长的玩家将被设置为新的所有者。 如果策略为“endsession”（若未提供，则为默认策略），则服务将结束会话，并从该会话中删除其余所有的玩家。


## <a name="search-handles"></a>搜索句柄

搜索句柄以 JSON 结构的形式存储中 MSPD 中。 除了包含对会话的引用，搜索手柄还包含与搜索相关的其他元数据，称为搜索属性。

在任何时候，只能为会话创建一个搜索句柄。

若要使用 Xbox Live API 为会话创建搜索句柄，首先要创建 `multiplayer::multiplayer_search_handle_request` 对象，然后将该对象传递到 `multiplayer::multiplayer_service::set_search_handle()` 方法。

### <a name="search-attributes"></a>搜索属性

搜索属性包含以下组成部分：

`tags` - 标记是字符串描述符，可用于对游戏会话进行分类，与标签类似。 标记必须以字母开头，不能包含空格，且必须少于 100 个字符。
Example tags: "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` - 字符串是文本变量，字符串名称必须以字母开头，不能包含空格，且必须少于 100 个字符。

字符串元数据示例：“Weapons"="knives+pistols+rifles”、“MapName"="UrbanCityAssault”、“description"="Fun casual game, new people welcome”。

`numbers` - 编号是数值变量，编号名称必须以字母开头，不能包含空格，且必须少于 100 个字符。 Xbox Live API 可将编号检索为浮点类型。

编号元数据示例：MinLevel"= 25"MaxRank"= 10。

>**注意：** 标记和字符串值的字母大小写形式保留在服务中，但是，在查询标记时，你必须使用 tolower() 函数。 这意味着，标记和字符串值当前都被视为小写，即使它们包含大写字符。

在 Xbox Live API 中，你可以使用 `multiplayer_search_handle_request` 对象的 `set_tags()`、`set_stringsmetadata()` 和 `set_numbers_metadata()` 方法设置搜索属性。


### <a name="additional-details"></a>其他详细信息

在检索搜索句柄时，结果还包含与会话相关的其他有用数据，例如，会话是否关闭，会话是否有任何加入限制，等等。

在 Xbox Live API 中，这些详细信息，连同搜索属性，都包含在搜索查询后返回的 `multiplayer_search_handle_details` 中。

### <a name="remove-a-search-handle"></a>删除搜索句柄

如果你要从会话浏览中删除会话，例如会话已满时，或者会话关闭时，则可以删除搜索句柄。

在 Xbox Live API 中，可以使用 `multiplayer_service::clear_search_handle()` 方法删除搜索句柄。

### <a name="example-create-a-search-handle-with-metadata"></a>示例：使用元数据创建搜索句柄

以下代码演示如何使用 C++ Xbox Live 多人游戏 API 为会话创建搜索句柄。

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>为会话创建搜索查询

检索搜索句柄列表时，你可以使用搜索查询，将结果限制到符合特定条件的会话。

搜索查询语法是 [OData](http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) 样式的语法，其中，只有以下运算符受到支持：

 运算符 | 描述
 --- | ---
 eq | 等于
 ne | 不等于
 gt | 大于
 ge | 大于或等于
 lt | 小于
 le | 小于或等于
 and | 逻辑 AND
 or | 逻辑 OR（请参见下方注释）

你还可以使用 lambda 表达式和 `tolower` canonical 函数。 目前不支持任何其他 OData 函数。

搜索标记或字符串值时，必须在搜索查询中使用“tolower”函数，因为服务当前仅支持搜索小写字符串。

Xbox Live 服务仅返回与搜索查询匹配的前 100 项结果。 如果结果范围太大，则你的游戏应允许玩家优化其搜索查询。

>[!NOTE]
>  筛选字符串查询支持逻辑 OR；但只允许一个 OR，并且必须位于查询的根目录处。 你不能在查询中使用多个 OR，也不能创建会导致 OR 不在查询结构最顶层的查询。

### <a name="search-handle-query-examples"></a>搜索句柄查询示例

在 RESTful 调用中，“Filter”是你要指定 OData Filter 语言字符串的位置，该语言字符串将在你查询所有搜索句柄时返回。  
对于多人游戏 2015 API，可以在 `multiplayer_service.get_search_handles()` 方法的 *searchFilter* 参数中指定搜索筛选器字符串。  

目前支持以下筛选器方案：

 筛选条件 | 搜索筛选器字符串
 --- | ---
 单个成员 xuid“1234566” | "session/memberXuids/any(d:d eq '1234566')"
 单个所有者 xuid“1234566” | "session/ownerXuids/any(d:d eq '1234566')"
 字符串“forzacarclass”等于“classb” | "tolower(strings/forzacarclass) eq 'classb'"
 数字“forzaskill”等于 6 | "numbers/forzaskill eq 6"
 数字“halokdratio”大于 1.5 | "numbers/halokdratio gt 1.5"
 标记“coolpeopleonly” | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 不包含标记“cursingallowed”的会话 | "tags/any(d:tolower(d) ne 'cursingallowed')"
 不包含等于 0 的数字“rank”的会话 | "numbers/rank ne 0"
 不包含等于“classa”的字符串“forzacarclass”的会话 | "tolower(strings/forzacarclass) ne 'classa'"
 标记“coolpeopleonly”和数字“halokdratio”等于 7.5 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true and numbers/halokdratio eq 7.5"
 数字“halodkratio”大于或等于 1.5，数字“rank”小于 60，数字“customnumbervalue”小于或等于 5 | "numbers/halokdratio ge 1.5 and numbers/rank lt 60 and numbers/customnumbervalue le 5"
 成就 ID“123456” | "achievementIds/any(d:d eq '123456')"
 语言代码“en” | "language eq 'en'"
 计划的时间，返回所有小于或等于指定时间的计划时间 | "session/scheduledTime le '2009-06-15T13:45:30.0900000Z'"
 发布的时间，返回所有小于指定时间的发布时间 | "session/postedTime lt '2009-06-15T13:45:30.0900000Z'"
 会话注册状态 | "session/registrationState eq 'registered'"
 如果会话成员数等于 5 | "session/membersCount eq 5"
 如果会话成员目标计数大于 1 | "session/targetMembersCount gt 1"
 如果最大会话成员计数小于 3 | "session/maxMembersCount lt 3"
 如果会话成员目标计数与会话成员数之间的差小于或等于 5 | "session/targetMembersCountRemaining le 5"
 如果最大会话成员计数与会话成员数之间的差大于 2 | "session/maxMembersCountRemaining gt 2"
 如果会话成员目标计数与会话成员数之间的差小于或等于 15。</br> 如果角色未指定目标，则此查询根据最大会话成员计数与会话成员数之间的差进行筛选。 | "session/needs le 15"
 角色类型为“lfg”的角色“confirmed”（如果具有该角色的成员数等于 5） | "session/roles/lfg/confirmed/count eq 5"
 角色类型为“lfg”的角色“confirmed”（如果该角色的目标成员数大于 1）。</br> 如果角色未指定目标，则改用最大角色数。 | "session/roles/lfg/confirmed/target gt 1"
 角色类型为“lfg”的角色“confirmed”（如果角色目标计数与具有该角色的成员数之间的差等于 15）。</br> 如果角色未指定目标，则此查询根据最大角色计数与具有该角色的成员数之间的差进行筛选。 | "session/roles/lfg/confirmed/needs le 15"
 指向包含特定关键字的会话的所有搜索句柄 | "session/keywords/any(d:tolower(d) eq 'level2')"
 指向属于特定 SCID 的会话的所有搜索句柄 | "session/scid eq '151512315'"
 指向使用特定模板名称的会话的所有搜索句柄 | "session/templateName eq 'mytemplate1'"
 具有标记“elite”或数字“guns”大于 15 且字符串“clan”等于“purple”的所有搜索句柄 | "tags/any(a:tolower(a) eq 'elite') or number/guns gt 15 and string/clan eq 'purple'"

### <a name="refreshing-search-results"></a>刷新搜索结果

 你的游戏应避免自动刷新会话列表，而是提供允许玩家手动刷新该列表（可能在优化搜索条件以更好地筛选结果后执行）的 UI。

 如果玩家尝试加入会话，但该会话已满或关闭，则你的游戏也应刷新搜索结果。

 搜索刷新过多，可能会导致服务限制，因此，你的作品应限制查询的刷新速率。

### <a name="example-query-for-search-handles"></a>示例：查询搜索句柄

 以下代码演示如何查询搜索句柄。 API 返回 `multiplayer_search_handle_details` 对象的集合，这些对象表示与查询匹配的所有搜索句柄。

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>使用搜索句柄加入会话

当你检索到要加入的会话的搜索句柄后，游戏应使用 `MultiplayerService::WriteSessionByHandleAsync()` 或 `multiplayer_service::write_session_by_handle()` 自行添加到会话。

> [!NOTE]
> `WriteSessionAsync()` 和 `write_session()` 方法无法用于加入会话浏览会话。

以下代码演示了如何在检索到搜索句柄后加入会话。

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```
