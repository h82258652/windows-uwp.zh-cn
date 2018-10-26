---
author: Xansky
description: 使用此部分中的 Java 代码示例了解有关使用 Microsoft Store 提交 API 提交游戏选项和预报片的详细信息。
title: Java 示例 - 使用游戏选项和预告片的应用提交
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 代码示例, 游戏选项, 预告片, 高级应用一览, java
ms.localizationpriority: medium
ms.openlocfilehash: d6d64e317d2ff75be4aeb1f0e7df512287ae914a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "5544673"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Java 示例：使用游戏选项和预告片的应用提交

本文提供 Java 代码示例演示如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md) 执行以下任务：

* 获取要用于 Microsoft Store 提交 API 的 Azure AD 访问令牌。
* 创建应用提交
* 配置用于应用提交的应用商店一览数据，包括[游戏](manage-app-submissions.md#gaming-options-object)和[预告片](manage-app-submissions.md#trailer-object)高级应用一览选项。
* 上传 ZIP 文件，其中包含程序包、应用一览图像和用于应用提交的预告片文件。
* 确认应用提交。

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>创建应用提交

```CreateAndSubmitSubmissionExample``` 类实现 ```main``` 程序，该程序调用其他示例方法，以使用 Microsoft Store 提交 API 来创建并确认包含游戏选项和预告片的应用提交。 要调整此代码供自己使用，请执行以下操作：

* 将 ```tenantId``` 变量指定为你应用的租户 ID，将 ```clientId``` 和 ```clientSecret``` 变量指定为你应用的客户端 ID 和密钥。 有关详细信息，请参阅[如何将 Azure AD 应用程序与你的 Windows 开发者中心帐户相关联](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)
* 将 ```applicationId``` 变量指定为要为其创建提交的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java#L1-L313)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>获取 Azure AD 访问令牌

```DevCenterAccessTokenClient``` 类定义一个帮助程序方法，该方法使用你的 ```tenantId```、```clientId``` 和 ```clientSecret``` 值来创建要用于 Microsoft Store 提交 API 的 Azure AD 访问令牌。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java#L1-L69)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>用于调用提交 API 和上传提交文件的帮助程序方法

```DevCenterClient``` 类定义帮助程序方法，这些方法在 Microsoft Store 提交 API 中调用多种方法并上传一个 ZIP 文件，其中包含应用提交的程序包、应用一览图像和预告片文件。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java#L1-L224)]

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
