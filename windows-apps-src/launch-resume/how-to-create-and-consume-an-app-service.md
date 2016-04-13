---
创建和使用应用服务
了解如何编写可以向其他 UWP 应用提供服务的通用 Windows 平台 (UWP) 应用，以及如何使用这些服务。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# 创建和使用应用服务


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


了解如何编写可以向其他 UWP 应用提供服务的通用 Windows 平台 (UWP) 应用，以及如何使用这些服务。

## 创建新的应用服务提供程序项目


在本操作方法中，我们将创建一个适用于简单解决方案的所有内容。

-   在 Microsoft Visual Studio 2015 中，创建一个新的 UWP 应用项目，并将其命名为“AppServiceProvider”。 （在**“新建项目”**对话框中，选择**“模板”>“其他语言”>“Visual C#”>“Windows”>“Windows 通用”>“空白应用(Windows 通用)”**。） 这将是可提供应用服务的应用。

## 将应用服务扩展添加到 package.appxmanifest


在 AppServiceProvider 项目的 Package.appxmanifest 文件中，将以下 AppService 扩展添加到 **<Application>** 元素。 此示例介绍了 `com.Microsoft.Inventory` 服务，以及将此应用识别为应用服务提供程序的内容。 实际服务将作为后台任务来实现。 应用服务应用将该服务公开给其他应用。 我们建议将反向域名样式用于服务名称。

``` syntax
... 
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

**Category** 属性将该应用程序识别为应用服务提供程序。

**EntryPoint** 属性可识别实现服务的类，我们将在下一步实现它。

## 创建应用服务


1.  应用服务作为后台任务实现。 这允许前台应用程序调用另一个应用程序中的应用服务以便执行后台任务。 将新的 Windows 运行时组件对象添加到名为“MyAppService”的解决方案（**“文件”>“添加”>“新建项目”**）。 （在**“添加新项目”**对话框中，选择**“已安装”>“其他语言”>“Visual C#”>“Windows”>“Windows 通用”>“Windows 运行时组件(Windows 通用)”**）
2.  在 AppServiceProvider 项目中，添加对 MyAppService 项目的引用。
3.  在 MyappService 项目中，将以下 **using** 语句添加到 Class1.cs 的顶部：
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  将 **Class1** 的存根区域代码替换为名为 **Inventory** 的新后台任务类：

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
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

    创建后台任务时会调用 **Run()**。 由于 **Run** 完成后会终止后台任务，因此代码执行延迟，以便后台任务可以继续服务请求。

    取消任务时会调用 **OnTaskCanceled()**。 当出现以下情形时会取消该任务：客户端应用释放 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704)，客户端应用暂停，操作系统关闭或处于睡眠状态，或操作系统耗尽用于运行该任务的资源。

## 编写应用服务的代码


**OnRequestedReceived()** 跟随应用服务的代码出现。 将 MyAppService 的 Class1.cs 中的存根区域 **OnRequestedReceived()** 替换为此示例中的代码。 此代码获取库存项目的索引，并将该索引以及命令字符串传递到服务以检索指定库存项目的名称和价格。 为了简便起见，已删除错误处理代码。

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don&#39;t want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &amp;&amp;
         inventoryIndex.Value >= 0 &amp;&amp;
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

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
}
```

请注意，**OnRequestedReceived()** 为 **async**，因为我们在此示例中使用对 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 的可等待方法调用。

