---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 .MSIX 包以及将其他新式组件合并到 WPF 应用。
title: 使用 XAML Islands 添加 UWP CalendarView 控件
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643374"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部分：使用 XAML Islands 添加 UWP CalendarView 控件

这是教程的第三部分, 该教程演示如何将名为 Contoso 支出的示例 WPF 桌面应用实现现代化。 有关下载示例应用的教程、先决条件和说明的概述, 请参阅[教程:现代化 WPF 应用](modernize-wpf-tutorial.md)。 本文假设你已完成[第2部分](modernize-wpf-tutorial-2.md)。

在本教程的虚构方案中, Contoso 开发团队想要更轻松地在已启用触控的设备上选择支出报表的日期。 在本教程的此部分中, 你将向应用程序添加 UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)控件。 这是任务栏上 Windows 10 日期和时间功能中使用的相同控件。

![CalendarViewControl 图](images/wpf-modernize-tutorial/CalendarViewControl.png)

与[第2部分](modernize-wpf-tutorial-2.md)中添加的**InkCanvas**控件不同, Windows 社区工具包不提供可在 WPF 应用中使用的 UWP **CalendarView**的包装版本。 作为替代方法, 你将在泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件中托管**InkCanvas** 。 您可以使用此控件承载 Windows SDK 或 WinUI 库提供的任何第三方 UWP 控件或第三方创建的任何自定义 UWP 控件。 **WindowsXamlHost**控件由`Microsoft.Toolkit.Wpf.UI.XamlHost`包 NuGet 包提供。 此包包含在`Microsoft.Toolkit.Wpf.UI.Controls` [第2部分](modernize-wpf-tutorial-2.md)中安装的 NuGet 包中。

> [!NOTE]
> 本教程只演示了如何使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)承载 Windows SDK 提供的第一方**CalendarView**控件。 有关演示如何承载自定义控件的演练, 请参见[使用 XAML 孤岛在 WPF 应用程序中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)。

若要使用**WindowsXamlHost**控件, 需要直接从 WPF 应用程序中的代码调用 WinRT api。 `Microsoft.Windows.SDK.Contracts` NuGet 包包含使你可以从应用程序调用 WinRT api 所需的引用。 此包还包含在`Microsoft.Toolkit.Wpf.UI.Controls` [第2部分](modernize-wpf-tutorial-2.md)中安装的 NuGet 包中。

## <a name="add-the-windowsxamlhost-control"></a>添加 WindowsXamlHost 控件

