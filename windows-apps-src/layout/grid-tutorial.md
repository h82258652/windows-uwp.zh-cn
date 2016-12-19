# <a name="create-a-simple-weather-app-by-using-grid-and-stackpanel"></a>通过使用网格和 StackPanel 创建一个简单的天气应用

使用 XAML  并使用**网格**和 **StackPanel** 元素创建简单的天气应用的布局。 使用这些工具，可以很容易地在运行 Windows 10 的任何设备上使应用看起来很棒。 本教程需要 10-20 分钟。

## <a name="prerequisites"></a>先决条件
- Windows 10 和 Microsoft Visual Studio 2015。 [单击此处了解如何设置 Visual Studio](../get-started/get-set-up.md)。
- 了解如何通过使用 XAML 和 C# 创建一个基本的“Hello World”应用。 如果还没有，[请单击此处以了解如何创建一个“Hellow World”应用](https://msdn.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="step-1-create-a-blank-app"></a>步骤 1：创建空白应用
1. 在 Visual Studio 菜单中，选择“文件” > “新建项目”。
2. 在“新建项目”对话框的左侧窗格中，依次选择“Visual C#” > “Windows” > “通用”，或者依次选择“Visual C++” > “Windows” > “通用”。
3. 在中心窗格中，选择“空白应用”。
4. 在“名称”框中，输入“WeatherPanel”并选择“确定”。
5. 若要运行程序，请从菜单中依次选择“调试” > “开始调试”，或选择 F5。

## <a name="step-2-define-a-grid"></a>步骤 2：定义网格
在 XAML 中，**网格**由一系列行和列组成。 通过指定**网格**内某个元素的行和列，你可以轻松地在用户界面内放置和间隔其他元素。 使用 **RowDefinition** 和 **ColumnDefinition** 元素定义行和列。

若要开始创建布局，通过使用**解决方案资源管理器**打开 **MainPage.xaml**，并将自动生成的**网格**元素替换为此代码。

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="3*"/>
        <ColumnDefinition Width="5*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="2*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
</Grid>
```

新**网格**将创建一个两行和两列的集合，它可定义应用界面的布局。 第一列的“宽度”为“3\ *”，第二列为“5\ *”，除以比率为 3:5 的两列之间的水平空间。 与此方式相同，两行的“高度”分别为“3\ *”和“\ *”，因此相对于第二行，**网格**为第一行分配三倍空间（“\ *”等同于“1 *”）。 即使在调整窗口大小或更改设备时，都会保留这些比率。

若要了解有关调整行和列大小的其他方法，请参阅[使用 XAML 定义布局](https://msdn.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#layout-properties)。

如果现在运行该应用程序，只会看到一个空白页面，因为任何**网格**区域都没有内容。 若要显示**网格**，让我们添加一些颜色。

## <a name="step-3-color-the-grid"></a>步骤 3：为网格着色
为了为**网格**着色，我们添加了三个**边框**元素，每个都带有不同的背景色。  通过使用 **Grid.Row** 和 **Grid.Column** 属性，也将每个元素分配到父**网格**中的行和列。 这些属性的值默认为 0 时，因此无需将其分配给第一个**边框**。 定义行和列后，将以下代码添加到**网格**元素。

```xml
<Border Background="#2f5cb6"/>
<Border Grid.Column ="1" Background="#1f3d7a"/>
<Border Grid.Row="1" Grid.ColumnSpan="2" Background="#152951"/>
```

请注意，对于第三个**边框**，我们使用一个额外属性 **Grid.ColumnSpan**，这将导致此**边框**在较低的行中跨越全部两列。 可以相同方式使用 **Grid.RowSpan**，并且这些属性一起允许你在任意数量的行和列上跨越元素。 此类跨越的左上角始终是在元素属性中指定的 **Grid.Column** 和 **Grid.Row**。

如果运行应用，结果将如下所示。

![为网格着色](images/grid-weather-1.png)

## <a name="step-4-organize-content-by-using-stackpanel-elements"></a>步骤 4：通过使用 StackPanel 元素组织内容
**StackPanel** 是我们将用于创建天气应用的第二个 UI 元素。 **StackPanel** 是许多基本应用布局的基础部分，允许你轻松垂直或水平堆叠元素。

在以下代码中，我们创建了两个 **StackPanel** 元素并使用三个 **Textblock** 填充每个元素。 将这些 **StackPanel** 元素添加到第 3 步中**边框**元素下面的**网格**中。 这会导致 **TextBlock** 元素呈现在我们之前创建的彩色**网格**顶部。

```xml
<StackPanel Grid.Column="1" Margin="40,0,0,0" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="Today - 64° F"/>
    <TextBlock Foreground="White" FontSize="25" Text="Partially Cloudy"/>
    <TextBlock Foreground="White" FontSize="25" Text="Precipitation: 25%"/>
</StackPanel>
<StackPanel Grid.Row="1" Grid.ColumnSpan="2" Orientation="Horizontal"
            HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="High: 66°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Low: 43°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Feels like: 63°"/>
</StackPanel>
```

在第一个 **Stackpanel** 中，每个 **TextBlock** 垂直堆叠在另一个下面。 这是 StackPanel 的默认行为，因此我们无需设置**方向**属性。 在第二个 StackPanel 中，我们希望子元素从左到右水平堆叠，因此我们将**方向**属性设置为“水平”。 我们还必须将 **Grid.ColumnSpan** 属性设置为“2”，以便文本在较低的**边框**上居中。

如果现在运行应用，结果将如下所示。

![添加 StackPanels](images/grid-weather-2.png)

## <a name="step-5-add-an-image-icon"></a>步骤 5：添加图像图标

最后，让我们使用表示今天天气（局部多云）的图像填写**网格**中空的部分。

下载下面的图像，并将其保存为 PNG，名称为“partially-cloudy”。

![局部多云](images/partially-cloudy.PNG)

在“解决方案资源管理器”中，右键单击“资源”文件夹，然后选择“添加” -> “现有项...”，在弹出的浏览器中查找 partially-cloudy.png，选择它，然后单击“添加”。

接下来，在 **MainPage.xaml** 中，添加第 4 步中 StackPanels 下的以下**映像**元素。

```xml
<Image Margin="20" Source="Assets/partially-cloudy.png"/>
```

由于我们需要第一个行和列中的图像，因此不需要设置其 **Grid.Row** 或 **Grid.Column** 属性，允许它们默认为“0”。

就这么简单！ 你已成功创建了一个简单天气应用程序的布局。 如果通过按 **F5** 运行该应用程序，应看到如下内容：

![天气窗格示例](images/grid-weather-3.PNG)

如果愿意，请尝试使用上面的布局，并探索可能代表天气数据的不同方式。

## <a name="related-articles"></a>相关文章
有关设计 UWP 应用布局的简介，请参阅 [UWP 应用设计简介](https://msdn.microsoft.com/en-us/windows/uwp/layout/design-and-ui-intro)

若要了解如何创建适应不同屏幕大小的响应性布局，请参阅[使用 XAML 定义页面布局](https://msdn.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml)


<!--HONumber=Dec16_HO1-->


