---
author: TylerMSFT
title: "创建和使用应用服务"
description: "了解如何编写可以向其他 UWP 应用提供服务的通用 Windows 平台 (UWP) 应用，以及如何使用这些服务。"
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: app to app
translationtype: Human Translation
ms.sourcegitcommit: d7d7edf8d1ed6ae1c4be504cd4827bb941f14380
ms.openlocfilehash: 13b9456d1f6ee2b592db0e5e38b9f9e7fe41764c

---

# 创建和使用应用服务


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


了解如何编写可以向其他 UWP 应用提供服务的通用 Windows 平台 (UWP) 应用，以及如何使用这些服务。

## 创建新的应用服务提供程序项目


在本操作方法中，我们将创建一个适用于简单解决方案的所有内容。

-   在 Microsoft Visual Studio 2015 中，创建一个新的 UWP 应用项目，并将其命名为“AppServiceProvider”。 （在“新建项目”****对话框中，依次选择“模板”&gt;“其他语言”&gt;“Visual C#”&gt;“Windows”&gt;“Windows 通用”&gt;“空白应用(Windows 通用)”****）。 这将是可提供应用服务的应用。

## 将应用服务扩展添加到 package.appxmanifest


在 AppServiceProvider 项目的 Package.appxmanifest 文件中，将以下 AppService 扩展添加到 **&lt;Application&gt;** 元素。 此示例介绍了 `com.Microsoft.Inventory` 服务，以及将此应用识别为应用服务提供程序的内容。 实际服务将作为后台任务来实现。 应用服务应用将该服务公开给其他应用。 我们建议将反向域名样式用于服务名称。

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