将使用延迟，以便该服务可以在 OnRequestReceived 处理程序中使用 **async** 方法。 确保对 OnRequestReceived 的调用不会在完成处理消息之前结束。 [
            **SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 用于在完成时发送响应。 **SendResponseAsync** 不会在调用完成时发出信号。 延迟完成时会发信号给 [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712)，以表明 OnRequestReceived 已完成。

应用服务使用 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 交换信息。 可以传递的数据大小仅受限于系统资源。 没有你可以在 **ValueSet** 中使用的预定义项。 你必须确定哪些项值将用于定义你的应用服务的协议。 请牢记，必须使用该协议编写调用方。 在此示例中，我们选择了名为“Command”的项，它具有一个值，用于指示我们是否希望应用服务提供库存项目的名称或其价格。 库存名称的索引存储在“ID”项下。 返回值存储在“Result”项下。

[
            **AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) 枚举将返回给调用方，以指示对应用服务的调用是否成功。 对应用服务的调用可能失败的原因示例：操作系统中止服务端点，资源耗尽等。 可以通过 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 返回其他错误信息。 在此示例中，我们使用名为“Status”的项将更详细的错误信息返回给调用方。

对 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 的调用将 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 返回给调用方。

## 部署服务应用并获取程序包系列名称


必须先部署应用服务提供程序应用，才可以通过客户端调用它。 还需要应用服务应用的程序包系列名称，以便调用它。

-   获取应用服务应用程序的程序包系列名称的一个方法是从 **AppServiceProvider** 项目内（例如，从 App.xaml.cs 中的 `public App()`）调用 [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) 并记下结果。 若要在 Microsoft Visual Studio 中运行 AppServiceProvider，请在“解决方案资源管理器”窗口中将其设置为启动项目并运行该项目。
-   获取程序包系列名称的另一个方法是部署解决方案（**“生成”>“部署解决方案”**）并记下输出窗口中的完整程序包名称（**“查看”>“输出”**）。 必须从输出窗口中的字符串中去除平台信息，以获取程序包名称。 例如，如果输出窗口中呈现的完整程序包名称为“9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra”，可以通过去除“9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra”提取“1.0.0.0\_x86\_\_”作为程序包系列名称。

## 编写客户端以调用应用服务


1.  将新的空白 Windows 通用应用项目添加到名为“ClientApp”的解决方案（**“文件”>“添加”>“新建项目”**）。 （在**“添加新项目”**对话框中，依次选择**“已安装”>“其他语言”>“Visual C#”>“Windows”>“Windows 通用”>“空白应用(Windows 通用)”**）。
2.  在 ClientApp 项目中，将以下 **using** 语句添加到 MainPage.xaml.cs 的顶部：
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  将文本框和按钮添加到 MainPage.xaml。
4.  添加按钮的按钮单击处理程序，并将关键字 **async** 添加到按钮处理程序的签名。
5.  将按钮单击处理程序内的存根区域替换为以下代码。 请确保包含 `inventoryService` 字段声明。

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider&#39;s Package.appxmanifest file in the &lt;Extension&gt; section. 
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
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
            // Get the data  that the service sent  to us.
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

        button.Content = result;
    }
    ```

    Replace the package family name in the line `this.inventoryService.PackageFamilyName = "replace with the package family name";` with the package family name of the **AppServiceProvider** project that you obtained in \[Step 5: Deploy the service app and get the package family name\].

    The code first establishes a connection with the app service. The connection will remain open until you dispose **this.inventoryService**. The app service name must match the **AppService Name** attribute that you added to the AppServiceProvider project's Package.appxmanifest file. In this example, it is `<uap:AppService Name="com.microsoft.inventory"/>`.

    A [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) named **message** is created to specify the command that we want to send to the app service. The example app service expects a command to indicate which of two actions to take. We get the index from the textbox in the ClientApp, and then call the service with the "Item" command to get the description of the item. Then, we make the call with the "Price" command to get the item's price. The button text is set to the result.

    Because [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) only indicates whether the operating system was able to connect the call to the app service, we check the "Status" key in the [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) we receive from the app service to ensure that it was able to fulfill the request.

6.  In Visual Studio, set the ClientApp project to be the startup project in the Solution Explorer window and run the solution. Enter the number 1 into the text box and click the button. You should get "Chair : Price = 88.99" back from the service.

    ![sample app displaying chair price=88.99](images/appserviceclientapp.png)

If the app service call fails, check the following in the ClientApp:

1.  Verify that the package family name assigned to the inventory service connection matches the package family name of the AppServiceProvider app. See: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  In **button\_Click()**, verify that the app service name that is assigned to the inventory service connection matches the app service name in the AppServiceProvider's Package.appxmanifest file. See: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Ensure that the AppServiceProvider app has been deployed (In the Solution Explorer, right-click the solution and choose **Deploy**).

## Debug the app service


1.  Ensure that the entire solution is deployed before debugging because the app service provider app must be deployed before the service can be called. (In Visual Studio, **Build &gt; Deploy Solution**).
2.  In the Solution Explorer, right-click the AppServiceProvider project and choose **Properties**. From the **Debug** tab, change the **Start action** to **Do not launch, but debug my code when it starts**.
3.  In the MyAppService project, in the Class1.cs file, set a breakpoint in OnRequestReceived().
4.  Set the AppServiceProvider project to be the startup project and press F5.
5.  Start ClientApp from the Start menu (not from Visual Studio).
6.  Enter the number 1 into the text box and press the button. The debugger will stop in the app service call on the breakpoint in your app service.

## Debug the client


1.  Follow the instructions in the preceding step to debug the app service.
2.  Launch ClientApp from the Start menu.
3.  Attach the debugger to the ClientApp.exe process (not the ApplicationFrameHost.exe process). (In Visual Studio, choose **Debug &gt; Attach to Process...**.)
4.  In the ClientApp project, set a breakpoint in **button\_Click()**.
5.  The breakpoints in both the client and the app service will now be hit when you enter the number 1 into the text box of the ClientApp and click the button.

## Remarks


This example provides a simple introduction to creating an app service and calling it from another app. The key things to note are the creation of a background task to host the app service, the addition of the windows.appservice extension to the app service provider app's Package.appxmanifest file, obtaining the package family name of the app service provider app so that we can connect to it from the client app, and using [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) to call the service.

## Full code for MyAppService


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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don&#39;t want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &amp;&amp;
                 inventoryIndex.Value >= 0 &amp;&amp;
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

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
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

## 相关主题


* [使用后台任务支持应用](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->


