---
author: mcleanbyron
Description: "在开发人员中心仪表板定义你的实验后，你就已经准备好在应用中编写实验代码。"
title: "针对实验为你的应用编码"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: 4e6706624e71c6d448a3d457c27d11c9f6ecc156

---

# 针对实验为你的应用编码

[在开发人员中心仪表板中定义某个实验](define-your-experiment-in-the-dev-center-dashboard.md)后，你已准备好更新通用 Windows 平台 (UWP) 应用的代码以获取实验的变量数据、使用此数据修改正在测试的功能的行为，并将视图事件和转换事件记录到开发人员中心。

以下部分介绍了获取实验变量和将事件记录到开发人员中心的一般过程。 有关演示如何创建和运行实验的端到端过程的演练，请参阅 [Create and run your first experiment with A/B testing（通过 A/B 测试来创建和运行你的第一个实验）](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 配置项目

若要开始，请在开发计算机上安装 Microsoft 官方商城协定和盈利 SDK，并将必要的参考添加到项目。

1. 安装 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk)。
2. 在 Visual Studio 中打开你的项目。
3. 在“解决方案资源管理器”中，展开项目节点、右键单击**“引用”**，然后选择**“添加引用”**。
3. 在**“引用管理器”**中，展开**“通用 Windows”**并单击**“扩展”**。
4. 在 SDK 列表中，选择 **Microsoft 官方商城协议 SDK** 旁边的复选框，并单击**“确定”**。

## 添加代码以获取变量设置

在你的项目中，找到你想要在实验中修改的功能代码。 添加检索变量设置的代码，并使用此数据修改正在测试的功能的行为。 你需要的具体代码将依应用而定，但在此处是一般步骤。 有关完整的代码示例，请参阅 [Create and run your first experiment with A/B testing（通过 A/B 测试创建和运行你的第一个实验）](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 声明用于检索实验变量的 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 对象和表示当前变量分配的 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 对象。
```CSharp
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. 初始化 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 对象，并将从仪表板**实验**页检索得来的 API 密钥传递到构造函数。 有关 API 密钥的详细信息，请参阅 [Define your experiment in the Dev Center dashboard（在开发人员中心仪表板中定义你的实验）](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key)。 以下所示的 API 密钥仅用作示例。
```CSharp
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. 通过调用 [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx) 方法，获取实验的当前缓存变量分配。 此方法将返回可通过 [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx) 属性访问变量分配的 [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx) 对象。
```CSharp
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. 检查 [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx) 属性，确定是否需要刷新缓存的变量分配。 如果它确实需要刷新，请调用 [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx) 方法检查服务器的更新的变量分配，并刷新本地缓存的变量。
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. 使用 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 对象的 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx)、[GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx)、[GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx) 或 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) 方法获取变量分配的设置。 在每种方法中，第一个参数即是你要检索的设置名称（正如你在开发人员仪表板中输入的名称一样）。 第二个参数是该方法无法检索开发人员中心的特定值（例如没有网络连接）并且缓存的变量版本不可用时所返回的默认值。

  以下示例使用 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) 获取名为 *buttonText* 的设置，并指定**灰色按钮**的默认设置值。
```CSharp
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. 在代码中，使用设置值来修改正在测试的功能的行为。 例如，以下代码将 *buttonText* 值分配到某个按钮的内容。
```CSharp
button.Content = buttonText;
```

## 添加代码以将视图和转换事件记录到开发人员中心

接下来，在开发人员中心中将记录视图和转换事件的代码添加到 A/B 测试服务。 你的代码应在用户开始查看实验的一部分变量时记录视图事件，并在用户达到实验目标时记录转换事件。

你需要的具体代码将依应用而定，但在此处是一般步骤。 有关完整的代码示例，请参阅 [Create and run your first experiment with A/B testing（通过 A/B 测试创建和运行你的第一个实验）](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在用户开始查看变量即会运行的代码中，调用 [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx) 对象的 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法。 传递在开发人员中心仪表板上的实验中定义的视图事件的名称和表示当前变量分配的 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 对象（此对象提供开发人员中心事件的上下文）。 以下示例记录名为 **userViewedButton** 的视图事件。
```CSharp
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. 在用户达到实验其中一个目标时运行的代码中，重新调用 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法，并传递你在实验中定义的转换事件的名称和 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 对象。 以下示例记录名为 **userClickedButton** 的转换事件。
```CSharp
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## 后续步骤

在开发人员中心仪表板定义实验并在你的应用中为实验编码后，你就已经准备好[在开发人员中心仪表板运行和管理实验](manage-your-experiment.md)。

## 相关主题

  * [在开发人员中心仪表板中定义你的实验](define-your-experiment-in-the-dev-center-dashboard.md)
  * [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
  * [通过 A/B 测试创建和运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