1.  应用服务作为后台任务实现。 这允许前台应用程序调用另一个应用程序中的应用服务以便执行后台任务。 将新的 Windows 运行时组件项目添加到名为“MyAppService”的解决方案（“文件”&gt;“添加”&gt;“新建项目”****）。 （在“添加新项目”****对话框中，依次选择“已安装”&gt;“其他语言”&gt;“Visual C#”&gt;“Windows”&gt;“Windows 通用”&gt;“Windows 运行时组件(Windows 通用)”****）
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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
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
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
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

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
}
```

请注意，**OnRequestedReceived()** 为 **async**，因为我们在此示例中使用对 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 的可等待方法调用。

将使用延迟，以便该服务可以在 OnRequestReceived 处理程序中使用 **async** 方法。 确保对 OnRequestReceived 的调用不会在完成处理消息之前结束。 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 用于在完成时发送响应。 **SendResponseAsync** 不会在调用完成时发出信号。 延迟完成时会发信号给 [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712)，以表明 OnRequestReceived 已完成。

应用服务使用 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 交换信息。 可以传递的数据大小仅受限于系统资源。 没有你可以在 **ValueSet** 中使用的预定义项。 你必须确定哪些项值将用于定义你的应用服务的协议。 请牢记，必须使用该协议编写调用方。 在此示例中，我们选择了名为“Command”的项，它具有一个值，用于指示我们是否希望应用服务提供库存项目的名称或其价格。 库存名称的索引存储在“ID”项下。 返回值存储在“Result”项下。

[**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) 枚举将返回给调用方，以指示对应用服务的调用是否成功。 对应用服务的调用可能失败的原因示例：操作系统中止服务端点，资源耗尽等。 可以通过 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 返回其他错误信息。 在此示例中，我们使用名为“Status”的项将更详细的错误信息返回给调用方。

对 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) 的调用将 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 返回给调用方。

## 部署服务应用并获取程序包系列名称


必须先部署应用服务提供程序应用，才可以通过客户端调用它。 还需要应用服务应用的程序包系列名称，以便调用它。

-   获取应用服务应用程序的程序包系列名称的一个方法是从 **AppServiceProvider** 项目内（例如，从 App.xaml.cs 中的 `public App()`）调用 [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) 并记下结果。 若要在 Microsoft Visual Studio 中运行 AppServiceProvider，请在“解决方案资源管理器”窗口中将其设置为启动项目并运行该项目。
-   获取程序包系列名称的另一个方法是部署解决方案（“生成”&gt;“部署解决方案”****）并记下输出窗口中的完整程序包名称（“查看”&gt;“输出”****）。 必须从输出窗口中的字符串中删除平台信息，以获取程序包名称。 例如，如果输出窗口中呈现的完整程序包名称为“9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra”，可以通过去除“9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra”提取“1.0.0.0\_x86\_\_”作为程序包系列名称。

## 编写客户端以调用应用服务


1.  将新的空白 Windows 通用应用项目添加到名为 ClientApp 的解决方案（“文件”&gt;“添加”&gt;“新建项目”****）。 （在“添加新项目”****对话框中，依次选择“已安装”&gt;“其他语言”&gt;“Visual C#”&gt;“Windows”&gt;“Windows 通用”&gt;“空白应用(Windows 通用)”****）。
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

            // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section.
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

    使用在 \[步骤 5：部署服务应用并获取程序包系列名称\] 中获取的 **AppServiceProvider** 项目的程序包系列名称来替换行 `this.inventoryService.PackageFamilyName = "replace with the package family name";` 中的程序包系列名称。

    该代码首先建立了与应用服务的连接。 该连接将保持打开状态，直到你释放 **this.inventoryService**。 应用服务名必须匹配添加到 AppServiceProvider 项目的 Package.appxmanifest 文件的 **AppService 名称**属性。 在此示例中为 `<uap:AppService Name="com.microsoft.inventory"/>`。

    创建了名为 **message** 的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)，以指定想要发送到应用服务的命令。 示例应用服务需要命令指示要采取两种操作中的哪一种操作。 我们从 ClientApp 中的文本框获取索引，然后通过“Item”命令调用该服务，以获取项目描述。 然后，我们使用“Price”命令进行调用，以获取项目的价格。 按钮文本设置为结果。

    由于 [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) 仅指示操作系统是否能够将调用连接到应用服务，所以我们要查看从应用服务中接收的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 的“Status”密钥，以确保它能满足该请求。

6.  在 Visual Studio 的“解决方案资源管理器”窗口中，将 ClientApp 项目设置为启动项目，并运行该解决方案。 在文本框中输入数字 1 并单击该按钮。 你应该从服务取回“Chair : Price = 88.99”。

    ![显示 chair price=88.99 的示例应用](images/appserviceclientapp.png)

如果应用服务调用失败，则检查 ClientApp 中的以下内容：

1.  验证分配给库存服务连接的程序包系列名称是否匹配 AppServiceProvider 应用的程序包系列名称。 请参阅：**button\_Click()**`this.inventoryService.PackageFamilyName = "...";`)。
2.  在 **button\_Click()** 中，验证分配给库存服务连接的应用服务名称是否匹配 AppServiceProvider 的 Package.appxmanifest 文件中的应用服务名称。 请参阅 `this.inventoryService.AppServiceName = "com.microsoft.inventory";`。
3.  确保已部署 AppServiceProvider 应用（在“解决方案资源管理器”中，右键单击该解决方案，然后选择“部署”****）。

## 调试应用服务


1.  确保调试之前已部署整个解决方案，因为必须先部署应用服务提供程序应用，服务才可以进行调用。 （在 Visual Studio 中，“生成”&gt;“部署解决方案”****。）
2.  在“解决方案资源管理器”中，右键单击 AppServiceProvider 项目，然后选择“属性”****。 在“调试”****选项卡上，将“开始操作”****更改为“不启动，但在开始时调试我的代码”****。
3.  在 MyAppService 项目的 Class1.cs 文件中，在 OnRequestReceived() 中设置断点。
4.  将 AppServiceProvider 项目设置为启动项目，并按 F5。
5.  从“开始”菜单（而不是从 Visual Studio）启动 ClientApp。
6.  在文本框中输入数字 1 并按该按钮。 调试程序将停止应用服务中的断点上的应用服务调用。

## 调试客户端


1.  按照前面步骤中的说明来调试应用服务。
2.  从“开始”菜单启动 ClientApp。
3.  将调试程序附加到 ClientApp.exe 进程（而不是 ApplicationFrameHost.exe 进程）。 （在 Visual Studio 中，依次选择“调试”&gt;“附加到进程...”****。）
4.  在 ClientApp 项目的 **button\_Click()** 中设置断点。
5.  在 ClientApp 的文本框中输入数字 1 并单击按钮时，将立刻命中客户端和应用服务中的断点。

## 备注


本示例简单介绍了创建应用服务并从另一个应用调用它的情形。 需要注意的重要事项是：创建后台任务以承载应用服务；添加 windows.appservice 扩展到应用服务提供程序应用的 Package.appxmanifest 文件；获取应用服务提供程序应用的程序包系列名称，以便我们可以从客户端应用与其连接，并使用 [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) 调用该服务。

## MyAppService 的完整代码


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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
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

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
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

 

 



<!--HONumber=Jul16_HO1-->


