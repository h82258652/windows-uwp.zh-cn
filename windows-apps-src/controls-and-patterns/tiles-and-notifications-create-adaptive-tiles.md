---
author: mijacobs
Description: "自适应磁贴模板是 Windows 10 中的一项新功能，允许你使用可适应不同屏幕密度的简单而灵活的标记语言来设计你自己的磁贴通知内容。"
title: "创建自适应磁贴"
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b80772109f0349f23feb6ff7f7440ab2e9242288
ms.sourcegitcommit: 9a1310468970c8d1ade0fb200126dff56ea8c9e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2017
---
# <a name="create-adaptive-tiles"></a>创建自适应磁贴

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


自适应磁贴模板是 Windows 10 中的一项新功能，允许你使用可适应不同屏幕密度的简单而灵活的标记语言来设计你自己的磁贴通知内容。 本文介绍了如何为通用 Windows 平台 (UWP) 应用创建自适应动态磁贴。 有关自适应元素和属性的完整列表，请参阅[自适应磁贴架构](tiles-and-notifications-adaptive-tiles-schema.md)。

（如果你愿意，你仍然可以在为 Windows 10 设计通知时使用 [Windows 8 磁贴模板目录](https://msdn.microsoft.com/library/windows/apps/hh761491)中的预设模板。）


## <a name="getting-started"></a>入门

**安装通知库。** 如果希望使用 C# 而不是 XML 来生成通知，请安装名为 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 程序包（搜索“notifications uwp”）。 本文中提供的 C# 示例使用 NuGet 程序包的版本 1.0.0。

**安装通知可视化工具。** 此免费 UWP 应用通过在你编辑时提供磁贴的即时可视预览来帮助你设计自适应动态磁贴，类似于 Visual Studio 的 XAML 编辑器/设计视图。 你可以阅读[此博客文章](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)获取详细信息，并且可以在[此处](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)下载通知可视化工具。


## <a name="how-to-send-a-tile-notification"></a>如何发送磁贴通知

请阅读我们的[发送本地磁贴通知的快速入门](tiles-and-notifications-sending-a-local-tile-notification.md)。 在此页面上的文档介绍使用自适应磁贴时所具有的所有视觉 UI 可能性。


## <a name="usage-guidance"></a>用法指南


自适应模板设计用于跨不同的外形规格和通知类型工作。 组和子组等元素将内容链接在一起，并且不会对其本身暗示特定的可视行为。 通知的最终外观应基于将显示它的特定设备，无论它是手机、平板电脑、台式机还是其他设备。

提示是可选属性，可添加到元素以实现特定的可视行为。 提示可以特定于设备，也可以特定于通知。

## <a name="a-basic-example"></a>基本示例


此示例演示自适应磁贴模板可以生成的内容。

```XML
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Jennifer Parker",
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Photos from our trip",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**结果：**

![快速示例磁贴](images/adaptive-tiles-quicksample.png)

## <a name="tile-sizes"></a>磁贴大小


每种磁贴大小的内容在 XML 负载内的单独 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素中单独指定。 通过将模板属性设置为以下值之一，选择目标大小：

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge（仅适用于桌面）

对于单个磁贴通知 XML 负载，请为你希望支持的每种磁贴大小提供 &lt;binding&gt; 元素，如此示例中所示：

```XML
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Large" }
                }
            }
        }
    }
};
```

**结果：**

![自适应磁贴大小：小、中、宽和大](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>品牌


你可以控制动态磁贴底部的品牌（显示名称和角徽标），方法是使用通知负载上的品牌属性。 你可以选择显示“无”、仅“名称”、仅“徽标”，或使用“nameAndLogo”显示两者。

**注意** Windows Mobile 不支持角徽标，因此“徽标”和“nameAndLogo”在 Mobile 上默认为“名称”。

 

```XML
<visual branding="logo">
  ...
</visual>
```

```CSharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**结果：**

![自适应磁贴、名称和磁贴](images/adaptive-tiles-namelogo.png)

可以使用以下两种方法之一为特定磁贴大小应用品牌：

