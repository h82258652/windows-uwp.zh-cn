---
Description: UWP 应用中表单的布局指南。
title: 窗体
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: e1a5b192ed57d3962b6ba4cbef69e3663bc1e2ec
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683990"
---
# <a name="forms"></a>窗体
表单是一组控件，用于收集和提交来自用户的数据。 表单通常用于设置页面、调查、创建帐户，等等。 

本文讨论为表单创建 XAML 布局的设计准则。

![表单示例](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>应在何时使用表单？
表单是一个专用于收集数据输入的页面，这些输入互相有明显的关联。 需要从用户显式收集数据时，应使用表单。 可以为用户创建以下用途的表单：
- 登录帐户
- 注册帐户
- 更改应用设置，例如隐私或显示选项
- 参与调查
- 购买项
- 提供反馈

## <a name="types-of-forms"></a>表单类型

考虑用户输入的提交和显示方式时，可以使用两种类型的表单：

### <a name="1-instantly-updating"></a>1.即时更新
![设置页面](images/control-examples/toggle-switch-news.png)

如果希望用户立即看到更改表单中的值会有什么样的结果，请使用即时更新表单。 例如，在设置页面中会显示当前的选择，并会立即应用对选择所做的任何更改。 若要确认应用中的更改，需向每个输入控件[添加事件处理程序](controls-and-events-intro.md)。 如果用户更改输入控件，则应用可以相应地进行响应。

### <a name="2-submitting-with-button"></a>2.使用按钮进行提交
另一类型的表单允许用户通过单击某个按钮来选择何时提交数据。

![日历的“添加新事件”页](images/calendar-form.png)

此类表单允许用户灵活进行响应。 通常情况下，此类表单包含更多自由的表单输入字段，因此会收到更多种类的响应。 若要确保提交后的用户输入有效且数据格式正常，请考虑以下建议：

- 使用正确的控件，确保无法提交无效的信息（也就是说，对日历日期要使用 CalendarDatePicker 而不是 TextBox）。 若要详细了解如何在表单中选择适当的输入控件，请参阅后面的“输入控件”部分。
- 使用 TextBox 控件时，请使用 [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) 属性向用户提示所需的输入格式。
- 使用 [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) 属性指出预期的控件输入，为用户提供适当的屏幕键盘。
- 在标签上将必需的输入标记为星号 *。
- 在用户填完所有必需信息之前，禁用提交按钮。
- 如果提交后存在无效数据，请对包含无效输入的控件使用突出显示的字段或边框进行标记，并要求用户重新提交表单。
- 对于其他错误（例如网络连接失败），请务必向用户显示相应的错误消息。 


## <a name="layout"></a>布局

若要简化用户体验，确保用户能够进行正确的输入，请在设计表单的布局时考虑以下建议： 

### <a name="labels"></a>标签
[标签](labels.md)应该左对齐并放置在输入控件上方。 许多控件都有用于显示标签的内置 Header 属性。 对于没有 Header 属性的控件，或要为多组控件添加标签，你可以改用 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

在[针对辅助功能进行设计](../accessibility/accessibility.md)时，请将所有的单个控件和控件组进行标记，使用户和屏幕阅读器都能清楚地看到。 

对于字体样式，请使用默认的 [UWP 类型渐变](../style/typography.md)。 将 `TitleTextBlockStyle` 用于页面标题，将 `SubtitleTextBlockStyle` 用于组标题，将 `BodyTextBlockStyle` 用于控件标签。

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
若要在视觉上将控件组彼此分开，请使用[对齐、边距和填充](../layout/alignment-margin-padding.md)。 单个输入控件的高度为 80px，应该隔开 24px 的距离。 输入控件组应该隔开 48px 的距离。

![表单组](images/forms-groups.png)

### <a name="columns"></a>列
创建列可以减少表单中不必要的空白，尤其是在屏幕大小较大的情况下。 但是，若要创建多列表单，则列数应该取决于页面上的输入控件数以及应用窗口的屏幕大小。 可以考虑为表单创建多个页面，而不是让屏幕充斥各种输入控件。  

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
表单应该在屏幕或窗口大小变化时重设大小，这样用户就不会忽略任何输入字段。 有关详细信息，请参阅[响应式设计技术](../layout/responsive-design.md)。 例如，可能需要让表单的特定区域始终位于视图中，不管屏幕大小如何变化。

![表单焦点](images/forms-focus2.png)

### <a name="tab-stops"></a>制表位
用户可以使用键盘，通过[制表位](../input/keyboard-interactions.md#tab-stops)来导航控件。 默认情况下，控件的 Tab 键顺序反映了其在 XAML 中的创建顺序。 若要覆盖默认的行为，请更改控件的 **IsTabStop** 或 **TabIndex** 属性。 

![表单中控件上的 Tab 键焦点](images/forms-focus1.png)

## <a name="input-controls"></a>输入控件
输入控件是 UI 元素，允许用户将信息输入表单中。 下面列出了一些可以添加到表单中的常用控件，并介绍了何时使用它们。

### <a name="text-input"></a>文本输入
控件 | 用途 | 示例
 - | - | -
[TextBox](text-box.md) | 捕获一个或多个文本行 | 名称、自由表单响应或反馈
[PasswordBox](password-box.md) | 通过隐藏字符来收集专用数据 | 密码、社会安全号码 (SSN)、PIN、信用卡信息 
[AutoSuggestBox](auto-suggest-box.md) | 当用户键入时，从相应的一组数据中选择一系列建议显示给用户 | 数据库搜索、mail to: 行、以前的查询
[RichEditBox](rich-edit-box.md) | 使用格式化文本、超链接和图像编辑文本文件 | 在应用中上传文件，然后进行预览和编辑

### <a name="selection"></a>选择
控件 | 用途 | 示例
- | - | - 
| [CheckBox](checkbox.md) | 选择或取消选择一个或多个操作项 | 同意条款和条件，添加可选项，选择所有适用的内容
[RadioButton](radio-button.md) | 从两个或多个选项中选择一个选项 | 选取类型、送货方式等
[ToggleSwitch](toggles.md) | 从两个互斥选项中选择一个 | 打开/关闭

> **注意**：如果有五个或更多个选择项，请使用列表控件。

### <a name="lists"></a>列表
控件 | 用途 | 示例
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | 最初处于紧凑状态，可展开以显示可选择项的列表 | 从项（例如州或国家/地区）的长列表中进行选择
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | 对项进行分类并为之分配组标头、拖放项、规划内容以及重新对项进行排序 | 分级选项
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | 排列和浏览基于图像的集合 | 选取照片、颜色、显示主题

### <a name="numeric-input"></a>数字输入
控件 | 用途 | 示例
- | - | -
[滑块](slider.md) | 从一系列相邻的数字值中选择一个数字 | 百分比、音量、播放速度
[评级](rating.md) | 使用星号评级 | 客户反馈

### <a name="date-and-time"></a>日期和时间

控件 | 用途 
- | - 
[CalendarView](calendar-view.md) | 从始终可见的日历中选取一个日期或日期范围 
[CalendarDatePicker](calendar-date-picker.md) | 从上下文日历中选取一个日期 
[DatePicker](date-picker.md) | 在上下文信息不重要时选取一个本地化的日期
[TimePicker](time-picker.md) | 选取一个时间值

### <a name="additional-controls"></a>其他控件 
如需 UWP 控件的完整列表，请参阅[按功能排序的控件索引](controls-by-function.md)。

如需更多复杂的自定义 UI 控件，请查看 [Telerik](https://www.telerik.com/)、[SyncFusion](https://www.syncfusion.com/uwp-ui-controls)、[DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)、[Infragistics](https://www.infragistics.com/products/universal-windows-platform)、[ComponentOne](https://www.componentone.com/Studio/Platform/UWP)、[ActiPro](https://www.actiprosoftware.com/products/controls/universal) 等公司提供的 UWP 资源。

## <a name="one-column-form-example"></a>单列表单示例
以下示例使用亚克力的[大纲/细节](master-details.md)[列表视图](lists.md)和 [NavigationView](navigationview.md) 控件。
![另一表单示例的屏幕截图](images/FormExample2.png)
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

## <a name="two-column-form-example"></a>双列表单示例
以下示例除了使用输入控件，还使用[透视](pivot.md)控件、[Acrylic](../style/acrylic.md) 背景和 [CommandBar](app-bars.md)。
![表单示例的屏幕截图](images/FormExample.png)
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
![客户订单数据库屏幕截图](images/customerorderform.png) 若要了解如何将表单输入连接到 **Azure** 数据库并查看完全实现的表单，请参阅[客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)应用示例。

## <a name="related-topics"></a>相关主题
- [输入控件](controls-and-events-intro.md)
- [版式](../style/typography.md)
