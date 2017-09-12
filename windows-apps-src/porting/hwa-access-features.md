---
author: seksenov
title: "托管 Web 应用 - 访问通用 Windows 平台 (UWP) 功能和运行时 API"
description: "访问通用 Windows 平台 (UWP) 本机功能和 Windows 10 运行时 API，包括 Cortona 语音命令、动态磁贴、用于实现安全的 ACUR、OpenID、OAuth 以及来自远程 JavaScript 中的全部内容。"
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "托管的 Web 应用, 适用于 JavaScript 的 WinRT API, Win10 Web 应用, Windows JavaScript 应用, ApplicationContentUriRules, ACUR, msapplication-cortanavcd, 适用于 Web 应用的 Cortana"
ms.openlocfilehash: 86661353916e64cb2ed4d7f0ca7b8830bfe95685
ms.sourcegitcommit: a704e3c259400fc6fbfa5c756c54c12c30692a31
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2017
---
# <a name="accessing-uwp-features"></a>访问 UWP 功能

你的 Web 应用程序可以具有对通用 Windows 平台 (UWP) 的完全访问权限，从而可以激活 Windows 设备上的本机功能、[受益于 Windows 安全](#keep-your-app-secure--setting-application-content-uri-rules-acurs)、从托管在服务器上的脚本直接[调用 Windows 运行时 API](#call-windows-runtime-apis)、利用 [Cortana 集成](#integrate-cortana-voice-commands)，以及使用[联机验证提供程序](#web-authentication-broker)。 [混合应用](##create-hybrid-apps--packaged-web-apps-vs-hosted-web-apps)也受支持，因为你可以包含可从托管脚本调用的本地代码，并在远程页面和本地页面之间管理应用导航。

## <a name="keep-your-app-secure--setting-application-content-uri-rules-acurs"></a>确保应用安全 – 设置应用程序内容 URI 规则 (ACUR)

通过 ACUR（也称为 URL 允许列表），你可以实现对远程 HTML、CSS 和 JavaScript 中的通用 Windows API 的远程 URL 直接访问。 已在 Windows 操作系统级别设置正确策略边界，允许 Web 服务器上托管的代码直接调用平台 API。 当你将构成托管 Web 应用的一组 URL 置于应用程序内容 URI 规则 (ACUR) 中时，请在应用包清单中定义这些边界。 你的规则应包括应用起始页和你希望作为应用页包括进来的任何其他页面。 你也可以选择排除特定 URL。

可通过多种方式在规则中指定 URI 匹配：

- 确切的主机名
- 已包括或排除含有某个主机名的任何子域的 URI 对应的主机名
- 确切的 URI
- 可以包含查询属性的确切 URI
- 部分路径和用于指示包含规则的特定文件扩展名的通配符。
- 排除规则的相对路径

如果你的用户导航到一个未包含在规则中的 URL，Windows 会使用浏览器打开目标 URL。

下面是几个 ACUR 示例。

```HTML
<Application
Id="App"
StartPage="https://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## <a name="call-windows-runtime-apis"></a>调用 Windows 运行时 API

如果 URL 定义在应用的边界 (ACUR) 内，则它可以使用“WindowsRuntimeAccess”属性调用使用 JavaScript 的 Windows 运行时 API。 当具有相应访问权限的 URL 载入应用主机时，Windows 命名空间会注入并呈现在脚本引擎中。 这使应用的脚本可以直接调用通用 Windows API。 作为一名开发人员，只需对你想要调用的 Windows API 执行功能检测，如果有的话，继续执行以启用平台功能。

若要启用此功能，需要在 ACUR 中使用以下任一值指定 `(WindowsRuntimeAccess="<<level>>")` 属性：

- **all**：远程 JavaScript 代码有权访问所有 UWP API 和任何本地封装组件。
- **allowForWeb**：远程 JavaScript 代码只有权访问程序包中的自定义代码。 本地访问自定义 C++/C# 组件。
- **none**：默认值。 指定的 URL 不具有任何平台访问权限。

下面是一个规则类型示例：

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

这会向在 https://contoso.com/ 上运行的脚本授予访问 Windows 运行时命名空间和该程序包中的自定义封装组件的权限。 请参阅 GitHub 上的 [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) 示例，以获取 Toast 通知。

下面是一个如何实现动态磁贴以及通过远程 JavaScript 对它进行更新的示例：

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

此代码将生成一个如下所示的磁贴：

![Windows 10 调用动态磁贴](images/hwa-to-uwp/hwa_livetile.png)

无论使用何种环境或技术，调用 Windows 运行时 API 对你来说都非常熟悉，只需将你的资源保留在服务器功能上（这些服务器功能在调用 Windows 功能之前，会先对它们进行检测）。 如果平台功能不可用，并且 Web 应用在另一台主机上运行，你可以向用户提供适用于该浏览器的默认标准体验。

## <a name="integrate-cortana-voice-commands"></a>集成 Cortana 语音命令

你可以通过在 html 页面中指定语音命令定义 (VCD) 文件来充分利用 Cortana 集成。 VCD 文件是一个用于将命令映射到特定短语的 xml 文件。 例如，用户可以点击“开始”按钮并说出“Contoso Books，显示畅销商品”，该操作可以同时启动 Contoso Books 应用并导航至“畅销商品”页面。

当你添加可列出 VCD 文件位置的 `<meta>` 元素标记时，Windows 将自动下载并注册语音命令定义文件。

下面是该标记在托管 Web 应用的 html 页面中的用法示例：

```HTML
<meta name="msapplication-cortanavcd" content="https:// contoso.com/vcd.xml"/>
```

有关 Cortana 集成和 VCD 的详细信息，请参阅“Cortana 交互”和“语音命令定义 (VCD) 元素和属性 v1.2”。

## <a name="create-hybrid-apps--packaged-web-apps-vs-hosted-web-apps"></a>创建混合应用 - 封装 Web 应用与托管 Web 应用

你拥有用于创建 UWP 应用的选项。 应用可能设计为从 Windows 应用商店下载，并完全在本地客户端上托管，通常称为**封装 Web 应用**。 这允许你在任何兼容的平台上脱机运行应用。 或该应用可能是在远程 Web 服务器上运行的完全托管 Web 应用，通常称为**托管 Web 应用**。 但也存在第三个选项：应用的一部分可在本地客户端上托管，一部分在远程 Web 服务器上托管。 我们将第三个选项称为**混合应用**，并且它通常使用 **WebView** 组件使远程内容看起来像本地内容。 混合应用可包含 HTML5、CSS 和在本地应用客户端中作为程序包运行的 Javascript 代码，并保留与远程内容交互的能力。

## <a name="web-authentication-broker"></a>Web 身份验证代理

如果你具有使用 Internet 身份验证和授权协议（如 OpenID 或 OAuth）的联机身份提供商，则可以使用 Web 身份验证代理为你的用户处理登录流程。 可以在你的应用的 html 页面上的 `<meta>` 标记中指定起始和结束 URI。 Windows 将检测到这些 URI，并将其传递到 Web 身份验证代理来完成登录流程。 起始 URI 由以下内容组成：你将身份验证请求发送到联机提供商的地址，其中附有其他所需信息，例如应用 ID 、用户在完成身份验证后发送的重定向 URI 以及预期响应类型。 你可以从你的提供商处找到需要的参数。 下面是 `<meta>` 标记的用法示例：

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

有关更多指南，请参阅[联机提供商的 Web 身份验证代理注意事项](../security/web-authentication-broker.md)。

## <a name="app-capability-declarations"></a>应用功能声明

如果应用需要以编程方式访问用户资源（如图片或音乐）或设备（如相机或麦克风），你必须声明相应的功能。 三种应用功能声明类别如下所示： 

- 适用于大多数常见应用场景的[通用功能](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#general-use-capabilities)。 
- 允许你的应用访问外围设备和内部设备的[设备功能](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#device-capabilities)。 
- 需要使用特殊的公司帐户才能提交到应用商店以供使用的[特殊用途的功能](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities)。 

有关公司帐户的详细信息，请参阅[帐户类型、位置和费用](https://docs.microsoft.com/en-us/windows/uwp/publish/account-types-locations-and-fees)。

> [!NOTE]
> 请务必了解，当客户从 Windows 应用商店获取你的应用时，会向他们告知该应用声明的所有功能。 因此，不要使用你的应用不需要的功能。

可以通过在应用的[程序包清单](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)中声明功能来请求访问权限。 有关详细信息，请参阅[打包通用 Windows 平台 (UWP) 应用](https://docs.microsoft.com/en-us/windows/uwp/packaging/index)上的这些文章。

某些功能为应用提供对敏感资源的访问权限。 由于这些资源可用于访问用户个人数据或花费用户金钱，因此它们被认为是敏感资源。 由“设置”应用管理的隐私设置允许用户动态控制对敏感资源的访问权限。 因此，你的应用不会假设敏感资源始终可用，这一点很重要。 有关访问敏感资源的详细信息，请参阅[隐私感知应用指南](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx)。

## <a name="manifoldjs-and-the-app-manifest"></a>manifoldjs 和应用部件清单

将网站转换为 UWP 应用的简单方法是使用**应用部件清单**和 **manifoldjs**。 应用部件清单是一个包含有关该应用元数据的 xml 文件。 它指定诸如应用名称、指向资源的链接、显示模式、URL 和其他描述应如何部署和运行应用的数据等内容。 manifoldjs 使此过程变得非常简单，即使在不支持 Web 应用的系统上也是如此。 有关其工作原理的详细信息，请转到 [manifoldjs.com](http://www.manifoldjs.com/)。 还可查看作为此 [Windows 10 Web 应用演示文稿](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession)一部分的 manifoldjs 演示。

## <a name="related-topics"></a>相关主题
- [Windows 运行时 API：JavaScript 代码示例](https://microsoft.github.io/WindowsRuntimeAPIs_Javascript_snippets/)
- [Codepen：用于调用 Windows 运行时 API 的沙盒](http://codepen.io/seksenov/pen/wBbVyb/)
- [Cortana 交互](https://developer.microsoft.com/en-us/cortana)
- [语音命令定义 (VCD) 元素和属性 v1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [联机提供商的 Web 身份验证代理注意事项](https://docs.microsoft.com/en-us/windows/uwp/security/web-authentication-broker)
- [应用功能声明](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations)
