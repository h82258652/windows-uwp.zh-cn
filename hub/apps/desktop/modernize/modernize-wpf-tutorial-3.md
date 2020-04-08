---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 MSIX 包以及将其他新式组件合并到 WPF 应用中。
title: 使用 XAML Islands 添加 UWP CalendarView 控件
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643374"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部分：使用 XAML Islands 添加 UWP CalendarView 控件

这是教程的第 3 部分，此教程演示如何使名为 Contoso Expenses 的示例 WPF 桌面应用实现现代化。 有关下载示例应用的教程、先决条件和说明的概述，请参阅[教程：实现 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假设你已完成[第 2 部分](modernize-wpf-tutorial-2.md)。

在本教程的虚构应用场景中，Contoso 开发团队想要更轻松地在支持触摸的设备上选择支出报表的日期。 在此教程的本部分，你将向应用添加 UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) 控件。 此控件与任务栏上 Windows 10 日期和时间功能中使用的控件相同。

![CalendarViewControl 图像](images/wpf-modernize-tutorial/CalendarViewControl.png)

与在[第 2 部分](modernize-wpf-tutorial-2.md)中添加的 InkCanvas 控件不同，Windows 社区工具包不提供可在 WPF 应用中使用的 UWP CalendarView 的包装版本   。 作为替代方法，你将在通用 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件中托管 InkCanvas  。 你可以使用此控件来托管由 Windows SDK 或 WinUI 库提供的任何第一方 UWP 控件，或由第三方创建的任何自定义 UWP 控件。 WindowsXamlHost 控件由 `Microsoft.Toolkit.Wpf.UI.XamlHost` 包（包含于 NuGet 包中）提供  。 此包包含于在[第 2 部分](modernize-wpf-tutorial-2.md)安装的 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 包中。

> [!NOTE]
> 本教程仅演示如何使用 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 来托管由 Windows SDK 提供的第一方 CalendarView 控件  。 若要详细演示如何托管自定义控件，请参阅[使用 XAML 岛在 WPF 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)。

为使用 WindowsXamlHost 控件，需要从 WPF 应用中的代码直接调用 WinRT API  。 `Microsoft.Windows.SDK.Contracts` NuGet 包包含了必需的引用，该引用使你能够从应用调用 WinRT API。 此包也包含于在[第 2 部分](modernize-wpf-tutorial-2.md)安装的 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 包中。

## <a name="add-the-windowsxamlhost-control"></a>添加 WindowsXamlHost 控件

