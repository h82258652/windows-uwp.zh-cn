---
Description: 以下是一些与广告中介相关的多个常见开发问题的解决方案。
title: 广告中介疑难解答
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
---

# 广告中介疑难解答


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下是一些与广告中介相关的多个常见开发问题的解决方案。

**无法将 AdMediatorControl 添加到设计图面**  
如果第一次将 **AdMediatorControl** 控件拖动到通用 Windows 平台 (UWP) 项目中的设计器，或者拖动到将 C# 或 Visual Basic 与 XAML 结合使用的 Windows 8.1 或 Windows Phone 8.1 项目中的设计器，Visual Studio 会将必要的广告中介程序集引用添加到你的项目，但该控件尚未添加到该设计器。 若要添加该控件，请在 Visual Studio 显示的消息中单击“确定”、等待设计器几秒钟以便刷新，然后再次将该控件拖回到设计器。

如果仍无法成功将该控件添加到设计器，请确保你的项目所面向的处理器体系结构适用于你的应用（例如，**“x86”**），而不是面向**任何 CPU**。 如果对于生成平台，该项目面向**任何 CPU**，该控件则无法添加到设计器。

*
            *AdMediatorControl 会在投放 Microsoft 的广告**时显示运行时错误“<*width*> x <*height*> 不支持”  
Microsoft Advertising 仅支持[由美国互动广告局 (IAB) 建议的几种广告大小](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising)。 在某些情况下，即使在设计器中或在你的 XAML 中将广告中介控件的高度和宽度设置为这些受支持的广告大小之一，缩放和舍入问题仍然可能会导致广告中介框架无法投放广告。 若要避免此问题，请在你的代码中将用于 Microsoft Advertising 的**“宽度”**和**“高度”**可选参数赋值为支持的广告大小之一。

以下代码示例将演示如何将 Microsoft Advertising 的 **Width** 和 **Height** 可选参数赋值为 728 x 90。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**广告中介不包含用于广告网络的位置（纬度/经度）**  
如果启用应用中的位置功能，该广告中介将自动获取纬度/经度坐标，并将它们提供给支持它们的广告网络。

**Smaato 广告控件未正确排队**  
尝试使用可选参数在 SDK 控件上设置值：

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Margin”] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Width”] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Height”] = 320d;
```

**AdDuplex 广告控件未按正确大小显示（它显示为 250×250）**  
广告中介未针对该大小设置任何值，因此你应使用可选参数 Size 更改它。 例如：

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex][“Size”] = “160×600″;
```

**你将收到错误“某些内容覆盖了广告控件”**  
如果广告在应用内在任何情况下都被遮盖，Ad Duplex 将始终显示一个错误。 [将它们的解决方案读取](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl)到此错误中。

**你将收到错误“两个文件之间存在冲突”**  
你已在应用的其他位置引用了 Microsoft Advertising 程序集。 广告中介专为在应用中工作而设计，如果使用对 Microsoft Advertising 程序集的其他引用，它将不起作用。 手动删除 Microsoft Advertising 引用，并重新安装 Microsoft 官方商城协定和盈利 SDK 以清除错误。

## 相关主题

* [选择和管理广告网络](select-and-manage-your-ad-networks.md)
* [添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)
* [测试广告中介实现](test-your-ad-mediation-implementation.md)
* [提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


