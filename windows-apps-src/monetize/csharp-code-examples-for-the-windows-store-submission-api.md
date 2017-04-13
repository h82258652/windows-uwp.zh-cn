---
author: mcleanbyron
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: "使用此部分中的 C# 代码示例了解有关使用 Windows 应用商店提交 API 的详细信息。"
title: "提交 API 的 C# 代码示例"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 应用商店提交 API, 代码示例"
ms.openlocfilehash: 59b9c0b2cc503a56e0a1c9a75ce5ef471983c699
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="c-code-examples-for-the-submission-api"></a>提交 API 的 C\# 代码示例

本文提供使用 *Windows 应用商店提交 API* 的 C# 代码示例。 有关此 API 的详细信息，请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

这些代码示例演示了以下任务：

* [更新应用提交](#update-app-submission)
* [创建新加载项提交](#create-add-on-submission)
* [更新加载项提交](#update-add-on-submission)
* [更新软件包外部测试版提交](#update-flight-submission)

你可以查看每个示例，了解有关它所演示的任务的详细信息，也可以将本文中的所有代码示例生成到控制台应用程序。 若要构建示例，请在 Visual Studio 中创建名为 **DeveloperApiCSharpSample** 的 C# 控制台应用程序、将每个示例复制到项目中单独的代码文件，然后构建该项目。

## <a name="prerequisites"></a>先决条件

这些示例使用以下库：

* Microsoft.WindowsAzure.Storage.dll。 此库在[用于 .NET 的 Azure SDK](https://azure.microsoft.com/downloads/) 中提供，或者可以通过安装 [WindowsAzure.Storage NuGet 程序包](https://www.nuget.org/packages/WindowsAzure.Storage)获取。
* Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json)。

## <a name="main-program"></a>主计划

以下示例实现了一个命令行计划，该计划可调用本文中的其他示例方法演示使用 Windows 应用商店提交 API 的不同方法。 若要调整此程序以供自己使用，请执行以下操作：

* 为你想管理的应用 ID、加载项（加载项也称为应用内产品或 IAP）和软件包外部测试版分配 ```ApplicationId```、```InAppProductId``` 和 ```FlightId``` 属性。 开发人员中心仪表板中会提供这些 ID。
* 将 ```ClientId``` 和 ```ClientSecret``` 属性指定为你应用的客户端 ID 和密钥，并将 ```TokenEndpoint``` URL 中的 *tenantid* 字符串更换为你应用的租户 ID。 有关详细信息，请参阅 [如何将 Azure AD 应用程序与你的 Windows 开发人员中心帐户相关联](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />
## <a name="clientconfiguration-helper-class"></a>ClientConfiguration 帮助程序类

示例应用使用 ```ClientConfiguration``` 帮助程序类将 Azure Active Directory 数据和应用传递到每个使用 Windows 应用商店提交 API 的示例方法。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="update-app-submission" />
## <a name="update-an-app-submission"></a>更新应用提交

以下示例实现了一个类，该类使用 Windows 应用商店提交 API 中的多种方法更新应用提交。 该类中的 ```RunAppSubmissionUpdateSample``` 方法创建新提交作为上次发布的提交的克隆，然后将克隆的提交更新并提交到 Windows 开发人员中心。 具体来说，```RunAppSubmissionUpdateSample``` 方法执行以下任务：

1. 首先，此方法[获取指定应用的数据](get-an-app.md)。
2. 接下来，此方法会[删除应用的挂起提交](delete-an-app-submission.md)（如果存在）。
3. 然后，此方法会[创建新的应用提交](create-an-app-submission.md)（新提交时是上次发布的提交副本）。
4. 它会更改新提交的部分详细信息并将新的提交包上载到 Azure Blob 存储。
5. 接下来，它会[更新](update-an-app-submission.md)并将新提交[提交](commit-an-app-submission.md)到 Windows 开发人员中心。
6. 最后，该方法会定期 [检查新提交的状态](get-status-for-an-app-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />
## <a name="create-a-new-add-on-submission"></a>创建新加载项提交

以下示例实现了一个类，该类使用 Windows 应用商店提交 API 中的多种方法创建新的加载项提交。 该类中的 ```RunInAppProductSubmissionCreateSample``` 方法执行以下任务：

1. 首先，该方法会 [创建新加载项](create-an-add-on.md)。
2. 然后，该方法会 [创建新的加载项提交](create-an-add-on-submission.md)。
3. 该方法会将包含提交图标的 ZIP 存档上传至 Azure Blob 存储。
4. 接下来，该方法会  [将新提交提交到 Windows 开发人员中心](commit-an-add-on-submission.md)。
5. 最后，该方法会定期 [检查新提交的状态](get-status-for-an-add-on-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />
## <a name="update-an-add-on-submission"></a>更新加载项提交

以下示例实现了一个类，该类使用 Windows 应用商店提交 API 中的多种方法更新现有的加载项提交。 该类中的 ```RunInAppProductSubmissionUpdateSample``` 方法创建新提交作为上次发布的提交的克隆，然后将克隆的提交更新并提交到 Windows 开发人员中心。 具体来说，```RunInAppProductSubmissionUpdateSample``` 方法执行以下任务：

1. 首先，此方法[获取指定加载项的数据](get-an-add-on.md)。
2. 接下来，此方法会[删除加载项的挂起提交](delete-an-add-on-submission.md)（如果存在）。
3. 然后，该方法会 [创建新的加载项提交](create-an-add-on-submission.md)（新提交是上次发布的提交的副本）。
5. 接下来，该方法会 [更新](update-an-add-on-submission.md)并将新提交 [提交](commit-an-add-on-submission.md) 到 Windows 开发人员中心。
6. 最后，该方法会定期 [检查新提交的状态](get-status-for-an-add-on-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="update-flight-submission" />
## <a name="update-a-package-flight-submission"></a>更新软件包外部测试版提交

以下示例实现了一个类，该类使用 Windows 应用商店提交 API 中的多种方法更新软件包外部测试版提交。 该类中的 ```RunFlightSubmissionUpdateSample``` 方法创建新提交作为上次发布的提交的克隆，然后将克隆的提交更新并提交到 Windows 开发人员中心。 具体来说，```RunFlightSubmissionUpdateSample``` 方法执行以下任务：

1. 首先，此方法[获取指定软件包外部测试版的数据](get-a-flight.md)。
2. 接下来，此方法会[删除软件包外部测试版的挂起提交](delete-a-flight-submission.md)（如果存在）。
3. 然后，此方法[会创建新的软件包外部测试版提交](create-a-flight-submission.md)（新提交时是上次发布的提交副本）。
4. 它会将新的提交程序包上载到 Azure Blob 存储。
5. 接下来，它会[更新](update-a-flight-submission.md)并将新提交[提交](commit-a-flight-submission.md)到 Windows 开发人员中心。
6. 最后，该方法会定期 [检查新提交的状态](get-status-for-a-flight-submission.md)，直到其成功提交。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />
## <a name="ingestionclient-helper-class"></a>IngestionClient 帮助程序类

```IngestionClient``` 类提供了相同应用中的其他方法使用的帮助程序方法来执行以下任务：

* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，此访问令牌可用于在 Windows 应用商店提交 API 中调用方法。 获取访问令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Windows 应用商店提交 API。 该令牌到期后，可以重新生成一个。
* 将包含新的应用或加载项提交资源的 ZIP 存档上传至 Azure Blob 存储。 有关将 ZIP 存档上传至应用和加载项提交的 Azure Blob 存储的详细信息，请参阅 [创建应用提交](manage-app-submissions.md#create-an-app-submission) 和 [创建加载项提交](manage-add-on-submissions.md#create-an-add-on-submission) 中的相关说明。
* 处理 Windows 应用商店提交 API 的 HTTP 请求。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
