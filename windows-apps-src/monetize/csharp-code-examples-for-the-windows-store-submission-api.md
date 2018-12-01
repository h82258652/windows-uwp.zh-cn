---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: 使用此部分中的 C# 代码示例了解有关使用 Microsoft Store 提交 API 的详细信息。
title: C# 示例 - 应用、加载项和外部测试版的提交
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 代码示例, C#
ms.localizationpriority: medium
ms.openlocfilehash: 27325938ef159dfcb29de174064314ee21d3a3f5
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8336796"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C\# 示例：应用、加载项和外部测试版的提交

本文提供 C# 代码示例演示如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md) 执行以下任务：

* [创建应用提交](#create-app-submission)
* [创建加载项提交](#create-add-on-submission)
* [更新加载项提交](#update-add-on-submission)
* [创建软件包外部测试版提交](#create-flight-submission)

你可以查看每个示例，了解有关它所演示的任务的详细信息，也可以将本文中的所有代码示例生成到控制台应用程序。 若要构建示例，请在 Visual Studio 中创建名为 **DeveloperApiCSharpSample** 的 C# 控制台应用程序、将每个示例复制到项目中单独的代码文件，然后构建该项目。

## <a name="prerequisites"></a>先决条件

这些示例使用以下库：

* Microsoft.WindowsAzure.Storage.dll。 此库在[用于 .NET 的 Azure SDK](https://azure.microsoft.com/downloads/) 中提供，或者可以通过安装 [WindowsAzure.Storage NuGet 程序包](https://www.nuget.org/packages/WindowsAzure.Storage)获取。
* [Newtonsoft.Json](http://www.newtonsoft.com/json) 来自 Newtonsoft 的 NuGet 程序包。

## <a name="main-program"></a>主计划

以下示例实现一个命令行计划，该计划可调用本文中的其他示例方法演示使用 Microsoft Store 提交 API 的不同方法。 若要调整此程序以供自己使用，请执行以下操作：

* 为你想管理的应用 ID、加载项和软件包外部测试版分配 ```ApplicationId```、```InAppProductId``` 和 ```FlightId``` 属性。
* 将 ```ClientId``` 和 ```ClientSecret``` 属性指定为你应用的客户端 ID 和密钥，并将 ```TokenEndpoint``` URL 中的 *tenantid* 字符串更换为你应用的租户 ID。 有关详细信息，请参阅[如何将 Azure AD 应用程序与合作伙伴中心帐户相关联](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>ClientConfiguration 帮助程序类

示例应用使用 ```ClientConfiguration``` 帮助程序类将 Azure Active Directory 数据和应用传递到每个使用 Microsoft Store 提交 API 的示例方法。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>创建应用提交

以下示例实现一个类，该类使用 Microsoft Store 提交 API 中的多种方法更新应用提交。 ```RunAppSubmissionUpdateSample```类中的方法创建新提交作为克隆的上次发布的提交，然后更新并提交到合作伙伴中心克隆的提交。 具体来说，```RunAppSubmissionUpdateSample``` 方法执行以下任务：

1. 首先，此方法[获取指定应用的数据](get-an-app.md)。
2. 接下来，此方法会[删除应用的挂起提交](delete-an-app-submission.md)（如果存在）。
3. 然后，此方法会[创建新的应用提交](create-an-app-submission.md)（新提交时是上次发布的提交副本）。
4. 它会更改新提交的部分详细信息并将新的提交包上载到 Azure Blob 存储。
5. 接下来，该[更新](update-an-app-submission.md)，然后[提交](commit-an-app-submission.md)到合作伙伴中心的新提交。
6. 最后，它会定期[检查新提交的状态](get-status-for-an-app-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>创建加载项提交

以下示例实现一个类，该类使用 Microsoft Store 提交 API 中的多种方法创建新的加载项提交。 该类中的 ```RunInAppProductSubmissionCreateSample``` 方法执行以下任务：

1. 首先，该方法会 [创建新加载项](create-an-add-on.md)。
2. 然后，该方法会 [创建新的加载项提交](create-an-add-on-submission.md)。
3. 该方法会将包含提交图标的 ZIP 存档上传至 Azure Blob 存储。
4. 接下来，它[提交到合作伙伴中心的新提交](commit-an-add-on-submission.md)。
5. 最后，该方法定期[检查新提交的状态](get-status-for-an-add-on-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>更新加载项提交

以下示例实现一个类，该类使用 Microsoft Store 提交 API 中的多种方法更新现有的加载项提交。 ```RunInAppProductSubmissionUpdateSample```类中的方法创建新提交作为克隆的上次发布的提交，然后更新并提交到合作伙伴中心克隆的提交。 具体来说，```RunInAppProductSubmissionUpdateSample``` 方法执行以下任务：

1. 首先，此方法[获取指定加载项的数据](get-an-add-on.md)。
2. 接下来，此方法会[删除加载项的挂起提交](delete-an-add-on-submission.md)（如果存在）。
3. 然后，此方法[会创建新的加载项提交](create-an-add-on-submission.md)（新提交时是上次发布的提交副本）。
5. 接下来，该[更新](update-an-add-on-submission.md)，然后[提交](commit-an-add-on-submission.md)到合作伙伴中心的新提交。
6. 最后，它会定期[检查新提交的状态](get-status-for-an-add-on-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>创建软件包外部测试版提交

以下示例实现一个类，该类使用 Microsoft Store 提交 API 中的多种方法更新软件包外部测试版提交。 ```RunFlightSubmissionUpdateSample```类中的方法创建新提交作为克隆的上次发布的提交，然后更新并提交到合作伙伴中心克隆的提交。 具体来说，```RunFlightSubmissionUpdateSample``` 方法执行以下任务：

1. 首先，此方法[获取指定软件包外部测试版的数据](get-a-flight.md)。
2. 接下来，此方法会[删除软件包外部测试版的挂起提交](delete-a-flight-submission.md)（如果存在）。
3. 然后，此方法[会创建新的软件包外部测试版提交](create-a-flight-submission.md)（新提交时是上次发布的提交副本）。
4. 它会将新的提交程序包上载到 Azure Blob 存储。
5. 接下来，该[更新](update-a-flight-submission.md)，然后[提交](commit-a-flight-submission.md)到合作伙伴中心的新提交。
6. 最后，该方法定期[检查新提交的状态](get-status-for-a-flight-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>IngestionClient 帮助程序类

```IngestionClient``` 类提供的帮助程序方法由示例应用中的其他方法用来执行以下任务：

* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，此访问令牌可用于在 Microsoft Store 提交 API 中调用方法。 获取令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Microsoft Store 提交 API。 该令牌到期后，可以重新生成一个。
* 将包含新的应用或加载项提交资源的 ZIP 存档上传至 Azure Blob 存储。 有关将 ZIP 存档上传至应用和加载项提交的 Azure Blob 存储的详细信息，请参阅 [创建应用提交](manage-app-submissions.md#create-an-app-submission) 和 [创建加载项提交](manage-add-on-submissions.md#create-an-add-on-submission) 中的相关说明。
* 处理 Microsoft Store 提交 API 的 HTTP 请求。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
