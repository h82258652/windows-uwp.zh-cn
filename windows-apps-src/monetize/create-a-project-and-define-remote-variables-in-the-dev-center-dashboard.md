---
author: mcleanbyron
Description: "必须先在开发人员中心仪表板中创建项目并定义远程变量，才能在具有 A/B 测试的通用 Windows 平台 (UWP) 应用中运行实验。"
title: "在开发人员中心仪表板中创建项目和定义远程变量"
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
translationtype: Human Translation
ms.sourcegitcommit: 32c1b379ee3913e267664e6d125fbc3daf480bb3
ms.openlocfilehash: 88a55c9ed64d5f52f959a1c68618dc5296dc24d6

---

# 在开发人员中心仪表板中创建项目和定义远程变量

若要开始实验，请在开发人员中心仪表板中为应用创建实验[项目](run-app-experiments-with-a-b-testing.md#terms)，并定义应用可访问的远程变量。

以下说明介绍了用于创建项目的核心步骤。 有关演示如何创建项目、然后运行实验的端到端过程的详细演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 说明

1. 登录到[开发人员中心仪表板](https://dev.windows.com/overview)。
2. 在**“你的应用”**下，选择你想为之创建实验的应用。
3. 在导航窗格中，选择“服务”，然后选择“实验”。
4. 在“实验”页面上，单击“项目”部分中的“新建项目”按钮。 如果你已经创建了一个或多个项目，这些项目会在“项目”部分中列出。
5. 在“新建项目”页面中，输入新项目的名称。
6. 在“远程变量”部分中，添加你希望用于此项目中所有实验的[变量](run-app-experiments-with-a-b-testing.md#terms)，并为每个变量定义默认值。 此处指定的默认值将用于实验的控件组，也将用于没有参与此实验的任何用户。
  1. 如果“远程变量”部分已折叠，请单击本部分标题上的“显示”。
  2. 单击“添加变量”创建每个你希望用于此项目中任何实验的新变量，并键入变量的变量名称和默认值。
  3. 添加变量完成后，请单击“保存”。
3. 在“SDK 集成”部分中，记下[项目 ID](run-app-experiments-with-a-b-testing.md#terms)值。 当你[为实验编写应用代码](code-your-experiment-in-your-app.md)时，必须在代码中引用此项目 ID，以便可以接收变体数据，并将查看和转换事件报告给开发人员中心。

>**注意**  在项目中的实验处于活动状态时，不可编辑、添加或删除远程变量。 此限制有助于保护活动实验的控件组的数据完整性。


## 后续步骤

创建项目后，可以[为实验编写应用代码](code-your-experiment-in-your-app.md)以开始检索应用中的变量值，并且可以[在项目中创建实验](define-your-experiment-in-the-dev-center-dashboard.md)。

## 相关主题

* [为实验编写应用代码](code-your-experiment-in-your-app.md)
* [在开发人员中心仪表板中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [在开发人员中心仪表板中管理你的实验](manage-your-experiment.md)
* [通过 A/B 测试创建和运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Nov16_HO1-->


