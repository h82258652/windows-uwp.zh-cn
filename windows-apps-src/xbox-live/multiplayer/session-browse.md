---
title: Multiplayer session browse
author: KevinAsgari
description: Learn how to implement multiplayer session browse by using Xbox Live multiplayer.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one
ms.openlocfilehash: 8b68663624c62800c1ab08d7714f6aafe301003c
ms.sourcegitcommit: fc695def93ba79064af709253ded5e0bfd634a9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="multiplayer-session-browse"></a>Multiplayer session browse

Multiplayer session browse is a new feature introduced in November 2016 that enables a title to query for a list of open multiplayer game sessions that meet the specified criteria.

## <a name="what-is-session-browse"></a>What is session browse?

In a session browse scenario, a player in a game is able to retrieve a list of joinable game sessions. Each session entry in this list contains some additional metadata about the game, which a player can use to help them select which session to join.  They can also filter the list of sessions based on the metadata. Once the player sees a game session that appeals to them, they can join the session.

A player can also create a new game session, and use session browse to recruit additional players instead of relying on matchmaking.

会话浏览不同于传统匹配方案。对于前者，玩家自行选择要加入的游戏会话。而在匹配方案中，玩家通常要单击“查找游戏”按钮，尝试自动将玩家放置在相应的游戏会话中。 虽然会话浏览是一个手动过程，速度相对较慢，可能无法始终选择客观上最好的游戏，但它让玩家拥有更大的控制权，可以说是更符合主观意愿的游戏选择。

游戏中既包括会话浏览，又包括匹配方案，这是很常见的。 Typically matchmaking is used for commonly played game modes, while session browse is used for custom games.

**Example:** John may be interested in playing a hero battle arena style multiplayer game, but wants to play a game where all players select their hero randomly. He can retrieve a list of open game sessions and find the ones that include "random heroes" in their description, or if the game UI allows it, he can select the "random hero" game mode and retrieve only the sessions that are tagged to indicate that they are "RandomHero" games.

When he finds a game that he likes, he joins the game. When enough people have joined the session, the host of the game session can start the game.

### <a name="roles"></a>Roles

A game in session browse may want to recruit players for specific roles. For example, a player may want to create a game session that specifies that the session contains no more than 5 assault classes, but must contain at least 2 healer roles, and at least 1 tank role.

When another player applies for the session, they can pre-select their role, and the service will not allow them to join the session if there are no open slots for the role they have selected.

Another example would be if a player wants to reserve 2 slots for their friends to join, the game can specify a "friends" role, and only players that are friends with the session host can fill the 2 slots dedicated to the "friends" role.

For more information about roles, see [multiplayer roles](multiplayer-roles.md).



## <a name="how-does-session-browse-work"></a>How does session browse work?

Session browse works primarily on the use of search handles. A search handle is a packet of data that contains a reference to the session, as well as additional metadata about the session, namely search attributes.

When a title creates a new game session that is eligible for session browse, it creates a search handle for the session. The search handle is stored in the Multiplayer Service Directory (MPSD), which maintains the search handles for the title.

When a title needs to retrieve a list of sessions, the title can send a search query to MPSD, which will return a list of search handles that meet the search criteria. The title can then use the list of sessions to display a list of joinable games to the player.

When a session is full, or otherwise cannot be joined, a title can remove the search handle from MPSD so that the session will no longer show up in session browse queries.


## <a name="set-up-a-session-for-session-browse"></a>Set up a session for session browse

若要将搜索句柄用于会话，该会话必须将以下功能设置为 true：

* `searchable`
* `userAuthorizationStyle`
* `hasOwners`

>[!NOTE]
> 只有 UWP 游戏需要 `userAuthorizationStyle` 和 `hasOwners` 功能，但我们建议为所有 Xbox Live 游戏（包括 XDK 游戏）应用这些功能，以确保将来的可移植性。

>[!NOTE]
> 设置 `userAuthorizationStyle` 功能会默认将会话的 `readRestriction` 和 `joinRestriction` 设置为 `local` 而不是 `none`。 这意味着游戏必须使用搜索句柄或转移手柄来加入游戏会话。

此外，由于可搜索的会话必须具有所有者，因此，必须将 `owernshipPolicy.migration` 设置为“oldest”或“endsession”。

You can set these capabilities in the session template when you configure your Xbox Live services.

