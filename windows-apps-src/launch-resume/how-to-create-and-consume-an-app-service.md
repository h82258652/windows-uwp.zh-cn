---
title: 创建和使用应用服务
description: 了解如何编写可以向其他 UWP 应用提供服务的通用 Windows 平台 (UWP) 应用，以及如何使用这些服务。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 应用程序通信，进程间通信，IPC，后台消息传递，后台通信，应用到应用，应用服务
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d6edc49bc97a336b8d722c496c1980a5f9b0efb
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393560"
---
# <a name="create-and-consume-an-app-service"></a>创建和使用应用服务

应用服务是可向其他 UWP 应用提供服务的 UWP 应用。 它们与设备上的 Web 服务类似。 应用服务作为后台任务在主机应用中运行，并可向其他应用提供其服务。 例如，应用服务可能会提供其他应用可能使用的条形码扫描仪服务。 应用的企业套件中可能有一个通用的拼写检查应用服务，该服务可供套件中的其他应用使用。  应用服务允许你创建应用可在同一设备上调用的无 UI 服务，从 Windows 10 版本 1607 开始，应用可在远程设备上调用这些服务。

从 Windows 10 版本 1607 开始，可以创建在与主机应用相同的进程中运行的应用服务。 本文主要介绍如何在单独后台进程中创建和使用应用服务。 有关在提供程序所在的同一进程中运行应用服务的更多详细信息，请参阅[将应用服务转换为在其托管应用所在的同一进程中运行的服务](convert-app-service-in-process.md)。

有关应用服务代码示例，请参阅[通用 Windows 平台 (UWP) 应用示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)。

## <a name="create-a-new-app-service-provider-project"></a>创建新的应用服务提供程序项目

在本操作方法中，我们将创建一个适用于简单解决方案的所有内容。

1. 在 Visual Studio 2015 或更高版本中，创建新的 UWP 应用项目，并将其命名为**AppServiceProvider**。
    1. 选择 "**文件" > 新建 > 项目 ...** 
    2. 在 "**创建新项目**" 对话框中，选择 "**空白应用（通用 Windows C#）** "。 这将是能够向其他 UWP 应用提供应用服务的应用。
    3. 单击 "**下一步**"，然后将项目命名为**AppServiceProvider**，为其选择位置，然后单击 "**创建**"。

2. 当系统提示选择项目的**目标**和**最小版本**时，请选择 "至少**10.0.14393**"。 如果要使用新的**SupportsMultipleInstances**属性，则必须使用 visual studio 2017 或 visual studio 2019，并使用目标**10.0.15063** （**Windows 10 创意者更新**）或更高版本。

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>向 appxmanifest.xml 添加应用服务扩展

在**AppServiceProvider**项目中，在文本编辑器中打开**appxmanifest.xml**文件： 

1. 在**解决方案资源管理器**中右键单击它。 
2. 选择 "**打开方式**"。 
3. 选择 " **XML （文本）编辑器**"。 

在`AppService` 元素`<Application>`内添加以下扩展。 此示例介绍了 `com.microsoft.inventory` 服务，以及将此应用识别为应用服务提供程序的内容。 实际服务将作为后台任务来实现。 应用服务项目将该服务公开给其他应用。 我们建议将反向域名样式用于服务名称。

请注意，只有面向 Windows SDK 版本 10.0.15063 或更高版本，`xmlns:uap4` 命名空间前缀和 `uap4:SupportsMultipleInstances` 属性才有效。 如果面向较旧的 SDK 版本，则可以放心地删除它们。

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

属性`Category`将此应用程序标识为应用服务提供程序。

`EntryPoint`特性标识实现服务的命名空间限定类，接下来我们将实现该服务。

此`SupportsMultipleInstances`属性指示每次调用应用服务时，它应在新进程中运行。 这不是必需的，但如果需要该功能并面向 10.0.15063 SDK （**Windows 10 创意者更新**）或更高版本，则可以使用此功能。 还应该对其加上 `uap4` 命名空间前缀。

## <a name="create-the-app-service"></a>创建应用服务

