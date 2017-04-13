---
author: mcleanbyron
ms.assetid: 3D6EE7D7-7D75-499D-AA7A-55DA1C485BA6
description: "在 Windows 应用商店分析 API 中使用此方法，可下载 Windows 10 驱动程序错误的 CAB 文件。 此方法仅用于 IHV。"
title: "下载 Windows 10 驱动程序错误的 CAB 文件"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 应用商店分析 API, 下载 CAB"
ms.openlocfilehash: 9a8539a3d9d8f85fd9277165eb393eb372459b0a
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="download-the-cab-file-for-a-windows-10-driver-error"></a>下载 Windows 10 驱动程序错误的 CAB 文件

在 Windows 应用商店分析 API 中使用此方法，可下载与特定 Windows 10 驱动程序错误相关的 CAB 文件。 开始使用此方法之前，你必须使用[获取 Windows 10 驱动程序错误的详细信息](get-details-for-a-windows-10-driver-error.md)方法来检索想要下载的 CAB 文件的 ID。

你可以在 Windows 应用商店分析 API 中使用[获取 Windows 10 驱动程序的错误报告数据](get-error-reporting-data-for-windows-10-drivers.md)和[获取 Windows 10 驱动程序错误的详细信息](get-details-for-a-windows-10-driver-error.md)方法来获取有关 OEM 硬件错误的其他信息。

> [!NOTE]
> 此方法仅可供属于 [Windows 硬件开发人员中心计划](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的开发人员帐户使用。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 获取想要下载的 CAB 文件的 ID。 若要获取此 ID，请使用[获取 Windows 10 驱动程序错误的详细信息](get-details-for-a-windows-10-driver-error.md)方法来检索特定驱动程序错误的详细信息，并使用该方法的响应正文中的 **cabIdHash** 值。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload``` |

<span/> 

### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span/> 

### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必填  |
|---------------|--------|---------------|------|
| cabIdHash | 字符串 | 想要下载的 CAB 文件的唯一 ID。 若要获取此 ID，请使用[获取 Windows 10 驱动程序错误的详细信息](get-details-for-a-windows-10-driver-error.md)方法来检索应用中的特定错误的详细信息，并使用该方法的响应正文中的 **cabIdHash** 值。 |  是  |

<span/>
 
### <a name="request-example"></a>请求示例

以下示例演示了如何使用此方法下载 CAB 文件。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

此方法将会返回一个 302（重定向）响应代码，并且会将响应中的**位置**标题分配给 CAB 文件的共享访问签名 (SAS) URI。 此调用程序将被重定向至此 URI 以自动下载 CAB 文件。

## <a name="related-topics"></a>相关主题

* [获取 Windows 10 驱动程序的错误报告数据](get-error-reporting-data-for-windows-10-drivers.md)
* [获取 Windows 10 驱动程序错误的详细信息](get-details-for-a-windows-10-driver-error.md)
