---
title: 身份验证和用户身份
description: 通用 Windows 平台 (UWP) 应用提供了多个选项可用于用户身份验证，范围从使用 Web 身份验证代理的简单的单一登录 (SSO) 到高度安全的双因素身份验证。
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: awkoren
---

# 身份验证和用户身份


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

通用 Windows 平台 (UWP) 应用提供了多个选项可用于用户身份验证，范围从使用 [Web 身份验证代理](web-authentication-broker.md)的简单的单一登录 (SSO) 到高度安全的双因素身份验证。

对于要连接到第三方标识提供商服务（如 Facebook、Twitter、Flick 等）的常规应用，可使用 [Web 身份验证代理](web-authentication-broker.md)。 为了方便起见，可使用[凭据保险箱](credential-locker.md)来保存和漫游用户的登录信息。

使用 Windows 10 的企业应极力考虑使用 [Microsoft Passport 和 Windows Hello](microsoft-passport.md)，因为它们支持高度安全的双重身份验证。 如果无法使用 Microsoft Passport，[智能卡](smart-cards.md)和[指纹生物识别](fingerprint-biometrics.md)可以添加额外的安全层。

| 主题                                                                                 | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [凭据保险箱](credential-locker.md)                                             | 本文介绍了应用可如何使用凭据保险箱安全存储和检索用户凭据，并使用用户的 Microsoft 帐户在设备间漫游用户凭据。 |
| [指纹生物识别](fingerprint-biometrics.md)                                   | 本文介绍了如何将指纹生物识别添加到应用。 在用户必须同意特定操作时将指纹身份验证请求囊括在内，将提升应用的安全性。 例如，可在授权应用内购买或对受限资源的访问权限之前要求指纹身份验证。 指纹身份验证使用 [Windows.Security.Credentials.UI](https://msdn.microsoft.com/library/windows/apps/hh701356) 命名空间中的 [UserConsentVerifier](https://msdn.microsoft.com/library/windows/apps/dn279134) 类进行管理。 |
| [Microsoft Passport 和 Windows Hello](microsoft-passport.md)                         | 本文介绍了新的 Windows 10 Microsoft Passport 技术，并讨论了开发人员可如何实现此技术来保护其应用和后端服务。 它重点介绍了这些技术的特定功能，这些功能有助于缓解来自传统凭据的威胁，并提供有关设计和部署这些技术作为 Windows 10 部署的一部分的指南。 |
| [创建 Microsoft Passport 登录应用](microsoft-passport-login.md)                  | 有关如何创建 Windows 10 UWP（通用 Windows 平台）应用的完整演练中的第 1 部分，将使用 Microsoft Passport 作为传统用户名和密码身份验证系统的替代项。 |
| [创建 Microsoft Passport 登录服务](microsoft-passport-login-auth-service.md) | 有关如何在 Windows 10 UWP（通用 Windows 平台）应用中使用 Microsoft Passport 作为传统用户名和密码身份验证系统的替代项的完整演练中的第 2 部分。 |
| [智能卡](smart-cards.md)                                                         | 本主题介绍了应用如何使用智能卡将用户连接到安全网络服务，包括如何访问物理智能卡读卡器、创建虚拟智能卡、与智能卡通信、对用户进行身份验证、重置用户 PIN 以及删除智能卡或断开智能卡连接。 |
| [在应用之间共享证书](share-certificates.md)                              | 需要用户 ID 和密码组合以外的安全身份验证的 UWP 应用可以使用证书进行身份验证。 当对用户进行身份验证时，认证身份验证提供高级别的信任。 在某些情况下，一组服务将要针对多个应用对用户进行身份验证。 本文介绍了如何使用同一个证书对多个应用进行身份验证，以及如何提供方便代码，用户可使用此代码导入提供的证书以访问安全的 Web 服务。 |
| [拥有配套设备 Windows 解锁](companion-device-unlock.md)   | 配套设备是可以与你的 Windows 10 桌面版一起使用来增强用户身份验证体验的设备。 通过使用配套设备框架，即使是在 Windows Hello 不可用时（例如 Windows 10 台式机缺少相机进行脸部身份验证或缺少指纹读取器设备），配套设备也能提供丰富的 Microsoft Passport 体验。 | 
| [Web 身份验证代理](web-authentication-broker.md)                             | 本文介绍了如何将应用连接到使用身份验证协议（如 OpenID 或 OAuth）的在线标识提供程序（如 Facebook、Twitter、Flickr、Instagram 等）。 [AuthenticateAsync](https://msdn.microsoft.com/library/windows/apps/br212066) 方法将请求发送给联机标识提供商，并取回描述应用访问的提供商资源的访问令牌。 |


<!--HONumber=Mar16_HO5-->


