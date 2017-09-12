---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 010663320b4011d853c53bc4121f86a14bfbfe0c
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2017
---
# <a name="custom-keyboard-interactions"></a>自定义键盘交互

在 UWP 应用中提供全面、一致的键盘交互体验，并为键盘高级用户以及残障人士和具有其他辅助功能要求的用户自定义控件。

在本主题中，我们主要的关注点是在电脑上通过 UWP 应用的自定义控件支持键盘输入。 但是，设计出色的键盘体验对于支持辅助功能工具（如 Windows 讲述人）非常重要，它们使用软件键盘，例如触摸键盘和屏幕键盘 (OSK)。

## <a name="providing-2d-directional-inner-navigation-a-namexyfocuskeyboardnavigation"></a>提供 2D 方向内部导航 <a name="xyfocuskeyboardnavigation">

使用 **XYFocusKeyboardNavigation** 属性来支持具有键盘（箭头键）、Xbox 游戏板（方向键和左摇杆按钮）和 Xbox 遥控器（方向键）的自定义控件和控件组的 2D 方向内部导航。

**注意**我们将控件或控件组的内部导航区域称为*方向区域*。

**XYFocusKeyboardNavigation** 的值为 **XYFocusKeyboardNavigationMode** 类型，可能的值如下：**自动**（默认）、**已启用**或**已禁用**。

此属性不会影响 Tab 键导航，它只会影响控件或控件组内的子元素的内部导航。 方向区域内的子元素不应包括在 Tab 导航中。

### <a name="default-behavior"></a>默认行为

方向导航行为基于元素的祖先，或者说是继承层次结构。 如果所有祖先均处于默认模式或设置为**自动**，则键盘不支持方向导航行为（游戏板和遥控器始终支持方向导航，除非已显式设为**已禁用**）。

### <a name="custom-behavior"></a>自定义行为

将此属性设置为**已启用**后，你的控件将通过控件内的每个 UI 元素支持 2D 内部导航（使用键盘箭头键）。

使用键盘方向键时，导航限制在方向区域（按 Tab 键会将焦点设置到方向区域外的下一个可聚焦元素）。

**注意**使用游戏板或遥控器时不存在这种情况，在这种情况下，导航将继续到方向区域外的下一个可聚焦控件。

此属性只影响通过箭头键的内部导航，不会影响 Tab 键导航。 所有控件将保持其预期的 Tab 顺序层次结构。

下图所示为方向区域内所含的三个按钮（A、B 和 C）和方向区域外的第四个按钮 (D)。

![方向区域](images/keyboard/directional-area.png)

*键盘箭头键可以在按钮 A-B-C 之间移动焦点，但不能将焦点移到 D 上*

下面的代码示例显示了启用 **XYFocusKeyboardNavigation** 时导航会有何影响。 以上一个图像为例，A 具有初始焦点，并且 Tab 键会在所有控件之间循环（A -&gt; B -&gt; C -&gt; D 并返回到 A），而箭头键将约束在方向区域内。

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" />
      </StackPanel>
      <Button Content="D" />
</StackPanel>
```

#### <a name="override-directional-navigation"></a>覆盖方向导航

使用 XYFocusRight/XYFocusLeft/XYFocusTop/XYFocusDown 属性覆盖默认导航行为。

下图与上述示例相同，它显示了方向区域内所含的三个按钮（A、B 和 C）和方向区域外的第四个按钮 (D)。

![方向区域](images/keyboard/directional-area.png)

*键盘箭头键可在按钮 A-B-C 之间移动焦点，并且可将焦点移出到 D*

此代码示例演示如何通过允许右箭头键导航到方向区域外的控件来覆盖该键的默认导航行为。 请注意，无法使用左箭头键重新进入方向区域。

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" XYFocusRight="{x:Bind ButtonD}" />
      </StackPanel>
      <Button Content="D" x:Name="ButtonD"/>
</StackPanel>
```

