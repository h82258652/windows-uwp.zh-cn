---
Description: This article describes best practices for creating and displaying app settings.
title: 应用设置指南
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a31464d208e8d9abe68703560552c99e5d957463
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9049132"
---
# <a name="guidelines-for-app-settings"></a>应用设置指南



应用设置是应用的用户可自定义部分，并位于应用设置页面内。 例如，新闻阅读器应用中的应用设置可以让用户指定要显示的新闻源或屏幕上要显示的列数，而天气应用的设置则可以让用户在摄氏度和华氏度之间选择默认的测量单位。 本文将介绍有关创建和显示应用设置的最佳做法。


## <a name="should-i-include-a-settings-page-in-my-app"></a>应用中应包含设置页面吗？

以下是属于应用设置页面的应用选项的示例：

-   影响应用的行为并且不需要频繁调整的配置选项，例如在天气应用中在摄氏度或华氏度之间选择默认的温度单位，更改邮件应用的帐户设置、通知设置或辅助选项。
-   取决于用户首选项的选项，如音乐、音效或颜色主题。
-   不经常访问的应用信息，例如隐私策略、帮助、应用版本或版权信息。

包含在典型应用工作流中的命令（例如在艺术应用中更改画笔大小）不应该在设置页面上。 若要了解有关命令放置的详细信息，请参阅 [命令设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958433)。

## <a name="general-recommendations"></a>常规建议


-   保持设置页面简洁，并使用二进制（开/关）控件。 通常，[切换开关](../controls-and-patterns/toggles.md)是二进制设置的最佳控件。
-   对于让用户从最多 5 个互相排斥的一组相关选项中选择一项的设置，请使用[单选按钮](../controls-and-patterns/radio-button.md)。
-   在应用设置页面上为所有应用设置创建入口点。
-   使设置保持简单。 定义智能默认值，并使设置数保持最少。
-   当用户更改设置时，应用应当立即反映所做的更改。
-   不要包括属于常见应用工作流的命令。

## <a name="entry-point"></a>入口点


用户进入应用设置页面的方式应取决于应用的布局。

**导航窗格**

对于导航窗格布局，应用设置应为导航选项列表中最后一项，并且应固定到底部：

![导航窗格的应用设置入口点](images/appsettings-entrypoint-navpane.png)

**应用栏**

如果使用[应用栏](../controls-and-patterns/app-bars.md)或工具栏，请将设置入口点作为最后一个项目放在“更多”溢出菜单中。 如果使用户更容易发现设置入口点对应用来说很重要，请将入口点直接放在应用栏上，而不是放在溢出菜单中。

![应用栏的应用设置入口点](images/appsettings-entrypoint-tabs.png)

**中心**

如果你要使用中心布局，应用设置的入口点应放置在应用栏的“更多”溢出菜单中。

**选项卡/透视表**

对于表或透视表布局，我们不推荐将应用设置入口点作为顶部项之一放在导航中。 相反，应用设置的入口点应放置在应用栏的“更多”溢出菜单中。

**大纲-细节**

与其将应用设置入口点深埋在大纲-细节窗格中，不如将其设置为高级大纲窗格上的最后一个固定项。

## <a name="layout"></a>布局


在桌面和移动设备上，应用设置窗口应全屏打开，并填满整个窗口。 如果应用设置菜单拥有最多 4 个顶级组，这些组应串联在一列中。

桌面设备：

![桌面上应用设置页面的布局](images/appsettings-layout-navpane-desktop.png)

移动设备：

![手机上应用设置页面的布局](images/appsettings-layout-navpane-mobile.png)

## <a name="color-mode-settings"></a>“颜色模式”设置