1.  应用服务可作为后台任务实现。 这允许前台应用程序调用另一个应用程序中的应用服务。 若要创建应用服务作为后台任务，请将新的 Windows 运行时组件项目添加到名为**MyAppService**的解决方案（**文件&gt; &gt;添加新项目**）。 在 "**添加新项目**" 对话框中，选择 "**安装C# > Visual > Windows 运行时组件（通用 Windows）** "。
2.  在**AppServiceProvider**项目中，添加对新的**MyAppService**项目的项目到项目引用（在**解决方案资源管理器**中，右键单击 " **AppServiceProvider** " 项目 > "添加"  >  **引用**项目解决方案，请选择 "MyAppService > "。 >  >  此步骤至关重要，因为如果不添加引用，应用服务将不会在运行时进行连接。
3.  在**MyAppService**项目中，将以下**using**语句添加到**Class1.cs**的顶部：
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  将**Class1.cs**重命名为**Inventory.cs**，并将**Class1**的存根代码替换为名为 "**清单**" 的新后台任务类：

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    此类位于应用服务将起作用的位置。

    创建后台任务时，将调用**Run** 。 由于 **Run** 完成后会终止后台任务，因此代码会执行延迟，以便后台任务可以继续服务请求。 作为后台任务实现的应用服务将在收到呼叫后的大约30秒内保持活动状态大约30秒，除非在该时间范围内再次调用它或延迟被停止。如果应用服务是在与调用方相同的进程中实现的，则应用服务的生存期将与调用方的生存期相关联。

    应用服务的生命周期取决于调用方：

    * 如果调用方处于前台，则应用服务生存期与调用方相同。
    * 如果调用方处于后台，应用服务将获得30秒运行。 执行延迟可一次多提供 5 秒。

    当任务被取消时，将调用**OnTaskCanceled** 。 当客户端应用程序释放[AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)、客户端应用程序已挂起、操作系统关闭或休眠或操作系统耗尽运行任务时，将取消该任务。

## <a name="write-the-code-for-the-app-service"></a>编写应用服务的代码

