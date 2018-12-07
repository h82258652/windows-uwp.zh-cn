---
description: 使用综合订阅源检索或创建最新和最热门的 Web 内容，这些订阅源通过 Windows.Web.Syndication 命名空间中的功能根据 RSS 和 Atom 标准生成。
title: RSS/Atom 源
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5b5312614c7060118fdb4678aa80ae51d6734486
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794167"
---
# <a name="rssatom-feeds"></a>RSS/Atom 源


**重要的 API**

-   [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)
-   [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)
-   [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)

使用综合订阅源检索或创建最新和最热门的 Web 内容，这些订阅源是使用 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空间中的功能根据 RSS 和 Atom 标准生成的。

## <a name="what-is-a-feed"></a>什么是订阅源？

Web 订阅源是一个文档，其中包含任意数量的由文本、链接和图像所组成的单个条目。 对订阅源的更新是以新条目的形式来进行的，这些新条目用于在整个 Web 上推广最新的内容。 内容消费者可以使用订阅源阅读器应用汇总和监视来自任何数量的单个内容作者的订阅源，并快速而方便地获取对最新内容的访问。

## <a name="which-feed-format-standards-are-supported"></a>支持哪些订阅源格式标准？

通用 Windows 平台 (UWP) 支持从 RSS 0.91 到 RSS 2.0 的 RSS 格式标准的订阅源检索，也支持从 0.3 到 1.0 的 Atom 标准的订阅源检索。 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空间中的类可定义订阅源和能够表示 RSS 和 Atom 元素的订阅源项目。

此外，Atom 1.0 和 RSS 2.0 格式都允许各自的订阅源文档包含在正式规范中未定义的元素或属性。 随着时间的推移，这些自定义元素和属性已经成为其他 Web 服务数据格式（如 GDATA 和 OData）用于指定域的特定信息的一种方式。 为了支持这一新增功能，[**SyndicationNode**](https://msdn.microsoft.com/library/windows/apps/br243585) 类表示泛型 XML 元素。 通过结合使用 **SyndicationNode** 和 [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819) 命名空间中的类，应用可以访问属性、扩展以及它们可能包含的任何内容。

请注意，对于综合内容的发布，根据 Atom 和 Atom Publication 标准，Atom 发布协议的 UWP 实现 ([**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)) 仅支持订阅源内容操作。

## <a name="using-syndicated-content-with-network-isolation"></a>使用带有网络隔离功能的综合内容

UWP 中的网络隔离功能使开发人员能够控制和限制 UWP 应用的网络访问。 并非所有的应用都需要访问网络。 然而，对于那些需要访问网络的应用，UWP 通过选择适当的功能为这些应用提供不同级别的网络访问权限。

网络隔离允许开发人员为每个应用定义所需网络访问的范围。 没有指定相应范围的应用被阻止访问特定类型的网络和特定类型的网络请求（出站客户端发起的请求或未经请求的入站请求和出站客户端发起的请求）。 设置并强制执行网络隔离功能可确保如果一个应用变得具有威胁，则该应用只能访问已明确授权它访问的网络。 这大大降低了对其他应用程序和 Windows 的范围影响。

网络隔离功能可影响尝试访问网络的 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 和 [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609) 命名空间中的任何类元素。 Windows 会主动强制实现网络隔离。 如果尚未启用相应的网络功能，则调用 **Windows.Web.Syndication** 或 **Windows.Web.AtomPub** 命名空间中的类元素可能会因为网络隔离导致网络访问失败。

在生成应用时，在应用清单中配置其网络功能。 网络功能通常添加开发应用时使用 Microsoft Visual Studio2015。 也可使用文本编辑器在应用清单文件中手动设置网络功能。

有关网络隔离和网络功能的详细信息，请参阅[网络基础知识](networking-basics.md)主题中的“功能”部分。

## <a name="how-to-access-a-web-feed"></a>如何访问 Web 订阅源

此部分展示了如何在采用 C# 或 Javascript 编写的 UWP 应用中使用 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空间中的类检索和显示 Web 订阅源。

**先决条件**

若要确保你的 UWP 应用能够使用网络，必须设置在项目 **Package.appxmanifest** 文件中所需的任何网络功能。 如果你的应用需要作为客户端连接到 Internet 上的远程服务，则需要具有 **“internetClient”** 功能。 有关详细信息，请参阅[网络基础知识](networking-basics.md)主题中的“功能”部分。

**检索来自 Web 订阅源的综合内容**

现在，我们将查看一些代码，用于演示如何检索订阅源，然后显示订阅源所包含的每个单独项。 在我们可以配置和发送请求前，我们将定义一些将在操作期间使用的变量，并初始化 [**SyndicationClient**](https://msdn.microsoft.com/library/windows/apps/br243456) 的实例，该实例定义我们将用于检索和显示订阅源的方法和属性。

如果传递给 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) 构造函数的 *uriString* 不是有效 URI，该构造函数将引起异常。 因此，我们使用 try/catch 块验证 *uriString*。

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;
// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;
// Use your own uriString for the feed you are connecting to.
string uriString = "";
try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
var client = new Windows.Web.Syndication.SyndicationClient();
// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

接下来，我们通过设置所有需要的服务器凭据（[**ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) 属性）、代理凭据（[**ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) 属性）和 HTTP 标头（[**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br243462) 方法）来配置请求。 已配置基本请求参数，即有效的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) 对象，它是使用应用提供的订阅源 URI 字符串创建的。 然后，**Uri** 对象将传递给 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) 函数以请求订阅源。

假设所需订阅源内容已返回，示例代码将在每个订阅源项上循环，从而调用 **displayCurrentItem**（将在下面定义），以通过 UI 以列表形式显示这些项及其内容。

当你调用大部分异步网络方法时，必须编写代码以处理异常。 异常处理程序可以检索关于异常原因的更详细的信息，以便更好地了解此次失败，然后作出正确的决策。

如果不能与 HTTP 服务器建立连接，或者 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br243460) 对象没有指向有效的 AtomPub 或 RSS 订阅源，[**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br226017) 方法会引发异常。 如果发生错误，Javascrip 示例代码将使用 **onError** 函数捕捉任何异常，并打印出关于异常的更详细的信息。

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    feed = await client.RetrieveFeedAsync(uri);
    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;
    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");
    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}
// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;
        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");
        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;
        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }
        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

在上面的步骤中，[**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) 返回了请求的订阅源内容，并且示例代码已在可用的订阅源项上实施循环访问。 这些项的每个项将使用 [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) 对象表示，其中包含相关联合标准（RSS 或 Atom）提供的所有项属性和内容。 在以下示例中，我们将看到作用于每个项和通过各种命名的 UI 元素显示其内容的 **displayCurrentItem** 函数。

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.
```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];
    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;
    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;
    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }
    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;
    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

如上面所述，根据用于发布订阅源的不同订阅源标准（RSS 或 Atom），[**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) 对象所表示的内容类型会有所不同。 例如，Atom 订阅源能够提供 [**Contributors**](https://msdn.microsoft.com/library/windows/apps/br243540) 的列表，但 RSS 订阅源则不能。 但是，不受任一标准支持的包含在订阅源项中的扩展元素（例如 Dublin Core 扩展元素）可以通过使用 [**SyndicationItem.ElementExtensions**](https://msdn.microsoft.com/library/windows/apps/br243543) 属性进行访问并随后显示，如以下示例代码所示。

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";
    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;
        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }
    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }
    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource
    });
}
```
