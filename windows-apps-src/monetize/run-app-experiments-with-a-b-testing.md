---
author: Xansky
Description: You can use the Windows Dev Center dashboard to run experiments for your Universal Windows Platform (UWP) apps with A/B testing.
title: 通过 A/B 测试运行应用实验
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: 90ff6ba9a3767570aa9e885d491b38053a1d5d52
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5442266"
---
# <a name="run-app-experiments-with-ab-testing"></a>通过 A/B 测试运行应用实验

可以使用 Windows 开发人员中心仪表板定义可在运行时从通用 Windows 平台 (UWP) 应用检索的远程变量，还可以测试这些值的变量以供用户标识用于驱动所需用户行为的最有效的值。 应用可以使用远程变量配置应用体验，例如应用内购买、注册流程、描述文字和广告发布。

A/B 测试的目标应该是标识远程变量值的变体，该变体可能会通过提供更具吸引力的应用体验提高转换率（例如更多应用内购买）。 在标识成功变体后，可以立即结束实验并在开发人员中心仪表板中为整个用户受众启用该变体，而无需重新发布你的应用。

## <a name="create-and-run-an-ab-test"></a>创建并运行 A/B 测试

若要创建并运行 A/B 测试，请执行以下步骤：

1. [在开发人员中心仪表板中创建项目和定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)。 此项目包含你的实验的变量和默认变量值。  
2. [针对实验为你的应用编码](code-your-experiment-in-your-app.md)。 在 Microsoft Store Services SDK 中使用 API 获取你在仪表板中创建的项目的变量值、使用此数据修改你正在测试的该功能的行为，并将视图事件和转换事件发送到开发人员中心。
3. [在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)。 在你的项目中创建一个可定义你的 A/B 测试的唯一目标和变体的测试。
4. [在开发人员中心仪表板中运行和管理实验](manage-your-experiment.md)。 激活实验、使用仪表板查看实验结果，并完成实验。

有关演示端到端过程的演练，请参阅[通过 A/B 测试创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="requirements"></a>要求

Windows 开发人员中心中的 A/B 测试仅受 UWP 应用支持。

在可以通过 A/B 测试运行实验之前，必须设置你的开发计算机：

* 按照[此处](../get-started/get-set-up.md)的说明为 UWP 开发设置你的开发计算机。
* [安装 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了实验的 API，此 SDK 还提供了其他功能（例如，可在应用上显示广告并引导你的客户到“反馈中心”收集反馈）的 API。

## <a name="best-practices"></a>最佳做法

对于最有用的结果，我们建议在通过 A/B 测试运行实验时遵循以下建议：

* 考虑通过变体分配的随机对半拆分分配的两个变体运行实验。
* 运行实验至少需要 2 – 4 周才能收集统计上重要且可操作的有效数据。

<span id="terms" />

## <a name="related-terms"></a>相关术语

|  术语  |  定义  |
|--------|--------------|
| 项目    |   你的应用可使用 Microsoft Store Services SDK 访问的远程变量及默认值的集合。 项目也可以选择包含一个或多个可共享相同远程变量的实验。  |
| 实验    |   可定义用户将接收的 A/B 测试的一组参数。 实验在项目的作用域中定义，并且每个实验包含以下内容： <p></p><ul><li>指示了用户开始查看属于实验的变体时的*视图事件*。</li><li>指示了到达目标时*转换事件*的一个或多个目标。</li><li>定义了实验使用的变量数据的一个或多个*变体*。 *控件*变体使用在实验的项目中定义的默认变量值。 除了控件变体，实验通常具有至少一个其他变体（含特定于该实验的默认值）。 </li></ul>          |
| 项目 ID    |   将你的应用与开发人员中心帐户中的项目关联起来的唯一 ID。 必须使用此 ID 与你的应用中的 A/B 测试服务连接在一起，才能接收变体数据并将视图和转换事件报告给开发人员中心。 有关详细信息，请参阅[针对实验为应用编码](code-your-experiment-in-your-app.md)。<p></p><p>每个项目和项目中的所有实验都只关联一个项目 ID。 你可以使用项目 ID 帮助区分不同的实验集。 例如，你可能有一组发布到组织测试人员的实验，还有另外一组仅发布到应用的外部用户的实验。  如果应用实现多个实验，它可以引用多个项目 ID。</p>         |
| 变体    |   你在实验中测试的一个或多个变量的集合。 每项实验必须拥有至少一个变量和两个变体（包括控件）。 实验可以有最多五个变体。           |
| 变量    |  你的应用用于初始化某个属性的值或应用中的另外一个值。 在实验中，变量值随变体更改而更改。 结束实验后，变量会分配由你选择发布到应用的所有用户的变体值。 变量可拥有以下类型：字符串、布尔值、双精度和整数。
| 视图事件    |  表示用户开始查看作为实验一部分的变体时，某项活动的任意字符串。 通常，这是你的代码中的某个事件的名称。 当用户开始查看变体时，你的应用代码会将此视图事件字符串发送到开发人员中心。 有关详细信息，请参阅[针对实验为应用编码](code-your-experiment-in-your-app.md)。
| 转换事件    |  表示实验目标的目的的任意字符串。 通常，这是你的代码中的某个事件的名称。 当用户达成此目的时，你的应用代码会将此转换事件字符串发送到开发人员中心。 有关详细信息，请参阅[针对实验为应用编码](code-your-experiment-in-your-app.md)。  

## <a name="related-topics"></a>相关主题

* [在开发人员中心仪表板中创建项目和定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [针对实验为应用编码](code-your-experiment-in-your-app.md)
* [在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
* [通过 A/B 测试创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
