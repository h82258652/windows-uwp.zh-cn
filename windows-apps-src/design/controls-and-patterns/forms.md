---
Description: UWP 应用中的窗体的布局准则。
title: 表单
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658152"
---
# <a name="forms"></a>表单
表单是一组控件，收集并提交来自用户的数据。 窗体通常用于设置页，创建帐户，以及更多的调查。 

本文讨论了创建窗体的 XAML 布局的设计准则。

![窗体的示例](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>何时应使用窗体？
表单是用于收集清楚地彼此相关的数据输入一个专用的页面。 当您需要显式从用户处收集数据时，应使用窗体。 您可以创建用于对用户的窗体：
- 登录到帐户
- 注册帐户
- 更改应用设置，如隐私或显示选项
- 参与调查
- 购买项
- 提供反馈

## <a name="types-of-forms"></a>类型的窗体

在考虑如何提交并显示用户输入，有两种类型的窗体：

### <a name="1-instantly-updating"></a>1.立即更新
![设置页](images/control-examples/toggle-switch-news.png)

当你希望用户能够立即看到更改窗体中的值的结果，请使用立即更新窗体。 例如，在设置页中，将显示当前的选择，并对所选内容进行任何更改会立即应用。 若要确认应用程序中的更改，你将需要[添加事件处理程序](controls-and-events-intro.md)到每个输入控件。 如果用户更改了输入的控件，你的应用可以适当地响应。

### <a name="2-submitting-with-button"></a>2.提交按钮
其他类型的窗体允许用户选择何时提交按钮的单击操作使用的数据。

![日历添加新的事件页](images/calendar-form.png)

这种类型的窗体使用户灵活地响应。 通常情况下，这种类型的窗体包含更多自由格式的输入的字段，并因此接收到更多的响应。 若要确保有效的用户输入和格式正确的数据提交后，请考虑以下建议：

- 使其无法使用正确的控件提交无效的信息 （即，使用 CalendarDatePicker 而不是文本框的日历日期）。 查看更多的输入控件部分更高版本中窗体中选择相应的输入的控件。
- 使用文本框控件时，为用户提供与所需的输入格式的提示[PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText)属性。
- 提供具有适当的用户声明的控件的预期的输入屏幕键盘[InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope)属性。
- 标记所需的输入带有星号 * 标签上。
- 禁用提交按钮，直到所有必需的信息填充的。
- 如果没有提交后的无效数据，则将标记具有与突出显示的字段或边框，无效的输入控件，并要求用户重新提交该窗体。
- 对于其他错误，例如网络连接失败，请确保向用户显示相应的错误消息。 


## <a name="layout"></a>布局

若要简化用户体验和确保用户能够输入正确的输入，请考虑以下建议设计的窗体的布局。 

### <a name="labels"></a>标签
[标签](labels.md)应该左对齐，输入控件上方。 许多控件具有内置的标头属性，以显示标签。 对于没有 Header 属性的控件，或要为多组控件添加标签，你可以改用 [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

向[可访问性的设计](../accessibility/accessibility.md)，标记所有个人和清晰性这两个用户和屏幕阅读器的控件组。 

对于字体样式，使用默认[UWP 类型校正，](../style/typography.md)。 使用`TitleTextBlockStyle`的页面标题`SubtitleTextBlockStyle`的组标题和`BodyTextBlockStyle`控件标签。

<div class="mx-responsive-img">
<table>
<tr><th>应做事项</th><th>禁止事项</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>间距
若要以可视方式彼此分隔控件组，请使用[对齐、 边距和填充](../layout/alignment-margin-padding.md)。 各个输入的控件的高度 80px 并且应相隔间距的 24px。 输入的控件组应相隔的空格分隔的 48px。

![窗体组](images/forms-groups.png)

### <a name="columns"></a>列
创建列可以减少在窗体，尤其是在使用更大的屏幕大小中不必要的空格。 但是，如果你想要创建多列窗体，列数应依赖于页面上的输入控件的数量和应用程序窗口的屏幕大小。 而不是严重影响了许多输入控件的屏幕，请考虑创建多个用于你的窗体页。  

<div class="mx-responsive-img">
<table>
<tr><th>应做事项</th><th>禁止事项</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>响应式布局
窗体应调整大小随屏幕或窗口大小的更改，因此用户不要忽略输入的任何字段。 有关详细信息，请参阅[响应式设计技术](../layout/responsive-design.md)。 例如，您可能想要在视图中，无论屏幕大小如何始终保持在窗体的特定区域。

![窗体焦点](images/forms-focus2.png)

### <a name="tab-stops"></a>制表位
用户可以使用键盘导航控件与[制表](../input/keyboard-interactions.md#tab-stops)。 默认情况下，控件的 tab 键顺序反映了在 XAML 中的创建的顺序。 若要重写默认行为，更改**IsTabStop**或**TabIndex**控件的属性。 

![控件在窗体上 tab 键定焦](images/forms-focus1.png)

## <a name="input-controls"></a>输入的控件
输入的控件是允许用户在窗体中输入信息的 UI 元素。 以及有关何时使用它们的信息，下面列出了可以添加到窗体一些公共控件。

### <a name="text-input"></a>文本输入
控件 | 将 | 示例
 - | - | -
[文本框](text-box.md) | 捕获一个或多个行文本 | 名称、 自由格式的响应或反馈
[PasswordBox](password-box.md) | 隐藏字符收集专用数据 | 密码、 社会安全号码 (SSN) 的 Pin，信用卡信息 
[AutoSuggestBox](auto-suggest-box.md) | 在键入时显示用户从一组对应的数据的建议列表 | 数据库搜索中，向发送邮件： 行前面的查询
[RichEditBox](rich-edit-box.md) | 编辑文本文件与带格式的文本、 超链接和图像 | 上传文件，预览并在应用中编辑

### <a name="selection"></a>选择
控件 | 将 | 示例
- | - | - 
| [复选框](checkbox.md) | 选择或取消选择一个或多个操作项目 | 同意条款和条件，将添加可选项，请选择所有适用
[单选按钮](radio-button.md) | 从两个或多个选项中选择一个选项 | 选择类型，传送方法，等等。
[ToggleSwitch](toggles.md) | 选择两个互相排斥的选项之一 | 打开/关闭

> **注意**：如果有五个或多个选择项，使用列表控件。

### <a name="lists"></a>列表
控件 | 将 | 示例
- | - | -
[组合框](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | 启动 compact 的状态和展开以显示选择项的列表 | 从项，例如州或国家/地区的长列表中选择
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | 对项进行分类和分配组标头、 拖动和删除项、 评判内容，和对项重新排序 | Rank 选项
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | 排列和浏览基于映像的集合 | 选取的照片，颜色、 显示主题

### <a name="numeric-input"></a>数字输入
控件 | 将 | 示例
- | - | -
[滑块](slider.md) | 选择一个数字从一系列连续的数字值 | 百分比、 卷、 播放速度
[分级](rating.md) | 使用星评级 | 客户反馈

### <a name="date-and-time"></a>日期和时间

控件 | 将 
- | - 
[日历视图](calendar-view.md) | 选择单个日期或中始终可见的日历的日期范围 
[CalendarDatePicker](calendar-date-picker.md) | 选择上下文在日历中的单一日期 
[DatePicker](date-picker.md) | 选择一个当上下文信息并不重要的本地化的日期
[TimePicker](time-picker.md) | 选择一个时间值

### <a name="additional-controls"></a>其他控件 
UWP 控件的完整列表，请参阅[函数按控件的索引](controls-by-function.md)。

对于更复杂和自定义 UI 控件，查看可从公司的 UWP 资源如[Telerik](https://www.telerik.com/)， [SyncFusion](https://www.syncfusion.com/products/uwp)， [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)， [Infragistics](https://www.infragistics.com/products/universal-windows-platform)， [ComponentOne](https://www.componentone.com/Studio/Platform/UWP)，和[ActiPro](https://www.actiprosoftware.com/products/controls/universal)。

## <a name="one-column-form-example"></a>一个列窗体示例
此示例使用染料[母版/详细信息](master-details.md)[列表视图](lists.md)并[NavigationView](navigationview.md)控件。
![另一个窗体示例的屏幕截图](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>两个列的窗体示例
此示例使用[Pivot](pivot.md)控件，[染料](../style/acrylic.md)背景，并[CommandBar](app-bars.md)除了输入控件。
![窗体示例的屏幕截图](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>客户订单数据库示例
![客户订单数据库屏幕截图](images/customerorderform.png)若要了解如何连接到的窗体输入**Azure**数据库中，请参阅完全实现窗体中，请参阅[客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)应用示例。

## <a name="related-topics"></a>相关主题
- [输入的控件](controls-and-events-intro.md)
- [版式](../style/typography.md)
