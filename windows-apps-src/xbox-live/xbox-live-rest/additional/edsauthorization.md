---
title: EDS 授权
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
author: KevinAsgari
description: " EDS 授权"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c4233555093823f09c6c43d92f6863574191068f
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5547621"
---
# <a name="eds-authorization"></a>EDS 授权
 
  * [简介](#ID4EN)
  * [授权流程](#ID4EFB)
  * [3.0 标记： 多用户与单个用户](#ID4EEC)
  * [EDS 是否支持多用户？](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>简介
 
娱乐发现服务 (EDS) 3.1 不会支持匿名流量。 所有请求 EDS 需要身份验证。 EDS 需要调用方进行正确身份验证的客户端从 XToken。 这些标记由 XSTS 和可以获得通过各种 Xbox 身份验证服务 (XAS)。 有单独的身份验证服务的设备、 用户和游戏将所有定义的令牌的标识。
 
XSTS 是 Xbox LIVE 的网关守卫。 它是确定如果用户或设备有权将连接到任何 Xbox LIVE 服务的第一个防线。 对用户进行身份验证后, XSTS 生成 XToken 它们可用于安全地将自己标识为在服务上的任何组件。 此 XToken 是生存你 passport。
 
用户并想要使用我们的服务的事项。 并且，我们希望大部分这些操作，用户能够使用我们的服务。 但如何执行我们确保操作不假装个人，而且用户实际上是他们所说的人？ 我们将向他们提供他们可以用它来确定自己给其他人的令牌。
 
这些标记由 XSTS 和通常称为 XTokens。 XToken 是一个广泛的术语，用于覆盖标记包含各种不同的内容，并可以有很多不同的形式，但是所有创建、 已签名，而且由 XSTS 服务器 （可选） 加密。 在内部，XSTS 使用 MXAN 来创建并格式化令牌。 MXAN 是不断提取 XToken 信息的唯一组件。 使用令牌的服务传递到 MXAN 以破解的请求标头。 处理和验证令牌和表示在令牌中的声明将返回到该服务。 服务然后可以使用这些声明值来标识调用用户或设备，并根据该信息执行操作。
 
基本标识标记中的用户、 设备和标题-提供通过 Xbox 身份验证服务 (XAS)。 每个 XAS 负责生成为他们将负责的各种声明指定的值的标识令牌。
 
   * XASD (适用于设备的 XAS): 创建 DToken 可提供设备标识
   * XASU (XAS 为用户): 创建 UToken 可提供用户标识
   * XAST (对于游戏 XAS): 创建 TToken 可提供作品标识
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>授权流程
 
   * 获取一个或多个标识标记。 你可以请求 XToken 最每个 D、 U 和 T 令牌之一。 你必须提供至少一个 D 或 u。 
     * 从 XASD 请求 DToken，通过提供设备身份验证的详细信息
     * 从 XASU 具有某种形式的用户身份验证请求 UToken。 用户身份验证可能 MSA (RPS) 标记的形式提供。
     * 请求 XAST TToken。 可用的标题取决于当前正在运行，因此必须提供 DToken XAST 以及平台。
  
   * 创建 XSTS 请求。
 
     * 定义要请求访问令牌的信赖方。
     * 填充请求属性与 D、 U，和/或 T 令牌。
    * 执行 XSTS 请求并缓存结果 XToken。 返回的 XToken （最） 包含所有标识标记和任何其他声明 （当前订阅状态、 用户组等） 的设备、 用户和游戏标识信息。
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 标记： 多用户与单个用户
 
这是 3.0 标记的形式： `XBL3.0 x=&lt;hash>;&lt;token>`
 
根据&lt;哈希 >，该令牌的处理方式是不同：
 
   * 如果&lt;哈希 > 等于 * （星号），则没有特定用户正在执行请求的令牌中的所有用户存在和反序列化的主体中。 这是真正的多用户表单。
   * 如果&lt;哈希 > 等于-(dash)，则没有用户正在执行的请求。 反序列化的主体中的任何用户都将去除。
   * 如果&lt;哈希 > 不等于 * 或-然后它是用于指示在令牌中的用户发出请求的标识符。 只有指定的用户将会出现在反序列化的主体中。 所有其他用户都将去除。这是单用户 3.0 标记。
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS 是否支持多用户？
 * 答案是没有。 中所述的情况下，控制台将始终发送单个用户令牌。 即使有多个用户登录，必须指定"调用方"，其他所有身份均都放置的位置。
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>详细信息 

[市场 URI](../uri/marketplace/atoc-reference-marketplace.md)

   