如果你的应用允许用户选择应用的颜色模式，请使用[单选按钮](../controls-and-patterns/radio-button.md)或[组合框](../controls-and-patterns/lists.md#drop-down-lists)提供这些选项，标头为“选择应用模式”。 选项应包括以下内容
- 浅色
- 深色
- Windows 默认模式

我们还建议添加一个指向 Windows 设置应用的“颜色”页面的超链接，用户可以访问和修改当前的默认应用模式。 对于超链接文本，请使用字符串“Windows 颜色设置”。

![“选择模式”部分](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](https://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>“关于”部分和“反馈”按钮


我们建议将“关于此应用”部分放在你的应用中作为专用页面或放在其自己的部分中。 如果你需要“发送反馈”按钮，请将该按钮放置在“关于此应用”页面的底部。

在“法律”副标题下，放置任何“使用条款”和“隐私声明”（应为带有环绕文本的[超链接按钮](../controls-and-patterns/hyperlinks.md)）以及其他法律信息，如版权。

![附带“提供反馈”按钮的“关于此应用”部分](images/appsettings-about.png)


## <a name="recommended-page-content"></a>推荐的页面内容


当你有一个要包括在应用设置页面中的项目列表后，请考虑以下指南：

-   将相似或相关的设置分到单个设置标签下。
-   尽力使设置总数保持在 4 个或 5 个以下。
-   无论应用的上下文如何，都显示相同的设置。 如果某些设置在特定的上下文不相关，则在应用“设置”浮出控件中禁用这些设置。
-   使用描述性的单个词标签来介绍设置。 例如，对于与帐户相关的设置，将该设置命名为“帐户”而非“帐户设置”。 如果希望设置只有一个选项并且设置不为其本身提供描述性标签，则请使用“选项”或“默认”。
-   如果设置直接链接到 Web 而非浮出控件，请使用可视化提示让用户知道这一情况，例如[超链接](../controls-and-patterns/hyperlinks.md)样式的“帮助（在线）”或“Web 论坛”。 请考虑将多个指向 Web 的链接分组到一个具有单个设置的浮出控件。 例如，“关于”设置可以打开具有指向“使用条款”、“隐私策略”和“应用支持”的链接的浮出控件。
-   将不太常用的设置组合到一个入口中，以便每个更常见的设置都有其各自的入口。 将仅包含信息的内容或链接放入“关于”设置。
-   不要复制“权限”窗格中的功能。 默认情况下，Windows 提供此窗格，你无法修改它。

-   向“设置”浮出控件添加设置内容
-   在单个列中从上至下展示内容，支持滚动（如果需要）。 滚动限制的最大值是屏幕高度的两倍。
-   使用以下应用设置控件：

    -   [切换开关](../controls-and-patterns/toggles.md)：用于让用户将值设置为打开或关闭。
    -   [单选按钮](../controls-and-patterns/radio-button.md)：用于让用户从多达 5 个互相排斥的一组相关选项中选择一个项。
    -   [文本输入框](../controls-and-patterns/text-block.md)：用于让用户输入文本。 使用与要从用户那里获取的文本类型（如电子邮件或密码）相对应的文本输入框类型。
    -   [超链接](../controls-and-patterns/hyperlinks.md)：用于将用户带到应用中的其他页面或外部网站。 当用户单击超链接时，“设置”浮出控件将会消失。
    -   [按钮](../controls-and-patterns/buttons.md)：用于让用户立即启动操作，不会消除当前的“设置”浮出控件。
-   如果禁用其中一个控件，则添加描述性消息。 将此消息置于禁用的控件上。
-   在为“设置”浮出控件和标头设置动画后，将内容和控件作为单个块进行动画处理。 使用左偏移 100px 的 [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) 或 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) 动画为内容创建动画。
-   使用节标题、段落和标签来协助组织和阐述内容（如果需要）。
-   如果你需要重复设置，请使用 UI 的其他级别或展开/折叠模式，但避免两级以上深度的层次结构。 例如，天气应用提供按城市设置（即，列出各个城市）并让用户在城市上点击以打开一个新浮出控件或展开以显示设置选项。
-   如果加载控件或 Web 内容需要花费时间，请使用不确定的进度控件以指示用户该信息正在加载。 有关详细信息，请参阅[进度控件指南](https://msdn.microsoft.com/library/windows/apps/hh465469)。
-   请勿使用导航按钮或提交更改按钮。 使用超链接导航到其他页面（而不是使用提交更改按钮）从而自动保存在用户取消“设置”浮出控件时对应用设置所做的更改。



## <a name="related-articles"></a>相关文章

* [命令设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958433)
* [进度控件指南](https://msdn.microsoft.com/library/windows/apps/hh465469)
* [存储和检索应用数据](https://msdn.microsoft.com/library/windows/apps/mt299098)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)
