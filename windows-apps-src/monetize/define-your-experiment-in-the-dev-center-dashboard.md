---
author: mcleanbyron
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must define your experiment in the Dev Center dashboard.
title: 在仪表板中定义实验
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: f690aaede800f76b2eb006355e982ea6b3509b78
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2837369"
---
# <a name="define-your-experiment-in-the-dashboard"></a>在仪表板中定义实验


            [在开发人员中心仪表板中创建项目并定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)以及[为实验编写应用代码](code-your-experiment-in-your-app.md)后，你可以随时在项目中创建实验。 在创建实验时，要定义目标以及用户将收到的变体。

有关演示如何创建并运行实验的端到端过程的演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>创建实验

1. 登录到[开发人员中心仪表板](https://dev.windows.com/overview)。
2. 在 **“你的应用”** 下，选择你想为之创建实验的应用。
3. 在导航窗格中，选择**服务**，然后选择**实验**。
4. 在**实验**页面上，标识项目表中要在其中添加实验的项目，然后单击该项目的**添加实验**链接。
5. 在**实验名称**字段中，键入你可以用来轻松标识实验的名称。 创建实验后，此名称将会同时显示在应用的**实验**页面和项目的页面上的现有实验列表中。
6. 如果想要在试验处于活动状态时对它进行编辑，请单击**可编辑实验**复选框。 仅当要创建实验通过内部测试验证所有变体时才选中此框。 有关详细信息，请参阅[创建用于内部测试的实验](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments)。
    > [!NOTE]
    > 如果你要创建发布给客户的实验（即，与提供给客户的应用版本中使用的项目 ID 相关联的实验），则请勿选中此框。 编辑处于活动状态的实验会导致实验结果失效。

7. 在**项目名称**下拉列表中，会自动选中当前项目。 如果想要将新实验添加到其他项目，可以从此处选择目标项目。 否则，请保留该选择。
8.   记下[项目 ID](run-app-experiments-with-a-b-testing.md#terms) 值。 当你[为实验编写应用代码](code-your-experiment-in-your-app.md)时，必须在代码中引用此 ID，以便可以接收变体数据，并将查看和转换事件报告给开发人员中心。
9. 在**视图事件**部分的**视图事件名称**字段中，键入实验的[视图事件](run-app-experiments-with-a-b-testing.md#terms)名称。
10. 在**目标和转换事件**部分中，为你的实验定义至少一个目标：
  * 在**目标名称**字段中，为目标键入描述性的名称。 运行实验后，此名称将显示在实验的结果摘要中。
  * 在**转换事件名称**字段中，为此目标键入[转换事件](run-app-experiments-with-a-b-testing.md#terms)的名称。
  * 在**目标**字段中，选择**最大化**或**最小化**，具体取决于你是想要最大化还是最小化转换事件的发生次数。 此信息将用于实验的结果摘要。

> [!NOTE]
> 开发人员中心中在 24 小时时间段内向每个用户视图仅报告第一个转换事件。 如果用户在 24 小时时段内在应用中触发多个转换事件，仅报告第一个转换事件。 这是为了在目标是最大化执行转换的用户数时，帮助防止单个用户扭曲一组示例用户得出的实验结果。

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>定义实验的远程变量和变体

接下来，定义实验的远程[变量](run-app-experiments-with-a-b-testing.md#terms)和[变体](run-app-experiments-with-a-b-testing.md#terms)。

1. 在**远程变量和变体**部分，应该可以看到**变体 A (控件)** 和**变体 B** 这两个默认变体。如果你想要更多变体，请单击**添加变体**。 或者，你可以重命名每个变体。
2. 默认情况下，变体会平均分配到应用用户。 如果你想要选择特定的分配百分比，清除**平均分配**复选框，并在**分配 (%)** 行中键入百分比。
3. 将远程变量添加到变体。 在该部分底部的下拉列表控件中，选择要添加的每个变量，然后单击**添加变量**。
    > [!NOTE]
    > 该控件中列出的变量继承自实验的项目。 变量（在项目中定义）的默认值会自动分配给控件变体。 如果想要创建此处未列出的新变量，请转到相关项目页面，然后在该页面中添加变量。

4. 编辑实验中每个唯一变体（即不同于控件变体的变体）的变量值。

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>保存并激活实验

当你完成输入实验的所需字段后，单击**保存**以保存实验。

如果你对实验参数感到满意，并且已准备好激活它以开始收集应用的实验数据，请单击**激活**。 实验已激活时，应用便可以检索变体变量，并向开发人员中心报告视图和转换事件。 有关详细信息，请参阅[在开发人员中心仪表板中运行和管理实验](manage-your-experiment.md)。

> [!IMPORTANT]
> 项目一次只可以包含一个活动实验。 激活实验后，不可再对实验参数进行修改，除非在创建实验时，选中了**可编辑实验**复选框。 在激活实验之前，我们建议你在应用中为实验编码。

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>创建用于内部测试的实验

你可能希望经由受控受众（例如，一组内部测试人员）测试你的实验，并在针对客户激活实验之前，确认所有变体都按预期工作。 通过创建选中了**可编辑实验**选项的实验，就可以达成上述目的。

若要在将实验发布给客户之前对实验进行测试，请遵循以下过程：

1. 创建两个项目：一个用于应用的公共版本，另一个用于应用的内部版本（仅提供给你的测试受众）。 以下说明分别适用于公共项目和测试项目。
2. 当你[为实验编写应用代码](code-your-experiment-in-your-app.md)时，请在应用的公共版本中引用来自你的公共项目中的项目 ID。 在应用的内部版本中，请引用来自你的测试项目中的项目 ID。
3. 在测试项目中创建实验，然后为该实验选中**可编辑实验**选项。
4. 激活测试项目中的实验。 将 100% 分发分配给一个变体，并验证对于测试人员而言该变体是否按预期工作。 针对其他变体重复该过程。
5. 变体经验证按预期工作后，对测试项目中的实验进行最后的更改。 当可以向客户发布实验时，请将该实验克隆到公共项目。 在该实验中，请勿选中**可编辑实验**选项。
4. 确保目标变体分发在克隆实验中正确无误。
5. 激活克隆实验，向客户发布该实验。

## <a name="next-steps"></a>后续步骤

在开发人员中心仪表板定义实验并在你的应用中为实验编码后，你就已经准备好[在开发人员中心仪表板运行和管理实验](manage-your-experiment.md)。

## <a name="related-topics"></a>相关主题

* [在开发人员中心仪表板中创建项目和定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [针对实验为你的应用编码](code-your-experiment-in-your-app.md)
* [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
* [通过 A/B 测试创建和运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)
