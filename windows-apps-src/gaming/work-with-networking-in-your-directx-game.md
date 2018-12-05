---
title: 游戏联网
description: 了解如何在你的 DirectX 游戏中开发并融入联网功能。
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 网络, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2697564e703cfe290e33f204125346f3e8bad8ac
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741029"
---
# <a name="networking-for-games"></a>游戏网络



了解如何在你的 DirectX 游戏中开发并融入联网功能。

## <a name="concepts-at-a-glance"></a>简要介绍相关的概念


无论你的 DirectX 游戏是简单的独立游戏，还是大规模多玩家游戏，都可以使用各种各样的网络功能。 对网络最简单的利用就是将用户名和游戏得分存储在中心网络服务器上。

使用基础结构（客户端服务器或 Internet 对等）模型的多玩家游戏和临时（本地对等）游戏都需要网络 API。 对于基于服务器的多玩家游戏，中心游戏服务器通常处理大部分游戏操作，而客户端游戏应用用于输入、显示图形、播放音频以及其他功能。 要提供令人满意的游戏体验，需要考虑网络传输的速度和延迟。

对于对等游戏，每个玩家的应用都会处理输入和图形。 在大多数情况下，游戏玩家位于邻近位置，因此网络延迟应该会降低，但仍需要考虑这个问题。 需要考虑如何发现对等方并建立连接。

对于单个玩家的游戏，中心 Web 服务器或服务通常用于存储用户名、游戏得分和其他信息。 在这些游戏中，不需要过多考虑网络传输的速度和延迟，因为它不会直接影响游戏操作。

