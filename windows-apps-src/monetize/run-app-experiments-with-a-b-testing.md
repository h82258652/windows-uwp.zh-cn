---
Description: 使用 Windows 开发人员中心仪表板通过 A/B 测试运行通用 Windows 平台 (UWP) 应用的实验。
title: 通过 A/B 测试运行应用实验
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
---

# 通过 A/B 测试运行应用实验

使用 Windows 开发人员中心仪表板通过 A/B 测试运行通用 Windows 平台 (UWP) 应用的实验。

在 A/B 测试中，通过开发人员中心在应用中试验程序变量赋值。 通过 A/B 测试程序变量生成应用逻辑，你可以为随机部分的用户受众启用应用的变体。 A/B 测试旨在识别变体，该变体很可能赢得你已改进的转换率（例如，更多应用内购买）。

在确定最适合你的业务目标的变体后，可以立即结束实验并在开发人员中心仪表板中为整个用户受众启用该变体，而无需重新发布你的应用。

## 创建并运行 A/B 测试

若要创建并运行 A/B 测试，请执行以下步骤：

1. [在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)。 每个实验包括：
  * 指示了用户开始查看属于实验的变体时的*视图事件*。
  * 指示了到达目标时*转换事件*的一个或多个目标。
  * 定义了实验使用的设置的一个或多个*变体*。
2. [针对实验为你的应用编码](code-your-experiment-in-your-app.md)。 在 Microsoft 官方商城协定和盈利 SDK 中使用 API 获取实验的变体设置、使用此数据来修改你正在测试的该功能的行为，并将视图事件和转换事件发送到开发人员中心。
3. [在开发人员中心仪表板中运行和管理实验](manage-your-experiment.md)。 使用仪表板查看实验结果，并完成实验。

有关演示端到端过程的演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 要求

Windows 开发人员中心中的 A/B 测试仅受 UWP 应用支持。

在可以通过 A/B 测试运行实验之前，必须设置你的开发计算机：

* 按照[此处](../get-started/get-set-up.md)的说明为 UWP 开发设置你的开发计算机。
* 安装 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk)。 除了实验的 API，此 SDK 还提供了其他功能（例如，可在应用上显示广告并引导你的客户到“反馈中心”收集反馈）的 API。 有关此 SDK 的详细信息，请参阅[通过官方商城协定和盈利 SDK 来获取应用收益](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)。

## 最佳做法

对于最有用的结果，我们建议在通过 A/B 测试运行实验时遵循以下建议：

* 考虑通过变体分配的随机对半拆分分配的两个变体运行实验。
* 运行实验至少需要 2 – 4 周才能收集统计上重要且可操作的有效数据。

## 相关主题

* [在开发人员中心仪表板中定义你的实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [针对实验为你的应用编码](code-your-experiment-in-your-app.md)
* [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
* [通过 A/B 测试创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


