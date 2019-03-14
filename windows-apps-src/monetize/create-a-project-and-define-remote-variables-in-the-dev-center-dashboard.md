---
Description: 可以使用一个对通用 Windows 平台 (UWP) 应用程序中运行试验之前 / B 测试，必须创建一个项目并在合作伙伴中心定义远程变量。
title: 在合作伙伴中心中创建实验项目
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: acfd654f02cb7fb727d35271175e59966e2abdc4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650792"
---
# <a name="create-an-experiment-project-in-partner-center"></a>在合作伙伴中心中创建实验项目

若要开始使用试验，创建试验[项目](run-app-experiments-with-a-b-testing.md#terms)合作伙伴中心中的应用，并定义你的应用可以访问的远程变量。

以下说明介绍了用于创建项目的核心步骤。 有关演示如何创建项目、然后运行实验的端到端过程的详细演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="instructions"></a>说明

1. 登录到[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **“你的应用”** 下，选择你想为之创建实验的应用。
3. 在导航窗格中，选择**服务**，然后选择**实验**。
4. 在**实验**页面上，单击**项目**部分中的**新建项目**按钮。 如果你已经创建了一个或多个项目，这些项目会在**项目**部分中列出。
5. 在**新建项目**页面中，输入新项目的名称。
6. 在**远程变量**部分中，添加你希望用于此项目中所有实验的[变量](run-app-experiments-with-a-b-testing.md#terms)，并为每个变量定义默认值。 此处指定的默认值将用于实验的控件组，也将用于没有参与此实验的任何用户。
  1. 如果**远程变量**部分已折叠，请单击本部分标题上的**显示**。
  2. 单击**添加变量**创建每个你希望用于此项目中任何实验的新变量，并键入变量的变量名称和默认值。
  3. 添加变量完成后，请单击**保存**。
3. 在 **SDK 集成**部分中，记下[项目 ID](run-app-experiments-with-a-b-testing.md#terms)值。 当您[代码应用程序进行试验](code-your-experiment-in-your-app.md)，必须在代码中引用此项目 ID，因此可以接收变体的数据和报告到合作伙伴中心视图和转换事件。

> [!NOTE]
> 在项目中的实验处于活动状态时，不可编辑、添加或删除远程变量。 此限制有助于保护活动实验的控件组的数据完整性。


## <a name="next-steps"></a>后续步骤

创建项目后，可以[为实验编写应用代码](code-your-experiment-in-your-app.md)以开始检索应用中的变量值，并且可以[在项目中创建实验](define-your-experiment-in-the-dev-center-dashboard.md)。

## <a name="related-topics"></a>相关主题

* [代码应用程序进行试验](code-your-experiment-in-your-app.md)
* [在合作伙伴中心定义试验](define-your-experiment-in-the-dev-center-dashboard.md)
* [管理在合作伙伴中心实验](manage-your-experiment.md)
* [创建并运行你的第一个试验使用 A / B 测试](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用一个运行应用试验 / B 测试](run-app-experiments-with-a-b-testing.md)
