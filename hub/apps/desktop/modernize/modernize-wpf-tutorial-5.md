---
description: 本教程演示如何添加 UWP XAML 用户界面、 创建 MSIX 包和其他新式组件合并到 WPF 应用。
title: 打包和部署与 MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420104"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部分：打包和部署与 MSIX

这是本教程演示如何实现名为 Contoso 费用的示例 WPF 桌面应用的现代化的最后一部分。 教程、 先决条件和说明下载示例应用程序的概述，请参阅[教程：使 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假定你已完成[第 4 部分](modernize-wpf-tutorial-4.md)。

在中[第 4 部分](modernize-wpf-tutorial-4.md)您学习了一些 WinRT Api，包括通知 API，需要的包标识，才可以在应用中使用。 可以通过打包 Contoso 费用使用获取包标识[MSIX](https://docs.microsoft.com/windows/msix)，打包和部署 Windows 应用程序的 Windows 10 中引入的打包格式。 MSIX 优点到表同时为开发人员和 IT 专业人员，包括：

- 优化网络使用情况和存储空间。
- 清理完成卸载，得益于其中执行应用程序的轻量容器。 没有注册表项和临时文件会保留在系统上。
- 将从应用程序更新和自定义项的操作系统更新中分离出来。
- 简化了安装、 更新和卸载过程。 

在本教程的此部分介绍如何打包 MSIX 包中的 Contoso 开支应用程序。

## <a name="package-the-application"></a>打包应用程序

Visual Studio 2019 提供了通过使用 Windows 应用程序打包项目打包桌面应用程序的简单方法。 

1. 在中**解决方案资源管理器**，右键单击**ContosoExpenses**解决方案，然后选择**添加-> 新建项目**。

    ![添加新项目](images/wpf-modernize-tutorial/AddNewProject.png)

3. 在中**添加新项目**对话框中，搜索`packaging`，选择**Windows 应用程序打包项目**中的项目模板C#类别，然后单击**下一步**.

    ![Windows 应用程序打包项目](images/wpf-modernize-tutorial/WAP.png)

4. 将新项目命名`ContosoExpenses.Package`然后单击**创建**。

5. 选择**Windows 10，版本 1903 (10.0;生成 18362）** 两个**目标版本**并**的最低版本**然后单击**确定**。

    **ContosoExpenses.Package**项目添加到**ContosoExpenses**解决方案。 此项目包含[包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，其中介绍了应用程序和程序菜单中的图标和开始屏幕中的磁贴等项使用某些默认资产。 但是，与 UWP 项目中，不同打包项目不包含代码。 其目的是将打包现有桌面应用程序。

6. 在中**ContosoExpenses.Package**项目，右键单击**应用程序**节点，然后选择**添加引用**。 此节点指定在解决方案中的应用程序将包括在包中。

7. 在项目列表中，选择**ContosoExpenses.Core**然后单击**确定**。

8. 展开**应用程序**节点，并确认**ContosoExpense.Core**项目引用和使用粗体突出显示。 这意味着，它将用作起始点的包。

9. 右键单击**ContosoExpenses.Package**项目，然后选择**设为启动项目**。

10. 按**F5**要打包的应用程序在调试器中启动。

此时，您可以注意到一些更改，指示应用正在作为运行打包：

- 任务栏中或在开始菜单中的图标现在是默认的资产中包含的每个**Windows 应用程序打包项目**。
- 如果您右键单击**ContosoExpense.Package**开始菜单中列出的应用程序，您会注意到选项通常为从 Microsoft Store 中，如下载的应用保留的**应用设置**，**速率和评审**并**共享**。

    ![在开始菜单中的 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 如果你想要卸载该应用，可以右键单击**ContosoExpense.Package**在开始菜单中选择**卸载**。 应用程序将立即删除，而无需离开任何昔日系统上。

## <a name="test-the-notification"></a>测试通知

既然已打包 MSIX 与 Contoso Expenses 应用程序，可以测试并没有发挥作用的末尾的通知方案[第 4 部分](modernize-wpf-tutorial-4.md)。

1. 在 Contoso Expenses 应用程序，从列表中选择某位员工，然后单击**添加新的支出**按钮。 
2. 完成窗体和按中的所有字段**保存**。
3. 确认你看到的操作系统通知。

![toast 通知](images/wpf-modernize-tutorial/ToastNotification.png)
