---
author: payzer
title: "Device Portal Xbox 开发人员 devkit 更新策略 API 参考"
description: "了解如何以编程方式为控制台设置更新策略。"
ms.openlocfilehash: f9313d3c8b93ba13074c547f1f63c9f3204f0f58
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
注意：此 API 将在下一个开发人员预览版中推出。

# <a name="system-update-policy-api-reference"></a>系统更新策略 API 参考   
可以使用此 API 查看哪个更新策略适用于控制台，并将更新策略更换为新策略。

重要提示：在尝试调用此 API 时，大多数控制台都将收到“访问被拒”的响应。 这是因为并非所有开发控制台均可更改其更新策略。

此 API 可影响开发人员模式下的控制台更新策略，而不能影响零售控制台的更新策略。

## <a name="get-the-console-update-policy"></a>获取控制台更新策略

**请求**

可以使用以下请求获取控制台的更新策略。

方法      | 请求 URI
:------     | :-----
GET | /ext/update/policy
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
该响应是 JSON 数组，包含控制台系统更新组成员身份。 每个对象都具有以下字段：   

Id -（字符串）更新组的 ID。   
友好名称 -（字符串）更新组的显示名称。   
描述 -（“字符串”）更新组的描述。
IsDevKitGroup - (true | false) 指示更新组是否适用于开发人员内部版本。
ResourceSetID -（字符串）忽略 - 由系统更新基础结构使用。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

## <a name="set-a-consoles-system-update-policy"></a>设置控制台的系统更新策略
可以使用此 API 切换控制台的系统更新组成员身份。

注意：控制台一次只能存在于一个系统更新组中。

**请求**

可以使用以下请求设置控制台的系统更新组成员身份。

方法      | 请求 URI
:------     | :-----
POST | /ext/update/policy
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   
请求正文是 JSON 对象，包含以下字段：   
GroupIdToJoin -（字符串）想要控制台加入的系统更新组 ID。  
GroupIdToLeave -（字符串）想要控制台退出的系统更新组 ID。

所有字段均为必填字段。

可能的 GroupID 有：   
* 无更新 -“b2dbed33-2845-44cc-a7a1-4a9afb29d8d9”   
* 最新的生产恢复 -“7432ae99-8c09-48dd-99f9-a2697499e240”   
* 最新的预览恢复 -“a8153054-1a1b-47cc-acc9-9aed90c1f8db”    

**响应**   

- 无

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox

