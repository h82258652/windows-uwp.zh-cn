---
title: 身份验证和用户身份
description: 通用 Windows 平台 (UWP) 应用提供了多个选项可用于用户身份验证，范围从使用 Web 身份验证代理的简单的单一登录 (SSO) 到高度安全的双因素身份验证。
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: 6f446299dcf1a0bcf93d483d13c926c6e4cd230f
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4351983"
---
# <a name="authentication-and-user-identity"></a>身份验证和用户身份



通用 Windows 平台 (UWP) 应用提供了多个选项可用于用户身份验证，范围从使用 [Web 身份验证代理](web-authentication-broker.md)的简单的单一登录 (SSO) 到高度安全的双因素身份验证。

对于要连接到第三方标识提供者服务（如 Facebook、Twitter、Flick 等）的常规应用，可使用 [Web 身份验证代理](web-authentication-broker.md)。 为了方便起见，可使用[凭据保险箱](credential-locker.md)来保存和漫游用户的登录信息。

使用 Windows 10 的企业应极力考虑使用 [Microsoft Passport 和 Windows Hello](microsoft-passport.md)，因为它们支持高度安全的双重身份验证。 如果无法使用 Microsoft Passport，[智能卡](smart-cards.md)和[指纹生物识别](fingerprint-biometrics.md)可以添加额外的安全层。

<table>
<tr><th>主题</th><th>说明</th></tr>
<tr><td><a href="credential-locker.md">凭据保险箱</a></td><td>本文介绍了应用可如何使用凭据保险箱安全存储和检索用户凭据，并使用用户的 Microsoft 帐户在设备间漫游用户凭据</td></tr>

<tr><td><a href="fingerprint-biometrics.md">指纹生物识别</a> </td><td>本文介绍了如何将指纹生物识别添加到应用。 在用户必须同意特定操作时将指纹身份验证请求囊括在内，将提升应用的安全性。 例如，可在授权应用内购买或对受限资源的访问权限之前要求指纹身份验证。 指纹身份验证使用 <a href="https://msdn.microsoft.com/library/windows/apps/hh701356">Windows.Security.Credentials.UI</a> 命名空间中的 <a href="https://msdn.microsoft.com/library/windows/apps/dn279134">UserConsentVerifier</a> 类进行管理。</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport 和 Windows Hello</a></td><td>本文介绍了新的 Windows 10 Microsoft Passport 技术，并讨论了开发人员可如何实现此技术来保护其应用和后端服务。 它重点介绍了这些技术的特定功能，这些功能有助于缓解来自传统凭据的威胁，并提供有关设计和部署这些技术作为 Windows 10 部署的一部分的指南。 </td></tr>
<tr><td><a href="microsoft-passport-login.md">创建 Microsoft Passport 登录应用</a></td><td>有关如何创建 Windows 10 UWP（通用 Windows 平台）应用的完整演练中的第 1 部分，将使用 Microsoft Passport 作为传统用户名和密码身份验证系统的替代项。</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">创建 Microsoft Passport 登录服务</a></td><td>有关如何在 Windows 10 UWP（通用 Windows 平台）应用中使用 Microsoft Passport 作为传统用户名和密码身份验证系统的替代项的完整演练中的第 2 部分。</td></tr>
<tr><td><a href="smart-cards.md">智能卡</a></td><td>本主题介绍了应用如何使用智能卡将用户连接到安全网络服务，包括如何访问物理智能卡读卡器、创建虚拟智能卡、与智能卡通信、对用户进行身份验证、重置用户 PIN 以及删除智能卡或断开智能卡连接。</td></tr>
<tr><td><a href="share-certificates.md">在应用之间共享证书</a></td><td>需要用户 ID 和密码组合以外的安全身份验证的 UWP 应用可以使用证书进行身份验证。 当对用户进行身份验证时，认证身份验证提供高级别的信任。 在某些情况下，一组服务将要针对多个应用对用户进行身份验证。 本文介绍了如何使用同一个证书对多个应用进行身份验证，以及如何提供方便代码，用户可使用此代码导入提供的证书以访问安全的 Web 服务。</td></tr>
<tr><td><a href="companion-device-unlock.md">具有配套 IoT 设备的 Windows 解锁</a></td><td>配套设备是可以与你的 Windows 10 桌面版一起使用来增强用户身份验证体验的设备。 通过使用配套设备框架，即使是在 Windows Hello 不可用时（例如 Windows 10 桌面版缺少相机进行面部身份验证或缺少指纹读取器设备），配套设备也能提供丰富的 Microsoft Passport 体验。</td></tr>
<tr><td><a href="web-account-manager.md">Web 帐户管理器</a></td><td>本文将介绍如何使用新的 Windows 10 Web 帐户管理器 API 来显示 AccountsSettingsPane 并将你的通用 Windows 平台 (UWP) 应用连接到外部标识提供者，如 Microsoft 或 Facebook。 你将了解如何请求用户的权限以使用其 Microsoft 帐户、获取访问令牌，并使用它来执行基本的操作（如获取配置文件数据或将文件上传到他们的 OneDrive）。 </td></tr>
<tr><td><a href="web-authentication-broker.md">Web 身份验证代理</a></td><td>本文介绍了如何将应用连接到使用身份验证协议（如 OpenID 或 OAuth）的在线标识提供程序（如 Facebook、Twitter、Flickr、Instagram 等）。 <a href="https://msdn.microsoft.com/library/windows/apps/br212066">AuthenticateAsync</a> 方法将请求发送给联机标识提供者，并取回描述应用访问的提供者资源的访问令牌。</td></tr>
</table>