For session browse, you should only create search handles on sessions that will be used for actual gameplay, not for lobby sessions.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>What does it mean to be an owner of a session?

While many game session types, such as SmartMatch or a friends only game, do not require an owner, every session browse session must have at least one owner. Only a member that is marked as an owner of a session can create a search handle for that session.

In addition, only owners can remove other members from the session, or change the ownership status of other members.

If an owner of a session has an Xbox Live member blocked, that member cannot join the session.

If all owners leave a session, then the service takes action on the session based the `ownershipPolicy.migration` policy that is defined for the session. If the policy is "oldest", then the player that has been in the session the longest is set to be the new owner. If the policy is "endsession", then the service ends the session and removes all remaining players from the session.


## <a name="search-handles"></a>Search handles

A search handle is stored in MSPD as a JSON structure. In addition to containing a reference to the session, search handles also contain additional metadata for searches, known as search attributes.

A session can only have one search handle created for it at any time.

To create a search handle for a session in by using the Xbox Live APIs, you first create a `multiplayer::multiplayer_search_handle_request` object, and then pass that object to the `multiplayer::multiplayer_service::set_search_handle()` method.

### <a name="search-attributes"></a>Search attributes

Search attributes consists of the following components:

`tags` - Tags are string descriptors that people can use to categorize a game session, similar to a hashtag. Tags must start with a letter, cannot contain spaces, and must be less than 100 characters.
Example tags: "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` - Strings are text variables, and the string names must start with a letter, cannot contain spaces, and must be less than 100 characters.

Example string metadata: "Weapons"="knives+pistols+rifles", "MapName"="UrbanCityAssault", "description"="Fun casual game, new people welcome."

`numbers` - Numbers are numeric variables, and the number names must start with a letter, cannot contain spaces, and must be less than 100 characters. The Xbox Live APIs retrieve number values as type float.

Example number metadata: "MinLevel" = 25, "MaxRank" = 10.

>**Note:** The letter casing of tags and string values is preserved in the service, but you must use the tolower() function when you query for tags. This means that tags and string values are currently all treated as lower case, even if they contain upper case characters.

In the Xbox Live APIs, you can set the search attributes by using the `set_tags()`, `set_stringsmetadata()`, and `set_numbers_metadata()` methods of a `multiplayer_search_handle_request` object.


### <a name="additional-details"></a>Additional details

When you retrieve a search handle, the results also include additional useful data about the session, such as if the session is closed, are there any join restrictions on the session, etc.

In the Xbox Live APIs, these details, along with the search attributes, are included in the `multiplayer_search_handle_details` that are returned after a search query.

### <a name="remove-a-search-handle"></a>Remove a search handle

When you want to remove a session from session browse, such as when the session is full, or if the session is closed, you can delete the search handle.

In the Xbox Live APIs, you can use the `multiplayer_service::clear_search_handle()` method to remove a search handle.

### <a name="example-create-a-search-handle-with-metadata"></a>Example: Create a search handle with metadata

The following code shows how to create a search handle for a session by using the C++ Xbox Live multiplayer APIs.

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


## <a name="create-a-search-query-for-sessions"></a>Create a search query for sessions

When retrieving a list of search handles, you can use a search query to restrict the results to the sessions that meet specific criteria.

The search query syntax is an  [OData](http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) style syntax, with only the following operators supported:

 Operator | Description
 --- | ---
 eq | 等于
 ne | 不等于
 gt | greater than
 ge | greater than or equal
 lt | less than
 le | less than or equal
 and | 逻辑 AND
 or | 逻辑 OR（请参见下方注释）

你还可以使用 lambda 表达式和 `tolower` canonical 函数。 No other OData functions are supported currently.

When searching for tags or string values, you must use the 'tolower' function in the search query, as the service only currently supports searching for lower-case strings.

The Xbox Live service only returns the first 100 results that match the search query. 如果结果范围太大，则你的游戏应允许玩家优化其搜索查询。

>[!NOTE]
>  筛选字符串查询支持逻辑 OR；但只允许一个 OR，并且必须位于查询的根目录处。 你不能在查询中使用多个 OR，也不能创建会导致 OR 不在查询结构最顶层的查询。

### <a name="search-handle-query-examples"></a>搜索句柄查询示例