有关详细信息，请参阅本主题后面的 [XYFocus 导航策略](#set-the-tab-navigation-behavior)。

#### <a name="restrict-navigation-with-disabled"></a>通过设为“已禁用”来限制导航

将 **XYFocusKeyboardNavigation** 设置为**已禁用**以将箭头键导航限制在方向区域内。

**注意**设置此属性确实会影响控件自身的键盘导航，而这仅限于控件的子元素。

在以下代码示例中，父元素 StackPanel 将  **XYFocusKeyboardNavigation** 设置为**已启用**，并且子元素 C 将 **XYFocusKeyboardNavigation** 设置为**已禁用**。 仅子元素 C 禁用了箭头键导航。

```XAML
<StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
        <Button Content="A" />
        <Button Content="B" />
        <Button Content="C" XYFocusKeyboardNavigation="Disabled" >
            ...
        </Button>
</StackPanel>
```

#### <a name="use-nested-directional-areas"></a>使用嵌套方向区域

你可以有多个级别的嵌套方向区域。 如果所有父元素都将 **XYFocusKeyboardNavigation** 设置为**已启用**，则箭头键导航将忽略区域边界。

下图显示了嵌套方向区域内所含的三个按钮和方向区域外的第四个按钮 (D)。

![嵌套方向区域](images/keyboard/nested-directional-area.png)

*键盘箭头键可以在按钮 A-B-C 之间移动焦点，但不能将焦点移到 D 上*

此代码示例演示如何将嵌套方向区域指定为支持跨区域边界的箭头键导航。

```XAML
<StackPanel Orientation="Horizontal">
        <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
            <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
                <Button Content="A" />
                <Button Content="B" />
            </StackPanel>
            <Button Content="C" />
        </StackPanel>
        <Button Content="D" />
 </StackPanel>
```

下图显示了四个按钮（A、B、C 和 D），其中 A 和 B 包含在嵌套方向区域内，而 C 和 D 则包含在不同的区域内。 由于父元素将 **XYFocusKeyboardNavigation** 设置为**已禁用**，因此无法使用箭头键跨越每个嵌套区域的边界。

![无方向区域](images/keyboard/non-directional-area.png)

*键盘箭头键可在按钮 A-B 和按钮 C-D 之间移动焦点，但不能在这两个区域之间移动*

此代码示例演示如何将嵌套方向区域指定为不支持跨区域边界的箭头键导航。

```XAML
<StackPanel Orientation="Horizontal">
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="A" />
    <Button Content="B" />
  </StackPanel>
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="C" />
    <Button Content="D" />
  </StackPanel>
</StackPanel>
```

以下是嵌套方向区域的较复杂示例，其中：

-   如果 A 具有焦点，则只能导航至 E（反之亦然），因为无方向区域边界使得无法通过箭头键来访问 B、C 和 D
-   如果 B 具有焦点，则只能导航至 C（反之亦然），因为 D 位于方向区域外，而无方向区域边界会阻止访问 A 和 E
-   如果 D 具有焦点，则必须使用 Tab 键在控件之间进行导航，因为无法进行箭头键导航

![嵌套无方向区域](images/keyboard/nested-non-directional-area.png)

*键盘箭头键可在按钮 A-E 和按钮 B-C 之间移动焦点，但不能在其他区域之间移动*

此代码示例演示如何将嵌套方向区域指定为支持跨区域边界的复杂箭头键导航。

```XAML
<StackPanel  Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
  <Button Content="A" />
    <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Disabled">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
        <Button Content="B" />
        <Button Content="C" />
      </StackPanel>
        <Button Content="D" />
    </StackPanel>
  <Button Content="E" />
</StackPanel>
```

## <a name="set-the-tab-navigation-behavior-a-nametab-navigation"></a>设置 Tab 键导航行为 <a name="tab-navigation">

UIElement.[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) 属性指定了整个对象树（或方向区域）的 Tab 键导航行为。

[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) 的值类型为 **TabFocusNavigationMode**，可能的值包括**一次**、**循环**或**局部**（默认）。

在下图中，根据方向区域的 Tab 键导航，系统通过以下方式移动焦点：

-   一次：A、B1、C、A
-   局部：A、B1、B2、B3、B4、B5、C、A
-   循环：A、B1、B2、B3、B4、B5、B1、B2、B3、B4、B5、（按 B 循环）

![Tab 键导航](images/keyboard/tab-navigation.png)

*基于 Tab 键导航模式的焦点行为*

以下代码示例演示如何使用模式为“一次”的 TabFocusNavigation。

```XAML
<Button Content="X" Click="OnAClick"/>
<StackPanel Orientation="Horizontal" XYFocusNavigation ="Local"
   TabFocusNavigation ="Local">
   <Button Content="A" Click="OnAClick"/>
   <StackPanel Orientation="Horizontal" TabFocusNavigation ="Once">
        <Button Content="B" Click="OnBClick"/>
        <Button Content="C" Click="OnCClick"/>
        <Button Content="D" Click="OnDClick"/>
   </StackPanel>
   <Button Content="E" Click="OnBClick"/>
</StackPanel>
```

*当焦点位于 X 时的 Tab 键导航为：A、B、E、X*

#### <a name="about-tabfocusnavigation-and-tabindex"></a>关于 TabFocusNavigation 和 TabIndex

UIElement.TabFocusNavigation 属性与 Control.TabNavigation 属性具有相同的行为，包括与 TabIndex 配合使用的方式。

如果某个控件未指定 TabIndex，则框架将为其分配一个高于当前最高索引值（和最低优先级）的索引值。 通过选择可视化树上的第一个元素，它将解析 ambiguate。 框架将根据范围解析 Tab 键索引。 控件的子元素视为一个范围，如果此子元素之一具有子元素，则视为另一个范围的一部分。

在下图中，根据方向区域的 Tab 键导航和元素的 Tab 键索引，系统通过以下方式移动焦点：

-   一次：A、B3、C、A。
-   局部：A、B3、B4、B5、B1、B2、C、A。
-   循环：A、B3、B4、B5、B1、B2、B3、B4、B5、B1、B2、（按 B 循环）

![Tab 键索引](images/keyboard/tab-index.png)

*基于 Tab 键导航模式和 Tab 键索引的焦点行为*

注意如何将方向区域视为一个范围以及焦点导航如何先移动到具有最高优先级的控件：B3。 事实上，有两个范围：一个用于 A、方向区域和 C，另一个范围用于方向区域。 由于方向区域不是 TabStop，框架会切换范围以查找最佳的候选项，然后以递归方式在任何子元素之间循环。
