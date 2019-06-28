---
description: 本教程演示如何添加 UWP XAML 用户界面、 创建 MSIX 包和其他新式组件合并到 WPF 应用。
title: 添加 UWP 日历视图控件使用 XAML 群岛
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420107"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部分：添加 UWP 日历视图控件使用 XAML 群岛

这是本教程演示如何实现名为 Contoso 费用的示例 WPF 桌面应用的现代化的第三个部分。 教程、 先决条件和说明下载示例应用程序的概述，请参阅[教程：使 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假定你已完成[第 2 部分](modernize-wpf-tutorial-2.md)。

在本教程的虚构方案，Contoso 开发团队希望使其更轻松地选择启用触摸的设备上的费用报表的日期。 在本教程的此部分中，您将添加 UWP[日历视图](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)到应用的控件。 这是在任务栏上的 Windows 10 的日期和时间功能中使用的同一个控件。

![CalendarViewControl 图像](images/wpf-modernize-tutorial/CalendarViewControl.png)

与不同**InkCanvas**控制你在中添加[第 2 部分](modernize-wpf-tutorial-2.md)，Windows 社区工具包不会提供已包装的 UWP 版本**日历视图**，可以使用 WPF 应用程序中. 作为替代方法，将承载**InkCanvas**在泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件。 可以使用此控件以承载平台或第三方供应商提供的任何 UWP 控件。 **WindowsXamlHost**控件提供的`Microsoft.Toolkit.Wpf.UI.XamlHost`包 NuGet 包。 此包是附带`Microsoft.Toolkit.Wpf.UI.Controls`中安装的 NuGet 包[第 2 部分](modernize-wpf-tutorial-2.md)。

若要使用**WindowsXamlHost**控件，您将需要直接从 WPF 应用程序中的代码调用 WinRT Api。 `Microsoft.Windows.SDK.Contracts` NuGet 包中包含使您能够从应用调用 WinRT Api 所需的引用。 此包也包含在`Microsoft.Toolkit.Wpf.UI.Controls`中安装的 NuGet 包[第 2 部分](modernize-wpf-tutorial-2.md)。

## <a name="add-the-windowsxamlhost-control"></a>添加 WindowsXamlHost 控件

1. 在中**解决方案资源管理器**，展开**视图**文件夹中的**ContosoExpenses.Core**项目，然后双击**AddNewExpense.xaml**文件。 这是用于向列表添加新的支出的形式。 下面是当前版本的应用程序中的显示方式。

    ![添加新的支出](images/wpf-modernize-tutorial/AddNewExpense.png)

    日期选取器控件在 WPF 中包含适用于使用鼠标和键盘的传统计算机。 选择带有触摸屏日期并不太现实，由于控件和日历中每个日期之间的有限的空间的较小。

2. 在顶部**AddNewExpense.xaml**文件中，添加以下特性**窗口**元素。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    添加此特性后**窗口**元素现在应如下所示。

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. 更改**高度**的属性**窗口**元素从 450 到 800。 这需要因为 UWP**日历视图**控件占用比在 WPF 日期选取器的更多空间。

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. 找到`DatePicker`元素靠近底部的文件，并将此元素替换为以下 XAML。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    此 XAML 将添加**WindowsXamlHost**控件。 **InitialTypeName**属性指示你想要托管的 UWP 控件的完整名称 (在这种情况下， **Windows.UI.Xaml.Controls.CalendarView**)。

5. 按 F5 生成并在调试器中运行应用程序。 从列表中选择某位员工，然后按**添加新的支出**按钮。 确认的以下页面承载新的 UWP**日历视图**控件。

    ![日历视图包装器](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 关闭应用。

## <a name="interact-with-the-windowsxamlhost-control"></a>与 WindowsXamlHost 控件进行交互

接下来，将更新应用程序以处理所选的日期，显示在屏幕上，并填充**费用**要在数据库中保存对象。

UWP[日历视图](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)包含对于此方案相关的两个成员：

- **SelectedDates**属性包含用户选定的日期。
- **SelectedDatesChanged**用户选择日期时引发事件。

但是， **WindowsXamlHost**控件是为泛型宿主控件*任何*UWP 控件的类型。 在这种情况下，它不会公开一个名为属性**SelectedDates**或者事件中调用**SelectedDatesChanged**，因为它们是特定的**日历视图**控件。 若要访问这些成员，必须编写代码强制转换**WindowsXamlHost**到**日历视图**类型。 若要执行此操作的最佳位置是为了响应**ChildChanged**的事件**WindowsXamlHost**控件，在呈现所承载的控件时引发。

1. 在**AddNewExpense.xaml**文件，添加一个事件处理程序用于**ChildChanged**的事件**WindowsXamlHost**前面添加的控件。 完成后， **WindowsXamlHost**元素应如下所示。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 在同一文件中，找到**Grid.RowDefinitions**元素的主**网格**。 添加一个详细**RowDefinition**具有元素**高度**等于**自动**末尾的子元素的列表。 完成后， **Grid.RowDefinitions**元素应如下所示 (现在应该 9 **RowDefinition**元素)。

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. 添加以下 XAML 后的**WindowsXamlHost**元素和之前**按钮**文件末尾附近的元素。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 找到**按钮**快要结束的文件和更改的元素**了一个 Grid.Row**属性从**7**到**8**。 这会转而下移一行网格中的按钮因为添加新行。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 打开**AddNewExpense.xaml.cs**代码文件。

7. 将以下语句添加到文件顶部。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 添加以下事件处理程序到`AddNewExpense`类。 该代码将实现**ChildChanged**的事件**WindowsXamlHost**之前在 XAML 文件中声明的控件。

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    此代码使用**子**的属性**WindowsXamlHost**控件，用于访问 UWP**日历视图**控件。 然后代码订阅**SelectedDatesChanged**事件，该用户从日历中选择一个日期时，会触发事件。 此事件处理程序将所选的日期传递到 ViewModel 的新**费用**创建对象并将其保存到数据库。 若要执行此操作，该代码使用 MVVM Light NuGet 包提供的消息传送基础结构。 该代码将发送一条消息调用**SelectedDateMessage**到 ViewModel，其中将接收它，以及设置**日期**具有所选值的属性。 在此方案中**日历视图**控件配置为单一选择模式，因此，集合将包含仅有一个元素。

    此时，项目仍不会编译，因为它缺少的实现**SelectedDateMessage**类。 以下步骤实现此类。

9. 在中**解决方案资源管理器**，右键单击**消息**文件夹，然后选择**添加-> 类**。 将新类命名**SelectedDateMessage**然后单击**添加**。

10. 内容替换为**SelectedDateMessage.cs**用下面的代码的代码文件。 此代码将添加一个 using 语句`GalaSoft.MvvmLight.Messaging`（从 MVVM Light NuGet 包） 的命名空间继承自**MessageBase**类，并添加**DateTime**通过初始化的属性公共构造函数。 完成后，该文件应如下所示。

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. 接下来，将更新以接收此消息，并填充 ViewModel**日期**ViewModel 的属性。 在中**解决方案资源管理器**，展开**Viewmodel**文件夹，然后打开**AddNewExpenseViewModel.cs**文件。

12. 找到的公共构造函数`AddNewExpenseViewModel`类，并将以下代码添加到构造函数的末尾。 此代码将注册以接收**SelectedDateMessage**，它通过从提取所选的日期**SelectedDate**属性，然后我们使用它来设置**日期**属性公开 ViewModel。 因为此属性绑定与**TextBlock**控件之前，添加您所见用户选定的日期。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    完成后，`AddNewExpenseViewModel`构造函数应如下所示。 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > `SaveExpenseCommand`同一个代码文件中的方法执行的工作保存到数据库的节省的费用。 此方法使用**日期**属性以创建新**费用**对象。 此属性现在正在通过填充**日历视图**通过控制`SelectedDateMessage`消息您刚创建。

13. 按 F5 生成并在调试器中运行应用程序。 选择其中一个可用的雇员，然后单击**添加新的支出**按钮。 完成窗体中的所有字段，并从新选择的日期**日历视图**控件。 确认所选的日期显示为一个字符串，该控件的下方。

14. 按**保存**按钮。 将关闭窗体和到支出列表的末尾添加新的费用。 费用日期在第一列应为中选定的日期**日历视图**控件。

## <a name="next-steps"></a>后续步骤

此时在教程中，您已成功替换 WPF 日期时间控件使用 UWP**日历视图**控件，它支持触摸和除了鼠标和键盘输入数字笔。 虽然 Windows 社区工具包没有提供包装的版本的 UWP**日历视图**控件，可直接在 WPF 应用中，你能够通过使用泛型宿主控件[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件。

你现已准备好[第 4 部分：添加 Windows 10 用户活动和通知](modernize-wpf-tutorial-4.md)。
