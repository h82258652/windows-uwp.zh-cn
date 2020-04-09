---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 MSIX 包以及将其他新式组件合并到 WPF 应用中。
title: 使用 XAML Islands 添加 UWP InkCanvas 控件
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6bb90fb9cbe7c9f54f60fd1920f0e73e174a3772
ms.sourcegitcommit: df0cd9c82d1c0c17ccde424e3c4a6ff680c31a35
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80482581"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>第 2 部分：使用 XAML Islands 添加 UWP InkCanvas 控件

本文是演示如何现代化名为 Contoso Expenses 的示例 WPF 桌面应用的教程的第 2 部分。 有关本教程的概述、先决条件以及有关下载示例应用的说明，请参阅[教程：实现 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假设你已学习完[第 1 部分](modernize-wpf-tutorial-1.md)。

在本教程的虚构方案中，Contoso 开发团队需要支持对 Contoso Expenses 应用进行数字签名。 对于这种情况，UWP InkCanvas  控件是一个不错的选择，因为它支持数字墨迹和 AI 功能，如识别文本和形状的功能。 为此，你将使用 Windows 社区工具包中提供的 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 包装的 UWP 控件。 此控件包装 UWP InkCanvas  控件的接口和功能，以便在 WPF 应用中使用。 有关包装的 UWP 控件的详细信息，请参阅[在桌面应用中托管 UWP XAML 控件（XAML 岛）](xaml-islands.md)。

## <a name="configure-the-project-to-use-xaml-islands"></a>将项目配置为使用 XAML 岛

若要将 InkCanvas  控件添加到 Contoso Expenses 应用，首先需要将项目配置为支持 UWP XAML 岛。

1. 在 Visual Studio 2019 中，在“解决方案资源管理器”中右键单击“ContosoExpenses.Core”项目，并选择“管理 NuGet 包”    。

    ![Visual Studio 中的“管理 NuGet 包”菜单](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. 在“NuGet 包管理器”窗口中，单击“浏览”   。 搜索 `Microsoft.Toolkit.Wpf.UI.Controls` 包，并安装版本 6.0.0 或更高版本。

    > [!NOTE]
    > 此包包含在 WPF 应用中托管 UWP XAML 岛所需的所有基础结构，包括 InkCanvas  包装的 UWP 控件。 名为 `Microsoft.Toolkit.Forms.UI.Controls` 的类似包可用于 Windows 窗体应用。

3. 在“解决方案资源管理器”中，右键单击“ContosoExpenses.Core”项目并选择“添加”->“新项”    。

4. 选择“应用程序清单文件”  ，将其命名为“app.manifest”  ，然后单击“添加”  。 有关应用程序清单的详细信息，请参阅[本文](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)。

5. 在清单文件中，取消注释适用于 Windows 10 的以下 `<supportedOS>` 元素。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. 在清单文件中，找到以下已注释的 `<application>` 元素。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. 删除此部分，并将其替换为以下 XML。 这会将应用配置为能识别 DPI，并更好地处理 Windows 10 支持的不同缩放比例。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. 保存并关闭 `app.manifest` 文件。

9. 在“解决方案资源管理器”中，右键单击“ContosoExpenses.Core”项目并选择“属性”    。

10. 在“应用程序”  选项卡的“资源”  部分中，确保将“清单”  下拉列表设置为“app.manifest”  。

    ![.NET Core 应用部件清单 (manifest)](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. 保存对项目属性的更改。

## <a name="add-an-inkcanvas-control-to-the-app"></a>向应用添加 InkCanvas 控件

现已将项目配置为使用 UWP XAML 岛，接下来可以将 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 包装的 UWP 控件添加到应用了。

1. 在解决方案资源管理器中，展开 ContosoExpenses.Core 项目的“Views”文件夹，然后双击“ExpenseDetail.xaml”文件     。

2. 在 XAML 文件顶部附近的 **Window** 元素中，添加以下属性。 这会引用 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 包装的 UWP 控件的 XAML 命名空间。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    添加此属性后，Window 元素现在应如下所示  。

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. 在 ExpenseDetail.xaml  文件中，找到紧邻 `<!-- Chart -->` 注释之前的结束 `</Grid>` 标记。 在结束 `</Grid>` 标记前面添加以下 XAML。 此 XAML 将添加一个 InkCanvas  控件（以之前定义为命名空间的 toolkit  关键字作为前缀）以及一个充当控件标头的简单 TextBlock  。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 保存 ExpenseDetail.xaml  文件。

6. 按 F5 在调试程序中生成应用。

7. 从列表中选择一名员工，然后从可用支出中选择一项。 请注意，“支出详细信息”页包含 InkCanvas  控件的区域。

    ![仅限使用墨迹画布笔](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    如果你的设备支持数字笔（如 Surface），并且你正在物理计算机上运行此实验室，请继续尝试使用它。 屏幕上会显示数字墨迹。 但是，如果没有支持笔的设备，并且尝试使用鼠标进行签名，则不会有任何活动。 发生这种情况的原因是，默认情况下，仅为数字笔启用 InkCanvas  控件。 不过，可以更改此行为。

8. 关闭应用，双击 ContosoExpenses.Core 项目的“Views”文件夹下的“ExpenseDetail.xaml.cs”文件    。

9. 将以下命名空间声明添加到类的顶部：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 找到 `ExpenseDetail()` 构造函数。

11. 在 `InitializeComponent()` 方法后添加以下代码行，并保存代码文件。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    可以使用 InkPresenter  对象自定义默认墨迹书写体验。 此代码使用 InputDeviceTypes  属性启用鼠标和笔输入。

12. 再次按 F5，在调试程序中生成并运行应用。 从列表中选择一名员工，然后从可用支出中选择一项。

13. 现在，请尝试用鼠标在签名区域中绘制一些内容。 这一次，屏幕上会显示墨迹。

    ![签名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>后续步骤

在本教程的此阶段，你已成功向 Contoso Expenses 应用添加 UWP InkCanvas  控件。 现在你已准备好学习[第 3 部分：使用 XAML 岛添加 UWP CalendarView 控件](modernize-wpf-tutorial-3.md)。
