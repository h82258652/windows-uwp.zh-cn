---
description: 依据 HTTP 2.0 和 HTTP 1.1 协议，使用 HttpClient 和其余的 Windows.Web.Http 命名空间 API 发送和接收信息。
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 6/5/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb098aae346c7a81771262793f5f6a042d62d5a3
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721607"
---
# <a name="httpclient"></a>HttpClient

**重要的 Api**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

依据 HTTP 2.0 和 HTTP 1.1 协议，使用 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 和其余的 [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空间 API 发送和接收信息。

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient 和 Windows.Web.Http 命名空间概述

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空间及相关 [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 和 [**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters) 命名空间中的类为充当 HTTP 客户端的通用 Windows 平台 (UWP) 应用提供了一个编程接口，以便于执行基本 GET 请求或实现下面列出的更高级的 HTTP 功能。

-   执行常见操作（**DELETE**、**GET**、**PUT** 和 **POST**）的方法。 上述每种请求都作为异步操作进行发送。

-   支持常见的身份验证设置和模式。

-   访问有关传输的安全套接字层 (SSL) 详细信息。

-   高级应用随附自定义筛选器的功能。

-   获取、设置和删除 Cookie 的功能。

-   异步方法上提供的 HTTP 请求进度信息。

[  **Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) 类用于声明由 [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 发送的 HTTP 请求消息。 [  **Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) 类用于声明从 HTTP 请求接收到的 HTTP 响应消息。 HTTP 消息由 IETF 在 [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) 中进行定义。

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空间用于声明 HTTP 内容作为 HTTP 实体正文和包含 Cookie 的标头。 HTTP 内容可以与 HTTP 请求或 HTTP 响应相关联。 **Windows.Web.Http** 命名空间提供很多不同的类来声明 HTTP 内容。

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent)。 缓冲区形式的内容
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent)。 使用 **application/x-www-form-urlencoded** MIME 类型编码的名称和值元组形式的内容
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent)。 中的窗体的内容**多部分 /\***  MIME 类型。
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent)。 编码为 **multipart/form-data** MIME 类型的内容。
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent)。 流（供 HTTP GET 方法接收数据和 HTTP POST 方法上载数据使用的内部类型）形式的内容
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent)。 字符串形式的内容。
-   [**IHttpContent** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) -开发人员能够创建他们自己的内容对象的基接口

“通过 HTTP 发送简单的 GET 请求”部分中的代码段使用 [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent) 类，以字符串的形式表示来自 HTTP GET 请求的 HTTP 响应。

[  **Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 命名空间支持创建 HTTP 标头和 Cookie，然后再将生成的 HTTP 标头和 Cookie 作为属性与 [**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) 和 [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) 对象相关联。

## <a name="send-a-simple-get-request-over-http"></a>通过 HTTP 发送简单的 GET 请求

正如本文前面提到的，[**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空间允许 UWP 应用发送 GET 请求。 下面的代码段演示如何将发送到 GET 请求 http://www.contoso.com使用[ **Windows.Web.Http.HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类和[ **Windows.Web.Http.HttpResponseMessage** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)类，以从 GET 请求读取响应。

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

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>通过 HTTP POST 二进制数据

[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)下面的代码示例演示如何使用窗体数据和 POST 请求发送少量的二进制数据作为文件上传到 web 服务器。 该代码使用[ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent)类来表示二进制数据，并且[ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent)类表示多部分窗体数据。

> [!NOTE]
> 调用**获取**（如下面的代码示例所示） 并不适合 UI 线程。 要在这种情况下使用的正确技术，请参阅[并发和异步操作与C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

若要发布的实际二进制文件 （而非显式上面使用的二进制数据） 的内容，您会发现它更轻松地使用[HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent)对象。 构造一个，并为其构造函数的参数，传递从调用返回的值[StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync)。 该方法返回的二进制文件中的数据的流。

此外，如果要上传大型文件 （大于约 10 MB），则建议使用 Windows 运行时[后台传输](/uwp/api/windows.networking.backgroundtransfer)Api。

## <a name="post-json-data-over-http"></a>通过 HTTP POST JSON 数据

以下示例将发布到的终结点，一些 JSON，然后写出响应正文。

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http 中的异常

将统一资源标识符 (URI) 的无效字符串传递给 [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 对象的构造函数时，将引发异常。

**.NET:**    [ **Windows.Foundation.Uri** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)类型显示为[ **System.Uri** ](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN)中C#和VB。

在 C# 和 Visual Basic 中，通过使用 .NET 4.5 中的 [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) 类和 [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads) 方法之一在构造 URI 之前测试从用户收到的字符串，可以避免该错误。

在 C++ 中，没有可用于试用字符串和将其解析到 URI 的方法。 如果应用获取 [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 用户输入，则构造函数应位于 try/catch 块中。 如果引发了异常，该应用可以通知用户并请求新的主机名。

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 缺少方便函数。 所以，使用 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 和该命名空间中其他类的应用需要使用 **HRESULT** 值。

使用.NET Framework 4.5 中的应用中C#，VB.NET [System.Exception](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN)发生异常时在应用程序执行过程中表示的错误。 [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult) 属性将返回分配到特定异常的 **HRESULT**。 [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message) 属性将返回用于描述异常的消息。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 应用可以筛选特定 **HRESULT** 值来根据异常原因修改应用行为。

在使用托管的 C++ 的应用中发生异常时，[Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) 表示应用执行期间的错误。 [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) 属性将返回分配到特定异常的 **HRESULT**。 [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) 属性将返回系统提供的与 **HRESULT** 值关联的字符串。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 应用可以筛选特定 **HRESULT** 值来根据异常原因修改应用行为。

对于大多数参数验证错误， **HRESULT**返回是**E\_INVALIDARG**。 对于某些非法方法调用， **HRESULT**返回是**E\_非法\_方法\_调用**。