In a restful call, "Filter" is where you would specify an OData Filter language string that will be run in your query against all search handles.  
In the multiplayer 2015 APIs, you can specify the search filter string in the *searchFilter* parameter of the `multiplayer_service.get_search_handles()` method.  

Currently, the following filter scenarios are supported:

 Filter by | Search filter string
 --- | ---
 A single member xuid '1234566’ | "session/memberXuids/any(d:d eq '1234566')"
 A single owner xuid '1234566’ | "session/ownerXuids/any(d:d eq '1234566')"
 A string 'forzacarclass' equal to 'classb‘ | "tolower(strings/forzacarclass) eq 'classb'"
 A number 'forzaskill' equal to 6 | "numbers/forzaskill eq 6"
 A number 'halokdratio' greater than 1.5 | "numbers/halokdratio gt 1.5"
 A tag 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 不包含标记“cursingallowed”的会话 | "tags/any(d:tolower(d) ne 'cursingallowed')"
 不包含等于 0 的数字“rank”的会话 | "numbers/rank ne 0"
 不包含等于“classa”的字符串“forzacarclass”的会话 | "tolower(strings/forzacarclass) ne 'classa'"
 标记“coolpeopleonly”和数字“halokdratio”等于 7.5 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true and numbers/halokdratio eq 7.5"
 A number 'halodkratio' greater than or equal to 1.5, a number 'rank' less than 60, and a number 'customnumbervalue' less than or equal to 5 | "numbers/halokdratio ge 1.5 and numbers/rank lt 60 and numbers/customnumbervalue le 5"
 An achievement id '123456' | "achievementIds/any(d:d eq '123456')"
 The language code 'en' | "language eq 'en'"
 Scheduled time, returns all scheduled times less than or equal to the specified time | "session/scheduledTime le '2009-06-15T13:45:30.0900000Z'"
 Posted time, returns all posted times less than the specified time | "session/postedTime lt '2009-06-15T13:45:30.0900000Z'"
 Session registration state | "session/registrationState eq 'registered'"
 Where the number of session members is equal to 5 | "session/membersCount eq 5"
 Where the session member target count is greater than 1 | "session/targetMembersCount gt 1"
 Where the max count of session members is less than 3 | "session/maxMembersCount lt 3"
 Where the difference between the session member target count and the number of session members is less than or equal to 5 | "session/targetMembersCountRemaining le 5"
 Where the difference between the max count of session members and the number of session members is greater than 2 | "session/maxMembersCountRemaining gt 2"
 Where the difference between the session member target count and the number of session members is less than or equal to 15.</br> If the role does not have a target specified, then this query filters against the difference between the max count of session members and the number of session members. | "session/needs le 15"
 Role "confirmed" of the role type "lfg" where the number of members with that role is equal to 5 | "session/roles/lfg/confirmed/count eq 5"
 Role "confirmed" of the role type "lfg" where the target of that role is greater than 1.</br> If the role does not have a target specified, then the max of the role is used instead. | "session/roles/lfg/confirmed/target gt 1"
 Role "confirmed" of the role type "lfg" where the difference between the target of the role and the number of members with that role is less than or equal to 15.</br> If the role does not have a target specified, then this query filters against the difference between the max of the role and the number of members with that role. | "session/roles/lfg/confirmed/needs le 15"
 All search handles that point to a session containing a particular keyword | "session/keywords/any(d:tolower(d) eq 'level2')"
 All search handles that point to a session belonging to a particular scid | "session/scid eq '151512315'"
 All search handles that point to a session that uses a particular template name | "session/templateName eq 'mytemplate1'"
 All search handles that have the tag 'elite' or have a number 'guns' greater than 15 and string 'clan' equal to 'purple' | "tags/any(a:tolower(a) eq 'elite') or number/guns gt 15 and string/clan eq 'purple'"

### <a name="refreshing-search-results"></a>Refreshing search results

 Your game should avoid automatically refreshing a list of sessions, but instead provide UI that allows a player to manually refresh the list (possibly after refining the search criteria to better filter the results).

 If a player attempts to join a session, but that session is full or closed, then your game should refresh the search results as well.

 Too many search refreshes can lead to service throttling, so your title should limit the rate at which the query can be refreshed.

### <a name="example-query-for-search-handles"></a>Example: query for search handles

 The following code shows how to query for search handles. API 返回 `multiplayer_search_handle_details` 对象的集合，这些对象表示与查询匹配的所有搜索句柄。

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
