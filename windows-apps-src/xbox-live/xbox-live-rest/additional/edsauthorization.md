---
title: EDS 授权
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " EDS 授权"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607602"
---
# <a name="eds-authorization"></a>EDS 授权
 
  * [简介](#ID4EN)
  * [授权过程](#ID4EFB)
  * [3.0 令牌：多用户 vs。单个用户](#ID4EEC)
  * [EDS 是否支持多用户？](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>简介
 
娱乐发现服务 (EDS) 3.1 将不支持匿名流量。 需要对 EDS 的所有请求身份验证。 EDS 将需要从调用方正确通过身份验证的客户端 XToken。 这些令牌生成的 XSTS，可以通过各种 Xbox 身份验证服务 (XAS) 中获得。 有单独的身份验证服务的设备、 用户和标题，用于定义的令牌的标识。
 
XSTS 是为 Xbox LIVE 守护程序。 若要确定用户或设备有权连接到任何 Xbox LIVE 服务的第一个防线它。 在对用户进行身份验证之后, XSTS 会生成可用于安全标识自身的服务上的任何组件 XToken。 此 XToken 是您的 passport 生存。
 
人员和希望使用我们的服务的内容。 并且我们希望大部分这些人员和事物要能够使用我们的服务。 但如何执行我们确保内容不伪装成人员，而且用户实际上是他们所说的人？ 我们向他们提供令牌，它们可用于向其他人进行自我标识。
 
这些令牌由 XSTS 生成，通常称为 XTokens。 XToken 是一个泛指的术语来覆盖包含各种不同的内容和可以有多种不同形式的令牌，但它们所有创建、 签名，并根据需要由 XSTS 服务器加密。 在内部，XSTS 使用 MXAN 来创建和格式化的令牌。 MXAN 是曾经从 XToken 中提取信息的唯一组件。 使用令牌的服务将请求标头传递到 MXAN 要被破解。 处理和验证令牌和声明表示令牌中返回给服务。 服务然后，可以使用这些声明值以确定调用的用户或设备，并根据该信息执行操作。
 
基本身份标识令牌中的用户、 设备和标题-提供通过 Xbox 身份验证服务 (XAS)。 每个 XAS 负责生成一个标识令牌，它指定要负责的各种声明值。
 
   * XASD (XAS 的设备): 创建 DToken 提供设备标识
   * XASU (XAS 用户): 创建 UToken 提供用户标识
   * XAST (XAS 的标题): 创建 TToken 提供标题标识
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>授权过程
 
   * 获取一个或多个标识令牌。 你可以请求与最多每个 D、 U 和 T 令牌 XToken。 必须提供至少一个 D 或 u。 
     * 设备身份验证详细信息，从而从 XASD 请求 DToken
     * 从 XASU 有某种形式的用户身份验证请求 UToken。 可能的 MSA (RPS) 令牌的形式提供用户身份验证。
     * 从 XAST 请求 TToken。 所示标题的可用取决于当前正在运行，因此必须也为 XAST 提供 DToken 的平台。
  
   * 创建 XSTS 请求。
 
     * 定义请求的令牌的信赖方。
     * 填充使用 D，U，和/或 T 令牌的请求属性。
    * 执行 XSTS 请求并缓存生成 XToken。 返回的 XToken （至少） 包含所有标识令牌和任何其他声明 （当前订阅状态、 用户组等） 中的设备、 用户和标题标识信息。
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 令牌：多用户 vs。单个用户
 
这是 3.0 令牌的形式： `XBL3.0 x=&lt;hash>;&lt;token>`
 
具体取决于&lt;哈希 >，以不同方式处理令牌：
 
   * 如果&lt;哈希 > 等于 * （星号），则没有特定用户执行请求的令牌中的所有用户都不在反序列化的主体存在。 这是真正的多用户表单。
   * 如果&lt;哈希 > 为等于-（连字符），则没有用户正在执行请求。 反序列化的主体中的任何用户都将去除。
   * 如果&lt;哈希 > 不等于 * 或-则，该值指示令牌中的用户正在发出请求的标识符。 指明的用户将会出现在反序列化的主体。 去除其他所有用户。这是单用户 3.0 令牌。
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS 是否支持多用户？
 * 答案是没有。 中所述的情况下，控制台会始终发送单个用户令牌。 即使有多个用户登录，必须指定"调用方"，其中所有其他标识将被删除。
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>详细信息 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   