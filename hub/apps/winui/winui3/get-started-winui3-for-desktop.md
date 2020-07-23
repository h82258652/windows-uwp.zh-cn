---
description: 本指南展示了如何开始使用 WinUI 3 UI 创建 .NET 和 C++/Win32 桌面应用。
title: 适用于桌面应用的 WinUI 3 入门
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 7393d4d1bae227bf3b586a54fba5d43ca2dcb53e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493402"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>适用于桌面应用的 WinUI 3 入门

WinUI 3 预览版 2 引入了新的项目模板，使你能够采用完全基于 WinUI 的用户界面创建托管桌面 C#/.NET Core 应用和原生 C++/Win32 桌面应用。 使用这些项目模板创建应用时，应用程序的整个用户界面都是使用 WinUI 3 提供的窗口、控件和其他 UI 类型实现的。 有关项目模板的完整列表，请参阅[本节](index.md#project-templates-for-winui-3)。

## <a name="prerequisites"></a>必备条件

若要如本文所述将 WinUI 3 用于桌面项目模板，请按照[此处](index.md#install-winui-3-preview-2)的说明配置开发计算机并安装 WinUI 3 预览版 2。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>创建适用于 C# 和 .NET 5 的 WinUI 3 桌面应用

1. 在 Visual Studio 2019 中，依次选择“文件” -> “新建” -> “项目”。  

2. 在“项目”下拉筛选器中，分别选择“C#”、“Windows”和“WinUI”。

3. 选择“打包的空白应用(桌面版 WinUI)”项目类型，然后单击“下一步”。

    ![空白应用项目模板](images/WinUI-csharp-newproject.png)

4. 输入项目名称，根据需要选择任何其他选项，然后单击“创建”。

5. 在下面的对话框中，将“目标版本”设置为 Windows 10 版本 1903（内部版本 18362），将“最低版本”设置为 Windows 10 版本 1803（内部版本 17134），然后单击“确定”。

    ![目标版本和最低版本](images/WinUI-min-target-version.png)

6. 此时，Visual Studio 生成两个项目：

    * **项目名称(桌面)** ：此项目包含应用的代码。 **App.xaml.cs** 代码文件定义一个表示你的应用实例的 `Application` 类，**MainWindow.xaml.cs** 代码文件定义一个表示应用所显示的主窗口的 `MainWindow` 类。 这些类派生自 WinUI 提供的 **Microsoft.UI.Xaml** 命名空间中的类型。

        ![应用项目](images/WinUI-csharp-appproject.png)

    * **项目名称(程序包)** ：这是一个 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，已配置该项目以将应用生成到 [MSIX 包](https://docs.microsoft.com/windows/msix/overview)中。 这提供了一种新式部署体验、通过包扩展与 Windows 10 功能集成的功能以及更多其他功能。 此项目包含应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，默认情况下它是你的解决方案的启动项目。

        ![应用项目](images/WinUI-csharp-packageproject.png)

7. 若要向应用项目中添加新项，请在**解决方案资源管理器**中右键单击“项目名称(桌面)”项目节点，然后选择“添加” -> “新项”。  在“添加新项”对话框中，选择“WinUI”选项卡，选择要添加的项，然后单击“添加”。 有关可用项的更多详细信息，请参阅[本节](index.md#item-templates-for-winui-3)。

    ![新项](images/WinUI-csharp-newitem.png)

8. 生成并运行解决方案，确认应用运行时不会出错。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>为 C++/Win32 创建 WinUI 3 桌面应用

1. 在 Visual Studio 2019 中，依次选择“文件” -> “新建” -> “项目”。  

2. 在“项目”下拉筛选器中，选择“C++”、“Windows”和“WinUI”。

3. 选择“打包的空白应用(桌面版 WinUI)”项目类型，然后单击“下一步”。

    ![空白应用项目模板](images/WinUI-cpp-newproject.png)

4. 输入项目名称，根据需要选择任何其他选项，然后单击“创建”。

5. 在下面的对话框中，将“目标版本”设置为 Windows 10 版本 1903（内部版本 18362），将“最低版本”设置为 Windows 10 版本 1803（内部版本 17134），然后单击“确定”。

    ![目标版本和最低版本](images/WinUI-min-target-version.png)

6. 此时，Visual Studio 生成两个项目：

    * **项目名称(桌面)** ：此项目包含应用的代码。 **App.xaml** 和各种**应用**代码文件定义一个表示应用实例的 `Application` 类，**MainWindow.xaml** 和各种 **MainWindow** 代码文件定义一个表示应用显示的主窗口的 `MainWindow` 类。 这些类派生自 WinUI 提供的 **Microsoft.UI.Xaml** 命名空间中的类型。

        ![应用项目](images/WinUI-cpp-appproject.png)

    * **项目名称(程序包)** ：这是一个 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，已配置该项目以将应用生成到 [MSIX 包](https://docs.microsoft.com/windows/msix/overview)中。 这提供了一种新式部署体验、通过包扩展与 Windows 10 功能集成的功能以及更多其他功能。 此项目包含应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，默认情况下它是你的解决方案的启动项目。

        ![程序包项目](images/WinUI-cpp-packageproject.png)

7. 若要向应用项目中添加新项，请在**解决方案资源管理器**中右键单击“项目名称(桌面)”项目节点，然后选择“添加” -> “新项”。  在“添加新项”对话框中，选择“WinUI”选项卡，选择要添加的项，然后单击“添加”。 有关可用项的更多详细信息，请参阅[本节](index.md#item-templates-for-winui-3)。

    ![新项](images/WinUI-cpp-newitem.png)

8. 生成并运行解决方案，确认应用运行时不会出错。

## <a name="known-issues-and-limitations"></a>已知问题和限制

有关已知问题和限制的列表，请参阅[本节](index.md#preview-2-limitations-and-known-issues)。

## <a name="related-topics"></a>相关主题

* [Windows UI 库 3](index.md)
