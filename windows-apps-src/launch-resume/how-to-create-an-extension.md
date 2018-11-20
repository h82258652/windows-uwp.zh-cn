---
author: TylerMSFT
title: 创建和使用应用扩展
description: 编写并托管通用 Windows 平台 (UWP) 应用扩展，这些扩展允许你通过用户可从 Microsoft Store 中安装的程序包来扩展你的应用。
keywords: 应用扩展, 应用服务, 后台
ms.author: twhitney
ms.date: 10/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c4c326dbafa719273c4535a42d58184c7ce360fe
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7286357"
---
# <a name="create-and-host-an-app-extension"></a>创建和托管应用扩展

本文向你介绍如何创建 UWP 应用扩展以及如何在 UWP 应用中托管该扩展。

本文伴有代码示例：
- 下载并解压缩[数学扩展代码示例](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)。
- 在 Visual Studio 2017 中，打开 MathExtensionSample.sln。 将生成类型设置为 x86（**生成** > **Configuration Manager**，然后针对两个项目将**平台**更改为 **x86**）。
- 部署解决方案：**生成** > **部署解决方案**。

## <a name="introduction-to-app-extensions"></a>应用扩展简介

在通用 Windows 平台 (UWP) 中，应用扩展提供的功能类似于插件、外接程序和加载项在其他平台上提供的功能。 例如，Microsoft Edge 扩展就属于 UWP 应用扩展。 Windows 10 周年纪念版（版本 1607，内部版本 10.0.14393）中引入了 UWP 应用扩展。

UWP 应用扩展是 UWP 应用，这些应用具有的扩展声明允许它们与主机应用共享内容和部署事件。 扩展应用可以提供多个扩展。

由于应用扩展只是 UWP 应用，因此它们也可以是功能齐全的应用、托管扩展以及向其他应用提供扩展，所有这一切都无需创建单独的应用包。

当你创建应用扩展主机时，就创造了围绕应用开发生态系统的机会，在此生态系统中，其他开发人员可以通过你可能未曾想到或无资源进行实施的方法来增强你的应用。 请考虑 Microsoft Office 扩展、Visual Studio 扩展、浏览器扩展等。这些扩展会针对应用创建更丰富的体验，远远超出了应用附带的功能。 扩展可以增加你的应用的价值和寿命。

**概述**

为了在高级别设置应用扩展关系，我们需要：

1. 将应用声明为扩展主机。
2. 将应用声明为扩展。
3. 确定是将扩展实现为应用服务、后台任务还是以某种其他方式实现扩展。
4. 定义主机及其扩展的通信方式。
5. 使用主机应用中的 [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions) API 访问扩展。

我们来看一下如何通过检查[数学扩展代码示例](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)来完成此操作，此代码示例实现了一个假想的计算器，你可以使用扩展向该计算器添加新的函数。 在 Microsoft Visual Studio 2017 中，从此代码示例加载 **MathExtensionSample.sln**。