1. 通过应用 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上的属性
2. 通过应用 [&lt;visual&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上的属性（这将影响整个通知负载），如果你没有为绑定指定品牌，它将使用可视元素上提供的品牌。

```XML
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,

        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },

        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**默认品牌结果：**

![磁贴上的默认品牌](images/adaptive-tiles-defaultbranding.png)

如果你未在通知负载中指定品牌，基本磁贴的属性将决定品牌。 如果基本磁贴显示了显示名称，则品牌将默认为“名称”。 否则，如果未显示显示名称，品牌将默认为“无”。

**注意** 这与 Windows 8.x 不同，在该版本中默认品牌为“徽标”。

 

## <a name="display-name"></a>显示名称


你可以通过输入你使用 **displayName** 属性选择的文本字符串来替代通知的显示名称。 和品牌一样，你可以在 [&lt;visual&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上指定它，这会影响整个通知负载，或者可以在 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上指定它，这仅影响个别磁贴。

**已知问题** 在 Windows Mobile 上，如果为磁贴指定一个 ShortName，将不使用通知中提供的显示名称（将始终显示 ShortName）。 

```XML
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",

        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },

        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**结果：**

![自适应磁贴显示名称](images/adaptive-tiles-displayname.png)

## <a name="text"></a>文本


[&lt;text&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素用于显示文本。 你可以使用提示来修改文本显示方式。

```XML
<text>This is a line of text</text>
```


```CSharp
new AdaptiveText()
{
    Text = "This is a line of text"
};
```

**结果：**

![自适应磁贴文本](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>文字环绕


默认情况下，文本不会换行，并将在磁贴边缘之外继续。 使用 **hint-wrap** 属性在文本元素上设置文本换行。 你还可以通过使用 **hint-minLines** 和 **hint-maxLines**（两者都接受正整数）来控制最小和最大行数。

```XML
<text hint-wrap="true">This is a line of wrapping text</text>
```


```CSharp
new AdaptiveText()
{
    Text = "This is a line of wrapping text",
    HintWrap = true
};
```

**结果：**

![带有文字环绕的自适应磁贴](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>文本样式


样式控制文本元素的字体大小、颜色和粗细。 有多种可用样式，包括每个样式的将不透明度设置为 60% 的“微妙”变体，这通常会使文本带有淡灰色阴影。

```XML
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```CSharp
new AdaptiveText()
{
    Text = "Header content",
    HintStyle = AdaptiveTextStyle.Base
},

new AdaptiveText()
{
    Text = "Subheader content",
    HintStyle = AdaptiveTextStyle.CaptionSubtle
}
```

**结果：**

![自适应磁贴文本样式](images/adaptive-tiles-textstyles.png)

**注意** 如果未指定提示样式，该样式将默认为描述文字。

 

**基本文本样式**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;text hint-style="\*" /&gt; | 字体高度               | 字体粗细 |
| 描述文字                        | 12 个有效像素 (epx) | 常规     |
| 正文                           | 15 epx                    | 常规     |
| 基本                           | 15 epx                    | 半粗    |
| 副标题                       | 20 epx                    | 常规     |
| 标题                          | 24 epx                    | 半细   |
| 副标题                      | 34 epx                    | 细体       |
| 标题                         | 46 epx                    | 细体       |

 

**数字文本样式变体**

这些变体减少了行高度，因此上方和下方的内容距离文本更近。

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**标题文本样式变体**

每种样式都有为文本提供 60% 不透明度的标题变体，这通常会使文本颜色带有淡灰色阴影。

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## <a name="text-alignment"></a>文本对齐


文本可以在水平方向上左对齐、居中对齐或右对齐。 在从左到右的语言（如英语）中，文本默认为左对齐。 在从右到左的语言（如阿拉伯语）中，文本默认为右对齐。 你可以在元素上使用 **hint-align** 属性来手动设置对齐方式。

```XML
<text hint-align="center">Hello</text>
```


```CSharp
new AdaptiveText()
{
    Text = "Hello",
    HintAlign = AdaptiveTextAlign.Center
};
```

**结果：**

![自适应磁贴文本对齐](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>组和子组


组允许你在语义上声明组内的内容有关联，并且必须完整显示才能使内容有意义。 例如，你可能有两个文本元素（一个标题和一个副标题），如果仅显示标题，则该内容没有意义。 通过将这些元素分组在一个子组中，元素将全部显示（如果它们可以容纳）或完全不显示（因为它们无法容纳）。

若要在各个设备和屏幕上提供最佳体验，请提供多个组。 如果有多个组，你的磁贴将可以适应更大的屏幕。

**注意** 组的唯一有效子级是子组。

 

```XML
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
```

```CSharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),

            // For spacing
            new AdaptiveText(),

            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}

...

private static AdaptiveGroup CreateGroup(string from, string subject, string body)
{
    return new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**结果：**

![自适应磁贴组和子组](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>子组（列）


子组还允许你将数据划分为组内的语义式部分。 对于动态磁贴，这在视觉上转换为列。

**hint-weight** 属性允许你控制列的宽度。 **hint-weight** 的值表示为可用空间的权重比例，这等同于 **GridUnitType.Star** 行为。 对于等宽的列，将每个权重分配为 1。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">提示权重</td>
<td align="left">宽度百分比</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">总权重：4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![子组，甚至列](images/adaptive-tiles-subgroups01.png)

若要使一个列的大小是另一个列的两倍，则向较小的列分配的权重为 1，向较大的列分配的权重为 2。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">提示权重</td>
<td align="left">宽度百分比</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33.3%</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66.7%</td>
</tr>
<tr class="even">
<td align="left">总权重：3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![子组，一个列的大小是另一个列的两倍](images/adaptive-tiles-subgroups02.png)

如果你希望你的第一列占总宽度的 20%，你的第二列占总宽度的 80%，请将第一个权重分配为 20，第二个权重分配为 80。 如果你的总权重等于 100，它们将以百分比形式操作。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">提示权重</td>
<td align="left">宽度百分比</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20%</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80%</td>
</tr>
<tr class="even">
<td align="left">总权重：100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![子组，总权重为 100](images/adaptive-tiles-subgroups03.png)

**注意** 两列之间将自动添加 8 个像素的边距。

 

当你拥有两个以上的子组时，则应指定 **hint-weight**，它仅接受正整数。 如果你未为第一个子组指定提示权重，将为其分配的权重为 50。 将为下一个没有特定提示权重的子组分配的权重等于 100 减去之前的权重总和，或如果该结果为零，则分配的权重为 1。 将为剩余没有指定提示权重的子组分配的权重为 1。

下面是一个天气磁贴的示例代码，演示你可以如何实现一个带有宽度相等的五个列的磁贴：

```XML
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
```

```CSharp
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**结果：**

![天气磁贴的示例](images/adaptive-tiles-weathertile.png)

## <a name="images"></a>图像


&lt;image&gt; 元素用于在磁贴通知上显示图像。 图像可以嵌入到磁贴内容中（默认）、作为内容背后的背景图像，或作为从通知顶部以动画形式进入的速览图像。

**注意** 存在[对图像的文件大小和尺寸的限制](https://msdn.microsoft.com/library/windows/apps/hh781198)。

 

在未指定任何额外行为的情况下，图像将均匀收缩或展开以填充可用的宽度。 以下示例显示一个使用两列和嵌入式图像的磁贴。 嵌入式图像可拉伸以填充列的宽度。

```XML
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```CSharp
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**结果：**

![图像示例](images/adaptive-tiles-images01.png)

放置在 &lt;binding&gt; 根或第一个组中的图像还可拉伸以适应可用的高度。

### <a name="image-alignment"></a>图像对齐

可以使用 **hint-align** 属性将图像设置为左对齐、居中对齐或右对齐。 这还会导致图像以其原始分辨率显示，而不是拉伸以填充宽度。

```XML
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
```

```CSharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveImage()
            {
                Source = "Assets/fable.jpg",
                HintAlign = AdaptiveImageAlign.Center
            }
        }
    }
}
```

**结果：**

![图像对齐示例（左对齐、居中对齐、右对齐）](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>图像边距

默认情况下，嵌入式图像在与图像上方或图像下方的任何内容之间都有 8 个像素的边距。 可以在图像上使用 **hint-removeMargin** 属性来删除此边距。 但是，图像始终与磁贴边缘保留 8 个像素的边距，并且子组（列）始终在列之间保留 8 个像素的填充。

```XML
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```CSharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Numbers/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

![提示删除边距示例](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>图像裁剪

可以使用 **hint-crop** 属性将图像裁剪为圆形，该属性当前仅支持值“none”（默认值）或“circle”。

```XML
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
```

```CSharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    new AdaptiveSubgroup() { HintWeight = 1 },
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 2,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },
                    new AdaptiveSubgroup() { HintWeight = 1 }
                }
            },
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Title,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.SubtitleSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

