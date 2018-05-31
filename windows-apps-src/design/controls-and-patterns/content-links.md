---
author: jwmsft
Description: Use content links to embed rich data in your text controls.
title: 文本控件中的内容链接
label: Content links
template: detail.hbs
ms.author: jimwalk
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 9970380b30c45d3dff6020c57b815562c12b322b
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691526"
---
# <a name="content-links-in-text-controls"></a>文本控件中的内容链接

内容链接控件提供了访问文本控件中的嵌入式格式数据的方法，让用户无需离开应用上下文即可查找和使用有关人员或位置的更多信息。

当用户在 RichEditBox 中对某个条目使用与号 (@) 前缀时，它们会显示人员列表和/或与相应条目匹配的位置建议。 例如，当用户选取一个位置时，该位置的 ContentLink 就会插入文本中。 当用户从 RichEditBox 调用内容链接时，就会显示一个浮出控件，该控件具有地图和与位置有关的其他信息。

> **重要 API**：[ContentLink 类](/uwp/api/windows.ui.xaml.documents.contentlink)、[ContentLinkInfo 类](/uwp/api/windows.ui.text.contentlinkinfo)、[RichEditTextRange 类](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> 内容链接 API 分布在以下命名空间中：Windows.UI.Xaml.Controls、Windows.UI.Xaml.Documents 和 Windows.UI.Text。



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>格式文本编辑与内容块控件中的内容链接

有两种不同的方法来使用内容链接：

1. 在 [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) 中，用户可以通过对文本加 @ 符号前缀来打开选取器，从而添加内容链接。 内容链接作为格式文本内容部分存储。
1. 在 [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) 或 [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock) 中，内容链接是用法和行为非常类似 [Hyperlink](/uwp/api/windows.ui.xaml.documents.hyperlink) 的文本元素。

下面是内容链接在 RichEditBox 和 TextBlock 中的默认外观。

![格式文本编辑框中的内容链接](images/content-link-default-richedit.png)
![文本块中的内容链接](images/content-link-default-textblock.png)

下面几节详细介绍在用法、呈现和行为方面的不同。 此表快速比较了 RichEditBox 和文本块中的内容链接之间的主要不同。

| 功能   | RichEditBox | 文本块 |
| --------- | ----------- | ---------- |
| Usage | ContentLinkInfo 实例 | ContentLink 文本元素 |
| Cursor | 由内容链接的类型决定，无法更改 | 由 Cursor 属性决定，默认为 **null** |
| ToolTip | 不会呈现 | 显示辅助文本 |

## <a name="enable-content-links-in-a-richeditbox"></a>在 RichEditBox 中启用内容链接

内容链接的最常见用法是通过在人员或位置名称的文本中加与号 (@) 前缀来让用户快速添加信息。 在 [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) 中启用时，这将打开选取器并允许用户从其联系人列表或附近的位置中（取决于你所启用的方式）插入一个人员。

内容链接可以利用富文本内容保存，你可以提取它以便在应用的其他部分使用。 例如，在电子邮件应用中，可以提取人员信息并用该信息和电子邮件地址填充“收件人”框。

> [!NOTE]
> 内容链接选取器是一个应用，它是 Windows 的一部分，因此它以独立于你的应用的进程运行。

### <a name="content-link-providers"></a>内容链接提供程序

你可以通过向 [RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders) 集合中添加一个或多个内容链接提供程序来在 RichEditBox 中启用内容链接。 XAML 框架中内置有两个内容链接提供程序。

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider) – 使用**人脉**应用查找联系人。
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider) – 使用**地图**应用查找位置。

> [!IMPORTANT]
> RichEditBox.ContentLinkProviders 属性的默认值为 **null**，不是空集合。 在添加内容链接提供程序前，需要显式创建 [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection)。

下面介绍如何在 XAML 中添加内容链接提供程序。

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

你也可以在 Style 中添加内容链接提供程序，然后对多个这样的 RichEditBox 进行应用。

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

下面介绍如何在代码中添加内容链接提供程序。

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>内容链接颜色

