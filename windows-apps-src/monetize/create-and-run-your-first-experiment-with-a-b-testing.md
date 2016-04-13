---
Description: 在本演练中，通过 A/B 测试创建和运行你的第一个实验。
title: 通过 A/B 测试创建和运行你的第一个实验
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
---

# 通过 A/B 测试创建和运行你的第一个实验

在本演练中，你将：
* 在 Windows 开发人员中心仪表板上创建测试成功更改应用按钮的背景色是否会增加按钮点击数的实验。
* 创建从开发人员中心检索变体设置的应用、使用此数据更改按钮背景色，并将视图和转换事件数据记录到开发人员中心。
* 运行该应用以收集实验数据。
* 在开发人员中心仪表板上查看实验结果、选择变体以为所有用户启用该应用，并完成实验。

有关开发人员中心的 A/B 测试概述，请参阅 [Run app experiments with A/B testing（通过 A/B 测试运行应用实验）](run-app-experiments-with-a-b-testing.md)。

## 先决条件

为执行此操作实例，必须拥有 Windows 开发人员中心帐户，并且必须按照 [Run app experiments with A/B testing（通过 A/B 测试运行应用实验）](run-app-experiments-with-a-b-testing.md)中所述配置开发计算机。

## 在 Windows 开发人员中心中创建实验

1. 登录到[开发人员中心仪表板](https://dev.windows.com/overview)。
2. 如果你在开发人员中心中已有想要用于创建实验的应用，则请在**“你的应用”**下选择该应用。 如果你在仪表板中尚未有应用，[请通过预留名称来创建新应用](../publish/create-your-app-by-reserving-a-name.md)，然后再在仪表板中选择该应用。
3. 在导航窗格中，单击**“服务”**，然后再单击**“实验”**。
4. 在 **API 密钥**部分中，选择**“新建 API 密钥”**来生成新 API 密钥，并输入该 API 密钥的名称**“我的第一个实验”**。 你将在本演练的下一部分中使用此 API 密钥。
5. 在**实验**部分中，单击**“新建实验”**。 在**“实验名称”**字段中，键入名称**“优化按钮单击”**。
6. 在**“视图事件名称”**字段中，键入名称 **userViewedButton**。 稍后在本演练中，你将添加在初始化应用主页并且用户可看到按钮时记录此视图事件的代码。
7. 在**目标和转换事件**部分中，输入以下值：
  * 在**“目标名称”**字段中，键入**“提高按钮单击量”**。
  * 在**“转换事件名称”**字段中，键入名称 **userClickedButton**。 稍后在本演练中，你将添加在按钮的单击事件处理程序中记录此转换事件的代码。
  * 在**“目标”**字段中，选择**“最大化”**。
8. 在**变体和设置**部分中，单击**“添加设置”**三次。 现在你有四行的空设置。
  * 在第一行中，键入 **buttonText** 作为设置名、在**变体 A** 列中键入**“灰色按钮”**，然后在**变体 B** 列中键入**“蓝色按钮”**。
  * 在第二行中，键入 **r** 作为设置名、在**变体 A** 列中键入 **128**，然后在**变体 B** 列中键入 **1**。
  * 在第三行中，键入 **g** 作为设置名、在**变体 A** 列中键入 **128**，然后在**变体 B** 列中键入 **1**。
  * 在第四行中，键入 **b** 作为设置名、在**变体 A** 列中键入 **128**，然后在**变体 B** 列中键入 **255**。  
9. 确认已选中**“平均分配”**复选框，以便变体将平均分配到你的应用。
10. 单击**“保存”**，然后单击**“激活”**。

> **重要提示** 激活实验后，你不可再对实验参数进行修复，除非该实验为测试实验（创建实验时，选中**“测试实验”**复选框）。 通常在激活实验之前，我们建议你在应用中为实验编码。 为简单起见，在本演练中你可以立即激活实验。

## 在应用中为实验编码

1. 在 Visual Studio 2015 中，使用 Visual C# 创建新的通用 Windows 平台项目。 将该项目命名为 **SampleExperiment**。
2. 在“解决方案资源管理器”中，展开项目节点、右键单击**“引用”**，然后选择**“添加引用”**。
3. 在**“引用管理器”**中，展开**“通用 Windows”**并单击**“扩展”**。
4. 在 SDK 列表中，选择 **Microsoft 官方商城协议 SDK** 旁边的复选框，并单击**“确定”**。
5. 在**“解决方案资源管理器”**中，双击 MainPage.xaml 以在应用中打开主页的设计器。
6. 将**“按钮”**从**“工具箱”**拖动到该页。
7. 在设计器上双击按钮以打开代码文件，并为 **Click** 事件添加事件处理程序。  
8. 将代码文件的所有内容替换为以下代码。

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
    public sealed partial class MainPage : Page
    {
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. 在以下代码行中，将 *apiKey* 变量分配到在上一部分中从开发人员中心仪表板获得的 API 密钥。 以下所示的 API 密钥仅用作示例。
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. 保存代码文件，并生成项目。

## 运行该应用以收集实验数据

1. 运行你在上一部分中创建的 **SampleExperiment** 应用。
2. 确认你是看到了灰色按钮还是蓝色按钮。 单击该按钮，然后关闭应用。
3. 在相同计算机上重复上述步骤几次，确认应用显示相同的按钮色。

## 查看结果并完成实验

完成上一部分后至少等待几小时，然后遵循以下这些步骤来查看实验结果并完成实验。

> **注意** 在激活某个实验时，开发人员中心会立即开始从已检测的所有应用中收集数据，从而为你的实验记录数据。 但是，可能需要几个小时实验数据才会显示在仪表板上。

1. 在开发人员中心，返回到应用的**实验**页面。
2. 在**实验**部分中，单击**“活动”**筛选器，然后单击**“优化按钮单击”**以转到此实验页面。
3. 确认在**结果摘要**和**结果详细信息**部分中显示的结果与你期望看到的结果相匹配。 有关这些部分的详细信息，请参阅 [Manage your experiment in the Dev Center dashboard（在开发人员中心仪表板中管理你的实验）](manage-your-experiment.md#review-the-results-of-your-experiment)。

  >**注意** 开发人员中心中在 24 小时时间段内向每位用户仅报告第一个转换事件。 如果用户在 24 小时时间段内触发多个转换事件，则将仅报告第一个转换事件。 这是为了帮助防止触发很多转换事件的单个用户扭曲一组示例用户得出的实验结果。

4. 现在，你已准备好结束实验了。 在**结果摘要**部分的**变体 B** 列中，单击**“切换”**。 这会将应用的所有用户切换到蓝色按钮。
5. 单击**“确定”**以确认你想要结束该实验。
6. 运行你在上一部分中创建的 **SampleExperiment** 应用。
7. 确认你看到了一个蓝色的按钮。 请注意，你的应用接收更新的变体分配最多需要两分钟。

## 相关主题

  * [在开发人员中心仪表板中定义你的实验](define-your-experiment-in-the-dev-center-dashboard.md)
  * [针对实验为你的应用编码](code-your-experiment-in-your-app.md)
  * [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
  * [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


