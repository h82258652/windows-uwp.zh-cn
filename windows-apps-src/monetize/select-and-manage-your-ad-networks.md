---
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: 在使用广告中介前，你将需要为要在应用中使用的每个广告网络设置帐户。
title: 选择和管理广告网络
---

# 选择和管理广告网络


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在使用广告中介前，你将需要为要在应用中使用的每个广告网络设置帐户。 建议先将广告中介控件添加到 Microsoft Visual Studio 项目，再执行此操作。 我们还建议为尽可能多的广告网络设置帐户，以便你能够灵活地优化广告中介性能。

## 支持的广告网络和项目类型

广告中介当前仅支持以下广告网络和项目类型。

|  广告网络    | 将 C# 或 Visual Basic 与 XAML 结合使用的通用 Windows 平台 (UWP) 应用 | 将 C# 或 Visual Basic 与 XAML 结合使用的 Windows 8.1 应用 | 将 C# 或 Visual Basic 与 XAML 结合使用的 Windows Phone 8.1 应用 | Windows Phone 8.x Silverlight 应用 |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **支持**                                      | **支持**                                            | **支持**                     | **支持** |
| [AdDuplex](#adduplex)                                                   | **支持**                                      | **支持**                                            | **支持**                     | **支持** |
| [Smaato](#smaato)                                                       | 不支持                                      | **支持**                                            | **支持**                     | **支持** |
| [AdMob (Google)](#admob)                                                | 不支持                                      | 不支持                                            | 不支持                     | **支持** |
| [Inneractive](#inneractive)                                             | 不支持                                      | 不支持                                            | 不支持                     | **支持** |
| [Vserv VMAX](#vserv)                                                    | 不支持                                      | 不支持                                            | **支持**                     | 不支持 | 

一些项目类型（例如 C++ 与 XAML 以及 JavaScript 与 HTML）当前不受广告中介支持。 对于这些项目类型，你可以从 Microsoft 中显示广告，而无需使用广告中介。 有关详细信息，请参阅 [XAML 和 .NET 中的 AdControl](https://msdn.microsoft.com/library/mt313186.aspx) 和 [HTML 5 和 JavaScript 中的 AdControl](https://msdn.microsoft.com/library/mt313130.aspx)。

## 每个网络的特定参数和详细信息


下面是针对每个广告网络你将需要的特定详细信息，包括如何设置帐户和如何使应用上架。 我们还列出了在[提交应用并配置广告中介](submit-your-app-and-configure-ad-mediation.md)（和/或在[用于测试广告中介实现](test-your-ad-mediation-implementation.md)的连接服务中）时，你需要提供的必需参数。 有关使用特定广告网络的详细信息，请访问其网站。

除所需参数外，每个广告网络也具有通过应用代码设置的其他可选参数。 有关示例，请参阅本主题后面的[可选广告网络参数](#optionalparameters)。 有关可选参数的完整列表，请参阅每个广告网络提供的文档。 广告中介会忽略或覆盖这些参数的其中一些参数，并且这些参数会列在以下部分中。 例如，与当前位置相关的参数通常由关于客户位置的信息所覆盖，该位置由应用本身内的位置功能确定。

请注意，当你[添加广告中介控件](add-and-use-the-ad-mediator-control.md)并指定要使用的广告网络时，Visual Studio 将尝试以编程方式获取某些广告网络所需的程序集。 如果存在任何无法自动检索到的程序集，可以使用下面显示的针对每个广告网络的链接来安装它们。

### Microsoft

| 网站                        | 若要配置广告网络参数，请使用 [Windows 开发人员中心仪表板](https://dev.windows.com/overview)上的[使用广告盈利](https://msdn.microsoft.com/library/windows/apps/mt170658)页面。   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置                   | [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk)。                                                                                                                                                                                                                         |
| 使应用上架              | 将广告中介控件添加到应用中，然后将该应用提交到 Windows 开发中心仪表板。                                                                                                                                                                                                            |
| 所需的参数            | ApplicationId 和 AdUnitId：当提交应用程序包时，这些参数会根据应用的内容自动为你填入。 但是，当[提交应用并配置广告中介](submit-your-app-and-configure-ad-mediation.md)时，你可以根据需要编辑这些参数。 <br> <br> Height 和 Width（仅为 Windows Phone 8 Silverlight 和 Windows Phone 8.1 Silverlight 所需）。                                                                                                                                                                                                           |
| 已覆盖/已忽略的参数 | Latitude（已覆盖）  <br><br> Longitude（已覆盖） <br><br> AutoRefreshIntervalInSeconds（已忽略） <br><br> IsAutoRefreshEnabled（已忽略） <br><br> IsAutoCollapsedEnabled（已忽略） <br><br> IsEngaged（已忽略） <br><br> IsSuspended（已忽略） |

 

### AdDuplex

| 网站                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| SDK 位置                   | 首先，尝试让广告中介控件通过连接的服务检索程序集，如[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)中所述。 如果你需要手动下载它们，请转到 AdDuplex 网站上的[入门](http://go.microsoft.com/fwlink/p/?LinkId=518029)部分。 |
| 使应用上架              | 在 AdDuplex 门户中，创建新的应用。                                                                                                                                                                                                                                                                                                        |
| 所需的参数            | AppKey <br><br> AdUnitId <br><br> Size（仅对于 Windows 8.1 XAML 是必需参数）  |
| 已覆盖/已忽略的参数 | Latitude（已覆盖） <br><br> Longitude（已覆盖） <br><br> AutoRefreshIntervalInSeconds（已忽略） <br><br> IsAutoRefreshEnabled（已忽略） <br><br> IsAutoCollapsedEnabled（已忽略） <br><br> IsEngaged（已忽略） <br><br> IsSuspended（已忽略） |
 

### Smaato

| 网站                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置                   | 首先，尝试让广告中介控件通过连接的服务检索程序集，如[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)中所述。 如果你需要手动下载它们，请转到 Smaato 门户中的“下载”选项卡。 |
| 使应用上架              | 在 Smaato 门户中，请转到“我的空间”并生成新的广告空间。                                                                                                                                                                                                                |
| 所需的参数            | Pub <br> <br> Adspace <br> <br> Height 和 Width（仅对于 Windows 8.1 XAML 是必需参数）  |
| 已覆盖/已忽略的参数 | Gps（已覆盖）                                                                                                                                                                                                                                                                |

 

### AdMob (Google)

| 网站                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| SDK 位置                   | 请从 [Google 移动广告 SDK 网站](http://go.microsoft.com/fwlink/p/?LinkId=518032)获取 Windows Phone 8 SDK。 |
| 使应用上架              | 在 AdMob 门户中，选择“利用新应用盈利”。                                                                          |
| 所需的参数            | AdUnitID <br> <br> 格式                                                                                              |
| 已覆盖/已忽略的参数 | 位置（已覆盖）  <br><br> ForceTesting（已忽略） <br><br> Refresh Rate（已忽略）                                |
 

### Inneractive

| 网站             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置        | 首先，尝试让广告中介控件通过连接的服务检索程序集，如[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)中所述。 如果你需要手动下载它们，请登录你的帐户并转到仪表板中的 SDK 选项卡，以下载 Windows Phone 8 SDK。 |
| 使应用上架   | 在 Inneractive 门户中，创建新应用。                                                                                                                                                                                                                                                                                           |
| 所需的参数 | AppID <br> <br> AdType（IaAdType_Banner 或 IaAdType_Text）                                                                               |
 

### Vserv VMAX

| 网站             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置        | 首先，尝试让广告中介控件通过连接的服务检索程序集，如[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)中所述。 如果你需要手动下载它们，请转到 VMAX 网站上的 [SDK](http://go.microsoft.com/fwlink/?LinkId=627078) 部分。 |
| 使应用上架   | 从 VMAX 网站获取应用的 Adspot ID（Adspot ID 在 VMAX API 中称为 ZoneId）。                                                                                                                                                                                                                     |
| 所需的参数 | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## 可选的广告网络参数


除所需参数外，每个广告网络也具有通过应用代码设置的其他可选参数。 有关可选参数的完整列表，请参阅每个广告网络提供的文档。 若要在你的代码中设置这些可选参数，请使用 **AdMediatorControl** 对象的 **AdSdkOptionalParameters** 属性。

以下示例展示如何设置 Microsoft Advertising 的 **CountryOrRegion** 参数。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

以下示例展示如何设置 Smaato 的 **Width** 和 **Height** 参数。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## 相关主题

* [添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)
* [测试广告中介实现](test-your-ad-mediation-implementation.md)
* [提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)
* [广告中介疑难解答](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