内容链接的外观是由其前景、背景和图标决定的。 在 RichEditBox 中，可以设置 [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) 和 [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) 属性来更改默认颜色。 

不能设置光标。 光标由 RichEditbox 根据内容链接的类型呈现 - 人员链接为 [Person](/uwp/api/windows.ui.core.corecursortype) 光标，位置链接为 [Pin](/uwp/api/windows.ui.core.corecursortype) 光标。

### <a name="the-contentlinkinfo-object"></a>ContentLinkInfo 对象

当用户从人员或位置选取器中进行选择时，系统会创建一个 [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo) 对象并将其添加到当前 [RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange) 的 **ContentLinkInfo** 属性中。

ContentLinkInfo 对象包含用于显示、调用和管理内容链接的信息。

- **DisplayText** – 这是当内容链接呈现时显示的字符串。 在 RichEditBox 中，用户可以在内容链接创建后编辑其文本，这会改变此属性的值。
- **SecondaryText** - 此字符串显示在呈现的内容链接的 ToolTip 中。
  - 在选取器创建的 Place 内容链接中，它包含位置的地址（如果可用）。
- **Uri** – 指向有关内容链接主题的更多信息的链接。 此 Uri 可打开已安装的应用或网站。
- **Id** - 这是 RichEditBox 控件创建的基于每个控件的只读计数器。 它用于在操作（如删除或编辑）期间跟踪此 ContentLinkInfo。 如果 ContentLinkInfo 被剪切并重新粘贴到控件中，它将获得一个新 Id。Id 值是递增的。
- **LinkContentKind** – 描述内容链接类型的字符串。 内置内容类型为 _Places_ 和 _Contacts_。 值区分大小写。

#### <a name="link-content-kind"></a>链接内容类型

有几种情况下 LinkContentKind 非常重要。

- 当用户复制一个 RichEditBox 中的内容链接并将其粘贴到另一个 RichEditBox 中时，这两个空间都必须有该内容类型的 ContentLinkProvider。 否则，链接将以文本形式粘贴。
- 你可以在 [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) 事件处理程序中使用 LinkContentKind 来确定在应用的其他部分使用内容链接时如何处理它。 请参阅“示例”部分了解更多信息。
- LinkContentKind 影响链接被调用时系统打开 Uri 的方式。 我们将在接下来关于 Uri 启动的讨论中看到这一点。

#### <a name="uri-launching"></a>Uri 启动

Uri 属性的工作方式与 Hyperlink 的 NavigateUri 属性非常类似。 当用户单击它时，会在默认浏览器或通过 Uri 值为特定协议注册的应用中启动 Uri。

下面介绍这两种内置链接内容类型的具体行为。

##### <a name="places"></a>Places

Places 选取器创建 Uri 根为 https://maps.windows.com/ 的 ContentLinkInfo。 可通过 3 种方式打开此链接：

- 如果 LinkContentKind = "Places"，将在浮出控件中打开信息卡。 浮出控件类似于内容链接选取器浮出控件。 它是 Windows 部件，在独立于应用的进程中运行。
- 如果 LinkContentKind 不是 "Places"，则将打开**地图**应用到指定的位置。 例如，如果你在 ContentLinkChanged 事件处理程序中修改了 LinkContentKind，就会发生这种情况。
- 如果 Uri 在“地图”应用中无法打开，则会在默认浏览器中打开地图。 这通常发生在用户的_网站应用_ 设置不允许使用**地图**应用打开 Uri 的情况下。

##### <a name="people"></a>People

People 选取器创建 Uri 使用 **ms-people** 协议的 ContentLinkInfo。

- 如果 LinkContentKind = "People"，将在浮出控件中打开信息卡。 浮出控件类似于内容链接选取器浮出控件。 它是 Windows 部件，在独立于应用的进程中运行。
- 如果 LinkContentKind 不是 "People"，则将打开**人脉**应用。 例如，如果你在 ContentLinkChanged 事件处理程序中修改了 LinkContentKind，就会发生这种情况。

