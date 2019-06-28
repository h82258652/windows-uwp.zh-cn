---
description: 本教程演示如何添加 UWP XAML 用户界面、 创建 MSIX 包和其他新式组件合并到 WPF 应用。
title: 添加 UWP InkCanvas 控件使用 XAML 群岛
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420094"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>第 2 部分：添加 UWP InkCanvas 控件使用 XAML 群岛

这是本教程演示如何实现名为 Contoso 费用的示例 WPF 桌面应用的现代化的第二部分。 教程、 先决条件和说明下载示例应用程序的概述，请参阅[教程：使 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假定你已完成[第 1 部分](modernize-wpf-tutorial-1.md)。

在本教程的虚构方案，Contoso 开发团队希望将对数字签名的支持添加到 Contoso Expenses 应用程序。 UWP **InkCanvas**控件是很适合此方案中，选择，因为它支持数字墨迹和 AI 支持的功能，例如识别文本和形状的功能。 若要执行此操作，将使用[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包装 Windows 社区工具包中提供的 UWP 控件。 此控件包装的接口和功能的 UWP **InkCanvas** WPF 应用中使用的控件。 已包装的 UWP 控件有关的更多详细信息，请参阅[桌面应用程序 （XAML 群岛） 中的主机 UWP XAML 控件](xaml-islands.md)。

## <a name="configure-the-project-to-use-xaml-islands"></a>将项目配置为使用 XAML 群岛

可以添加之前**InkCanvas** Contoso Expenses 应用程序，首先需要配置项目以支持 UWP XAML 岛的控件。

1. 在 Visual Studio 2019，右键单击**ContosoExpenses.Core**项目中**解决方案资源管理器**，然后选择**管理 NuGet 包**。

    ![管理 Visual Studio 中的 NuGet 包菜单](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. 在中**NuGet 包管理器**窗口中，单击**浏览**。 选择**包括预发行版**选项，请搜索`Microsoft.Toolkit.Wpf.UI.Controls`包，并安装包结果中显示的最新预览版本。

    > [!NOTE]
    > 此包包含用于在 WPF 应用中，托管 UWP XAML 群岛所有必要的基础结构包括**InkCanvas**包装 UWP 控件。 类似的包命名为`Microsoft.Toolkit.Forms.UI.Controls`适用于 Windows 窗体应用程序。

3. 右键单击**ContosoExpenses.Core**项目中**解决方案资源管理器**，然后选择**添加-> 新项**。

4. 选择**应用程序清单文件**，其命名**app.manifest**，然后单击**添加**。

5. 在打开清单文件中，找到**兼容性**部分，并识别以下带注释的条目。

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. 此项下面添加以下项。

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. 取消注释**supportedOS**适用于 Windows 10 的条目。 此部分现在应如下所示。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > 此项指定了该应用程序需要 Windows 10，版本 1903年 （内部 18362） 或更高版本。 这是支持 XAML 岛的 Windows 10 的第一个版本。 没有此应用程序清单中的条目，则应用将引发运行时异常。

8. 在清单文件中，找到以下注释**应用程序**部分。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. 删除此部分并将其替换为以下 XML。 这会配置为 DPI 感知和更好的句柄的比例系数不同 Windows 10 支持的应用。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. 保存并关闭`app.manifest`文件。

12. 在中**解决方案资源管理器**，右键单击**ContosoExpenses.Core**项目，然后选择**属性**。

13. 在中**资源**一部分**应用程序**选项卡上，确保**清单**下拉列表设置为**app.manifest**。

    ![.NET core 应用程序清单](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. 将所做的更改保存到项目属性。

## <a name="add-an-inkcanvas-control-to-the-app"></a>InkCanvas 控件添加到应用

现在，已配置项目以使用 UWP XAML 群岛，你现已准备好添加[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包装到应用的 UWP 控件。

1. 在中**解决方案资源管理器**，展开**视图**文件夹**ContosoExpenses.Core**项目，然后双击**ExpenseDetail.xaml**文件。

2. 在中**窗口**XAML 文件的顶部附近的元素中添加以下属性。 这会引用的 XAML 命名空间[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包装 UWP 控件。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    添加此特性后**窗口**元素现在应如下所示。

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

4. 在中**ExpenseDetail.xaml**文件中，找到结束`</Grid>`标记后面紧跟`<!-- Chart -->`注释。 添加以下 XAML，只需在关闭前的`</Grid>`标记。 此 XAML 将添加**InkCanvas**控件 (前缀**工具包**关键字之前定义为一个命名空间) 和一个简单**TextBlock** ，用作控件的标头。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 保存**ExpenseDetail.xaml**文件。

6. 按 F5 在调试器中运行应用程序。

7. 从列表中选择某位员工，然后选择一个可用的费用。 请注意，费用详细信息页具有空间来容纳**InkCanvas**控件。

    ![墨迹画布笔输入](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    如果你有支持数字笔，如图面上，设备和物理计算机上运行此实验，请继续并尝试使用它。 你将看到显示在屏幕上的数字墨迹。 但是，如果您没有笔支持的设备，并且你尝试使用鼠标进行签名，不会发生。 这种情况发生，因为**InkCanvas**控件默认情况下启用仅为数字的笔。 但是，我们可以更改此行为。

8. 关闭应用程序，然后双击**ExpenseDetail.xaml.cs**文件下**视图**文件夹**ContosoExpenses.Core**项目。

9. 在类的顶部添加以下命名空间声明：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 找到`ExpenseDetail()`构造函数。

11. 添加以下行后的代码立即`InitializeComponent()`方法并保存代码文件。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    可以使用**InkPresenter**对象自定义默认的墨迹书写体验。 此代码使用**InputDeviceTypes**属性，以便启用鼠标以及笔输入。

12. 按 F5 再次重新生成并运行应用程序在调试器中。 从列表中选择某位员工，然后选择一个可用的费用。

13. 现在尝试使用鼠标的签名空间中绘制一些内容。 此时，您将看到显示在屏幕上的墨迹。

    ![签名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>后续步骤

此时在教程中，您已成功添加了 UWP **InkCanvas** Contoso Expenses 应用程序的控制。 你现已准备好[第 3 部分：添加 UWP 日历视图控件使用 XAML 群岛](modernize-wpf-tutorial-3.md)。
