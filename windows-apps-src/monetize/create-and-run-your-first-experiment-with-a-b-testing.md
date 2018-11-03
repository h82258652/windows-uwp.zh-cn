---
author: Xansky
Description: In this walkthrough, you will create, run, and manage your first experiment with A/B testing.
title: 创建并运行你的第一个实验
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: e5a4c3607486a7163648c7aa5a0e1d03d37e421f
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5985008"
---
# <a name="create-and-run-your-first-experiment"></a>创建并运行你的第一个实验

在本操作实例中，你将：
* 在定义多个表示的文本和颜色的应用按钮的远程变量的合作伙伴中心中创建实验[项目](run-app-experiments-with-a-b-testing.md#terms)。
* 带有代码可检索远程变量值、 使用此数据来更改按钮的背景色并记录视图和转换事件数据返回到合作伙伴中心创建应用。
* 在项目中创建用于测试成功更改应用按钮的背景色是否会增加按钮单击数的实验。
* 运行该应用以收集实验数据。
* 查看合作伙伴中心中的实验结果、 选择变体以为所有用户的应用中，启用并完成实验。

大致 A / B 测试的合作伙伴中心，请参阅[运行应用实验，通过 A / B 测试](run-app-experiments-with-a-b-testing.md)。

## <a name="prerequisites"></a>先决条件

若要按照本演练中，必须具有合作伙伴中心帐户，并且中所述，你必须配置你的开发计算机[运行应用实验，通过 A / B 测试](run-app-experiments-with-a-b-testing.md)。

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>使用合作伙伴中心中的远程变量创建项目

1. 登录到[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 如果你已拥有你想要用于创建实验的合作伙伴中心中的应用，请在合作伙伴中心中选择该应用。 如果你尚未执行此合作伙伴中心中，[创建新的应用通过保留名称](../publish/create-your-app-by-reserving-a-name.md)中有一个应用，并且再在合作伙伴中心中选择该应用。
3. 在导航窗格中，单击**服务**，然后单击**实验**。
4. 在下一页的**项目**部分中，单击**新建项目**按钮。
5. 在**新建项目**页上，为你的新项目输入名称**按钮单击实验**。
6. 展开**远程变量**部分，然后单击**添加变量**四次。 现在应该有四个空变量行。
  * 在第一行中，键入 **buttonText** 作为变量名称，在**默认值**列中键入 **Grey Button**。
  * 在第二行中，键入 **r** 作为变量名称，在**默认值**列中键入 **128**。
  * 在第三行中，键入 **g** 作为变量名称，在**默认值**列中键入 **128**。
  * 在第四行中，键入 **b** 作为变量名称，在**默认值**列中键入 **128**。
7. 单击**保存**并记下在 **SDK 集成**部分中显示的[项目 ID](run-app-experiments-with-a-b-testing.md#terms) 值。 在下一部分中，你将更新你的应用代码，并在你的代码中引用此值。

## <a name="code-the-experiment-in-your-app"></a>在应用中为实验编码

1. 在 Visual Studio 中，创建新的通用 Windows 平台项目使用 Visual C#。 将该项目命名为 **SampleExperiment**。
2. 在“解决方案资源管理器”中，展开项目节点、右键单击 **“引用”**，然后选择 **“添加引用”**。
3. 在**引用管理器**中，展开**通用 Windows** 并单击**扩展**。
4. 在 SDK 列表中，选择 **Microsoft 协议框架**旁边的复选框，然后单击**确定**。
5. 在**解决方案资源管理器**中，双击 MainPage.xaml 以在应用中打开主页的设计器。
6. 将 **“按钮”** 从 **“工具箱”** 拖动到该页。
7. 在设计器上双击按钮以打开代码文件，并为 **Click** 事件添加事件处理程序。  
8. 将代码文件的所有内容替换为以下代码。 分配```projectId```变量为从上一部分中的合作伙伴中心获得的[项目 ID](run-app-experiments-with-a-b-testing.md#terms)值。
    [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. 保存代码文件并生成项目。

## <a name="create-the-experiment-in-partner-center"></a>在合作伙伴中心中创建实验

1. 返回到合作伙伴中心中的**按钮单击实验**项目页。
2. 在**实验**部分中，单击**新建实验**按钮。
3. 在**实验详细信息**部分中，在**实验名称**字段中键入名称**优化按钮单击**。
4. 在**视图事件**部分中，在**视图事件名称**字段中键入 **userViewedButton**。 请注意，此名称与在上一部分添加的代码中记录的视图事件字符串相匹配。
5. 在**目标和转换事件**部分中，输入以下值：
  * 在 **“目标名称”** 字段中，键入 **“提高按钮单击量”**。
  * 在**转换事件名称**字段中，键入名称 **userClickedButton**。 请注意，此名称与在上一部分添加的代码中记录的转换事件字符串相匹配。
  * 在**目标**字段中，选择**最大化**。
6. 在**远程变量和变体**部分中，确认已选中**平均分配**复选框，以便变体将平均分配到你的应用。
7. 将变量添加到你的实验：
    1. 单击下拉列表控件、选择 **buttonText**，然后单击**添加变量**。 字符串 **Grey Button** 应自动显示在**变体 A** 列中（此值派生自项目设置）。 在**变体 B** 列中，键入 **Blue Button**。
    2. 再次单击下拉列表控件、选择 **r**，然后单击**添加变量**。 字符串 **128** 应自动显示在**变体 A** 列中。 在**变体 B** 列中，键入**1**。
    3. 再次单击下拉列表控件、选择 **g**，然后单击**添加变量**。 字符串 **128** 应自动显示在**变体 A** 列中。 在**变体 B** 列中，键入 **1**。  
    4. 再次单击下拉列表控件、选择 **b**，然后单击**添加变量**。 字符串 **128** 应自动显示在**变体 A** 列中。 在**变体 B** 列中，键入 **255**。  
8. 单击**保存**，然后单击**激活**。

> [!IMPORTANT]
> 激活实验后，不可再对实验参数进行修改，除非在创建实验时，选中了**可编辑实验**复选框。 通常我们建议你在激活实验之前，在应用中为实验编码。

## <a name="run-the-app-to-gather-experiment-data"></a>运行该应用以收集实验数据

1. 运行你在之前创建的 **SampleExperiment** 应用。
2. 确认你看到的是灰色按钮还是蓝色按钮。 单击该按钮，然后关闭应用。
3. 在相同计算机上重复上述步骤几次，确认应用显示相同的按钮色。

## <a name="review-the-results-and-complete-the-experiment"></a>查看结果并完成实验

完成上一部分后至少等待几小时，然后遵循以下步骤查看实验结果并完成实验。

> [!NOTE]
> 只要激活某个实验时，合作伙伴中心会立即开始从进行检测以数据为你的实验记录的任何应用中收集数据。 但是，可能需要几个小时才实验数据，以显示在合作伙伴中心。

1. 在合作伙伴中心中，返回到你的应用的**实验**页面。
2. 在**激活实验**部分中，单击**优化按钮单击**转到此实验页面。
3. 确认在**结果摘要**和**结果详细信息**部分中显示的结果与你期望看到的结果相匹配。 有关这些部分的更多详细信息，请参阅[管理你在合作伙伴中心中的实验](manage-your-experiment.md#review-the-results-of-your-experiment)。
    > [!NOTE]
    > 合作伙伴中心仅报告第一个转换事件的每个用户在 24 小时时间段内。 如果用户在 24 小时时段内在应用中触发多个转换事件，仅报告第一个转换事件。 这是为了帮助防止触发很多转换事件的单个用户扭曲一组示例用户得出的实验结果。

4. 现在，你已准备好结束实验了。 在**结果摘要**部分的**变体 B** 列中，单击 **“切换”**。 这会将应用的所有用户切换到蓝色按钮。
5. 单击 **“确定”** 以确认你想要结束该实验。
6. 运行你在上一部分中创建的 **SampleExperiment** 应用。
7. 确认你看到了一个蓝色的按钮。 请注意，你的应用接收更新的变体分配最多需要两分钟。

## <a name="related-topics"></a>相关主题

* [创建项目并在合作伙伴中心中定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [为实验编写应用代码](code-your-experiment-in-your-app.md)
* [在合作伙伴中心中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [合作伙伴中心中管理实验](manage-your-experiment.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)
