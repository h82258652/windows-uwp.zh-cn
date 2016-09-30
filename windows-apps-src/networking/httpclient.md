---
author: DelfCo
description: "依据 HTTP 2.0 和 HTTP 1.1 协议，使用 HttpClient 和其余的 Windows.Web.Http 命名空间 API 发送和接收信息。"
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b1908e83ffcab562c12c82cfcf7b5fe281d7ada1

---

# HttpClient

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

依据 HTTP 2.0 和 HTTP 1.1 协议，使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 和其余的 [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间 API 发送和接收信息。

## HttpClient 和 Windows.Web.Http 命名空间概述

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间及相关 [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) 和 [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623) 命名空间中的类为充当 HTTP 客户端的通用 Windows 平台 (UWP) 应用提供了一个编程接口，以便于执行基本 GET 请求或实现下面列出的更高级的 HTTP 功能。

-   执行常见操作（**DELETE**、**GET**、**PUT** 和 **POST**）的方法。 上述每种请求都作为异步操作进行发送。

-   支持常见的身份验证设置和模式。

-   访问有关传输的安全套接字层 (SSL) 详细信息。

-   高级应用随附自定义筛选器的功能。

-   获取、设置和删除 Cookie 的功能。

-   异步方法上提供的 HTTP 请求进度信息。

[**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) 类用于声明由 [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 发送的 HTTP 请求消息。 [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 类用于声明从 HTTP 请求接收到的 HTTP 响应消息。 HTTP 消息由 IETF 在 [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642) 中进行定义。

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间用于声明 HTTP 内容作为 HTTP 实体正文和包含 Cookie 的标头。 HTTP 内容可以与 HTTP 请求或 HTTP 响应相关联。 **Windows.Web.Http** 命名空间提供很多不同的类来声明 HTTP 内容。

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625)。 缓冲区形式的内容
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685)。 使用 **application/x-www-form-urlencoded** MIME 类型编码的名称和值元组形式的内容
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708)。 采用 **multipart/\*** MIME 类型格式的内容。
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596)。 编码为 **multipart/form-data** MIME 类型的内容。
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649)。 流（供 HTTP GET 方法接收数据和 HTTP POST 方法上载数据使用的内部类型）形式的内容
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661)。 字符串形式的内容。
-   [**IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684) - 供开发人员创建其自己的内容对象的基本界面

“通过 HTTP 发送简单的 GET 请求”部分中的代码段使用 [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) 类，以字符串的形式表示来自 HTTP GET 请求的 HTTP 响应。

[**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) 命名空间支持创建 HTTP 标头和 Cookie，然后再将生成的 HTTP 标头和 Cookie 作为属性与 [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) 和 [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 对象相关联。

## 通过 HTTP 发送简单的 GET 请求

正如本文前面提到的，[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间允许 UWP 应用发送 GET 请求。 以下代码段演示了如何使用 [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 和 [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 类读取来自 GET 请求的响应，来将 GET 请求发送到 http://www.contoso.com。

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

## Windows.Web.Http 中的异常

将统一资源标识符 (URI) 的无效字符串传递给 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 对象的构造函数时，将引发异常。

**.NET：**[**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 类型在 C# 和 VB 中显示为 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)。

在 C# 和 Visual Basic 中，通过使用 .NET 4.5 中的 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 类和 [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) 方法之一在构造 URI 之前测试从用户收到的字符串，可以避免该错误。

在 C++ 中，没有可用于试用字符串和将其解析到 URI 的方法。 如果应用获取 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 用户输入，则构造函数应位于 try/catch 块中。 如果引发了异常，该应用可以通知用户并请求新的主机名。

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 缺少方便函数。 所以，使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 和该命名空间中其他类的应用需要使用 **HRESULT** 值。

在采用 C#、VB.NET 编写的使用 .NET Framework 4.5 的应用中发生异常时，[System.Exception](http://msdn.microsoft.com/library/system.exception.aspx) 表示应用执行期间的错误。 [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx) 属性将返回分配到特定异常的 **HRESULT**。 [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx) 属性将返回用于描述异常的消息。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 应用可以筛选特定 **HRESULT** 值来根据异常原因修改应用行为。

在使用托管的 C++ 的应用中发生异常时，[Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx) 表示应用执行期间的错误。 [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx) 属性将返回分配到特定异常的 **HRESULT**。 [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx) 属性将返回系统提供的与 **HRESULT** 值关联的字符串。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 应用可以筛选特定 **HRESULT** 值来基于异常原因修改应用行为。

对于大多数参数验证错误，返回的 **HRESULT** 为 **E\_INVALIDARG**。 对于某些非法的方法调用，返回的 **HRESULT** 为 **E\_ILLEGAL\_METHOD\_CALL**。




<!--HONumber=Jun16_HO4-->


