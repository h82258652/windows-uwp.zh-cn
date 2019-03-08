---
title: 有关加密的导出限制
description: 使用此信息可以确定应用使用加密的方式是否可能会阻止它被列在 Microsoft Store 中。
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 6d445e5164d542a7e10f136a5fb238c575f35c2d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656012"
---
# <a name="export-restrictions-on-cryptography"></a>有关加密的导出限制



使用此信息可以确定应用使用加密的方式是否可能会阻止它被列在 Microsoft Store 中。

美国商务部工业与安全局负责对使用某些加密类型的技术出口进行监管。 Microsoft Store 中列出的所有应用都必须遵守这些法律和法规，因为这些应用文件可以存储在美国。 甚至其他国家或地区的应用开发者上载在美国之外分发的应用也必须遵守这些法规。 因此，向 Microsoft Store 提交应用时，所有应用开发者必须确保他们的应用不包含这些法规所限制的任何技术。

> **请注意**  此处提供的信息提供了一些指导，但你有责任作为应用开发人员 Microsoft Store 以确保您的应用程序符合所有适用法律和法规中发布应用。

 

有关美国的详细信息商务部的局的行业和安全性，请参阅[有关美国商务部工业与安全](https://go.microsoft.com/fwlink/p/?LinkID=245644)。

有关监管技术出口（包括加密）的出口管理条例 (EAR) 的信息，请参阅[对使用加密的项进行 EAR 控制](https://go.microsoft.com/fwlink/p/?LinkID=245645)。

## <a name="governed-uses"></a>监管使用

首先，确定应用是否使用了出口管理条例监管的加密类型。 该问题包括显示在以下列表中的示例；但是切记，此列表不包括每种可能的加密应用。

> **重要**  考虑不仅为您的应用程序，但还所有软件的库、 实用程序和操作系统组件，包括您的应用程序，或链接到编写的代码。

-   对数字签名的任何使用，例如身份验证或完整性检查
-   对你的应用使用或访问的任何数据或文件的加密
-   密钥管理、证书管理或与公钥基础结构交互的任何内容
-   使用安全通信通道，例如 NTLM、Kerberos、安全套接字层 (SSL) 或传输层安全 (TLS)
-   加密密码或其他形式的信息安全
-   复制保护或数字版权管理 (DRM)
-   防病毒保护

有关加密应用程序的最新完整列表，请参阅[使用加密的项的 EAR 控制](https://go.microsoft.com/fwlink/p/?LinkID=245645)。

## <a name="non-restricted-uses"></a>非限制使用

请注意，某些加密的应用程序不受限制。 以下是不受限制的任务：

-   密码加密
-   复制保护
-   Authentication
-   数字版权管理
-   使用数字签名

有关加密应用程序的最新完整列表，请参阅[使用加密的项的 EAR 控制](https://go.microsoft.com/fwlink/p/?LinkID=245645)。

如果应用为不在此列表中的任何任务调用、支持、包含或使用加密，则它需要出口商品分类编号 (ECCN)。

如果没有 ECCN，请参阅 [ECCN 问题与答案](https://go.microsoft.com/fwlink/p/?LinkID=245646)。
