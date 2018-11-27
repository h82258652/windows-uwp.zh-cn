---
title: 有关加密的出口限制
description: 使用此信息可以确定应用使用加密的方式是否可能会阻止它被列在 Microsoft Store 中。
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: e0e57e28fe36ae506d29e2b51a31c756513fdd08
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826838"
---
# <a name="export-restrictions-on-cryptography"></a>有关加密的出口限制



使用此信息可以确定应用使用加密的方式是否可能会阻止它被列在 Microsoft Store 中。

美国商务部工业与安全局负责对使用某些加密类型的技术出口进行监管。 Microsoft Store 中列出的所有应用都必须遵守这些法律和法规，因为这些应用文件可以存储在美国。 甚至其他国家或地区的应用开发者上传在美国之外分发的应用也必须遵守这些法规。 因此，向 Microsoft Store 提交应用时，所有应用开发者必须确保他们的应用不包含这些法规所限制的任何技术。

> **注意**此处提供的信息提供了一些指南，但也作为应用开发者以确保应用遵守所有适用法律和法规的约束在 Microsoft Store 中发布应用你自己的责任。

 

有关美国商务部工业与安全局的详细信息，请参阅[关于工业与安全局](http://go.microsoft.com/fwlink/p/?LinkID=245644)。

有关监管技术出口（包括加密）的出口管理条例 (EAR) 的信息，请参阅[对使用加密的项进行 EAR 控制](http://go.microsoft.com/fwlink/p/?LinkID=245645)。

## <a name="governed-uses"></a>监管使用

首先，确定应用是否使用了出口管理条例监管的加密类型。 该问题包括显示在以下列表中的示例；但是切记，此列表不包括每种可能的加密应用。

> **重要提示**不仅要考虑代码为你的应用，但还所有软件库、 实用工具和你的应用包括或链接到的操作系统组件。

-   对数字签名的任何使用，例如身份验证或完整性检查
-   对你的应用使用或访问的任何数据或文件的加密
-   密钥管理、证书管理或与公钥基础结构交互的任何内容
-   使用安全通信通道，例如 NTLM、Kerberos、安全套接字层 (SSL) 或传输层安全 (TLS)
-   加密密码或其他形式的信息安全
-   复制保护或数字版权管理 (DRM)
-   防病毒保护

有关加密应用程序的最新完整列表，请参阅[使用加密的项的 EAR 控制](http://go.microsoft.com/fwlink/p/?LinkID=245645)。

## <a name="non-restricted-uses"></a>非限制使用

请注意，某些加密的应用程序不受限制。 以下是不受限制的任务：

-   密码加密
-   复制保护
-   身份验证
-   数字版权管理
-   使用数字签名

有关加密应用程序的最新完整列表，请参阅[使用加密的项的 EAR 控制](http://go.microsoft.com/fwlink/p/?LinkID=245645)。

如果应用为不在此列表中的任何任务调用、支持、包含或使用加密，则它需要出口商品分类编号 (ECCN)。

如果没有 ECCN，请参阅 [ECCN 问题与答案](http://go.microsoft.com/fwlink/p/?LinkID=245646)。