由于网络条件可能随时发生更改，因此任何使用网络 API 的游戏都需要处理可能发生的网络异常。 若要了解有关处理网络异常的详细信息，请参阅[网络基础知识](https://msdn.microsoft.com/library/windows/apps/mt280233)。

防火墙和 Web 代理十分常见，它们可以影响使用网络功能的能力。 需要为使用网络的游戏做好准备，使其正确处理防火墙和代理。

对于移动设备，重要的是在按流量计费的网络上监视可用的网络资源，并执行相应的操作；在按流量计费的网络上可能发生大量漫游或产生大量数据成本。

网络隔离是 Windows 所使用的应用安全模型的组成部分。 Windows 会主动发现网络边界，并为网络隔离强制实现网络访问限制。 为了定义网络访问范围，应用必须声明网络隔离功能。 如果没有声明这些功能，你的应用将不能访问网络资源。 若要 了解有关 Windows 如何为应用强制执行网络隔离的详细信息，请参阅[如何配置网络隔离功能](https://msdn.microsoft.com/library/windows/apps/hh770532)。

## <a name="design-considerations"></a>设计注意事项


在 DirectX 游戏中可以使用各种各样的网络 API。 因此，选择正确的 API 非常重要。 Windows 支持各种各样的网络 API， 你的应用可以使用它们通过 Internet 或专用网络与其他计算机和设备进行通信。 第一个步骤是确定你的应用需要哪些网络功能。

用于游戏的较受欢迎的网络 API 包括：

-   TCP 和套接字 – 提供可靠连接。 可以将 TCP 用于不需要安全性的游戏操作。 因为 TCP 允许服务器轻松缩放，所以它通常用于使用基础结构（客户端服务器或 Internet 对等）模型的游戏。 临时（本地对等）游戏也可以通过 Wi-Fi Direct 和 BlueTooth 使用 TCP。 TCP 通常用于游戏对象移动、角色交互、文本聊天和其他操作。 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)类提供 TCP 套接字，可在 Microsoft Store 游戏中。 **StreamSocket** 类与 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间中的相关类搭配使用。
-   使用 SSL 的 TCP 和套接字 – 提供防止窃听的可靠连接。 将带有 SSL 的 TCP 连接用于需要安全性的游戏操作。 由于 SSL 的加密和开销将导致延迟并降低性能，所以仅在需要安全性时使用它。 带有 SSL 的 TCP 通常用于登录、购买和交易资源、游戏角色创建和管理。 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 类提供支持 SSL 的 TCP 套接字。
-   UDP 和套接字 – 提供具有较低开销的不可靠网络传输。 UDP 用于要求较少延迟，但可以容忍一些数据包丢失的游戏操作。 它经常用于打斗游戏、射击和跟踪、网络音频以及语音聊天。 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)类提供 UDP 套接字，可在 Microsoft Store 游戏中。 **DatagramSocket** 类与 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间中的相关类搭配使用。
-   HTTP 客户端 – 提供到 HTTP 服务器的可靠连接。 最常见的网络方案是访问网站来检索或存储信息。 一个简单的示例是使用网站来存储用户信息和游戏得分的游戏。 当与 SSL 一起使用来确保安全性时，HTTP 客户端可以用于登录、购买、交易资源、游戏角色创建以及管理。 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)类提供的现代 HTTP 客户端 API 使用的 Microsoft 应用商店游戏中。 **HttpClient** 类与 [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间中的相关类搭配使用。

## <a name="handling-network-exceptions-in-your-directx-game"></a>在 DirectX 游戏中处理网络异常


当 DirectX 游戏中发生网络异常时，这表示存在重大问题或故障。 使用网络 API 时，许多原因会导致发生异常。 使用远程主机或服务器时，网络连接发生更改或其他网络问题通常会导致异常。

使用网络 API 时导致异常的一些原因包括以下几条：

-   用户输入的主机名或 URI 包含错误而且无效。
-   在查找主机名或 URI 时名称解析失败。
-   网络连接中断或发生更改。
-   使用套接字或 HTTP 客户端 API 连接网络失败
-   网络服务器或远程终结点错误。
-   其他网络错误。

随时可能由网络错误（例如，连接中断或发生更改、连接失败和服务器失败）引发异常。 这些错误将引发异常。 如果应用不处理异常，它可能导致整个应用在运行时终止。

当你调用大部分异步网络方法时， 必须编写代码以处理异常。 有时，在发生异常时，可以重试网络方法来解决问题。 在其他时候，应用可能需要计划使用之前的缓存数据在没有网络连接的情况下继续工作。

通用 Windows 平台 (UWP) 应用通常引发单个异常。 异常处理程序可以检索有关异常原因的更详细信息，以更好地了解此次失败，并作出适当的决策。

当在属于 UWP 应用的 DirectX 游戏中发生异常时，可以检索表示错误原因的 **HRESULT** 值。 *Winerror.h* include file 包含一个 **HRESULT** 可能值的大型列表，其中包括网络错误。

网络 API 支持使用不同方法来检索有关异常原因的详细信息。

-   用于检索导致异常的错误的 **HRESULT** 值的方法。 可能的 **HRESULT** 值的列表较大且尚未指定。 当使用任何网络 API 时，可以检索 **HRESULT** 值。
-   可以将 **HRESULT** 值转换为枚举值的帮助程序方法。 已指定可能枚举值的列表，该列表相对较小。 帮助程序方法可用于 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 中的套接字类。

### <a name="exceptions-in-windowsnetworkingsockets"></a>Windows.Networking.Sockets 中的异常

如果传递的字符串不是有效的主机名（包含主机名中不允许的字符），则与套接字一起使用的 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 类的构造函数会引发异常。 如果应用从用户处获取了游戏对等方连接的 **HostName** 输入，则构造函数应位于 try/catch 块中。 如果引发了异常，该应用可以通知用户并请求新的主机名。

添加代码以验证用户输入的用于主机的字符串

```cpp

    // Define some variables at the class level.
    Windows::Networking::HostName^ remoteHost;

    bool isHostnameFromUser = false;
    bool isHostnameValid = false;

    ///...

    // If the value of 'remoteHostname' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^hostString = remoteHostname;

    try 
    {
        remoteHost = ref new Windows::Networking:Host(hostString);
        isHostnameValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
        return;
    }

    isHostnameFromUser = true;


    // ... Continue with code to execute with a valid hostname.
```

[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间具有方便的帮助程序方法和枚举，以便在使用套接字时处理错误。 这有助于在应用中分别处理特定网络异常。

[**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)、[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 或 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 操作上发生的错误将引发异常。 异常原因是一个错误值，表示为 **HRESULT** 值。 [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) 方法用于将来自套接字操作的网络错误转换为 [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457) 枚举值。 大部分 **SocketErrorStatus** 枚举值对应由本机 Windows 套接字操作返回的错误。 应用可以筛选特定 **SocketErrorStatus** 枚举值来基于异常原因修改应用行为。

对于参数验证错误，应用还可以使用来自异常的 **HRESULT** 了解有关导致该异常的错误的更详细信息。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 对于大多数参数验证错误，返回的 **HRESULT** 为 **E\_INVALIDARG**。

尝试连接流套接字时，添加处理异常的代码

```cpp
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;

    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http 中的异常

如果传递的字符串不是有效的 URI（包含 URI 中不允许的字符），则与 [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 一起使用的 [**Windows::Foundation::Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 类的构造函数会引发异常。 在 C++ 中，没有可用于试用字符串和将其解析到 URI 的方法。 如果应用获取的用户输入提供给 **Windows::Foundation::Uri**，则构造函数应位于 try/catch 块中。 如果引发了异常，该应用可以通知用户并请求新的 URI。

因为 [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 仅支持 HTTP 和 HTTPS 架构，所以你的应用还应该检查 URI 中的架构是 HTTP 还是 HTTPS。

添加代码以验证用户输入的用于 URI 的字符串

```cpp

    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

[**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/windows.web.http.aspx) 命名空间缺少方便函数。 所以，使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 和该命名空间中其他类的应用需要使用 **HRESULT** 值。

在使用 C++ 的应用中，发生异常时，[**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) 表示应用执行期间的错误。 [**Platform::Exception::HResult**](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) 属性将返回分配给特定异常的 **HRESULT**。 [**Platform::Exception::Message**](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) 属性将返回系统提供的与 **HRESULT** 值关联的字符串。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 应用可以筛选特定 **HRESULT** 值来基于异常原因修改应用行为。

对于大多数参数验证错误，返回的 **HRESULT** 为 **E\_INVALIDARG**。 对于非法的方法调用，返回的 **HRESULT** 为 **E_ILLEGAL_METHOD_CALL**。

当尝试使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 连接到 HTTP 服务器时，添加处理异常的代码

```cpp
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
    

```

## <a name="related-topics"></a>相关主题


**其他资源**

* [使用数据报套接字进行连接](https://msdn.microsoft.com/library/windows/apps/xaml/jj635238)
* [借助流套接字连接到网络资源](https://msdn.microsoft.com/library/windows/apps/xaml/jj150599)
* [连接到网络服务](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
* [连接到 Web 服务](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
* [网络基础知识](https://msdn.microsoft.com/library/windows/apps/mt280233)
* [如何配置网络隔离功能](https://msdn.microsoft.com/library/windows/apps/hh770532)
* [如何启用环回和调试网络隔离](https://msdn.microsoft.com/library/windows/apps/hh780593)

**引用**

* [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)
* [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
* [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)
* [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
* [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

**示例**

* [DatagramSocket 示例](http://go.microsoft.com/fwlink/p/?LinkID=243037)
* [HttpClient 示例]( http://go.microsoft.com/fwlink/p/?linkid=242550)
* [邻近感应示例](http://go.microsoft.com/fwlink/p/?linkid=245082)
* [StreamSocket 示例](http://go.microsoft.com/fwlink/p/?linkid=243037)