![数学扩展代码示例](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>将应用声明为扩展主机

应用通过在其 Package.appxmanifest 文件中声明 `<AppExtensionHost>` 元素，将自己标识为应用扩展主机。 请参阅 **MathExtensionHost** 项目中的 **Package.appxmanifest** 文件以查看如何完成此操作。

_MathExtensionHost 项目中的 Package.appxmanifest_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

请注意 `xmlns:uap3="http://..."`，以及 `IgnorableNamespaces` 中 `uap3` 的状态。 由于我们使用的是 uap3 命名空间，因此这些项是必需的。

`<uap3:Extension Category="windows.appExtensionHost">` 将此应用标识为扩展主机。

`<uap3:AppExtensionHost>` 中的 **Name ** 元素是_扩展约定_名称。 当扩展指定相同的扩展约定名称时，主机将能够找到它。 按照惯例，我们建议使用你的应用或发布者名称来生成扩展约定名称，以避免可能与其他扩展约定名称发生冲突。

你可以在同一应用中定义多个主机和多个扩展。 在此示例中，我们将声明一个主机。 另一个应用中定义了扩展。

## <a name="declare-an-app-to-be-an-extension"></a>将应用声明为扩展

应用通过在其 **Package.appxmanifest** 文件中声明 `<uap3:AppExtension>` 元素，将自己标识为应用扩展。 打开 **MathExtension** 项目中的 **Package.appxmanifest** 文件以查看如何完成此操作。

_MathExtension 项目中的 Package.appxmanifest：_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

同样，请注意 `xmlns:uap3="http://..."` 行，以及 `IgnorableNamespaces` 中 `uap3` 的状态。 由于我们使用的是 `uap3` 命名空间，因此这些项是必需的。

`<uap3:Extension Category="windows.appExtension">` 将此应用标识为扩展。

`<uap3:AppExtension>` 属性的含义如下：

|属性|说明|必需|
|---------|-----------|:------:|
|**Name**|这是扩展约定名称。 当它与主机中声明的 **Name** 匹配时，该主机将能够找到此扩展。| :heavy_check_mark: |
|**ID**| 唯一标识此扩展。 因为可能存在多个使用相同扩展约定名称的扩展（请设想一个支持一些扩展的画图应用），所以你可以使用 ID 将它们区分开来。 应用扩展主机可以使用 ID 推断有关扩展类型的内容。 例如，你可能具有一个针对桌面设计的扩展和另一个针对移动设备设计的扩展，并且可以用 ID 进行区分。 为此，你还可以使用以下讨论的 **Properties** 元素。| :heavy_check_mark: |
|**DisplayName**| 可以从主机应用中使用它来识别用户的扩展。 它可以从[新资源管理系统](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) 中查询，并且可以使用该系统进行本地化。 本地化的内容从应用扩展包中加载，而不是从主机应用中加载。 | |
|**Description** | 可以从主机应用中使用它来描述用户的扩展。 它可以从[新资源管理系统](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) 中查询，并且可以使用该系统进行本地化。 本地化的内容从应用扩展包中加载，而不是从主机应用中加载。 | |
|**PublicFolder**|相对于程序包根目录的文件夹（你可以与扩展主机共享该文件夹中的内容）的名称。 按照惯例，此名称是“Public”，但是你可以使用与扩展中的文件夹匹配的任何名称。| :heavy_check_mark: |

`<uap3:Properties>` 是一个包含主机可在运行时读取的自定义元数据的可选元素。 在代码示例中，扩展以应用服务形式来实现，因此主机需要一种方法来获取该应用服务的名称，以便主机可以调用该服务。 此应用服务的名称在我们所定义的 <Service> 元素中定义（我们可以将其叫做我们想要的任何名称）。 此代码示例中的主机会在运行时查找此属性以了解应用服务的名称。

## <a name="decide-how-you-will-implement-the-extension"></a>确定你将如何实现扩展。

[版本 2016 的应用扩展相关会话](https://channel9.msdn.com/Events/Build/2016/B808)演示了如何使用在主机和扩展之间共享的公共文件夹。 在该示例中，扩展通过存储在公共文件夹中并且由主机调用的 Javascript 文件来实现。 此方法的优点是简单、不需要编译，并且可以支持制作默认登录页面，从而提供扩展说明以及指向主机应用 Microsoft Store 页面的链接。 有关详细信息，请参阅[版本 2016 的应用扩展代码示例](https://github.com/Microsoft/App-Extensibility-Sample)。 具体而言，请参阅 **InvertImageExtension** 项目以及 **ExtensibilitySample** 项目内 ExtensionManager.cs 中的 `InvokeLoad()`。

在此示例中，我们将使用应用服务实现扩展。 应用服务具有以下优势：

- 如果扩展发生崩溃，则它不会关闭主机应用，因为主机应用在其自己的进程中运行。
- 你可以使用你选择的语言来实现该服务。 它不必与用于实现主机应用的语言匹配。
- 应用服务有权访问其自己的应用容器 - 该容器具有的功能可能与主机具有的功能不同。
- 服务中的数据与主机应用是分开的。

### <a name="host-app-service-code"></a>主机应用服务代码

下面是调用扩展的应用服务的主机代码：

_MathExtensionHost 项目中的 ExtensionManager.cs_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

这是用于调用应用服务的典型代码。 有关如何实现和调用应用服务的详细信息，请参阅[如何创建和使用应用服务](how-to-create-and-consume-an-app-service.md)。

需要注意的一点是如何确定要调用的应用服务的名称。 因为主机没有关于扩展实现的信息，所以扩展需要提供其应用服务的名称。 在此代码示例中，扩展在 `<uap3:Properties>` 元素中声明其文件中的应用服务的名称：

_MathExtension 项目中的 Package.appxmanifest_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

你可以在 `<uap3:Properties>` 元素中定义自己的 XML。 在本例中，我们将定义应用服务的名称，以便应用服务调用扩展时主机可以使用该应用服务。

当主机加载扩展时，像这样的代码会从扩展的 Package.appxmanifest 中定义的属性中提取服务的名称：

_`Update()` 在 MathExtensionHost 项目内的 ExtensionManager.cs 中_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

在 `_serviceName` 中存储应用服务的名称后，主机可以使用它来调用应用服务。

调用应用服务还需要包含应用服务的程序包的包系列名称。 幸运的是，应用扩展 API 可提供此信息，此信息从以下行中获取： `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>定义主机和扩展的通信方式

应用服务使用 [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) 交换信息。 作为主机的作者，你需要提供一个灵活的协议以与扩展进行通信。 在此代码示例中，这意味着要考虑将来可能会采用 1 个、2 个或更多个参数的扩展。

对于此示例，参数协议是 **ValueSet**，其中包含名为“Arg”的键值对以及参数编号，例如 `Arg1` 和 `Arg2`。 主机将传递 **ValueSet** 中的所有参数，扩展将使用它所需要的参数。 如果扩展能够计算结果，则主机期望从扩展中返回的 **ValueSet** 具有一个名为 `Result` 且包含计算值的键。 如果该键不存在，则主机认为扩展无法完成计算。

### <a name="extension-app-service-code"></a>扩展应用服务代码

在此代码示例中，扩展的应用服务未实现为后台任务。 而是使用了单一进程应用服务模型，在此模型中，应用服务与托管它的扩展应用在相同进程中运行。 此进程还与主机应用的进程不同，它具有进程分离的优点，同时可避免在扩展进程与实现应用服务的后台进程之间进行跨进程通信，从而获得了一些性能优势。 请参阅[将应用服务转换为与其主机应用在同一个进程中运行](convert-app-service-in-process.md)，以查看作为后台任务运行的应用服务与在同一进程中运行的应用服务之间的区别。

激活应用服务时，系统会调用 `OnBackgroundActivate()`。 该代码设置事件处理程序，以在实际应用服务调用 (`OnAppServiceRequestReceived()`) 到来时对其进行处理，并处理内务事件，例如获取一个处理取消或关闭事件的延迟对象。

_MathExtension 项目中的 App.xaml.cs。_
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

执行扩展的工作的代码在 `OnAppServiceRequestReceived()` 中。 调用应用服务来执行计算时会调用此函数。 它从 **ValueSet** 中提取需要的值。 如果它可以进行计算，则会将结果放在返回到主机的 **ValueSet** 中名为 **Result** 的键下面。 根据为该主机与其扩展的通信方式定义的协议重新调用该函数，**Result** 键的状态将指示成功；否则指示失败。

_MathExtension 项目中的 App.xaml.cs。_
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>管理扩展

现在，我们已经了解了如何实现主机与其扩展之间的关系，让我们来看看主机如何查找系统上所安装的扩展，以及主机如何对添加和删除包含扩展的程序包做出反应。

Microsoft Store 提供程序包形式的扩展。 [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) 查找包含与主机扩展约定名称匹配的扩展的已安装程序包，并提供在安装或删除与主机相关的应用扩展包时触发的事件。

在此代码示例中，`ExtensionManager` 类（在 **MathExtensionHost** 项目内的 **ExtensionManager.cs** 中定义）包装用于加载扩展和响应扩展包的安装和卸载的逻辑。

`ExtensionManager` 构造函数使用 `AppExtensionCatalog` 查找系统上与主机具有相同扩展约定名称的应用扩展：

_MathExtensionHost 项目中的 ExtensionManager.cs。_
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

安装扩展包后，`ExtensionManager` 会收集程序包中与主机具有相同扩展约定名称的扩展的相关信息。 安装可能代表更新，在这种情况下，会更新受影响的扩展的信息。 卸载扩展包后，`ExtensionManager` 会删除有关受影响的扩展的信息，以便用户了解哪些扩展不再可用。

我们为此代码示例创建了 `Extension` 类（在 **MathExtensionHost** 项目内的 **ExtensionManager.cs** 中定义），以访问扩展的 ID、描述、徽标和特定于应用的信息，如用户是否启用了扩展。

如果说加载了扩展（请参阅 **ExtensionManager.cs** 中的 `Load()`），就意味着程序包状态是好的，并且我们已经获取了它的 ID、徽标、描述和公共文件夹（本示例中未使用 - 它只是为了向你介绍获取方法）。 扩展包本身并未加载。

卸载的概念用于跟踪应该不会再提供给用户的扩展。

`ExtensionManager` 提供了 `Extension` 实例集合，以便扩展、其名称、描述和徽标可以是绑定到 UI 的数据。 **ExtensionsTab** 页面会绑定到此集合并提供 UI 以启用/禁用扩展以及删除它们。

![扩展选项卡示例 UI](images/mathextensionhost-extensiontab.png)

 删除扩展时，系统将提示用户确认他们是否想要卸载包含该扩展的程序包（可能包含其他扩展）。 如果用户同意，则会卸载此程序包，并且 `ExtensionManager` 会从向主机应用提供的扩展列表中删除已卸载程序包中的扩展。

 ![卸载 UI](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>调试应用扩展和主机

通常，扩展主机和扩展不属于同一解决方案。 在此情况下，若要调试主机和扩展，请执行以下操作：

1. 在 Visual Studio 的一个实例中加载你的主机项目。
2. 在 Visual Studio 的另一个实例中加载你的扩展。
3. 在调试程序中启动主机应用。
4. 在调试程序中启动扩展应用。 （如果你想要部署扩展，而不是调试扩展，那么若要测试主机的程序包安装事件，请改为执行**生成 &gt; 部署解决方案**）。

现在，你将能够在主机和扩展中命中断点。
如果你开始调试扩展应用本身，则将看到空白的应用窗口。 如果你不希望看到空白窗口，则可以更改扩展项目的调试设置，从而不启动应用，但改为在启动时调试应用（右键单击扩展项目，选择**属性** > **调试**> 选择**不启动，但在启动时调试代码**）。你仍然需要开始调试 (**F5**) 扩展项目，但它将等到主机激活扩展，然后将命中扩展中的断点。

**调试代码示例**

在此代码示例中，主机和扩展在同一个解决方案中。 请执行下列操作以进行调试：

1. 确保 **MathExtensionHost** 是启动项目（右键单击 **MathExtensionHost** 项目，单击**设为启动项目**）。
2. 将断点放在 **MathExtensionHost** 项目内 ExtensionManager.cs 中的 `Invoke` 上。
3. 按 **F5** 运行 **MathExtensionHost** 项目。
4. 将断点放在 **MathExtension** 项目内 App.xaml.cs 中的 `OnAppServiceRequestReceived` 上。
5. 开始调试 **MathExtension** 项目（右键单击 **MathExtension** 项目，选择**调试 > 启动新实例**），这将部署此项目并在主机中触发程序包安装事件。
6. 在 **MathExtensionHost** 应用中，导航到**计算**页面，然后单击 **x^y** 以激活扩展。 首先会命中 `Invoke()` 断点，并且你可以看到正在进行的扩展应用服务调用。 然后会命中扩展中的 `OnAppServiceRequestReceived()` 方法，并且你可以看到该应用服务计算结果并返回结果。

**作为应用服务实现的扩展疑难解答**

如果你的扩展主机在连接到扩展的应用服务时遇到问题，请确保 `<uap:AppService Name="...">` 属性与你放入 `<Service>` 元素中的内容匹配。 如果它们不匹配，则你的扩展提供给主机的服务名称将与你实现的应用服务名称不匹配，并且主机将无法激活你的扩展。

_MathExtension 项目中的 Package.appxmanifest：_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>要测试的基本方案的清单

当你构建一个扩展主机并准备测试它对扩展的支持程度时，下面是要尝试的一些基本方案：

- 运行主机，然后部署扩展应用  
    - 主机是否会选取运行时出现的新扩展？  
- 部署扩展应用，然后部署并运行主机。
    - 主机是否会选取之前已有的扩展？  
- 运行主机，然后删除扩展应用。
    - 主机是否会正确检测到删除？
- 运行主机，然后将扩展应用更新为较新的版本。
    - 主机是否会选择更改并正确卸载旧版本的扩展？  

**要测试的高级方案：**

- 运行主机，将扩展应用移动到可移动媒体，删除该媒体
    - 主机是否会检测到程序包状态的变化并禁用扩展？
- 运行主机，然后销毁扩展应用（使其失效、以不同方式签名等等。）
    - 主机是否会检测到篡改的扩展并对其进行正确处理？
- 运行主机，然后部署具有无效内容或属性的扩展应用
    - 主机是否会检测到无效的内容并对其进行正确处理？

## <a name="design-considerations"></a>设计注意事项

- 提供 UI，以向用户显示可用的扩展并允许他们启用/禁用它们。 你还可以考虑为因程序包脱机等原因变得不可用的扩展添加字形。
- 将用户指引到可以获得扩展的地方。 或许你的扩展页面可以提供一个 Microsoft Store 搜索查询，从而显示可用于你的应用的扩展列表。
- 考虑在添加和删除扩展时如何通知用户。 你可以创建一个安装新扩展时发出的通知，并邀请用户启用扩展。 扩展默认情况下应该处于禁用状态，以便用户可以进行控制。

## <a name="how-app-extensions-differ-from-optional-packages"></a>应用扩展与可选包的区别

[可选包](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)与应用扩展之间的主要区别在于：一个是开放式生态系统，一个是封闭式生态系统；一个是依赖性程序包，一个是独立程序包。

应用扩展属于开放式生态系统。 如果你的应用可以托管应用扩展，则只要遵循你的扩展信息传递/接收方法，任何人都可以为你的主机编写扩展。 这与属于封闭式生态系统的可选包不同，在该系统中，发布者决定允许谁制作可与应用配合使用的可选包。

应用扩展是独立的程序包，并且可能是独立的应用。 它们不能依赖于另一个应用进行部署。可选包需要主程序包，并且没有主程序包就无法运行。

游戏扩展包将非常适合作为可选包，因为它紧紧地绑定到游戏上，不能独立于游戏而运行，并且你可能不希望扩展包随便由生态系统中的任何开发者进行创建。

如果该同一游戏具有可自定义的 UI 加载项或主题，则应用扩展可能是一个不错的选择，因为提供扩展的应用可独立运行，并且任何第三方都可以制作它们。

## <a name="remarks"></a>备注

本主题介绍应用扩展。 以下是需要注意的重要事项：创建主机并在其 Package.appxmanifest 文件内将其标记为主机、创建扩展并在其 Package.appxmanifest 文件内将其标记为扩展、确定如何实现扩展（如应用服务、后台任务或其他方式）、定义主机与扩展的通信方式，以及使用 [AppExtensions API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) 访问和管理扩展。

## <a name="related-topics"></a>相关主题

* [应用扩展简介](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [版本 2016 的应用扩展相关会话](https://channel9.msdn.com/Events/Build/2016/B808)
* [版本 2016 的应用扩展代码示例](https://github.com/Microsoft/App-Extensibility-Sample)
* [使用后台任务支持应用](support-your-app-with-background-tasks.md)
* [如何创建和使用应用服务](how-to-create-and-consume-an-app-service.md)。
* [AppExtensions 命名空间](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [用服务、扩展和包扩展应用](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)