1. 在**解决方案资源管理器**中, 展开**ContosoExpenses**项目中的**Views**文件夹, 然后双击**AddNewExpense**文件。 这是用来向列表中添加新支出的窗体。 它显示在应用程序的当前版本中。

    ![添加新支出](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF 中包含的日期选取器控件适用于带有鼠标和键盘的传统计算机。 使用触摸屏选择日期并不是真正可行的, 因为控件的大小较小, 日历中每天的空间有限。

2. 在**AddNewExpense**文件的顶部, 将以下属性添加到**Window**元素。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    添加此属性后, **Window**元素现在应如下所示。

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

3. 将**Window**元素的**Height**属性从450更改为800。 这是必需的, 因为 UWP **CalendarView**控件比 WPF 日期选取器占用更多的空间。

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

4. 找到文件`DatePicker`底部附近的元素, 并将此元素替换为以下 XAML。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    此 XAML 将添加**WindowsXamlHost**控件。 **InitialTypeName**属性指示要承载的 UWP 控件的全名 (在这种情况下为**CalendarView**) 的全名。

5. 按 F5 在调试器中生成并运行应用。 从列表中选择员工, 然后按 "**添加新支出**" 按钮。 确认以下页面承载了新的 UWP **CalendarView**控件。

    ![CalendarView 包装器](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 关闭应用。

## <a name="interact-with-the-windowsxamlhost-control"></a>与 WindowsXamlHost 控件交互

接下来, 你将更新应用程序以处理选定日期, 将其显示在屏幕上, 并填充要保存在数据库中的**支出**对象。

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)包含与此方案相关的两个成员:

- **SelectedDates**属性包含用户选定的日期。
- 当用户选择日期时, 将引发**SelectedDatesChanged**事件。

但是, **WindowsXamlHost**控件是适用于*任何*类型的 UWP 控件的通用主机控件。 因此, 它不公开名为**SelectedDates**的属性或名为**SelectedDatesChanged**的事件, 因为它们特定于**CalendarView**控件。 若要访问这些成员, 必须编写将**WindowsXamlHost**转换为**CalendarView**类型的代码。 实现此目的的最佳位置是响应**WindowsXamlHost**控件的**ChildChanged**事件, 该控件在已呈现托管控件时引发。

1. 在**AddNewExpense**文件中, 为之前添加的**WindowsXamlHost**控件的**ChildChanged**事件添加事件处理程序。 完成后, **WindowsXamlHost**元素应如下所示。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 在同一文件中, 找到主**网格**的**RowDefinitions**元素。 再添加一个**system.windows.controls.rowdefinition.height**元素, 其**高度**等于子元素列表末尾的 "**自动**"。 完成后, **RowDefinitions**元素应如下所示 (现在应有9个**system.windows.controls.rowdefinition.height**元素)。

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

4. 将以下 XAML 添加到文件末尾附近的**WindowsXamlHost**元素和**Button**元素之前。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 找到文件末尾附近的**按钮**元素, 并将网格的 "**行**" 属性从**7**更改为**8**。 这会将按钮向下移动到网格中的某一行, 因为添加了新行。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 打开**AddNewExpense.xaml.cs**代码文件。

7. 将以下语句添加到文件顶部。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 向`AddNewExpense`类添加以下事件处理程序。 此代码实现前面在 XAML 文件中声明的**WindowsXamlHost**控件的**ChildChanged**事件。

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

    此代码使用**WindowsXamlHost**控件的**子**属性来访问 UWP **CalendarView**控件。 然后, 该代码订阅**SelectedDatesChanged**事件, 当用户从日历中选择日期时, 将触发该事件。 此事件处理程序将所选日期传递到 ViewModel, 此时将在其中创建新的**支出**对象并将其保存到数据库中。 为此, 该代码使用 MVVM 轻型 NuGet 包提供的消息传递基础结构。 该代码将名为**SelectedDateMessage**的消息发送到 ViewModel, 后者将接收该消息并使用所选值设置**日期**属性。 在这种情况下, **CalendarView**控件配置为单一选择模式, 因此集合将只包含一个元素。

    此时, 项目仍然不会编译, 因为它缺少**SelectedDateMessage**类的实现。 以下步骤实现此类。

9. 在**解决方案资源管理器**中, 右键单击 "**消息**" 文件夹, 然后选择 "**外接程序 > 类**"。 将新类命名为**SelectedDateMessage** , 然后单击 "**添加**"。

10. 将**SelectedDateMessage.cs**代码文件的内容替换为以下代码。 此代码为`GalaSoft.MvvmLight.Messaging`命名空间添加一个 using 语句 (来自 MVVM light NuGet 包), 继承自**MessageBase**类, 并添加通过公共构造函数初始化的**DateTime**属性。 完成后, 该文件应如下所示。

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

11. 接下来, 你将更新 ViewModel 以接收此消息, 并填充 ViewModel 的**日期**属性。 在**解决方案资源管理器**中, 展开**viewmodel**文件夹并打开**AddNewExpenseViewModel.cs**文件。

12. 找到`AddNewExpenseViewModel`类的公共构造函数, 并将以下代码添加到构造函数的末尾。 此代码将注册以接收**SelectedDateMessage**, 从其提取所选日期通过**SelectedDate**属性, 并使用它来设置 ViewModel 公开的**日期**属性。 由于此属性与前面添加的**TextBlock**控件绑定, 因此可以查看用户选择的日期。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    完成后, `AddNewExpenseViewModel`构造函数应如下所示。 

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
    > 同一`SaveExpenseCommand`代码文件中的方法执行将支出保存到数据库的工作。 此方法使用**Date**属性来创建新的**支出**对象。 **CalendarView**控件现在正在通过刚刚创建的`SelectedDateMessage`消息填充此属性。

13. 按 F5 在调试器中生成并运行应用。 选择一个可用员工, 然后单击 "**添加新支出**" 按钮。 填写表单中的所有字段, 并从新的**CalendarView**控件中选择日期。 确认所选日期在控件下显示为字符串。

14. 按 "**保存**" 按钮。 将关闭窗体, 并将新的费用添加到支出列表的末尾。 带有支出日期的第一列应是在**CalendarView**控件中选择的日期。

## <a name="next-steps"></a>后续步骤

在本教程的这篇教程中, 您已成功将 WPF 日期时间控件替换为 UWP **CalendarView**控件, 该控件除了支持鼠标和键盘输入以外, 还支持触控和数字笔。 尽管 Windows 社区工具包未提供可直接在 WPF 应用程序中使用的 UWP **CalendarView**控件的包装版本, 但你可以使用泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件承载控件。

你现在已准备好[第4部分:添加 Windows 10 用户活动和通知](modernize-wpf-tutorial-4.md)。
