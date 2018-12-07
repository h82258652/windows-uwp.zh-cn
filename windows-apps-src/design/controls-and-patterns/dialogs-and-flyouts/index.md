---
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 对话框和浮出控件
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2d5635a41bec716487c08dd57e6ba2ac360649ad
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795349"
---
# <a name="dialogs-and-flyouts"></a>对话框和浮出控件



对话框和浮出控件是当发生需要通知、批准或来自用户的其他信息的操作时出现的瞬态 UI 元素。

> **重要 API**：[ContentDialog 类](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[Flyout 类](/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::row:::
    :::column:::
        **Dialogs**
        
        ![Example of a dialog](../images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](../images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::row-end:::


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

对话框和浮出控件确保用户知道重要信息，但它们也会干扰用户体验。 由于对话框是模式对话框（阻止），因此它们会干扰用户，在用户与该对话框交互前阻止它们执行任何其他操作。 浮出控件提供较和谐的体验，但显示过多的浮出控件可能令人分心。

确定要使用对话框还是浮出控件后，你需要选择使用哪一个。

鉴于对话框会阻止交互而浮出控件不会，因此对话框应专门用于你希望用户放下一切事务专注于特定的部分信息或回答问题的情况。 另一方面，当你希望将注意力吸引到某些内容，但如果用户想要忽略它也无妨时，可以使用浮出控件。

:::row:::
    :::column:::
   <p><b>将对话框用于...</b> <br/>
<ul>
<li>表明用户在继续操作之前<b>必须</b>阅读并确认的重要信息。 示例包括：
<ul>
  <li>当用户的安全可能受到威胁时</li>
  <li>当用户准备永久改变宝贵资产时</li>
  <li>当用户准备删除宝贵资产时</li>
  <li>确认应用内购买</li>
</ul>

</li>
<li>应用于整个应用上下文的错误消息，如连接错误。</li>
<li>问题，在应用需要询问用户阻止问题时（例如当应用不能代表用户进行选择时）。 阻止问题不能忽略或延迟，并且应该向用户提供明确定义的选项。</li>
</ul>
</p>
    :::column-end:::
    :::column:::
   <p><b>将浮出控件用于...</b> <br/>
<ul>
<li>收集在可以完成某个操作前所需的其他信息。</li>
<li>显示仅在一些时间内相关的信息。 例如，在照片库应用中，当用户单击某个图像缩略图时，你可以使用浮出控件显示图像的大型版本。</li>
<li>显示详细信息，例如页面上某个项目的详细信息或较长说明。</li>
</ul></p>
    :::column-end:::
:::row-end:::


## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>若要避免使用对话框和浮出控件的方式

请考虑要共享的信息的重要性：它的重要性是否足以要干扰用户？ 此外，请考虑需要显示该信息的频率；如果你要每几分钟显示一个对话框或通知，你可能希望改为在主 UI 中为此信息分配空间。 例如，在聊天客户端中，你可能显示当前在线的好友列表，并在好友登录时突出显示他们，而不是每次有好友登录时都显示浮出控件。

对话框经常用于在执行某个操作（如删除某个文件）前确认该操作。 如果你预期用户经常执行某个特定操作，请考虑为用户提供在犯错时撤销操作的方法，而不是强迫用户每次都确认该操作。

## <a name="how-to-create-a-dialog"></a>如何创建对话框

请参阅[对话框文章](dialogs.md)。 

## <a name="how-to-create-a-flyout"></a>如何创建浮出控件

请参阅[浮出控件文章](flyouts.md)。 

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

