---
Description: 使用数据模板选择器根据项属性自定义项的样式。
title: 数据模板选择
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 956ac13dcdc1a2e6367e590bb8885c8722f41e2c
ms.sourcegitcommit: cb7f80100c99d4b6466a819bea191006ec3d616c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2019
ms.locfileid: "73640874"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>数据模板选择：根据项属性设置项样式

集合控件的自定义设计由 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 管理。 数据模板定义每个项的布局和样式，并将标记应用于集合中的每个项。 本文介绍如何使用 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 在集合上应用不同的数据模板，并根据所选的特定项属性或值选择要使用的数据模板。

> **重要的 API**：[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)、[DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 是启用自定义模板选择逻辑的类。 它可让你定义规则，以指定要用于集合中特定项的数据模板。 若要实现此逻辑，请在代码隐藏中创建 DataTemplateSelector 的子类，并定义逻辑，以确定要用于特定项类别的数据模板（例如，特定类型的项或具有特定属性值的项等）。 在 XAML 文件的 Resources 节中声明此类的实例，以及要使用的数据模板的定义。 使用 `x:Key` 值标识这些资源，从而使你可以在 XAML 中引用它们。

## <a name="prerequisites"></a>必备条件

- 如何[使用和创建集合控件，如 ListView 或 GridView。](listview-and-gridview.md)
- 如何[使用 DataTemplate 自定义项的外观](item-containers-templates.md#data-template)。

## <a name="when-not-to-use-a-datatemplateselector"></a>何时不使用 DataTemplateSelector

通常，不应为 ListView 或 GridView 中的每个项都指定完全不同的布局/样式，这是对 DataTemplateSelector 的不当使用，且会对性能产生负面影响。

通过绑定某些属性，可以仅使用一个数据模板来控制列表项视觉对象显示的某些元素。 例如，通过绑定到数据模板中的图标源属性，并为每个项赋予不同的图标源属性值，每个项都可以具有不同的图标。 与使用 DataTemplateSelector 相比，这将获得更好的性能。

## <a name="when-to-use-a-datatemplateselector"></a>何时使用 DataTemplateSelector

若要在一个集合控件中使用多个数据模板，则应创建一个 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)。 `DataTemplateSelector` 使你可以灵活地使某些项突出显示，同时仍将项保持在相似的布局中。 在许多用例中，DataTemplateSelector 很有帮助，而在某些情况下，最好重新考虑所使用的控件和策略。

集合控件通常绑定到所有项均为一种类型的集合。 但是，即使项可能为同一类型，它们也可能对某些属性具有不同的值，或表示不同的含义。 某些项也可能比其他项更重要，或者某个项可能特别重要或不同，且需要直观地突出显示。在这些情况下，DataTemplateSelector 将很有帮助。

此外，还可以绑定到包含不同类型的项的集合 - 绑定集合可以混合使用字符串、整数、自定义类对象等。 这使得 DataTemplateSelector 尤其有用，因为它可以根据项的对象类型分配不同的数据模板。

以下是何时可以使用数据模板选择器的一些示例：

- 在 ListView 内表示不同级别的员工 - 每种类型/级别的员工可能需要不同的颜色背景以轻松区分。
- 在使用 GridView 的产品库中表示促销商品 - 某个促销商品可能需要红色背景或不同的颜色字体，以使其较正价商品更加醒目。
- 在使用 FlipView 的图片库中表示获奖者/最佳照片。
- 需要在 ListView 中以不同方式表示负数/正数，或短字符串/长字符串。

## <a name="create-a-datatemplateselector"></a>创建 DataTemplateSelector

创建数据模板选择器后，请在代码中定义模板选择逻辑，并在 XAML 中定义数据模板。

### <a name="code-behind-component"></a>代码隐藏组件

若要使用数据模板选择器，请首先在代码隐藏中创建 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 的子类（从其派生的类）。 在类中，将每个模板声明为类的一个属性。 然后，替代 [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) 方法以加入自己的模板选择逻辑。

下面是名为 `MyDataTemplateSelector` 的简单 `DataTemplateSelector` 子类的示例。

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

`MyDataTemplateSelector` 类派生自 `DataTemplateSelector` 类，并首先定义两个 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 对象：`Normal` 和 `Accent`。 这些暂时是空声明，但将在 XAML 文件中使用标记“填充”。

`SelectTemplateCore` 方法采用项对象（即每个集合项），并使用在特定情况下 `DataTemplate` 要返回的特定规则进行替代。 在这种情况下，如果项为奇数，则它将接收 `Accent` 数据模板，如果为偶数，则接收 `Normal` 数据模板。

### <a name="xaml-component"></a>XAML 组件

其次，必须在 XAML 文件的 Resources 节中创建此新 `MyDataTemplateSelector` 类的实例。 所有资源都需要 `x:Key`，使用它以将其绑定到集合控件的 `ItemTemplateSelector` 属性（在后续步骤中）。 此外，还要创建两个 `DataTemplate` 对象的实例，并在 Resources 节中定义其布局。 将这些数据模板分配给在 `MyDataTemplateSelector` 类中声明了的 `Accent` 和 `Normal` 属性。

以下是所需 XAML 资源和标记的示例：

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

如上所示，定义了两个数据模板 `Normal` 和 `Accent` - 它们都将项显示为按钮，但是，`Accent` 数据模板针对背景使用主题色画笔，而 `Normal` 数据模板使用灰色画笔 (`SystemChromeLowColor`)。 然后，将这两个数据模板分配给 `Normal` 和 `Accent` DataTemplate 对象，这些对象是在 C# 代码隐藏中创建的 MyDataTemplateSelector 类的属性。

最后一步是将 `DataTemplateSelector` 绑定到集合控件（在本例中为 ListView）的 `ItemTemplateSelector` 属性。 从而无需为 `ItemTemplate` 属性赋值。 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

代码编译完成后，每个集合项都将通过 `MyDataTemplateSelector` 中已替代的 `SelectTemplateCore` 方法运行，并将呈现为相应的 DataTemplate。

> [!IMPORTANT]
> 将 `DataTemplateSelector` 与 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2) 结合使用时，可将 `DataTemplateSelector` 绑定到 `ItemTemplate` 属性。 `ItemsRepeater` 没有 `ItemTemplateSelector` 属性。

## <a name="datatemplateselector-performance-considerations"></a>DataTemplateSelector 性能注意事项

将 ListView 或 GridView 与大型数据集合结合使用时，滚动和平移性能可能是一个问题。 为保持大型集合的良好运行，可采取一些步骤来提高数据模板的性能。 [ListView 和 GridView UI 优化](/windows/uwp/debug-test-perf/optimize-gridview-and-listview)中更详细地介绍了这些内容。

-  按项目减少元素 - 将数据模板中的 UI 元素数保持在合理的最小值。
- 带有异类集锦的容器循环
  - 使用 ChoosingItemContainer 事件  - 此事件是一种针对不同项使用不同数据模板的高性能方法。 若要获得最佳性能，应优化缓存并为特定数据选择数据模板。
  - 使用项模板选择器  - 在某些实例中，应避免使用项模板选择器 (`DataTemplateSelector`)，因为它会影响性能。