**OnRequestReceived**是应用服务代码的位置。 将**MyAppService**的**Inventory.cs**中的存根**OnRequestReceived**替换为此示例中的代码。 此代码获取库存项目的索引，并将该索引以及命令字符串传递到服务以检索指定库存项目的名称和价格。 对于你自己的项目，请添加错误处理代码。

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if (inventoryIndex.HasValue &&
        inventoryIndex.Value >= 0 &&
        inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

请注意， **OnRequestReceived**是**异步**的，因为在此示例中，我们对[SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync)进行了可等待方法调用。

采用延迟，以便服务可以在**OnRequestReceived**处理程序中使用**异步**方法。 它可确保对 **OnRequestReceived** 的调用不会在完成处理消息之前结束。  [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync)将结果发送到调用方。 **SendResponseAsync** 不会在调用完成时发出信号。 这是延迟的完成，该延迟表明**OnRequestReceived**已完成[SendMessageAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) 。 对**SendResponseAsync**的调用包装在 try/finally 块中，因为即使**SendResponseAsync**引发了异常，也必须完成延迟。

应用服务使用[ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)对象来交换信息。 可以传递的数据大小仅受限于系统资源。 没有你可以在 **ValueSet** 中使用的预定义项。 你必须确定哪些项值将用于定义你的应用服务的协议。 请牢记，必须使用该协议编写调用方。 在此示例中，我们选择了名为 `Command` 的项，它具有一个值，用于指示我们是否希望应用服务提供库存项目的名称或其价格。 库存名称的索引存储在 `ID` 项下。 返回值存储在 `Result` 项下。

将[AppServiceClosedStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus)枚举返回给调用方，以指示对应用服务的调用是成功还是失败。 对应用服务的调用可能失败的原因示例：操作系统中止服务端点，因为资源已耗尽。 可以通过 [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) 返回其他错误信息。 在此示例中，我们使用名为 `Status` 的项将更详细的错误信息返回给调用方。

对 [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 的调用将 [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) 返回给调用方。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>部署服务应用并获取程序包系列名称

必须先部署应用服务提供程序，然后才能从客户端调用它。 可以通过在 Visual Studio 中选择 "**生成 > 部署解决方案**" 来部署它。

还需要应用服务提供商的包系列名称才能调用它。 可以通过在设计器视图中打开**AppServiceProvider**项目的**appxmanifest.xml**文件（在**解决方案资源管理器**中双击该文件）来获取此文件。 选择 "**打包**" 选项卡，复制 "**包系列名称**" 旁边的值，并将其粘贴到记事本之类的位置。

## <a name="write-a-client-to-call-the-app-service"></a>编写客户端以调用应用服务

1.  将新的空白 Windows 通用应用项目添加到解决方案（**文件 &gt; 添加 &gt; 新建项目**）。 在 "**添加新项目**" 对话框中，选择 "**已C#安装 > Visual > 空白应用（通用 Windows）** 并将其命名为**ClientApp**。

2.  在**ClientApp**项目中，将以下**using**语句添加到**MainPage.xaml.cs**的顶部：
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  向**MainPage**添加一个名为**textBox**和一个按钮的文本框。

4.  添加一个名为 " **button_Click**" 的按钮的按钮单击处理程序，并将关键字**async**添加到该按钮处理程序的签名。

5. 将按钮单击处理程序内的存根区域替换为以下代码。 请确保包含 `inventoryService` 字段声明。
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
               return;
           }
       }

       // Call the service.
       int idx = int.Parse(textBox.Text);
       var message = new ValueSet();
       message.Add("Command", "Item");
       message.Add("ID", idx);
       AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
       string result = "";

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data  that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result = response.Message["Result"] as string;
           }
       }

       message.Clear();
       message.Add("Command", "Price");
       message.Add("ID", idx);
       response = await this.inventoryService.SendMessageAsync(message);

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result += " : Price = " + response.Message["Result"] as string;
           }
       }

       textBox.Text = result;
   }
   ```
    
    使用在[部署服务应用并获取包系列名称](#deploy-the-service-app-and-get-the-package-family-name)中上面获取的 **AppServiceProvider** 项目的包系列名称来替换行 `this.inventoryService.PackageFamilyName = "Replace with the package family name";` 中的包系列名称。

    > [!NOTE]
    > 请确保粘贴字符串文本，而不是将其放在变量中。 如果使用变量，它将不起作用。

    该代码首先建立了与应用服务的连接。 该连接将保持打开状态，直到你释放 `this.inventoryService`。 应用服务名称必须与添加到`AppService` **AppServiceProvider**项目`Name`的**appxmanifest.xml**文件的元素的属性匹配。 在此示例中为 `<uap3:AppService Name="com.microsoft.inventory"/>`。

    创建名为`message`的 [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)，以指定要发送到应用服务的命令。 示例应用服务需要命令指示要采取两种操作中的哪一种操作。 我们从客户端应用程序中的文本框获取索引，然后通过`Item`命令调用服务，以获取项的说明。 然后，我们使用 `Price` 命令进行调用，以获取项目的价格。 按钮文本设置为结果。

    由于[AppServiceResponseStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus)仅指示操作系统是否能够连接到应用服务的调用，因此我们会检查从应用服务接收`Status`的[ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)中的密钥，以确保它能够满足需要.

6. 将**ClientApp**项目设置为启动项目（右键单击 "**解决方案资源管理器** > **设置为启动项目**"），然后运行解决方案。 在文本框中输入数字 1 并单击该按钮。 应该会获得 "椅子：价格 = 88.99 "从服务返回。

    ![显示 chair price=88.99 的示例应用](images/appserviceclientapp.png)

如果应用服务调用失败，请在**ClientApp**项目中检查以下内容：

1.  验证分配给清单服务连接的包系列名称是否与**AppServiceProvider**应用的包系列名称匹配。 在**按钮\_中单击** `this.inventoryService.PackageFamilyName = "...";`"行"。
2.  在**按钮\_单击**中，验证分配给清单服务连接的应用服务名称是否与**AppServiceProvider**的**appxmanifest.xml**文件中的应用服务名称匹配。 请参阅 `this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  确保已部署**AppServiceProvider**应用。 （在**解决方案资源管理器**中，右键单击该解决方案，然后选择 "**部署解决方案**"）。

## <a name="debug-the-app-service"></a>调试应用服务

