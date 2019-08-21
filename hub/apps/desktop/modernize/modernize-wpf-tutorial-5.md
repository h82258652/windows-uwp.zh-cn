---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 .MSIX 包以及将其他新式组件合并到 WPF 应用。
title: 使用 MSIX 打包和部署
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 940a81d21e071558d510e565785d1f52ca0bb1a3
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643382"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部分：使用 MSIX 打包和部署

这是本教程的最后一部分, 该教程演示如何将名为 Contoso 支出的示例 WPF 桌面应用实现现代化。 有关下载示例应用的教程、先决条件和说明的概述, 请参阅[教程:现代化 WPF 应用](modernize-wpf-tutorial.md)。 本文假设你已完成[第4部分](modernize-wpf-tutorial-4.md)。

在[第4部分](modernize-wpf-tutorial-4.md)中, 您已了解到某些 WinRT api (包括通知 API) 需要包标识, 才能在应用程序中使用它们。 可以通过使用[.msix](https://docs.microsoft.com/windows/msix)(Windows 10 中引入的打包格式) 打包和部署 windows 应用程序, 来获取包标识。 .MSIX 为开发人员和 IT 专业人员提供了优势, 包括:

- 优化网络使用情况和存储空间。
- 完成全新卸载, 因为在其中执行了应用的轻型容器。 系统上没有注册表项和临时文件。
- 将 OS 更新与应用程序更新和自定义分离。
- 简化了安装、更新和卸载过程。

在本教程的此部分中, 你将了解如何将 Contoso 支出应用打包到 .MSIX 包中。

## <a name="package-the-application"></a>打包应用程序

Visual Studio 2019 通过使用 Windows 应用程序打包项目提供一种简单的方法来打包桌面应用程序。 

1. 在**解决方案资源管理器**中, 右键单击**ContosoExpenses**解决方案, 然后选择 "**添加->" "新建项目**"。

    ![添加新项目](images/wpf-modernize-tutorial/AddNewProject.png)

3. 在 "**添加新项目**" 对话框中, 搜索`packaging`, 在 " C#类别" 中选择 " **Windows 应用程序打包项目**" 项目模板, 然后单击 "**下一步**"。

    ![Windows 应用程序打包项目](images/wpf-modernize-tutorial/WAP.png)

4. 为新项目`ContosoExpenses.Package`命名, 然后单击 "**创建**"。

5. 选择**Windows 10, 版本 1903 (10.0;** 对于**目标版本**和**最小版本**均为 18362), 然后单击 **"确定"** 。

    **ContosoExpenses**项目添加到**ContosoExpenses**解决方案中。 此项目包含一个[包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), 其中描述了应用程序以及一些用于项目的默认资产, 如 "程序" 菜单中的图标和 "开始" 屏幕中的 "磁贴"。 但是, 与 UWP 项目不同的是, 打包项目不包含代码。 其目的是打包现有的桌面应用程序。

6. 在**ContosoExpenses**项目中, 右键单击 "**应用程序**" 节点, 然后选择 "**添加引用**"。 此节点指定解决方案中将包含哪些应用程序。

6. 在项目列表中, 选择 " **ContosoExpenses** ", 然后单击 **"确定"** 。

7. 展开 "**应用程序**" 节点, 确认**ContosoExpense**项目已引用并突出显示为粗体。 这意味着它将用作包的起点。

8. 右键单击 " **ContosoExpenses** " 项目, 然后选择 "**设为启动项目**"。

9. 在解决方案资源管理器中, 右键单击**ContosoExpenses**项目节点, 然后选择 "**编辑项目文件**"。

10. 在文件中找到 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。

11. 将此元素替换为以下 XML。

    ``` xml
    <ItemGroup>
        <SDKReference Include="Microsoft.VCLibs,Version=14.0">
        <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
        <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
        <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
        <Implicit>true</Implicit>
        </SDKReference>
    </ItemGroup>
    <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
    <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
        <ItemGroup>
        <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
        <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
        <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
            <SourceProject>
            </SourceProject>
        </_FilteredNonWapProjProjectOutput>
        </ItemGroup>
    </Target>
    ```

12. 保存并关闭项目文件。

13. 按**F5**在调试器中启动打包应用。

此时, 你可以注意到一些更改现在以打包方式运行应用:

- 任务栏或 "开始" 菜单中的图标现在是每个**Windows 应用程序打包项目**中包含的默认资产。
- 如果右键单击 "开始" 菜单中列出的 " **ContosoExpense** " 应用程序, 你会注意到通常为从 Microsoft Store 下载的应用保留的选项, 例如**应用设置**、**速率和查看**和**共享**.

    !["开始" 菜单中的 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 如果要卸载该应用, 可以在 "开始" 菜单中右键单击 " **ContosoExpense** ", 然后选择 "**卸载**"。 将立即删除该应用, 而不会在系统上留下任何剩余的。

## <a name="test-the-notification"></a>测试通知

现在, 你已使用 .MSIX 打包了 Contoso 支出应用, 你可以测试在[第4部分](modernize-wpf-tutorial-4.md)结束时未使用的通知方案。

1. 在 Contoso 支出应用程序中, 从列表中选择员工, 然后单击 "**添加新支出**" 按钮。 
2. 填写表单中的所有字段, 并按 "**保存**"。
3. 确认你看到了 OS 通知。

![Toast 通知](images/wpf-modernize-tutorial/ToastNotification.png)
