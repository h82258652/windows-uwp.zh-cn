---
author: mcleanbyron
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: "安装 Microsoft 应用商店参与和盈利 SDK 后，请按照本主题中的说明在你的应用中使用广告中介控件。"
title: "添加和使用广告中介控件"
translationtype: Human Translation
ms.sourcegitcommit: 8c3f1997427a7c3d4f4b4b7acc876a2a091e4553
ms.openlocfilehash: a0d73b50207d251c079714265845a816f4ac23da

---

# 添加和使用广告中介控件


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[安装 Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk) 后，请按照本主题中的说明在你的应用中使用广告中介控件。 有关广告中介当前支持的广告网络和项目类型的列表，请参阅[选择和管理广告网络](select-and-manage-your-ad-networks.md)。

## 将广告中介控件添加到项目


将广告中介控件的实例添加到项目：

1.  在 Visual Studio 中，打开你的项目。
2.  如果你要将广告中介添加到已通过由广告中介支持的任何[广告网络](select-and-manage-your-ad-networks.md)创造收益的应用，请先删除现有广告实现及其所有引用，然后再继续操作。
3.  在“解决方案资源管理器”****中，找到应用中你希望显示广告的页面，然后双击该页面以在设计器中打开它。
4.  从“工具箱”****中，将新的 **AdMediatorControl** 拖动到设计器中（请确保将该控件拖动到设计器中，而不是拖动到你的 XAML 代码中）。 将控件放置在你希望显示广告的位置。 如果你希望在应用的多个区域中显示广告，则可以添加多个控件。

    **AdMediatorControl** 位于以下“工具箱”****位置中：

    -   在通用 Windows 平台 (UWP) 项目中，使用位于“AdMediator 通用”****部分下的 **AdMediatorControl**。
    -   在将 C# 或 Visual Basic 与 XAML 结合使用的 Windows 8.1 或 Windows Phone 8.1 项目中，使用位于“AdMediator”****部分下的 **AdMediatorControl**。
    -   在 Windows Phone Silverlight 项目中，使用位于“所有 Windows Phone 控件”****部分下的 **AdMediatorControl**。

    > **注意** 在你首次将 **AdMediatorControl** 控件拖动到 UWP 中的设计器，或者拖动到将 C# 或 Visual Basic 与 XAML 结合使用的 Windows 8.1 或 Windows Phone 8.1 项目中的设计器时，Visual Studio 会将必要的广告中介程序集引用添加到你的项目，但该控件尚未添加到该设计器中。 若要添加该控件，请在 Visual Studio 显示的消息中单击“确定”、等待设计器几秒钟以便刷新，然后再次将该控件拖回到设计器。 如果仍无法成功将该控件添加到设计器，请确保你的项目所面向的处理器体系结构适用于你的应用（例如，**“x86”**），而不是面向**任何 CPU**。 如果该项目面向生成平台的**任何 CPU**，则该控件将无法添加到设计器。

5.  Visual Studio 将广告中介程序集引用添加到项目，并将广告中介控件的 XAML 插入当前页面中，包括该控件的唯一 ID 和名称。 程序集引用和 XAML 因你的目标平台而异。 例如，对于通用 Windows 平台 (UWP) 应用，程序集名称为 **Microsoft.AdMediator.Universal**，生成的 XAML 类似于以下示例。

    ```xml
    // Code that gets added to the XAML page header
    xmlns:Universal="using:Microsoft.AdMediator.Universal"

    // Code that gets added for the ad mediator control
    <Universal:AdMediatorControl x:Name="AdMediator_3D4884"
      Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250"
      Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0"
      VerticalAlignment="Top" Width="300"/>
    ```

    当你配置广告中介时，**Name** 元素将帮助你标识应用中的特定控件。 你可以将该控件更改为所需的任何控件，但请确保不要更改或复制 **Id** 元素。 对于你的应用内的每个控件，此 **Id** 必须是唯一的。