> [!TIP]
> 有关从你的应用打开其他应用和网站的更多信息，请参阅[使用 URI 启动应用] (/windows/uwp/launch-resume/launch-app-with-uri) 下的主题。

#### <a name="invoked"></a>Invoked

当用户调用内容链接时，会在启动 Uri 的默认行为发生之前引发 [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked) 事件。 你可以处理此事件以替代或取消默认行为。

此示例演示如何替代 Place 内容链接的默认启动行为。 不是在 Place 信息卡、“地图”应用或默认浏览器中打开地图，而是将事件标记为“已处理”并在应用内的 [WebView](/uwp/api/windows.ui.xaml.controls.webview) 控件中打开地图。

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

可以使用 [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) 事件在内容链接发生添加、修改或删除时收到通知。 这将允许你从文本中提取内容链接并在应用中的其他位置使用。 “示例”的接下来部分将对此进行介绍。

[ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs) 属性为：

- **ChangedKind** - 此 ContentLinkChangeKind 枚举是 ContentLink 内的操作。 例如，如果插入、删除或编辑了 ContentLink。
- **Info** - 作为操作目标的 ContentLinkInfo。

对于每个 ContentLinkInfo 操作都会引发此事件。 例如，如果用户一次向 RichEditBox 中复制并粘贴多个内容链接，对于每个添加的项，都会引发此事件。 或者，如果用户同时选择并删除了多个内容链接，对于每个被删除的项，都会引发此事件。

此事件在 RichEditBox 上引发，引发时间在 **TextChanging** 事件之后，**TextChanged** 事件之前。

#### <a name="enumerate-content-links-in-a-richeditbox"></a>枚举 RichEditBox 中的内容链接

你可以使用 RichEditTextRange.ContentLink 属性获取格式文本编辑文档中的内容链接。 TextRangeUnit 枚举具有 ContentLink 值，该值指定当导航文本范围时要作为一个单元使用的内容链接。

此示例显示如何枚举 RichEditBox 中的所有内容链接并将人员提取到一个列表中。

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>ContentLink 文本元素

要在 TextBlock 或 RichTextBlock 控件中使用内容链接，可以使用 [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink) 文本元素（来自 Windows.UI.Xaml.Documents 命名空间）。

文本块中的 ContentLink 的典型来源有：

- 从 RichTextBlock 控件中提取的由选取器创建的内容链接。
- 在代码中创建的位置的内容连接。

对于其他情况，Hyperlink 文本元素通常适用。

### <a name="contentlink-appearance"></a>ContentLink 外观

内容链接的外观是由其前景、背景和光标决定的。 在文本块中，可以设置 Foreground（从 TextElement）和 [Background](/uwp/api/windows.ui.xaml.documents.contentlink.background) 属性来更改默认颜色。

默认情况下，当用户将光标悬停在内容链接上方时会显示 [Hand](/uwp/api/windows.ui.core.corecursortype) 光标。 与 RichEditBlock 不同，文本块控件不会根据链接类型自动更改光标。 你可以设置 [Cursor](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor) 属性来根据链接类型或其他因素更改光标。

下面是 TextBlock 中使用的 ContentLink 示例。 代码中创建了 ContentLinkInfo 并将其分配给在 XAML 中创建的 ContentLink 文本元素的 Info 属性。

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> 在文本控件中使用 ContentLink 而其他文本元素采用 XAML 时，请将内容放置在 [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) 容器中，并对 Span 应用 `xml:space="preserve"` 属性以在 ContentLink 和其他元素之间保留空白区域。

## <a name="examples"></a>示例

在此示例中，用户可在 RickTextBlock 中输入人员或位置内容链接。 你将处理 ContentLinkChanged 事件来提取内容链接，并在人员列表或位置列表中保持它们的最新状态。

在此列表的项模板中，使用具有 ContentLink 文本元素的 TextBlock 来显示内容链接信息。 ListView 为每个项提供它自己的背景，因此 ContentLink 背景设置为 Transparent。

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```