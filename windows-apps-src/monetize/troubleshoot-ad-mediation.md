---
author: mcleanbyron
Description: "以下是一些与广告中介相关的多个常见开发问题的解决方案。"
title: "广告中介疑难解答"
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
translationtype: Human Translation
ms.sourcegitcommit: 10dcf3c2b8ea530b94e9c17ada80aaa98e9418fe
ms.openlocfilehash: f32dc28c9b199c11a1932639f49ab4c29d3e1e8f

---

# 广告中介疑难解答


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下是一些与广告中介相关的多个常见开发问题的解决方案。

**无法将 AdMediatorControl 添加到设计图面**  
如果第一次将 **AdMediatorControl** 控件拖动到通用 Windows 平台 (UWP) 项目中的设计器，或者拖动到将 C# 或 Visual Basic 与 XAML 结合使用的 Windows 8.1 或 Windows Phone 8.1 项目中的设计器，Visual Studio 会将必要的广告中介程序集引用添加到你的项目，但该控件尚未添加到该设计器。 若要添加该控件，请在 Visual Studio 显示的消息中单击“确定”、等待设计器几秒钟以便刷新，然后再次将该控件拖回到设计器。

如果仍无法成功将该控件添加到设计器，请确保你的项目所面向的处理器体系结构适用于你的应用（例如，**“x86”**），而不是面向**任何 CPU**。 如果该项目面向生成平台的**任何 CPU**，则该控件将无法添加到设计器。

当投放来自 Microsoft 的广告时，**AdMediatorControl 会在运行时显示错误“&lt;*宽度* &gt; x &lt;*高度*&gt; 不支持”**Microsoft Advertising 仅支持[互动广告局 (IAB) 建议的某些广告大小](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising)。在某些情况下，即使在设计器中或在你的 XAML 中将广告中介控件的高度和宽度设置为这些受支持的广告大小之一，缩放和舍入问题仍然可能会导致广告中介框架无法投放广告。若要避免此问题，需要将代码中用于 Microsoft Advertising 的** Width **和** Height** 可选参数分配给任一受支持的广告大小。

以下代码示例将演示如何将 Microsoft Advertising 的 **Width** 和 **Height** 可选参数赋值为 728 x 90。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**广告中介不包含用于广告网络的位置（纬度/经度）**  
如果启用应用中的位置功能，该广告中介将自动获取纬度/经度坐标，并将它们提供给支持它们的广告网络。

**Smaato 广告控件未正确排队**  
尝试使用可选参数在 SDK 控件上设置值：

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**AdDuplex 广告控件未按正确大小显示（它显示为 250x250）**  
广告中介未针对该大小设置任何值，因此你应使用可选 **Size** 参数更改它。 例如：

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**你将收到错误“某些内容覆盖了广告控件”**  
如果广告在应用内在任何情况下都被遮盖，Ad Duplex 将始终显示一个错误。 [将它们的解决方案读取](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl)到此错误中。

**你将收到错误“两个文件之间存在冲突”**  
你已在应用的其他位置引用了 Microsoft Advertising 程序集。 广告中介专为在应用中工作而设计，如果使用对 Microsoft Advertising 程序集的其他引用，它将不起作用。 手动删除 Microsoft Advertising 引用，并重新安装 Microsoft 官方商城协定和盈利 SDK 以清除错误。

**更改 AdMediator.config 文件中的 RefreshRate 值后，你会遇到意外的行为**

配置广告网络（通过在 Visual Studio 中运行“添加连接的服务”****对话框中的“广告中介”****组件）后，默认配置信息会保存到项目中的 AdMediator.config 文件。 你的目的并不是要直接修改此文件。 而是在你将应用包上传到 Windows 开发人员中心仪表板后[为应用配置广告中介设置](submit-your-app-and-configure-ad-mediation.md)时，你可以修改此信息（包括新广告的刷新率）。

如果你更改 AdMediator.config 文件中的 **RefreshRate** 值，请注意，此值必须包含一个 30 到 120 之间的整数，表示以秒为单位的刷新率。 如果你将此值设置为小于 30 或大于 120 的整数，广告中介框架将自动使用 60 秒作为刷新率。

## 相关主题

* [选择和管理广告网络](select-and-manage-your-ad-networks.md)
* [添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)
* [测试广告中介实现](test-your-ad-mediation-implementation.md)
* [提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


