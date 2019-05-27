---
title: 使用应用 URI 处理程序启用“网站的应用”功能
description: 通过支持“网站的应用”功能推动用户使用应用。
keywords: 深层链接 Windows
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 464aa35aa8362049042ad23a6816c4de21b25634
ms.sourcegitcommit: 559d3387d5929431212d147d9e085895e162916a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66184518"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>使用应用 URI 处理程序启用“网站的应用”功能

“网站”应用用于将你的应用与网站关联，从而使得在某人打开网站链接时，启动你的应用而不是打开浏览器。 如果未安装你的应用，则会像往常一样在浏览器中打开你的网站。 用户可以信任此体验，因为只有经验证的内容所有者可以注册链接。 用户将能够转到“设置”>“应用”>“网站的应用”检查其所有 Web 到应用链接。

若要启用 Web 到应用链接，你将需要：
- 在清单文件中标识你的应用将处理的 URI
- 用于定义你的应用与网站之间的关联的 JSON 文件。 带有应用程序包系列名称，位于应用部件清单声明所在的相同主机根上。
- 处理应用中的激活。

> [!Note]
> 从 Windows 10 创意者更新开始，支持的 Microsoft Edge 中单击的链接将启动相应的应用程序。 如果在其他浏览器（如 Internet Explorer 等）中点击受支持的链接，你将仍能够维持当前的浏览体验。

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>注册以处理应用部件清单中的 http 和 https 链接

你的应用需要针对它将处理的网站标识 URI。 若要执行此操作，请将 **Windows.appUriHandler** 扩展注册添加到你的应用部件清单文件 **Package.appxmanifest**。

例如，如果你的网站地址是“msn.com”，你将在应用部件清单中创建以下条目：

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

上述声明将你的应用注册为处理来自指定主机的链接。 如果你的网站具有多个地址 (例如： m.example.com、 www\.example.com 和 example.com) 然后添加一个单独`<uap3:Host Name=... />`内的条目`<uap3:AppUriHandler>`为每个地址。

## <a name="associate-your-app-and-website-with-a-json-file"></a>将你的应用和网站与 JSON 文件关联

若要确保只有你的应用可以打开你的网站上的内容，请将应用的程序包系列名称包含在某个 JSON 文件中，该文件位于 Web 服务器根中或域上的已知目录中。 这表示你的网站同意列出的应用打开你的站点上的内容。 可以在应用部件清单设计器中的程序包部分中查找程序包系列名称。

>[!Important]
> JSON 文件不应具有.json 文件后缀。

创建一个名为 **windows-app-web-link** 的 JSON 文件（不带有 .json 文件扩展名），并提供你的应用的程序包系列名称。 例如：

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows 将建立与你的网站的 https 连接，并将在你的 Web 服务器上查找相应的 JSON 文件。

### <a name="wildcards"></a>通配符

上述 JSON 文件示例演示了通配符的用法。 通配符允许你使用较少的代码行支持多种链接。 Web 到应用链接在 JSON 文件中支持两种类型的通配符：

| **Wildcard** | **说明**               |
|--------------|-------------------------------|
| **\***       | 表示任何子字符串      |
| **?**        | 表示单个字符 |

例如，给定上述示例中的 `"excludePaths" : [ "/news/*", "/blog/*" ]`，你的应用将支持以你的网站地址（例如 msn.com）开头的所有路径，**除了**`/news/` 和 `/blog/` 下的路径。 **msn.com/weather.html**将支持，但不是**msn.com/news/topnews.html**。

### <a name="multiple-apps"></a>多个应用

如果你有两个应用要链接到你的网站，请在 **windows-app-web-link** JSON 文件中列出这两个应用程序程序包系列名称。 这两个应用都可以受支持。 如果同时安装这两个应用，用户将需要选择哪个应用是默认链接。 如果他们希望在以后更改默认链接，可以在 **“设置”&gt;“应用网站”** 中更改它。 开发人员还可以随时更改 JSON 文件，并且最早在更新当天最晚在更新八天后就能看到更改。

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, e.g. MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