1.  确保调试之前已部署解决方案，因为必须先部署应用服务提供程序应用，才能调用服务。 （在 Visual Studio 中，**生成&gt;部署解决方案**）。
2.  在**解决方案资源管理器**中，右键单击**AppServiceProvider**项目，然后选择 "**属性**"。 从**调试**选项卡中，将**开始操作**更改为**不启动，但在启动时调试代码**。 （请注意，如果你以前使用 C++ 来实现应用服务提供程序，则应从**调试**选项卡中将**启动应用程序**更改为**否**）。
3.  在**MyAppService**项目中的**Inventory.cs**文件中，在**OnRequestReceived**中设置一个断点。
4.  将**AppServiceProvider**项目设置为启动项目，然后按**F5**。
5.  从 "开始" 菜单（而不是从 Visual Studio）启动**ClientApp** 。
6.  在文本框中输入数字 1 并按该按钮。 调试程序将停止应用服务中的断点上的应用服务调用。

## <a name="debug-the-client"></a>调试客户端

1.  按照前面步骤中的说明来调试调用应用服务的客户端。
2.  从 "开始" 菜单启动**ClientApp** 。
3.  将调试器附加到**ClientApp**进程（而不是**ApplicationFrameHost**进程）。 （在 Visual Studio 中，依次选择**调试&gt;附加到进程...** 。）
4.  在**ClientApp**项目中，在**按钮\_单击**中设置断点。
5.  当你在 " **ClientApp** " 文本框中输入数字1并单击该按钮时，将会命中客户端和应用服务中的断点。

## <a name="general-app-service-troubleshooting"></a>常规应用服务疑难解答

如果尝试连接到应用服务后遇到**AppUnavailable**状态，请检查以下内容：

- 确保部署了应用服务提供程序项目和应用服务项目。 二者都需要在运行客户端之前进行部署，否则客户端将没有可连接到的任何对象。 你可以使用**版本** > **部署解决方案**从 Visual Studio 中部署。
- 在**解决方案资源管理器**中，确保应用服务提供程序项目具有对实现应用服务的项目的项目到项目的引用。
- 验证是否已将 `<Extensions>`条目及其子元素添加到属于应用服务提供程序项目的**Package.appxmanifest** 文件中，如上面的[将应用服务扩展添加到appxmanifest.xml](#appxmanifest)中所指定的那样。
- 确保客户端中调用应用服务提供程序的[AppServiceConnection AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename)字符串与应用服务提供程序`<uap3:AppService Name="..." />`项目的**appxmanifest.xml**文件中的指定匹配。
- 确保[AppServiceConnection](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname)与在[将应用服务扩展添加到包](#appxmanifest)中指定的应用服务提供程序组件的包系列名称匹配。 appxmanifest.xml
- 对于进程外应用服务（如本示例中的服务），验证应用服务提供程序`EntryPoint`项目的 appxmanifest.xml `<uap:Extension ...>`文件的元素中指定的是否与公共的命名空间和类名匹配 **。** 实现应用服务项目中的[IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask)的类。

### <a name="troubleshoot-debugging"></a>调试疑难解答

如果调试程序在应用服务提供程序或应用服务项目中的断点处未停止，请检查以下各项：

- 确保部署了应用服务提供程序项目和应用服务项目。 二者都需要在运行客户端之前进行部署。 你可以使用**版本** > **部署解决方案**从 Visual Studio 中部署它们。
- 确保要调试的项目设置为启动项目，并且在按下**F5**时，该项目的调试属性设置为 "不运行项目"。 右键单击项目，然后依次单击**属性**和**调试**（或者在 C++ 中单击**调试**）。 在 C# 中，将**开始操作**更改为**不启动，但在启动时调试代码**。 在 C++ 中，将**启动应用程序**设置为**否**。

## <a name="remarks"></a>备注

本示例介绍了创建一个作为后台任务运行的应用服务并从另一个应用调用它的情形。 需要注意的重要事项如下：

* 创建用于托管应用服务的后台任务。
* 将扩展添加到应用服务提供商的**appxmanifest.xml 文件。** `windows.appService`
* 获取应用服务提供商的包系列名称，以便我们可以从客户端应用程序连接到该服务提供商。
* 将应用服务提供程序项目中的项目到项目引用添加到应用服务项目。
* 使用[windows.applicationmodel.resources.core. AppService. AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)调用服务。

## <a name="full-code-for-myappservice"></a>MyAppService 的完整代码

```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## <a name="related-topics"></a>相关主题

* [将应用服务转换为与其主机应用在同一个进程中运行](convert-app-service-in-process.md)
* [支持包含后台任务的应用](support-your-app-with-background-tasks.md)
* [应用服务代码示例（C#、 C++和 VB）](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