**结果：**

![图像裁剪示例](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>背景图像

若要设置背景图像，请将图像元素放置在 &lt;binding&gt; 的根中，并将放置属性设置为“background”。

```XML
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
```

```CSharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg"
        },

        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**结果：**

![背景图像示例](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>速览图像

你可以指定从磁贴顶部“速览”的图像。 速览图像使用动画从磁贴顶部向下/向上滑动、速览到视图中，然后再向外滑动以在磁贴上显示主要内容。 若要设置速览图像，请将图像元素放置在 &lt;binding&gt; 的根中，并将放置属性设置为“peek”。

```XML
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
```

```CSharp
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Apps/Hipstame/hipster.jpg"
        },
        Children =
        {
            new AdaptiveText()
            {
                Text = "New Message"
            },
            new AdaptiveText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintWrap = true
            }
        }
    }
}
```

![速览图像的示例](images/adaptive-tiles-imagepeeking.png)

**用于速览和背景图像的圆裁剪**

将提示裁剪属性用于速览和背景图像，以执行圆裁剪：

```XML
<image placement="peek" hint-crop="circle" src="Assets/Apps/Hipstame/hipster.jpg"/>
```

```CSharp
new TilePeekImage()
{
    HintCrop = TilePeekImageCrop.Circle,
    Source = "Assets/Apps/Hipstame/hipster.jpg"
}
```

结果如下所示：

![用于速览和背景图像的圆裁剪](images/circlecrop-image.png)

**使用速览和背景图像**

若要在磁贴通知上使用速览和背景图像，请在你的通知负载中指定速览图像和背景图像。

结果如下所示：

![结合使用的速览和背景图像](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>速览和背景图像覆盖

可以使用 **hint-overlay** 在背景和速览图像上设置黑色覆盖，该属性接受从 0 到 100 的整数，其中 0 表示无覆盖，100 表示全黑覆盖。 你可以使用覆盖来帮助确保磁贴上的文本是可读。

**在背景图像上使用提示覆盖**

背景图像将默认为 20%覆盖，只要在负载中具有一些文本元素（否则它将默认为 0% 覆盖）。

```XML
<binding template="TileWide">
  <image placement="background" hint-overlay="60" src="Assets\Mostly Cloudy-Background.jpg"/>
  ...