若要为用户提供最佳体验，请使用排除路径以确保仅联机内容已从 JSON 文件中的受支持路径中排除。

先检查排除路径，如果存在匹配项，则将使用浏览器（而不是指定应用）打开相应的页面。 在上述示例中，/news/\*包括任何页时该路径下的 / 新闻\*（没有正斜杠路线新闻） 包括在任何路径新闻\*如 newslocal /，newsinternational /，依次类推。

## <a name="handle-links-on-activation-to-link-to-content"></a>处理用于链接到内容的“激活”上的链接

在应用的 Visual Studio 解决方案中导航到 **App.xaml.cs**，并在 **OnActivated()** 中添加对链接内容的处理。 在以下示例中，在应用中打开的页面取决于 URI 路径：

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**重要提示** 请确保将最终的 `if (rootFrame.Content == null)` 路径替换为 `rootFrame.Navigate(deepLinkPageType, e);`，如上述示例所示。

## <a name="test-it-out-local-validation-tool"></a>它进行测试：本地验证工具

你可以测试应用和网站的配置，方法是运行在以下位置提供的应用主机注册验证程序工具：

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

通过使用以下参数运行此工具来测试应用和网站的配置：

**AppHostRegistrationVerifier.exe** *主机名 packagefamilyname 文件路径*

-   主机名：你的网站 (例如 microsoft.com)
-   包系列名称 (PFN):你的应用的 PFN
-   文件路径：本地验证的 JSON 文件 (例如 c:\\SomeFolder\\windows 应用的 web 链接)

如果工具未范围任何内容，则上传时对该文件进行验证即可。 如果返回错误代码，则不起作用。

你可以启用以下注册表项，以便将旁加载应用的路径匹配强制作为本地验证的一部分。

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

键名：`ForceValidation` 值： `1`

## <a name="test-it-web-validation"></a>对其进行测试：Web 验证

关闭应用程序以验证当你单击某个链接时应用是否激活。 然后，在网站中复制受支持路径之一的地址。 例如，如果你的网站地址为"msn.com"，而且支持路径之一是"path1"，则将使用 `http://msn.com/path1`

验证你的应用是否已关闭。 按 **Windows 键 + R** 打开 **“运行”** 对话框，并在窗口中粘贴该链接。 应启动你的应用，而不是 Web 浏览器。

此外，你可以通过使用 [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx) API 从其他应用启动你的应用来测试它。 你也可以使用此 API 在手机上进行测试。

如果你希望遵循协议激活逻辑，请在 **OnActivated** 事件处理程序中设置断点。

## <a name="appurihandlers-tips"></a>AppUriHandlers 提示：

- 请确保仅指定你的应用可以处理的链接。
- 列出你将支持的所有主机。  请注意该 www\.example.com 和 example.com 是不同的主机。
- 用户可以在“设置”中选择他们更希望哪个应用处理网站。
- 你的 JSON 文件必须上载到 https 服务器。
- 如果你需要更改你希望支持的路径，可以重新发布你的 JSON 文件，而无需重新发布你的应用。 用户将在 1-8 天内看到更改。
- 带有 AppUriHandlers 的所有旁加载应用都将在安装时验证主机的链接。 不需要上载 JSON 文件即可测试该功能。
- 只要你的应用是使用 [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx) 启动的 UWP 应用或使用 [ShellExecuteEx](https://msdn.microsoft.com/library/windows/desktop/bb762154(v=vs.85).aspx) 启动的 Windows 桌面应用，此功能就可行。 如果 URL 对应于注册的应用 URI 处理程序，则将启动应用，而不是浏览器。

## <a name="see-also"></a>请参阅

[Web 到应用示例项目](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[windows.protocol 注册](https://msdn.microsoft.com/library/windows/apps/br211458.aspx)
[处理 URI 激活](https://msdn.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[关联启动示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)诠释了如何使用 LaunchUriAsync() API。