1. 在解决方案资源管理器中，展开 ContosoExpenses.Core 项目中的 Views 文件夹，然后双击 AddNewExpense.xaml 文件     。 这是用于向列表中添加新支出的窗体。 下面是它在当前版本应用中的显示方式。

    ![添加新支出](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF 中包含的日期选取器控件适用于带有鼠标和键盘的传统计算机。 由于控件尺寸较小且日历中每一天之间的空间有限，因此使用触摸屏选择日期实际上是不可行的。

2. 在 AddNewExpense.xaml 文件的顶部，将以下属性添加到 Window 元素   。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    添加此属性后，Window 元素现在应如下所示  。

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

3. 将 Window 元素的 Height 属性从 450 更改为 800   。 这是必需的，因为 UWP CalendarView 控件占用的空间比 WPF 日期选取器要多  。

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

4. 在文件底部附近找到 `DatePicker` 元素，并将其替换为以下 XAML。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    此 XAML 将添加 WindowsXamlHost 控件  。 InitialTypeName 属性指示要托管的 UWP 控件的完整名称（在本例中为 Windows.UI.Xaml.Controls.CalendarView）   。

5. 按 F5 在调试程序中生成并运行应用。 从列表中选择员工，然后按“添加新支出”按钮  。 确认以下页面托管新的 UWP CalendarView 控件  。

    ![CalendarView 包装器](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 关闭应用。

## <a name="interact-with-the-windowsxamlhost-control"></a>与 WindowsXamlHost 控件进行交互

接下来，你将更新应用以处理选定日期，将其显示在屏幕上，然后填充要保存到数据库中的 Expense 对象  。

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 包含与此应用场景相关的两个成员：

- SelectedDates 属性包含用户选定的日期  。
- 用户选择日期时，将引发 SelectedDatesChanged 事件  。

但是，WindowsXamlHost 控件是任何类型的 UWP 控件均通用的主机控件   。 因此，它不会公开名为 SelectedDates 的属性或名为 SelectedDatesChanged 的事件，因为它们特定于 CalendarView 控件    。 若要访问这些成员，必须编写将 WindowsXamlHost 强制转换为 CalendarView 类型的代码   。 实现此目的的最佳位置是响应 WindowsXamlHost 控件的 ChildChanged 事件，该事件在呈现托管控件时引发   。

1. 在 AddNewExpense.xaml 文件中，为之前添加的 WindowsXamlHost 控件的 ChildChanged 事件添加事件处理程序    。 完成后，WindowsXamlHost 元素应如下所示  。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 在同一文件中，找到主 Grid 的 Grid.RowDefinitions 元素   。 在子元素列表的末尾再添加一个 Height 等于 Auto 的 RowDefinition 元素    。 完成后，Grid.RowDefinitions 元素应如下所示（现在应有 9 个 RowDefinition 元素）   。

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

4. 在文件末尾附近的 WindowsXamlHost 元素之后和 Button 元素之前添加以下 XAML   。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 找到文件末尾附近的 Button 元素，并将 Grid.Row 属性从 7 更改为 8     。 这会将按钮在网格中向下移动一行，因为添加了新行。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 打开 AddNewExpense.xaml.cs 代码文件  。

7. 将以下语句添加到文件顶部。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 将以下事件处理程序添加到 `AddNewExpense` 类。 此代码可实现先前在 XAML 文件中声明的 WindowsXamlHost 控件的 ChildChanged 事件   。

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

    此代码使用 WindowsXamlHost 控件的 Child 属性访问 UWP CalendarView 控件    。 然后，代码将预订 SelectedDatesChanged 事件，该事件在用户从日历中选择日期时触发  。 该事件处理程序将选定日期传递到 ViewModel，在此 ViewModel 中创建新的 Expense 对象并将其保存到数据库  。 为执行此操作，该代码使用 MVVM 轻量 NuGet 包提供的消息传递基础结构。 该代码将名为 SelectedDateMessage 的消息发送到 ViewModel，后者将接收该消息，并使用选定值设置 Date 属性   。 在这种情况下，将为单一选择模式配置 CalendarView 控件，因此集合将仅包含一个元素  。

    此时，该项目仍无法编译，因为它缺少 SelectedDateMessage 类的实现  。 以下步骤可实现此类。

9. 在解决方案资源管理器中，右键单击 Messages 文件夹，然后选择“添加”->“类”    。 将新类命名为 SelectedDateMessage，然后单击“添加”   。

10. 将 SelectedDateMessage.cs 代码文件的内容替换为以下代码  。 此代码为 `GalaSoft.MvvmLight.Messaging` 命名空间（来自 MVVM 轻量 NuGet 包）添加 using 语句，从 MessageBase 类继承，并添加通过公共构造函数初始化的 DateTime 属性   。 完成后，该文件应如下所示。

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

11. 接下来，你将更新 ViewModel 以接收此消息，并填充 ViewModel 的 Date 属性  。 在解决方案资源管理器中，展开 ViewModels 文件夹，然后打开 AddNewExpenseViewModel.cs 文件    。

12. 找到 `AddNewExpenseViewModel` 类的公共构造函数，然后将以下代码添加到该构造函数的末尾。 此代码将登记接收 SelectedDateMessage，通过 SelectedDate 属性从其中提取所选日期，然后使用它来设置由 ViewModel 公开的 Date 属性    。 由于此属性与先前添加的 TextBlock 控件绑定，因此可以查看用户选择的日期  。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    完成后，`AddNewExpenseViewModel` 构造函数应如下所示。 

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
    > 同一代码文件中的 `SaveExpenseCommand` 方法执行将支出保存到数据库的工作。 此方法使用 Date 属性创建新的 Expense 对象   。 此属性现在由 CalendarView 控件通过刚创建的 `SelectedDateMessage` 消息进行填充  。

13. 按 F5 在调试程序中生成并运行应用。 选择一个可用员工，然后单击“添加新支出”按钮  。 填写窗体中的所有字段，然后从新的 CalendarView 控件中选择一个日期  。 确认选定日期显示为控件下方的字符串。

14. 按“保存”按钮  。 将关闭窗体，并将新的支出添加到支出列表的末尾。 带有支出日期的第一列应是你在 CalendarView 控件中选择的日期  。

## <a name="next-steps"></a>后续步骤

本教程进行到这里，你已成功将 WPF 日期时间控件替换为 UWP CalendarView 控件，该控件除了支持鼠标和键盘输入以外，还支持触摸和数字笔  。 尽管 Windows 社区工具包不提供可直接在 WPF 应用中使用的 UWP CalendarView 控件的包装版本，但你可以使用通用 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件来托管该控件  。

现在你已准备好学习[第 4 部分：添加 Windows 10 用户活动和通知](modernize-wpf-tutorial-4.md)。
