---
description: 在 Microsoft Store 分析 API 中使用此方法，可下载桌面应用程序中错误的 CAB 文件。
title: 下载桌面应用程序中错误的 CAB 文件
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 分析 API, 下载 CAB, 桌面应用程序
ms.localizationpriority: medium
ms.openlocfilehash: 1e3535f18b8127ea18bca234cdcc9b695e89ebfd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607782"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>下载桌面应用程序中错误的 CAB 文件

在 Microsoft Store 分析 API 使用此方法，可下载与已添加到 [Windows 桌面应用程序计划](https://msdn.microsoft.com/library/windows/desktop/mt826504)的桌面应用程序特定错误关联的 CAB 文件。 此方法仅可以下载过去 30 天内发生的应用错误的 CAB 文件。 CAB 文件下载也会出现在[运行状况报告](https://msdn.microsoft.com/library/windows/desktop/mt826504)合作伙伴中心中的桌面应用程序。

开始使用此方法之前，必须先使用[获取桌面应用程序中错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)方法来检索你想下载的 CAB 文件的 ID 哈希。

## <a name="prerequisites"></a>必备条件


若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 获取想要下载的 CAB 文件的 ID 哈希。 若要获取此值，请使用[获取桌面应用程序中的错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)方法来检索应用中特定错误的详细信息，并使用该方法的响应正文中的 **cabIdHash** 值。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  |
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要下载 CAB 文件的桌面应用程序的产品 ID。 若要获取的桌面应用程序的产品 ID，请打开任意[合作伙伴中心分析报告，以便您桌面应用程序](https://msdn.microsoft.com/library/windows/desktop/mt826504)(如**运行状况报告**)，并从 URL 检索产品 ID。 |  是  |
| cabIdHash | 字符串 | 想要下载的 CAB 文件的唯一 ID 哈希。 要获取此值，请使用[获取桌面应用程序中的错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)方法来检索应用程序中特定错误的详细信息，并使用该方法的响应正文中的 **cabIdHash** 值。 |  是  |


### <a name="request-example"></a>请求示例

以下示例演示如何使用此方法下载 CAB 文件。 将 *applicationId* 和 *cabIdHash* 值替换为桌面应用程序的相应值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

此方法将会返回一个 302（重定向）响应代码，并且会将响应中的 **Location** 标头分配给 CAB 文件的共享访问签名 (SAS) URI。 调用程序将被重定向至此 URI 以自动下载 CAB 文件。

## <a name="related-topics"></a>相关主题

* [运行状况报告](../publish/health-report.md)
* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取错误报告为桌面应用程序的数据](get-desktop-application-error-reporting-data.md)
* [获取在桌面应用程序中错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)
* [获取桌面应用程序中的错误的堆栈跟踪](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