</binding>
```

```CSharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg",
            HintOverlay = 60
        },

        ...
    }
}
```

**提示覆盖结果：**

![图像提示覆盖的示例](images/adaptive-tiles-image-hintoverlay.png)

**在速览图像上使用提示覆盖**

在 Windows 10 版本 1511 中，我们也支持速览图像的覆盖，就像背景图像一样。 将速览图像元素上的提示覆盖指定为从 0 到 100 的整数。 速览图像的默认覆盖为 0（无覆盖）。

```XML
<binding template="TileMedium">
  <image hint-overlay="20" src="Assets\Map.jpg" placement="peek"/>
  ...
</binding>
```

```CSharp
TileMedium = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Map.jpg",
            HintOverlay = 20
        },
        ...
    }
}
```

本示例以 20% 不透明度（左）和 0% 不透明度（右）显示速览图像：

![速览图像上的提示覆盖](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>垂直对齐方式（文本堆叠）


你可以控制磁贴上的内容的垂直对齐方式，方法是在 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素和 [&lt;subgroup&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上使用 **hint-textStacking** 属性。 默认情况下，所有内容都垂直向顶部对齐，但你还可以使内容向底部或中心对齐。

### <a name="text-stacking-on-binding-element"></a>绑定元素上的文本堆叠

当在 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 级别上应用时，文本堆叠设置通知内容（作为一个整体）的垂直对齐方式，在品牌/锁屏提醒区域上方的可用垂直空间中对齐。

```XML
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
```

```CSharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Base,
                HintAlign = AdaptiveTextAlign.Center
            },

            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

![绑定元素上的文本堆叠](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>子组元素上的文本堆叠

当在 [&lt;subgroup&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 级别上应用时，文本堆叠设置子组（列）内容的垂直对齐方式，在整个组内的可用垂直空间中对齐。

```XML
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
```

```CSharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    // Image column
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 33,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },

                    // Text column
                    new AdaptiveSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
                        Children =
                        {
                            new AdaptiveText()
                            {
                                Text = "Hi,",
                                HintStyle = AdaptiveTextStyle.Subtitle
                            },

                            new AdaptiveText()
                            {
                                Text = "MasterHip",
                                HintStyle = AdaptiveTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## <a name="related-topics"></a>相关主题


* [自适应磁贴架构](tiles-and-notifications-adaptive-tiles-schema.md)
* [快速入门：发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)
* [GitHub 上的通知库](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)
* [特殊磁贴模板目录](tiles-and-notifications-special-tile-templates-catalog.md)
 

 




