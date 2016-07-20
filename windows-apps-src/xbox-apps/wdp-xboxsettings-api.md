---
author: payzer
title: "Device Portal Xbox 开发人员设置 API 参考"
description: "了解如何访问 Xbox 开发人员设置。"
translationtype: Human Translation
ms.sourcegitcommit: a9a2b6e58dfa0d1e77164a59f204deabf8f5c3e0
ms.openlocfilehash: e3637f5a8481c0800af42c011fb811b908b946b1

---

# 开发人员设置 API 参考   
你可以使用此 API 访问有助于开发的 Xbox One 设置。

## 一次性获取所有开发人员设置

**请求**

你可以使用以下请求，在一次请求中获取所有开发人员设置。

方法      | 请求 URI
:------     | :-----
GET | /ext/settings
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
该响应是一个设置 JSON 数组，包含所有设置。 每个设置对象都包含以下字段：   

Name -（字符串）设置的名称。   
Value -（字符串）设置的值。   
RequiresReboot -（“Yes”|“No”）此字段指示该设置是否需要重新启动才能生效。
Category -（字符串）设置的类别

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

## 一次获取一个设置
设置也可以逐个检索。

**请求**

你可以使用以下请求来获取有关各个设置的信息。

方法      | 请求 URI
:------     | :-----
GET | /ext/settings/<setting name>
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
该响应是一个具有以下字段的 JSON 对象：   

Name -（字符串）设置的名称。   
Value -（字符串）设置的值。   
RequiresReboot -（“Yes”|“No”）此字段指示该设置是否需要重新启动才能生效。
Category -（字符串）设置的类别

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

## 设置某个设置的值
你可以设置某个设置的值。

**请求**

你可以使用以下请求设置某个设置的值。

方法      | 请求 URI
:------     | :-----
PUT | /ext/settings/<setting name>
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   
请求正文是 JSON 对象，包含以下字段：   
Value -（字符串）设置的新值。

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




<!--HONumber=Jul16_HO2-->


