---
author: mcleanbyron
Description: "若要在具有 A/B 测试的通用 Windows 平台 (UWP) 应用中运行实验，必须在你的应用中为实验编码。"
title: "针对实验为你的应用编码"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# 针对实验为你的应用编码

[在开发人员中心仪表板中创建项目并定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)后，你可以随时在通用 Windows 平台 (UWP) 应用中更新代码，以便：
* 从 Windows 开发人员中心接收远程变量值。
* 使用远程变量为你的用户配置应用体验。
* 将指示用户查看实验并执行所需操作时（也称为*转换*）的事件记录到开发人员中心。

以下部分介绍了获取实验变体和将事件记录到开发人员中心的一般过程。 针对实验为你的应用编码后，你可以[在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)。 有关演示如何创建并运行实验的端到端过程的演练，请参阅[通过 A/B 测试创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 配置项目

若要开始，请在开发计算机上安装 Microsoft Store Services SDK，并将必要的参考添加到项目。

1. 安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。
2. 在 Visual Studio 中打开你的项目。
3. 在“解决方案资源管理器”中，展开项目节点、右键单击**“引用”**，然后选择**“添加引用”**。
3. 在“引用管理器”****中，展开“通用 Windows”****并单击“扩展”****。
4. 在 SDK 列表中，选择“Microsoft 协议框架”****旁边的复选框，然后单击“确定”****。

## 添加代码以获取变体数据

在你的项目中，找到你想要在实验中修改的功能代码。 添加检索变体数据的代码，并使用此数据修改正在测试的功能的行为。 你需要的具体代码将依应用而定，但在此处是一般步骤。 有关完整的代码示例，请参阅[通过 A/B 测试创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 声明可表示当前变体分配的 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 对象和用于将视图和转换事件记录到开发人员中心的 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 对象。
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. 通过调用静态 [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) 方法为你的实验获取当前缓存的变体分配，并将你的实验的[项目 ID](run-app-experiments-with-a-b-testing.md#terms) 传递给该方法。 此方法将返回可通过 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx) 属性访问变体分配的 [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) 对象。
  >**注意**&nbsp;&nbsp;当[在开发人员中心仪表板中创建一个项目](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)时获取项目 ID。 以下所示的项目 ID 仅用作示例。

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. 检查 [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) 属性，确定是否需要通过服务器的远程变体分配刷新缓存的变体分配。 如果它确实需要刷新，请调用静态 [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) 方法检查服务器的更新的变体分配，并刷新本地缓存的变体。
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
{
      result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

      // If the call succeeds, use the new result. Otherwise, use the cached value.
      if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
      {
          variation = result.ExperimentVariation;
      }
}
```

5. 使用 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 对象的 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx)、[GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx)、[GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) 或 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) 方法获取变体分配的值。 在每种方法中，第一个参数即是你要检索的变体名称（这与你在开发人员中心仪表板中输入的变体名称相同）。 第二个参数是该方法无法检索开发人员中心的特定值（例如没有网络连接）并且缓存的变体版本不可用时所返回的默认值。

  以下示例使用 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) 获取名为 *buttonText* 的变量，并指定 **Grey Button** 的默认变量值。
```CSharp
var buttonText = variation.GetString("buttonText", "Grey Button");
```
4. 在代码中，使用变量值修改正在测试的功能的行为。 例如，以下代码将 *buttonText* 值分配到某个按钮的内容。
```CSharp
button.Content = buttonText;
```

## 添加代码以将视图和转换事件记录到开发人员中心

接下来，在开发人员中心中将记录[视图事件](run-app-experiments-with-a-b-testing.md#terms)和[转换事件](run-app-experiments-with-a-b-testing.md#terms)的代码添加到 A/B 测试服务。 你的代码应在用户开始查看实验的一部分变体时记录视图事件，并在用户达到实验目标时记录转换事件。

你需要的具体代码将依应用而定，但在此处是一般步骤。 有关完整的代码示例，请参阅[通过 A/B 测试创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在用户开始查看变体时运行的代码中，将 ```logger``` 字段初始化为 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 对象并调用 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法。 传递表示当前变体分配的 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 对象（此对象提供开发人员中心事件的上下文）和视图事件的名称（这与你在开发人员中心仪表板中输入的视图事件的名称相同）。 以下示例记录名为 **userViewedButton** 的视图事件。

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. 在用户达到实验其中一个目标时运行的代码中，重新调用 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法，并传递 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 对象和转换事件的名称（这与你在开发人员中心仪表板中输入的转换事件的名称相同）。 以下示例记录名为 **userClickedButton** 的转换事件。
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## 后续步骤

在你的应用中为实验编码后，你可以随时执行以下步骤：
1. [在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)。 创建可为 A/B 测试定义视图事件、转换事件和唯一变体的实验。
2. [在开发人员中心仪表板中运行和管理实验](manage-your-experiment.md)。


## 相关主题

* [在开发人员中心仪表板中创建项目和定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
* [通过 A/B 测试创建和运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