6.  根据需要调整控件的大小和位置。 有关详细信息，请参阅[调整大小和位置](#adjust-size-and-position)。

## 配置广告网络

添加所需的所有控件后，你可以随时通过“连接的服务”配置广告网络。

> **重要提示** 如果以后添加其他 AdMediatorControl，需要通过“已连接的服务”重新配置它。 否则，新的控件将不能使用广告中介。

若要配置广告网络：

1.  在“解决方案资源管理器”中，右键单击项目名称、单击“添加”****，然后单击“已连接的服务…”**** 启动“添加已连接的服务”****窗口 (Visual Studio 2015) 或“服务管理器”****窗口 (Visual Studio 2013)。
2.  如果你使用的是 Visual Studio 2015，请单击“广告中介”****，然后单击“配置”****即可打开“广告中介”****窗口。 如果你使用的是 Visual Studio 2013，只需单击“服务管理器”****的左侧窗格中的“广告中介”****。

    AdMediator.config 文件将添加到你的项目。 此文件是初始广告网络配置设置本地保存在你的项目中的位置。

3.  在“广告中介”****(Visual Studio 2015) 或“服务管理器”****(Visual Studio 2013) 窗口中，单击“选择广告网络”****、选择要使用的广告网络，然后在“选择广告网络”****窗口中单击“确定”****。

    > **提示** 即使你不打算立即在应用中使用你的帐户具有的所有网络，最好也添加这些网络。 发布应用后，你将能够配置每个网络在开发人员中心中的使用频率（或开始使用之前没有使用的网络），而无需更改代码以及重新提交该应用。

    Visual Studio 将为已选择的广告网络获取所需程序集，并将这些程序集引用添加到你的项目中。 完成此过程后，在“获取状态”****对话框中单击“确定”****。

4.  在“广告中介”****(Visual Studio 2015) 或“服务管理器”****(Visual Studio 2013) 窗口中，可以选择每个网络并单击“配置”****，以为要在测试应用时使用的每个网络输入配置信息。 此信息将保存到你的项目中的 AdMediator.config 文件。 当你在 Windows 开发人员中心仪表板上配置广告网络行为时，你将可以修改此信息。 有关详细信息，请参阅[提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)。
    > **注意** 如果你在执行此步骤期间未输入配置信息，当你在开发计算机（用于 UWP 和 Windows 8.1 XAML 应用）或者在仿真器或设备（用于 Windows Phone 应用）上运行你的应用时，广告中介将自动使用测试配置值。

5.  在“广告中介”****(Visual Studio 2015) 或“服务管理器”****(Visual Studio 2013) 窗口中，确认你选择的每个广告网络都显示“已获取”****。 单击“确定”****将更改提交到你的项目。

> **注意** 如果你稍后升级到更高版本的 Microsoft 官方商城协定和盈利 SDK，则将需要再次启动“已连接的服务”****，以确保任何自动获取的广告网络 DLL 都已正确更新。

### 声明所需的功能

每个广告网络都可能需要某些应用功能。 由“广告中介”****（适用于 Visual Studio 2015）或“服务管理器”****（适用于 Visual Studio 2013）窗口中的每个提供程序显示这些功能。 请确保在应用的清单中声明所有所需功能，以便正确显示广告。

以下屏幕截图显示 Windows 8.1 或 Windows Phone 8.1 XAML 应用中的几个广告网络所需的功能。

![显示了已获取所有引用的服务管理器](images/ad-med-8.jpg)

以下屏幕截图显示 Windows Phone 8.1 Silverlight 应用中的几个广告网络所需的功能。

![显示了已获取所有引用的服务管理器](images/ad-med-6.jpg)
### 手动获取广告网络 DLL

在某些情况下，可能会看到没有获取某些 DLL。 在此情况下，你将需要手动添加它们。 有关下载单独程序集的链接，请参阅[选择和管理广告网络](select-and-manage-your-ad-networks.md)。

> **注意** 当手动添加 DLL 时，你可能会收到错误消息，指出“无法将对更高版本或不兼容程序集的引用添加到项目。” 若要解决此错误，请右键单击“资源管理器”中的 DLL，然后选择“属性”****。 在“安全”部分中，单击“取消阻止”****。

![取消阻止用于解决错误消息的按钮](images/ad-med-4.png)
## 调整大小和位置

你可以配置设计器中或 XAML 代码中的广告中介控件的大小和位置。 请确保此大小足够大，以适应将通过你的广告网络显示的所有广告。 如果某些广告网络检测到画布大小不足以完整显示广告，则它们可能不提供广告服务。 如果你的任何广告单元都比默认大小更大，则可以调整画布大小以适应最大的广告。

将控件拖动到设计器中时，默认控件大小为：

-   UWP 和 Windows 8.1 XAML：300 宽度 x 250 高度。
-   Windows Phone 8.1 XAML：400 宽度 x 67 高度。
-   Windows Phone 8 和 Windows Phone 8.1 Silverlight：480 宽度 x 80 高度。

使用 **Width** 和 **Height** 可选参数可以替代默认的广告大小，如下所示。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

还可以指定如何定位每个控件以适应各种大小和放置，具体取决于你的应用的需求。 对于某些广告网络，你可以使用可选参数进行调整。 例如，将 Microsoft Advertising 的广告对齐到左下角：

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Microsoft Advertising 支持的广告大小

Microsoft Advertising 仅支持由互动广告局 (IAB) 针对运行在以下平台上的应用建议的以下几种标准大小的广告。

-   Windows 10 和 Windows 8.1：
    -   160 x 600
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 移动版、Windows Phone 8.1 和 Windows Phone 8：
    -   300 x 50
    -   320 x 50
    -   480 x 80（仅 Windows Phone Silverlight 支持此大小）
    -   640 x 100

你可能希望指定不与 Microsoft Advertising 支持的广告大小之一匹配的广告中介控件大小（例如，当你的应用的 UI 更适合其他大小时或者当你还面向支持其他广告大小的广告网络时，你可能希望这样做）。 若要执行此操作，请指定设计器中或 XAML 代码中所需的确切控件大小，然后将 Microsoft Advertising 的 **Width** 和 **Height** 可选参数分配到适合控件的边界内最接近的支持大小。 该控件将显示为你在设计器中指定的确切大小，但 Microsoft Advertising 将提供匹配使用 **Width** 和 **Height** 可选参数指定的大小的广告。

例如，如果你有一个 UWP 应用并且希望广告中介控件以 300 x 300 大小显示，则可在设计器中或在 XAML 代码中将该控件设置为 300 x 300。 然后，为 Microsoft Advertising 将 **Width** 可选参数分配为 300 且将 **Height** 可选参数分配为 250，如以下代码所示。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

若要验证你的大小设置是否与 Microsoft Advertising 兼容，请[测试广告中介实现](test-your-ad-mediation-implementation.md)并确保显示来自 Microsoft Advertising 的测试广告。

## 暂停、恢复和禁用广告中介

如果你想要在应用运行时期间在任何给定的时间量内暂停广告中介，则使用 AdMediatorControl.Pause() 方法。 请注意，当你执行此操作时，最新的广告将继续显示，直到你通过调用 AdMediatorControl.Resume() 方法恢复中介。

若要完全禁用广告中介者，请使用 AdMediatorControl.Disable() 方法。 这将删除任何显示的广告，并且它将最小化中介者的内存占用。 你可以调用 AdMediatorControl.Resume() 来恢复中介，但请注意，禁用广告中介后，启动时间将比平常慢很多。

## 设置超时

你可以指定在从该广告网络请求某个广告之后，在禁用该请求并改为向其他网络发出请求之前，广告中介应等待的秒数（从 2 到 60）。 默认情况下，所有广告网络的超时都为 15 秒。

下面的代码显示了如何指定 Microsoft Advertising 的超时持续时间。 你可以根据需要修改持续时间和网络。

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10);
```

> **注意** 此外，你还可以在开发人员中心仪表板上的“利用广告来盈利”****页面中设置超时值。 如果你在代码中和仪表板中设置超时，则你在代码中设置的值将替代仪表板值。

## 事件处理

添加用于记录事件并捕获广告中介错误的代码可帮助进行故障排除。 下面的代码示例将从控件添加特定事件的事件处理程序。

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## 处理来自广告网络的未处理的异常

> **注意** 作为我们的测试的一部分，我们已标识大量来自特定广告网络的未处理的异常，必须在应用内处理这些异常以避免出现与这些异常相关的应用崩溃。 我们强烈建议你将下面的代码示例复制并粘贴到 App.xaml.cs 文件。

用于使用 C# 和 XAML 的 UWP、Windows 8.1 或 Windows Phone 应用的代码

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

用于 Windows Phone Silverlight 应用的代码

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) && exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }

   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## 相关主题

* [选择和管理广告网络](select-and-manage-your-ad-networks.md)
* [测试广告中介实现](test-your-ad-mediation-implementation.md)
* [提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)
* [广告中介疑难解答](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


