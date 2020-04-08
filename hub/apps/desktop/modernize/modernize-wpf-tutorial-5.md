---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 MSIX 包以及将其他新式组件合并到 WPF 应用中。
title: 使用 MSIX 打包和部署
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 27906d9d389c065ab1fdf7124151cd1915f850eb
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726010"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部分：使用 MSIX 打包和部署

本文是演示如何现代化名为 Contoso Expenses 的示例 WPF 桌面应用的教程的最后一个部分。 有关本教程的概述、先决条件以及有关下载示例应用的说明，请参阅[教程：实现 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假设你已学习完[第 4 部分](modernize-wpf-tutorial-4.md)。

在[第 4 部分](modernize-wpf-tutorial-4.md)中，你已了解某些 WinRT API（包括通知 API）需要程序包标识符才能在应用中使用。 可以通过使用 [MSIX](https://docs.microsoft.com/windows/msix)（在 Windows 10 中引入的打包格式）打包和部署 Windows 应用程序来获取程序包标识符。 MSIX 为开发人员和 IT 专业人员提供了以下优势：

- 优化的网络使用情况和存储空间。
- 完全干净的卸载，这得益于在其中执行了应用的轻型容器。 系统上没有残余的注册表项和临时文件。
- 将 OS 更新与应用程序更新和自定义脱钩。
- 简化了安装、更新和卸载过程。

在本教程的这一部分中，你将了解如何将 Contoso Expenses 应用打包到 MSIX 包中。

## <a name="package-the-application"></a>打包应用程序

Visual Studio 2019 提供了一种使用 Windows 应用程序打包项目打包桌面应用程序的简便方法。 

1. 在“解决方案资源管理器”中，右键单击“ContosoExpenses”解决方案，然后选择“添加”>“新建项目”    。

    ![添加新项目](images/wpf-modernize-tutorial/AddNewProject.png)

3. 在“添加新项目”  对话框中，搜索 `packaging`，在 C# 类别中选择“Windows 应用程序打包项目”  项目模板，然后单击“下一步”  。

    ![Windows 应用程序打包项目](images/wpf-modernize-tutorial/WAP.png)

4. 将新项目命名为 `ContosoExpenses.Package`，然后单击“创建”  。

5. 针对“目标版本”  和“最低版本”  选择“Windows 10 版本 1903(10.0; 版本 18362)”，然后单击“确定”   。

    ContosoExpenses.Package  项目随即被添加到 ContosoExpenses  解决方案中。 此项目包含一个[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，它描述了应用程序，以及一些用于项目的默认资产，例如“程序”菜单中的图标和“开始”屏幕中的磁贴。 但是，与 UWP 项目不同的是，打包项目不包含代码。 其目的是打包现有的桌面应用。

6. 在“ContosoExpenses.Package”项目中，右键单击“应用程序”节点，然后选择“添加引用”    。 此节点指定解决方案中的哪些应用程序将包含在包中。

6. 在项目列表中，选择“ContosoExpenses.Core”  并单击“确定”  。

7. 展开“应用程序”  节点，并确认已引用 ContosoExpense.Core  项目并以粗体突出显示。 这意味着它将用作包的起点。

8. 右键单击“ContosoExpenses.Package”项目，并选择“设为启动项目”   。

9. 按 F5  在调试器中启动打包应用。

此时，你会注意到一些更改，这些更改指示该应用现在已作为打包应用运行：

- 任务栏或“开始”菜单中的图标现在是每个 Windows 应用程序打包项目  中包含的默认资产。
- 如果右键单击“开始”菜单中列出的“ContosoExpense.Package”  应用程序，你会注意到通常为从 Microsoft Store 下载的应用保留的选项，如“应用设置”  、“打分并评价”  和“共享”  。

    ![“开始”菜单中的 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 如果要卸载该应用，可以在“开始”菜单中右键单击“ContosoExpense.Package”  ，然后选择“卸载”  。 此操作将立即删除该应用，并且不会在系统上留下任何残余文件。

## <a name="test-the-notification"></a>测试通知

现在，你已使用 MSIX 打包了 Contoso Expenses 应用，可以测试在[第 4 部分](modernize-wpf-tutorial-4.md)末尾无法使用的通知情况。

1. 在 Contoso Expenses 应用中，从列表中选择一名员工，然后单击“添加新的支出”按钮  。
2. 填写表单中的所有字段，然后按“保存”  。
3. 确认你看到了 OS 通知。

![Toast 通知](images/wpf-modernize-tutorial/ToastNotification.png)
