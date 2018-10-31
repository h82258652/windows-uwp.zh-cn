---
author: TylerMSFT
title: 针对结果启动应用
description: 了解如何从其他应用启动某个应用，以及在这两者之间交换数据。 这就是针对结果启动应用。
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbe1978cc59afcc7d681331dadc9a06e3eb2d0
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5860782"
---
# <a name="launch-an-app-for-results"></a>针对结果启动应用




**重要的 API**

-   [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
-   [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

了解如何从其他应用启动某个应用，以及在这两者之间交换数据。 这就是*针对结果启动应用*。 此处示例演示了如何针对结果使用 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) 启动应用。

新应用到应用通信 windows 10 中的 Api 使 Windows 应用 （和 Windows Web 应用） 来启动应用并交换数据和文件。 这样你便可以从多个应用生成混合解决方案。 使用这些新 API，使得原本需要用户使用多个应用才能完成的复杂任务现在可以无缝地进行处理。 例如，你的应用可启动社交网络应用来选择联系人，或启动结算应用来完成支付流程。

将针对结果启动的应用称为启动应用。 启动应用的应用称为调用应用。 针对此示例，将编写调用应用和启动应用。

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>步骤 1：注册将在针对结果所启动的应用中处理的协议


在启动应用的 Package.appxmanifest 文件中，将协议扩展添加到 **&lt;Application&gt;** 部分。 下面的示例使用了一个名为 **test-app2app** 的虚构协议。

协议扩展中的 **ReturnResults** 属性接受以下值之一：

-   **optional** - 可通过使用 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) 方法针对结果启动应用，如果不针对结果，则使用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 启动应用。 当你使用 **optional** 时，启动应用必须确定它是否针对结果启动。 可以通过检查 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件参数进行确认。 如果参数的 [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 属性返回 [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693)，或事件参数的类型是 [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)，则应用通过 **LaunchUriForResultsAsync** 启动。
-   **always** - 可以仅针对结果启动应用；也就是说，它只能响应 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)。
-   **none** - 无法针对结果启动应用，它只能响应 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)。

在此协议扩展示例中，应用只能针对结果进行启动。 这将简化 **OnActivated** 方法（将在下面进行讨论）内部的逻辑，因为我们只需要处理“针对结果启动”的情况，而无需处理可激活应用的其他方式。

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>步骤 2：重写针对结果启动的应用中的 Application.OnActivated


如果此方法尚未存在于该启动应用中，请在 App.xaml.cs 中定义的 `App` 类内创建它。

在允许你从社交网络选取朋友的应用中，此函数可以位于你打开人员选取器页面的位置。 在接下来的此示例中，在针对结果激活应用后，将显示名为 **LaunchedForResultsPage** 的页面。 确保 **using** 语句包含在文件的顶部。

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

因为 Package.appxmanifest 文件中的协议扩展已将 **ReturnResults** 指定为 **always**，所以上述代码可以直接将 `args` 转换为 [**ProtocolForResultsActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn906905)，并确信对于此应用，只有 **ProtocolForResultsActivatedEventArgs** 将会发送到 **OnActivated**。 如果你的应用可以采用不同于针对结果进行启动的方式进行激活，则你可以通过检查 [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 属性是否返回 [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693) 来确定该应用是否针对结果而启动。

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>步骤 3：向针对结果启动的应用添加一个 ProtocolForResultsOperation 字段


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

当启动应用可以随时将结果返回给调用应用时，你将使用 [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) 字段进行指示。 在此示例中，需要将该字段添加到 **LaunchedForResultsPage** 类，因为将通过该页面完成针对结果启动操作，并且将需要访问它。

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>步骤 4：重写针对结果启动的应用中的 OnNavigatedTo()


重写页面（该页面将在针对结果启动应用时显示）上的 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 方法。 如果此方法尚未存在，请为页面在 &lt;pagename&gt;.xaml.cs 中定义的类内创建它。 确保以下 **using** 语句包含在该文件顶部：

```cs
using Windows.ApplicationModel.Activation
```

[**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 方法中的 [**NavigationEventArgs**](https://msdn.microsoft.com/library/windows/apps/br243285) 对象包含从调用应用传递的数据。 数据不得超过 100KB 且存储在 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 对象中。

在此示例代码中，启动应用预期从调用应用发送的数据存储在名为 **TestData** 的项下的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 中，因为这就是编码示例的调用应用旨在发送的内容。

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>步骤 5：编写代码以将数据返回到调用应用


在启动应用中，请使用 [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) 将数据返回给调用应用。 在此示例代码中，将创建一个 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)，其中包含要返回给调用应用的值。 然后，使用 **ProtocolForResultsOperation** 字段将该值发送到调用应用。

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>步骤 6：编写代码以针对结果启动应用并获取返回的数据


从调用应用的 async 方法内启动应用，如此示例代码中所示。 注意 **using** 语句，编译代码时不可缺少此类语句：

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

在此示例中，包含项 **TestData** 的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 将传递到启动应用。 启动应用将使用名为 **ReturnedData** 的项（其中包含已返回到调用方的结果）创建一个 **ValueSet**。

你必须先生成并部署将针对结果启动的应用，然后才能运行你的调用应用。 否则，[**LaunchUriResult.Status**](https://msdn.microsoft.com/library/windows/apps/dn906892) 将报告 **LaunchUriStatus.AppUnavailable**。

当设置 [**TargetApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/dn893511) 时，将需要启动应用的系列名称。 获取该系列名称的一个方法是从启动应用内执行以下调用：

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>备注


本操作方法中的示例将通过“hello world”介绍针对结果启动应用。 需要注意的重要事项是，新的 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) API 允许你异步启动应用并通过 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 类通信。 通过 **ValueSet** 传递的数据量被限制为 100KB。 如果需要传递更多数据，可以通过使用 [**SharedStorageAccessManager**](https://msdn.microsoft.com/library/windows/apps/dn889985) 类创建可在应用之间传递的文件标记来共享文件。 例如，存在一个名为 `inputData` 的 **ValueSet**，你可以将该标记存储到要与启动应用共享的文件中：

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

然后通过 **LaunchUriForResultsAsync** 将其传递到启动应用。

## <a name="related-topics"></a>相关主题


* [**LaunchUri**](https://msdn.microsoft.com/library/windows/apps/hh701476)
* [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
* [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

 

 
