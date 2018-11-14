---
author: GrantMeStrength
Description: Compare platform features between iOS, Android, and Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: 面向 Android 和 iOS 开发人员的 Windows 应用概念映射
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: f7e211ebfa28421340e716c0176cab80a9511671
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6447351"
---
#<a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>适用于 Android 和 iOS 开发人员的 Windows 应用概念映射

如果你是具有 Android 或 iOS 技能和/或代码的开发人员，并且希望移动到 Windows 10 和通用 Windows 平台 (UWP)，则此资源具有在三个平台之间映射平台功能（以及你的知识）所需的一切。

另请参阅[从 iOS 移动到 UWP](ios-to-uwp-root.md) 中的移植内容。 本文档也可供[下载](https://www.microsoft.com/download/details.aspx?id=52041)。

## <a name="user-interface-ui"></a>用户界面 (UI)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>设计语言。</strong><br><br>一组规定平台中的应用应当具有的外观和行为的约定。</td>
<td align="left"><strong>Android 材料设计</strong>指南提供了一种视觉语言，供 Android 设计人员和开发人员遵循。</td>
<td align="left"><strong>人机接口指南</strong>为 iOS 设计人员和开发人员提供了相关建议。</td>
<td align="left"><a href="https://dev.windows.com/design"><strong>UWP Windows 应用设计</strong></a>展示了如何创建一款在所有 Windows 10 设备上都很出色的应用。 你可以在其中找到用户界面 (UI) 设计基础知识、响应式设计技术和完整详细的指南列表。<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>用户界面标记语言。</strong> <br><br>一种呈现和描述 UI 及其组件的标记语言。 每个平台都提供了用于视觉编辑和标记编辑的编辑器。<br/></td>
<td align="left">使用 <strong>Android Studio</strong> 或 <strong>Eclipse</strong> 编辑的 <strong>XML 布局</strong>。</td>
<td align="left">使用 Xcode 内的 <strong>Interface Builder</strong> 编辑的 <strong>XIB</strong> 和<strong>情节提要</strong>。</td>
<td align="left">使用 <strong><a href="https://www.visualstudio.com/">Microsoft Visual Studio</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend for Visual Studio</a></strong> 编辑的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185595.aspx">XAML</a></strong>。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228259.aspx">XAML 平台</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228349.aspx">使用 XAML 创建 UI</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228350.aspx">使用 XAML 定义布局</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>内置的用户界面控件。</strong> <br><br>由该平台提供的可重用 UI 元素，如按钮、列表控件和文本控件。</td>
<td align="left">预建的<strong>视图</strong>和<strong>视图组</strong>类指的是小组件、布局、文本字段、容器、日期/时间控件和专家控件。</td>
<td align="left"><strong>视图</strong>和<strong>控件</strong>位于 Xcode 对象库中，并在 UIKit 用户界面目录中列出。 视图包括图像视图、选取器视图和滚动视图。 控件包括按钮、日期选取器和文本字段。</td>
<td align="left">XAML 平台为你提供了大量的<strong>内置控件</strong>，如按钮、列表控件、面板、文本控件、命令栏、选取器、媒体和墨迹书写。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">添加控件和处理事件</a></td>
</tr>
<tr class="even">
<td align="left"><strong>控制事件处理。</strong> <br><br>定义在 UI 控件内触发事件时运行的逻辑。</td>
<td align="left"><strong>事件处理程序</strong>和<strong>事件侦听器</strong>已使用 XML 或以编程方式添加。</td>
<td align="left">控件会向<strong>目标</strong>发送<strong>操作</strong>消息。</td>
<td align="left">你可以定义一些方法来处理<strong>代码隐藏文件</strong>（已附加到 XAML 页面）中 XAML 控件的事件。 <strong>事件处理程序</strong>始终使用代码编写。 但是，你可以将这些处理程序连接到 XAML 标记或代码中的事件。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">添加控件和处理事件</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185584.aspx">事件和路由事件概述</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>数据绑定。</strong> <br><br>允许应用 UI 呈现数据并可以与该数据保持同步的软件设计模式。</td>
<td align="left">提供了<strong>数据绑定库</strong>，尽管仍是 Beta 版。</td>
<td align="left">iOS 中没有任何内置绑定系统。 可以以<strong>键值观察</strong>为基础，通过使用第三方库或编写其他代码来执行数据绑定。 控件使用委托/回调方法来获取数据。</td>
<td align="left">UWP 平台会为你处理<strong>数据绑定</strong>。 使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204783.aspx">{x:Bind}</a></strong> 标记扩展可以充分利用高性能绑定，使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204782.aspx">{Binding}</a></strong> 可以充分利用更多的功能。 这只是你为了选择该平台是使用<strong>单向绑定</strong>在 UI 中显示某个数据源中的值还是当这些值因<strong>双向绑定</strong>发生更改时仍按照这些值更新 UI 而配置绑定的其中一种情况。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">数据绑定</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI 自动化。</strong> <br><br>以编程方式访问 UI 元素、使应用可供辅助技术产访问并启用自动测试脚本与 UI 交互。</td>
<td align="left"><strong>Text labels</strong>、<strong>contentDescription</strong> 和 <strong>hint</strong> 值有助于确保可以自动化找到 UI 元素。 Android Studio 允许你使用 <strong>UI Automator</strong> 和 <strong>Espresso</strong> 测试框架编写 UI 测试。</td>
<td align="left"><strong>自动化检测</strong>允许你使用<strong>辅助功能</strong>设置或<strong>元素层次结构</strong>中的元素位置编写自动 UI 测试脚本来验证元素。</td>
<td align="left">你可以直接通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/ee684076.aspx">UI 自动化</a></strong>以编程方式访问 UWP 中内置的 UI 元素。<br/><strong><a href="https://msdn.microsoft.com/library/windows/apps/mt297667.aspx">自定义的自动化对等</a></strong>允许你为自己的自定义 UI 类提供自动化支持。 Visual Studio 中<strong><a href="https://msdn.microsoft.com/library/dd286726.aspx#VerifyingCodeUsingCUITCreate">编码的 UI 测试项目</a></strong>允许你通过 UI 自动测试整个应用程序，或者单独测试 UI。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>更改控件的外观。</strong> <br><br>编辑大小、颜色和其他属性。</td>
<td align="left">控件具有可以使用设计器工具以 XML 标记或编程方式编辑的<strong>属性</strong>。</td>
<td align="left">控件具有可以使用 Interface Builder 中的<strong>属性检查器</strong>或者以编程方式编辑的<strong>属性</strong>。</td>
<td align="left">你可以使用 Visual Studio 和 Blend for Visual Studio 在 XAML 标记中或以编程方式编辑控件的<strong>属性</strong>。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">添加控件和处理事件</a></td>
</tr>
<tr class="even">
<td align="left"><strong>可重用视觉样式。</strong> <br><br>以可重用格式将视觉更改应用于多个控件。</td>
<td align="left"><strong>XML 样式</strong>是应用于一个或多个控件的属性集。</td>
<td align="left">iOS 不支持自带的可重用视觉样式，但 UIAppearance 协议允许多个控件共享通用属性。</td>
<td align="left">你可以创建可重用的<strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx">样式</a></strong>，这些样式可应用于多个控件并存储在 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx">ResourceDictionary</a></strong> 以便于重用。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465381.aspx">快速入门：设置控件样式</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>编辑控件的视觉结构。</strong> <br><br>除了修改属性或特性之外，还可以自定义控件的视觉结构，例如移动复选框下的复选框文本。</td>
<td align="left">在 Android 中，编辑控件的视觉结构没有任何简易方法。</td>
<td align="left">在 iOS 中，编辑控件的视觉结构没有任何简易方法。</td>
<td align="left">若要自定义控件的视觉结构，可以在 XAML 标记中复制并编辑其<strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx">控件模板</a></strong>。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465374.aspx">快速入门：控件模板</a></td>
</tr>
<tr class="even">
<td align="left"><strong>内置的触控笔势。</strong> <br><br>通过处理高级别的抽象化笔势事件（例如点击和双击视图和控件）提供自定义的触控支持。</td>
<td align="left"><strong>笔势检测器</strong>可检测常用触控笔势，包括滚动、长按、点击、双击和投掷。</td>
<td align="left">UIKit 框架提供了内置的<strong>笔势识别器</strong>，可检测诸如点击、收缩、平移、轻扫、旋转和长按之类触控笔势。</td>
<td align="left"><strong>UI 元素</strong>允许你处理<strong>静态笔势事件</strong>（包括点击、双击、右键点击和按住）和<strong>操作笔势事件</strong>（包括滑动、轻扫、转动、收缩和拉伸）。 笔势事件属于<strong>路由事件</strong>并且可以通过包含子 UIElement 的父对象处理。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">触摸交互</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx#gestures__manipulations__and_interactions">自定义用户交互 - 手势、操作和交互</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">导航和应用结构</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>布局。</strong> <br><br>布局定义用户界面的结构。</td>
<td align="left">布局由多个<strong>视图组</strong>组成（如 <strong>LinearLayout</strong> 和 <strong>RelativeLayout</strong>），它们可以嵌套其他视图组或视图。</td>
<td align="left">布局中还有一个包含 <strong>UIView</strong>（可以嵌套）的 <strong>UIViewController</strong>。</td>
<td align="left">XAML 提供了一个灵活的布局系统，该系统由用于静态和响应式布局的<strong>布局面板类</strong>（如 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.canvas.aspx">Canvas</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx">Grid</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.relativepanel.aspx">RelativePanel</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx">StackPanel</a></strong>）组成。 <strong><a href="https://msdn.microsoft.com/library/ms171352.aspx">属性</a></strong>用于控制元素的大小和位置。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx">使用 XAML 定义布局</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>对等导航。</strong> <br><br>通过在具有同等层次结构重要性的页面之间导航向用户呈现。</td>
<td align="left"><strong>选项卡</strong>、<strong>轻扫视图</strong>和<strong>导航箱</strong>均提供<strong>横向导航</strong>。</td>
<td align="left"><strong>选项卡栏控制器</strong>、<strong>拆分视图控制器</strong>和<strong>页面视图控制器</strong>允许在同等层次结构的视图之间导航。</td>
<td align="left">你可以使用<strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997788.aspx">选项卡/透视</a></strong>在内容上方显示链接/选项卡的永久性列表。 <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997787.aspx">导航窗格/拆分视图</a></strong>允许你在内容旁显示一列链接。<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">导航</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">在两个页面之间导航</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>分层导航。</strong> <br><br>在层次结构的父页面和子页面之间导航。</td>
<td align="left">当与<strong>意图</strong>一起用于加载其他<strong>活动</strong>时，<strong>列表</strong>、<strong>网格列表</strong>、<strong>按钮</strong>及其他控件提供<strong>下级导航</strong>。</td>
<td align="left"><strong>导航控制器</strong>允许用户在层次结构的各级别之间导航。</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/dn449149.aspx">中心</a></strong>使你可以向用户展示可以选来用于导航到子页面的内容的预览。 <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997765.aspx">大纲/细节</a></strong>使用户可以从显示在相应细节部分旁边的项目摘要列表中进行选取。<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">导航</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">在两个页面之间导航</a></td>
</tr>
<tr class="even">
<td align="left"><strong>“后退”按钮导航。</strong> <br><br>在应用中进行后退导航。</td>
<td align="left">操作栏中的“后退”<strong></strong>和“向上”<strong></strong>按钮使用 <strong>Back 堆栈</strong>提供<strong>上级</strong>和<strong>临时</strong>导航。</td>
<td align="left"><strong>导航控制器</strong>可以向它添加一个后退按钮。<br/></td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.backstack.aspx">Back 堆栈属性</a></strong>轻松地处理软件或硬件的按下后退按钮操作，该属性允许你的用户遍历<strong>导航历史记录</strong>。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt465734.aspx">“后退”按钮导航</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>初始屏幕。</strong> <br><br>在应用启动时显示图像，主要是用于品牌推广。</td>
<td align="left">默认情况下，不提供初始屏幕，并通过编辑前几个活动的<strong>主题背景</strong>实现。</td>
<td align="left">应用必须具有<strong>静态启动映像</strong>或 <strong>XIB/情节提要启动文件</strong>。</td>
<td align="left">使用<strong>图像</strong>和彩色背景创建初始屏幕。 <a href="https://msdn.microsoft.com/library/windows/apps/mt187309.aspx">初始屏幕时间可以延长</a>。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187306.aspx">添加初始屏幕</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">自定义输入</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>语音。</strong> <br><br>语音输入的语音识别和其他语音功能。</td>
<td align="left">实现 <strong>RecognizerIntent</strong> 的任何应用（如 <strong>Google 语音搜索</strong>）均可以提供语音输入。 <strong>SpeechRecognizer</strong> 类允许应用使用 Google 的语音识别 API。</td>
<td align="left">应用可以使用 <strong>SFSpeechRecognizer</strong> 类来实现语音输入和语音识别。</td>
<td align="left">你可以使用<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185615.aspx">语音识别</a></strong> API 与前台的应用交互。 你可以使用基于语音的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185598.aspx">Cortana 交互</a></strong>来启动前台或后台中的应用，并与后台应用交互。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185614.aspx">语音交互</a></td>
</tr>
<tr class="even">
<td align="left"><strong>自定义用户输入。</strong> <br><br>处理键盘、鼠标、触笔和其他输入。</td>
<td align="left">受支持的交互包括<strong>触控</strong>、<strong>触控板</strong>、<strong>触笔</strong>、<strong>鼠标</strong>和<strong>键盘</strong>。 报告移动和输入的方式与触控相同，但可以检测到有关<strong>输入设备</strong>的详细信息。</td>
<td align="left">提供对<strong>触控</strong>、<strong>Apple Pencil</strong> 和硬件<strong>键盘</strong>的支持。</td>
<td align="left">你将发现支持各种各样的交互，如<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">触控</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187313.aspx">触控板</a></strong>、采用数字墨迹的<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187311.aspx">笔/触笔</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187308.aspx">鼠标</a></strong>和<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185607.aspx">键盘</a></strong>。 应用无需知道所使用的输入设备即可处理数据，并且可以访问原始输入设备数据（如果需要）。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404610.aspx">处理指针输入</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx">自定义用户交互</a></td>
</tr>
</tbody>
</table>
<h2 id="data">数据</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>本地应用数据。</strong> <br><br>在本地存储与应用相关的设置和文件。</td>
<td align="left">可以使用 <strong>openFileOutput</strong> 和 <strong>openFileInput</strong> 保存本地文件。 可以使用 <strong>getSharedPreferences</strong> 访问<strong>共享首选项文件</strong>中的设置。</td>
<td align="left">本地文件可以存储在<strong>应用程序支持</strong>目录中，可以通过 <strong>NSFileManager</strong> 类访问。 <strong>首选项</strong>文件中的设置可以通过 <strong>NSUserDefaults</strong> 类访问。</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/br230562.aspx">Windows.Storage</a></strong> 类会为你统一处理本地数据存储。 将设置存储为 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdatacontainer.aspx">ApplicationDataContainer</a></strong> 对象，可通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localsettings.aspx">ApplicationData.LocalSettings</a></strong> 属性访问。 将文件存储在 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefolder.aspx">StorageFolder</a></strong> 对象中，可通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localfolder.aspx">ApplicationData.LocalFolder</a></strong> 属性访问。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt299098.aspx">存储和检索设置以及其他应用数据</a></td>
</tr>
<tr class="even">
<td align="left"><strong>本地数据库存储。</strong> <br><br>使用对象关系映射程序 (ORM) 将应用数据存储在关系数据库中（如果适用）。</td>
<td align="left">提供 <strong>SQLite</strong> 数据库。 未内置任何 ORM。 SQL 查询使用 <strong>SQLiteDatabase</strong> 类运行。</td>
<td align="left">提供 <strong>SQLite</strong> 数据库。 <strong>CoreData</strong> 是内置的对象图形框架，可以与 SQLite 结合使用并提供可与 ORM 相媲美的功能。</td>
<td align="left">你可以使用 <strong>SQLite</strong> 存储数据。 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592863.aspx">实体框架</a></strong>是内置的 ORM，无需编写大量数据访问代码，并使你可以轻松地在不编写 SQL 的情况下查询数据库。 你可以通过 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592864.aspx">SQLite 库</a>直接运行 SQL 查询。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592862.aspx">数据访问</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>适用于 REST 访问的 HTTP 库。</strong> <br><br>内置库使你可以通过 HTTP 与 Web 服务和 Web 服务器进行通信。<br/></td>
<td align="left">HTTP 库 <strong>HttpURLConnection</strong> 和 <strong>Volley</strong>。</td>
<td align="left"><strong>NSURLSession</strong>、<strong>NSURLConnection</strong> 和 <strong>NSURLDownload</strong>。</td>
<td align="left">你可以使用内置的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpclient">HttpClient</a></strong> API 来访问常见的 HTTP 功能（包括 GET、DELETE、PUT、POST）、常见的身份验证模式、SSL、Cookie 和进度信息。</td>
</tr>
<tr class="even">
<td align="left"><strong>云备份服务。</strong> <br><br>应用数据的平台提供的备份服务。</td>
<td align="left">Android 的 <strong>备份管理器</strong>在 Google 的 <strong>Android 备份服务</strong>中处理应用程序数据的备份。</td>
<td align="left">用户可以配置 <strong>iCloud 备份</strong>来处理其备份，包括应用数据。 使用 iCloud 兼容<strong>核心数据</strong>、<strong>iCloud 键值应用商店</strong>和 <strong>iCloud 文档存储</strong>的应用。</td>
<td align="left">你使用漫游 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx">ApplicationData API</a></strong>（包括 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingfolder.aspx">RoamingFolder</a></strong> 和 <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingsettings.aspx"><strong>RoamingSettings</strong></a>）存储的任何应用数据将自动同步到云和用户的其他设备。 同步通过用户的 Microsoft 帐户完成。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/hh465094.aspx">漫游应用数据指南</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP 文件下载。</strong> <br><br>通过 HTTP 下载大型和小型文件。</td>
<td align="left">可以使用 <strong>URLConnection</strong> 和 <strong>HTTPURLConnection</strong> 通过 HTTP 和 FTP 进行下载，也可以使用系统<strong>下载管理器</strong>在后台进行下载。</td>
<td align="left"><strong>NSURLSession</strong> 和 <strong>NSURLConnection</strong> 可以用于通过 HTTP 和 FTP 下载文件。</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.backgroundtransfer.aspx">后台传输 API</a></strong> 使你可以可靠地通过 HTTP 和 FTP 传输文件，考虑应用暂停、连接丢失并根据连接和电池使用时间进行调整。 你还可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.httpclient.aspx">HttpClient</a></strong>，它非常适合较小的文件。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">选择哪一种网络技术？</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280377.aspx">后台传输</a></td>
</tr>
<tr class="even">
<td align="left"><strong>套接字。</strong> <br><br>创建低级别的 UDP 数据报和 TCP 套接字，以便使用自己的协议与其他设备进行通信。</td>
<td align="left"><strong>Socket</strong> 类提供 TCP 套接字，<strong>DatagramSocket</strong> 类提供 UDP 套接字。</td>
<td align="left"><strong>NSStream</strong> 和 <strong>CFStream</strong> 提供 TCP 套接字，<strong>CFSocket</strong> 提供 UDP 套接字。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br241319">DatagramSocket</a></strong> 类通过 UDP 数据报套接字进行通信，并使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br226882">StreamSocket</a></strong> 类通过 TCP 或蓝牙 RFCOMM 进行通信。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">网络基础知识</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">选择哪一种网络技术？</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280234.aspx">套接字概述</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets。</strong> <br><br>在客户端和服务器之间提供双向通信，从而启用实时数据传输。</td>
<td align="left">Android 中无任何内置 WebSockets 库。</td>
<td align="left">iOS 中无任何内置 WebSockets 库。</td>
<td align="left">对于带有接收通知的较小消息，安全连接到支持 Websockets 的服务器可以通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.messagewebsocket.aspx">MessageWebSocket</a></strong> 类实现，对于可以分段读取的较大的二进制文件传输，则可以通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamwebsocket.aspx">StreamWebSocket</a></strong> 类实现。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">网络基础知识</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">选择哪一种网络技术？</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt186447.aspx">Websocket 概述</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth 库。</strong> <br><br>允许访问第三方 OAuth 提供程序和内置于平台的任何帐户管理的 OAuth 库。</td>
<td align="left">不提供任何通用 OAuth 库。 针对 OAuth 身份验证的 <strong>GoogleAuthUtil</strong> 类通过 Google Play 服务提供。<br/></td>
<td align="left">不提供任何通用 OAuth 库。 <strong>帐户框架</strong>提供对已存储在设备上的用户帐户（如 Facebook 和 Twitter）的访问权限。</td>
<td align="left">通用 OAuth 库 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270196.aspx">Web 身份验证代理</a></strong>允许你连接到第三方标识提供者服务。 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270189.aspx">凭据保险箱</a></strong>允许你的用户保存其登录名并在多台设备上使用。 <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn896755.aspx">Microsoft.Live</a></strong> 命名空间使你可以轻松地访问 Live SDK OAuth，进而访问 Microsoft 服务。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt270184.aspx">身份验证和用户身份</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.aspx">Windows.Security.Authentication.Web API 文档</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker 代码示例</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">工具</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE。</strong> <br><br>用于创建应用的工具集。</td>
<td align="left"><strong>Android Studio</strong> 和 <strong>Eclipse</strong>，随着 Google 推动开发人员转向使用 Android Studio。</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://www.visualstudio.com/features/universal-windows-platform-vs.aspx">Visual Studio</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend for Visual Studio</a></strong> 具有对 UWP 应用进行编码、设计、连接、调试、分析、优化和测试所需的所有工具。 Visual Studio 还为你提供了适用于 Windows 10 设备的<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt188754.aspx">仿真器</a></strong>，以便你可以在一系列仿真设备中测试应用。<br/><br/><a href="https://dev.windows.com/downloads">适用于 UWP 的下载和工具</a></td>
</tr>
<tr class="even">
<td align="left"><strong>代码整理。</strong> <br><br>应用的基本文件夹结构，通常从初始模板中创建。</td>
<td align="left"><strong>AndroidManifest</strong> 文件、包含源文件的 <strong>java</strong> 文件夹、<strong>res</strong> 文件夹（带有的资源包括布局和值）、Android Studio 中的 <strong>Gradle</strong> 生成脚本和 Eclipse 中的 <strong>Ant</strong> 生成脚本。</td>
<td align="left">源文件和<strong>支持文件</strong>、<strong>Info.plist</strong> 文件、<strong>Main.storyboard</strong> 和 <strong>LaunchScreen.storyboard</strong>。 图像存储在<strong>资源库</strong>中。</td>
<td align="left">你的 UWP 应用包含应用的 XAML 和代码文件（名为 Example.xaml 和 Example.xaml.cs）、<strong>资源文件夹</strong>中的各种图像、起始页（如 <strong>MainPage.xaml</strong> 和 <strong>MainPage.xaml.cs</strong>）和清单。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn765018.aspx">创建 Hello World 应用</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">应用生命周期</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>应用生命周期。</strong> <br><br>在应用启动、暂停、恢复和关闭时处理事件，以便有机会保存/还原应用程序状态并运行其他任务。</td>
<td align="left">每个活动都有其自己的<strong>活动生命周期</strong>，具有<strong>已恢复</strong>之类的状态。 在你<strong>活动类</strong>中实现如<strong>onResume</strong> <strong>生命周期回调</strong>。</td>
<td align="left"><strong>应用程序生命周期</strong>具有<strong>已挂起</strong>之类的状态。 如 <strong>applicationDidEnterBackground:</strong> 之类的方法在<strong>应用程序委托对象</strong>中实现，以便在状态发生更改时运行代码。</td>
<td align="left">应用程序具有<strong>应用执行状态</strong>，如 NotRunning、Activated、Running、Suspending、Suspended 和 Resuming。<br/><br/>你可以在应用中实现<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.aspx">应用程序类</a></strong>方法（如 OnLaunched、OnActivated、Suspending 或 Resuming），以便在状态发生更改时运行代码。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt243287.aspx">应用生命周期</a></td>
</tr>
<tr class="even">
<td align="left"><strong>后台任务。</strong> <br><br>执行后台操作并在应用不再位于前台的情况下继续运行的任务。</td>
<td align="left">应用可以启动<strong>服务</strong>，这些服务可在应用不再位于前台的情况下执行后台操作。 服务有其自己的<strong>生命周期</strong>并且已在清单中注册。</td>
<td align="left"><strong>后台执行</strong>仅限于特定的任务类型。<br/><br/>应用使用 <strong>supported background tasks</strong> 在 Info.plist 文件中声明<strong>受支持的后台任务</strong>。<br/><br/>系统控制后台任务何时运行以及运行时长。</td>
<td align="left">你可以创建一个后台任务，方法是实现 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx">IBackgroundTask</a></strong> 接口并在应用程序清单中注册该任务。 你可以将任务设置为由<a href="https://msdn.microsoft.com/library/windows/apps/mt186458.aspx"><strong>计时器</strong></a>、<a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtriggertype.aspx"><strong>系统触发器</strong></a>和<a href="https://msdn.microsoft.com/library/windows/apps/mt185632.aspx"><strong>维护触发器</strong></a>触发。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299103.aspx">使用后台任务支持应用</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299100.aspx">创建和注册后台任务</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187310.aspx">后台任务指南</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">性能</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>性能最佳做法。</strong> <br><br>有关生成快速、具有响应能力、电池使用时间较长且启动时间较短的应用的指南。</td>
<td align="left">Android 提供<strong>性能的最佳做法</strong>培训指南。</td>
<td align="left">iOS 提供<strong>性能概述</strong>文档。</td>
<td align="left">你可以仔细阅读<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270266.aspx">性能指南</a></strong>中涉及以下主题的部分：设置性能目标、测量性能、内存管理、平滑动画、高效文件系统访问以及可用于分析和性能的工具。</td>
</tr>
<tr class="even">
<td align="left"><strong>响应式 UI 的视图优化。</strong> <br><br>通过优化视图改善性能。</td>
<td align="left">使用层次结构查看器工具优化<strong>布局层次结构</strong>、<strong>重复使用布局</strong>和加载<strong>按需视图</strong>，所有这些技巧都是为了帮助保持 UI 线程具有响应能力，并避免&quot;“出现应用程序无响应”&quot;对话框（属于 <strong>ANR</strong>）。<br/></td>
<td align="left">使用<strong>核心动画</strong>工具修复<strong>离屏呈现</strong>、<strong>混合层</strong>、<strong>光栅化</strong>的 UI 问题，有助于保持 UI 线程有响应。</td>
<td align="left">通过几个简单的步骤即可轻松地<strong>优化</strong> XAML <strong>标记</strong>和<strong>布局</strong>。 这些技巧包括精简布局结构、最大程度减少元素计数和过度绘制。 <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185403.aspx">保持 UI 线程有响应</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204779.aspx">优化 XAML 标记</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404609.aspx">优化 XAML 布局</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>线程。</strong> <br><br>使用线程来维护<strong>响应式 UI</strong> 并运行多个<strong>并行任务</strong>。</td>
<td align="left">实现线程的方法是使用类 <strong>Runnable</strong>、<strong>Handler</strong>、<strong>ThreadPoolExecutor</strong> 和更高级别的 <strong>AsyncTask</strong>。</td>
<td align="left">也可使用 <strong>NSThread</strong>、<strong>Grand Central Dispatch</strong> 和更高级别的 <strong>NSOperation</strong> 实现线程。</td>
<td align="left">通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpool.runasync.aspx">RunAsync</a></strong> 将<strong>工作项</strong>提交到<strong>线程池</strong>即可使用线程。 你可以通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230590.aspx">CreateTimer</a></strong> 使用计时器提交工作项，也可以通过 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230589.aspx">CreatePeriodicTimer</a></strong> 创建重复工作项。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187339.aspx">向线程池提交工作项</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187341.aspx">使用计时器提交工作项</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187338.aspx">创建定期工作项</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187336.aspx">使用线程池的最佳做法</a></td>
</tr>
<tr class="even">
<td align="left"><strong>异步编程。</strong> <br><br>通过利用异步编程模式避免线程的复杂性来保持 UI 线程有响应。</td>
<td align="left">创建你自己的异步类<strong>需要使用线程</strong>。 某些内置的类都是异步类。</td>
<td align="left">创建你自己的异步类<strong>需要使用线程</strong>。 某些内置的类都是异步类。</td>
<td align="left">当你创建自己的 API 时，可以使用异步模式来避免阻止主线程，例如使用 C# 和 Visual Basic 中的 <strong>Async</strong> 和 <strong>Await</strong>。 你可以使用以单词 <strong>Async</strong> 结尾的异步内置 API。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187335.aspx">异步编程</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187337.aspx">使用 C# 或 Visual Basic 调用异步 API</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>列表视图优化。</strong> <br><br>帮助优化数据列表的内置模式，当需要显示的数据量很大时，性能通常很差</td>
<td align="left"><strong>ViewHolder</strong> 设计模式用于避免多个视图查找，从而使你可以使用可重用的 UI 元素。</td>
<td align="left">可以进行一系列的优化来改善 <strong>UITableView</strong> 的性能，不内置任何内容。</td>
<td align="left">你可以使用自带 <strong>UI 虚拟化</strong>的 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx">ListView</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx">GridView</a> 控件，从而提供流畅的平移和滚动体验以及更快速的启动时间。 你还可以在数据源中实现 <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.ilist.aspx">IList</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged.aspx">INotifyCollectionChanged</a>，从而提供<strong>数据虚拟化</strong>并进一步改善性能。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204776.aspx">ListView 和 GridView UI 优化</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt574120.aspx">ListView 和 GridView 数据虚拟化</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">盈利</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>应用内购买。</strong> <br><br>允许用户在你的应用中进行购买的平台功能。</td>
<td align="left"><strong>应用内计费</strong>由 Google Services 提供。 产品已添加到 <strong>Google Play 开发人员控制台</strong>。 应用内购买通过 <strong>Google Play 计费库</strong>实现。</td>
<td align="left">产品已添加到 <strong>iTunes Connect</strong>。 使用 <strong>StoreKit</strong> 框架实现应用内购买。<br/><br/>使用 <strong>SKMutablePayment</strong> 和 <strong>SKPaymentQueue</strong> 购买产品。</td>
<td align="left">你可以为应用创建应用内产品购买，方法是<a href="https://msdn.microsoft.com/library/windows/apps/mt148551.aspx">将应用内产品购买添加到应用并提交到应用商店</a>。 <br/><br/>你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx">CurrentApp 类</a></strong>来定义应用内购买。 <br/><br/>你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.requestproductpurchaseasync.aspx">CurrentApp.RequestProductPurchaseAsync</a></strong> 来显示允许客户购买产品的 UI。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219684.aspx">启用应用内产品购买</a></td>
</tr>
<tr class="even">
<td align="left"><strong>易消耗品应用内购买。</strong> <br><br>可购买、使用并再次购买的应用内产品。</td>
<td align="left">常规购买后通过 <strong>consumePurchase</strong> 将其消耗掉、使其可购买、使用并再次购买，即可启用易消耗品购买。</td>
<td align="left">易消耗产品在 iTunes Connect 中<strong>定义为易消耗产品</strong>。</td>
<td align="left">你也可以支持易消耗品，方法是<a href="https://msdn.microsoft.com/library/windows/apps/mt148534.aspx">在将它们提交到应用商店时将它们的产品类型定义为“易消耗品”</a>即可。 然后，在进行易消耗品购买后调用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync.aspx">CurrentApp.ReportConsumableFulfillmentAsync</a></strong>，以便允许客户访问它。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219683.aspx">启用易消耗品应用内购买</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>测试应用内购买。</strong> <br><br>使你能够测试应用内购买代码，无需将应用放在应用商店中。</td>
<td align="left">使用<strong>应用内计费沙盒</strong>进行测试。</td>
<td align="left"><strong>沙盒测试人员帐户</strong>用于进行测试。</td>
<td align="left">只需使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx">CurrentAppSimulator</a></strong> 类代替 CurrentApp 即可测试应用内购买。<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>试用版。</strong> <br><br>使你能够轻松地限制内容或删除基于应用试用版的广告。</td>
<td align="left">Google Play <strong>不对应用试用版提供正式支持</strong>。 通过创建应用内购买并在确认该购买成功后采用相应的代码路径，即可获得试用版或删除广告。</td>
<td align="left">App Store <strong>不对应用试用版提供正式支持</strong>。 通过创建应用内购买并在确认该购买成功后采用相应的代码路径，即可获得试用版或删除广告。</td>
<td align="left">在你将应用提交到应用商店时使用<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt148548.aspx">“免费试用版”选项</a></strong>，可提供应用的免费试用版。 然后，你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.istrial.aspx">LicenseInformation.IsTrial</a></strong> 来检查应用的试用状态，并相应地显示不同的代码路径。 你可以注册 <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged 事件</a>，以便用户在应用运行的同时更改试用版状态后收到通知。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219685.aspx">排除或限制试用版中的功能</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">适应多个平台</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>自适应 UI：灵活的布局。</strong> <br><br>采用灵活的高度和宽度支持不同的屏幕大小。</td>
<td align="left">使用 LinearLayout 对象中的 <strong>wrap_content</strong> 和 <strong>match_parent</strong> 值，或使用 RelativeLayout 对象进行对齐，即可实现灵活的布局。</td>
<td align="left">使用带有通用情节提要的<strong>自适应模型</strong>、利用带有<strong>约束</strong>和<strong>特征</strong>的<strong>自动布局</strong>（如应用于视图控制器的 horizontalSizeClass 和 displayScale），也可实现灵活的布局。</td>
<td align="left">你可以使用<strong>布局属性</strong>和<strong>面板</strong>并结合固定和动态大小调整，创建流动布局。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#layout_overview">使用 XAML 定义布局 - 布局属性和面板</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">响应式设计 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>自适应 UI：定制的布局。</strong> <br><br>使用单独的有针对性的布局支持不同的屏幕大小。</td>
<td align="left">使用<strong>配置限定符</strong>（如<strong>小</strong>、<strong>大</strong>、<strong>LDPI</strong> 和 <strong>HDPI</strong>），针对资源目录中的不同屏幕配置提供备用布局文件可允许你根据不同大小和密度的屏幕来定制自定义布局。</td>
<td align="left">定义一个<strong>单独的 iPhone 和 iPad 情节提要</strong>，以便针对跨平台应用中的不同设备系列定制布局。</td>
<td align="left">你可以通过为每个设备系列定义<strong>不同的 XAML 标记文件</strong>来生成定制的布局。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#tailored_layouts">使用 XAML 定义布局 - 定制的布局</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>自适应 UI：响应式布局。</strong> <br><br>响应屏幕大小的更改（如旋转），或窗口大小的更改。</td>
<td align="left">结合使用灵活布局和 <strong>LinearLayout</strong> 与 <strong>RelativeLayout</strong>，或为不同的方向提供备用布局文件来启用响应式布局。</td>
<td align="left">当视图的<strong>大小</strong>或<strong>特征</strong>发生更改时，将应用在情节提要中指定的<strong>约束</strong>。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.aspx">VisualState</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager.aspx">VisualStateManager</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.aspx">AdaptiveTrigger</a></strong> 轻松地在运行时重排、重新定位、调整大小、展示或替换部分 UI，以响应窗口大小更改。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#visual_states_and_state_triggers">使用 XAML 定义布局 - 视觉状态和状态触发器</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">响应式设计 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>支持不同的设备功能。</strong> <br><br>利用高级硬件功能，同时仍支持没有这些功能的设备。</td>
<td align="left">使用 <strong>PackageManager.hasSystemFeature</strong> 在运行时对设备功能进行测试，使你可以决定是否可以运行硬件特定代码。</td>
<td align="left">你不可以在运行时执行<strong>单个检查</strong>来对设备功能进行测试，以特定方法测试每项功能，以便确定是否可以运行硬件特定代码。</td>
<td align="left">你可以将<strong>平台扩展 SDK</strong> 添加到程序包，以将在不同设备系列（包括手机、台式机和 IoT）中找到的其他功能作为目标。 你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">ApiInformation API</a></strong> 对在运行时出现的类型和成员进行测试，并且仅当这些类型和成员存在时才能调用它们。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>支持不同的设备功能。</strong> <br><br>利用高级硬件功能，同时仍支持没有这些功能的设备。</td>
<td align="left">可以将 <strong>Android 支持库</strong> 与你的应用打包在一起，以使某些较新的 API 可用于采用较旧版本的 Android 的 API。 可以在运行时使用 <strong>Build.Version.SDK_INT</strong> 完成对 API 级别的测试。</td>
<td align="left">标准运行时检查用于确定 API 是否可用，例如使用 <strong>class</strong> 方法来检查类是否存在，使用 <strong>respondsToSelector:</strong> 来检查有关类的方法。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn949005.aspx">ApiInformation.IsApiContractPresent</a></strong> 来确定是否存在带有指定主次编号的 API 合约。 你也可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">ApiInformation API</a></strong> 对在运行时出现的类型和成员进行测试，并且仅当这些类型和成员存在时才能调用它们。</td>
</tr>
</tbody>
</table>
<h2 id="notifications">通知</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>磁贴和锁屏提醒。</strong> <br><br>在主屏幕上向用户显示更新。</td>
<td align="left"><strong>应用小组件</strong>是应用程序中的视图，可以嵌入主屏幕并且可以接收定期更新。 Android 中<strong>没有锁屏提醒系统</strong>。 也没有等同的磁贴系统。</td>
  <td align="left">iOS 中的<strong>小组件</strong>出现在通知中心中，并作为<strong>应用扩展</strong>实现。 你还可以向图标中添加<strong>锁屏提醒</strong>，并加上可在响应本地或远程通知时发生更改的编号。 没有磁贴系统。</td>
<td align="left">应用的<strong>磁贴</strong>可以固定到“开始”屏幕，并通过字形和编号来显示你选择的文本、图像和<strong>锁屏提醒</strong>。 你可以通过推送通知或按预定义计划从应用更新磁贴的内容。 磁贴具有自适应性，并且可以根据它们显示的位置进行更改。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185605.aspx">创建磁贴</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt590880.aspx">创建自适应磁贴</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">选择通知传递方法</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465403.aspx">磁贴和锁屏提醒指南</a></td>
</tr>
<tr class="even">
<td align="left"><strong>显示通知。</strong> <br><br>可以显示的通知类型。</td>
<td align="left">通知可以在<strong>通知区域</strong>和<strong>通知箱</strong>中显示，<strong>警告通知</strong>会在一个小浮动窗口中显示通知。 通过定义 <strong>PendingIntent</strong>，通知可以向自身添加操作。</td>
<td align="left">弹出通知会以<strong>横幅</strong>或<strong>警报</strong>形式显示。 你可以将自定义操作按钮添加到使用 <strong>UIMutableUserNotificationAction</strong> 定义的<strong>可操作通知</strong>。</td>
<td align="left">你可以创建自适应的弹出通知，称为 <strong>Toast 通知</strong>。 你可以使用可视内容、<strong>操作</strong>（可以是按钮或输入和音频）在 XML 中定义 Toast。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">自适应和交互式 Toast 通知</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">选择通知传递方法</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465391.aspx">Toast 通知指南</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>计划本地通知。</strong> <br><br>在计划时间由应用发送的本地通知。</td>
<td align="left">通知和操作可使用 <strong>NotificationCompat.Builder</strong> 进行定义，并且可以使用 <strong>AlarmManager</strong> 和 <strong>BroadcastReceiver</strong> 在应用内进行计划和处理。</td>
<td align="left">本地通知是使用 <strong>UILocalNotification</strong> 创建的，可以使用 <b> UILocalNotification.scheduleLocalNotification:<strong> 来计划。| 可以使用 </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtoastnotification.aspx">ScheduledToastNotification</a><strong> 来计划 toast 通知。可以使用 </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.tilenotification.aspx">TileNotification 类</a><strong>从应用中发送磁贴通知，或使用 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtilenotification.aspx">ScheduledTileNotification</a> 计划磁贴通知。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">自适应和交互式 Toast 通知</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt593299.aspx">发送本地磁贴通知</a> | | </strong>发送推送通知。</b> 从推送通知服务器发送并可以选择在应用内处理的通知。</td>
<td align="left"><strong>Google 云消息</strong>提供对 Android 的推送通知支持。</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">媒体捕获和呈现</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>捕获媒体。</strong> <br><br>录制音频和视觉内容。</td>
<td align="left">使用<strong>意图</strong>（如 MediaStore.ACTION_VIDEO_CAPTURE）允许使用现有的相机应用捕获媒体。 使用 <strong>android.hardware.camera2</strong> 或<strong>相机</strong>库可以实现自定义相机接口。 <strong>MediaRecorder</strong> API 可以用于捕获音频。</td>
<td align="left"><strong>UIImagePickerController</strong> 允许通过系统 UI 捕获视频和照片。 <strong>AVFoundation</strong> 类（如 <strong>AVCaptureSession</strong>）支持直接访问相机。 <br/><strong>AVAudioRecorder</strong> 类支持音频录制。</td>
<td align="left">你可以结合使用内置的相机 UI 与 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.cameracaptureui.aspx">CameraCaptureUI 类</a></strong>来捕获照片和视频。 你可以在较低的级别上与相机进行交互，并使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.aspx">Windows.Media.Capture</a></strong> 中的类（如 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.mediacapture.aspx">MediaCapture API</a></strong>）捕获音频。 <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt282142.aspx">使用 CameraCaptureUI 捕获照片和视频</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243896.aspx">使用 MediaCapture 捕获照片和视频</a></td>
</tr>
<tr class="even">
<td align="left"><strong>媒体播放。</strong> <br><br>播放音频和视频文件。</td>
<td align="left"><strong>MediaPlayer</strong> 和 <strong>AudioManager</strong> 类用于播放音频和视频文件。</td>
<td align="left"><strong>AVKit framework</strong>、<strong>AVAudioPlayer</strong> 和 <strong>Media Player Framework</strong> 用于播放音频和视频文件。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.core.mediasource.aspx">MediaSource 类</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx">MediaElement</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx">MediaPlayer</a></strong> 类，从源（如本地和远程文件）播放音频和视频。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592657.aspx">使用 MediaSource 的媒体播放</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>编辑媒体。</strong> <br><br>从现有录制文件合成新的媒体文件，并应用特效。</td>
<td align="left">低级别的类（如 <strong>MediaCodec</strong>、<strong>MediaMuxer</strong> 和 <strong>android.media.effect</strong>）可用于内容编辑。</td>
<td align="left"><strong>AV Foundation</strong> 框架中的类（如 <strong>AVMutableComposition</strong>、<strong>AVMutableVideoComposition</strong> 和 <strong>AVMutableAudioMix</strong>）可用于内容编辑。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.aspx">Windows.Media.Editing</a></strong> API（如 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediacomposition.aspx">MediaComposition</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediaclip.aspx">MediaClip</a></strong>）从音频和视频文件创建媒体合成。 你可以添加视频和图像覆盖、合并视频剪辑、添加背景音频以及应用音频和视频效果。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204792.aspx">媒体合成和编辑</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">传感器</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>传感器。</strong> <br><br>检测设备移动、位置和环境属性。</td>
<td align="left"><strong>传感器框架</strong>用于访问带有类（如 <strong>SensorManager</strong> 和 <strong>SensorEvent</strong>）的硬件和软件传感器。</td>
<td align="left"><strong>核心运动框架</strong>用于访问原始和经过处理的传感器数据。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.aspx">Windows.Devices.Sensors</a></strong> 中的类来访问传感器读数以及从传感器接收到新读取数据时触发的事件。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187358.aspx">传感器</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">位置和映射</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>位置。</strong> <br><br>查找设备的<strong>当前</strong>位置并跟踪<strong>位置变动</strong>。</td>
<td align="left">Google Play 服务位置 API 使用 <strong>getLastLocation</strong> 和 <strong>requestLocationUpdates</strong> 方法，通过<strong>融合的位置提供程序</strong>提供对<strong>最后已知位置</strong>的高级别访问权限。 在 Android 库中，低级别访问权限通过 <strong>LocationManager</strong> 提供。</td>
<td align="left"><strong>核心位置</strong> <strong>CLLocationManager</strong> 类用于监控设备的位置，<strong>startUpdatingLocation</strong> 用于标准位置服务，<strong>startMonitoringSignificantLocationChanges</strong> 用于 <strong>significant-change</strong> 位置服务。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.aspx">Windows.Devices.Geolocation</a></strong> 中的类跟踪设备位置。 对于一次性读取，使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br225537.aspx">Geolocator.GetGeopositionAsync</a></strong>。 使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.positionchanged.aspx">Geolocator.PositionChanged</a></strong> 来获取定期使用计时器的位置，或者收到位置变动通知。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219698.aspx">获取用户位置</a></td>
</tr>
<tr class="even">
<td align="left"><strong>显示地图。</strong> <br><br>显示<strong>交互式内置地图</strong>并添加<strong>目标点</strong>。</td>
<td align="left"><strong>Google 地图 Android API</strong> 中的 <strong>GoogleMap</strong>、<strong>MapFragment</strong> 和 <strong>MapView</strong> 类允许地图嵌入在应用中。 可以使用<strong>标记</strong>和可自定义的 <strong>Marker</strong> 类显示目标点。</td>
<td align="left">使用 <strong>MapKit 框架</strong>中的 <strong>MKMapView</strong> 类将地图嵌入 iOS 应用中。 可以使用对象类（如 <strong>MKPointAnnotation</strong>）和视图类（如 <strong>MKPinAnnotationView</strong>）向应用添加<strong>注释</strong>，以显示目标点。</td>
<td align="left">你可以使用内置的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx">MapControl</a></strong> XAML 控件（提供 2D、3D 和街景视图）将地图嵌入应用中。 你可以使用类（如 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapicon.aspx">MapIcon</a></strong>、<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolygon.aspx">MapPolygon</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolyline.aspx">MapPolyline</a></strong>）通过图钉、图像或形状添加目标点。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219695.aspx">使用 2D、3D 和街景视图显示地图</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219696.aspx">在地图上显示目标点 (POI)</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>地理围栏。</strong> <br><br>监视进入和离开特定地理区域。</td>
<td align="left">使用 Google Play Services SDK 中的<strong>定位服务</strong>监视地理围栏。</td>
<td align="left">使用 <strong>CLCircularRegion</strong> 类监视区域，使用 <strong>CLLocationManager.startMonitoringForRegion:</strong> 注册区域。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofence.aspx">Geofence</a></strong> 类创建地理围栏，并定义<strong>监视状态</strong>（如进入或离开某个区域）。 使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofencemonitor.aspx">GeofenceMonitor 类</a></strong>处理前台地理围栏事件，后台地理围栏事件要使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.locationtrigger.aspx">LocationTrigger 后台类</a></strong>处理。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219702.aspx">设置地理围栏</a></td>
</tr>
<tr class="even">
<td align="left"><strong>地理编码和反向地理编码。</strong> <br><br>将地址转换为地理位置（地理编码），将地理位置转换为地址（反向地理编码）。<br/></td>
<td align="left"><strong>Geocoder</strong> 类用于地理编码和反向地理编码。</td>
<td align="left"><strong>CLGeocoder</strong> 类用于地理编码。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong> 中的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx">MapLocationFinder 类</a></strong>执行地理编码。 对于地理编码，使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsasync.aspx">FindLocationsAsync</a></strong>，而对于反向地理编码，使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsatasync.aspx">FindLocationsAtAsync</a></strong>。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219697.aspx">执行地理编码和反向地理编码</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>路线和方向。</strong> <br><br>提供两个地理位置之间的路线、距离和方向。</td>
<td align="left">Google 提供的 Web 服务 <strong>Google 地图路线 API</strong> 可以在 Android 上使用，即使没有提供 SDK。</td>
<td align="left">地图工具包提供 <strong>MKDirections</strong> API，可用于获取有关路线和方向的信息。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong> 中的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutefinder.aspx">MapRouteFinder</a></strong> 类，请求步行或驾车路线。 路线会作为 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproute.aspx">MapRoute</a></strong> 实例返回，并轻松地在 MapControl 上显示。 方向会在 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutemaneuver.aspx">MapRouteManeuver</a></strong> 对象内部返回。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219701.aspx">在地图上显示路线和方向</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">应用到应用的通信</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>调用另一个应用。</strong> <br><br>启动另一个应用，并且可以选择共享数据（如链接、文本、照片、视频和文件）。</td>
<td align="left">可使用<strong>隐式意图</strong>启动另一个应用，方法是定义<strong>意图</strong>中的<strong>操作</strong>和可选数据，并使用 <strong>startActivityForResult</strong> 对其进行调用。<br/></td>
<td align="left"><strong>应用扩展</strong>可用于向另一个应用提供对应用数据的访问权限。 <strong>URL 方案</strong>使 URL 可以传递到另一个应用。</td>
<td align="left">你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriasync.aspx">Launcher.LaunchUriAsync</a></strong> 启动另一个已注册 URI 的应用，或使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriforresultsasync.aspx">Launcher.LaunchUriForResultsAsync</a></strong> 针对结果进行启动，并从启动的应用重新获取数据。 你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/hh701471.aspx">Launcher.LaunchFileAsync</a></strong> 将文件传递到另一个应用进行处理。<br/><br/>你可以使用<strong>共享合约</strong>在应用之间轻松地共享数据。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228340.aspx">启动 URI 的默认应用</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">针对结果启动应用</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299102.aspx">启动文件的默认应用</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243293.aspx">共享数据</a></td>
</tr>
<tr class="even">
<td align="left"><strong>允许对应用进行调用。</strong> <br><br>允许应用响应来自另一个应用的请求。</td>
<td align="left">应用向<strong>意图筛选器</strong>注册<strong>意图处理活动</strong>，以响应来自另一个应用的隐式意图。</td>
<td align="left">将<strong>应用扩展</strong>打包可使数据与其他应用共享。 应用可以使用 Info.plist 中的 <strong>CFBundleURLTypes</strong> 项注册<strong>自定义 URL 方案</strong>。</td>
<td align="left">通过在程序包清单中注册一项<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.activationkind.aspx#Protocol">协议</a></strong>并更新 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx">Application.OnActivated</a></strong> 事件处理程序，（可选）从而返回结果，你可以将应用注册为 <strong>URI 方案名称</strong>的默认处理程序。 同样，也可以通过在程序包清单中添加一项声明并处理 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onfileactivated.aspx">Application.OnFileActivated</a></strong> 事件，将应用注册为特定文件类型的默认处理程序。<br/><br/>你可以通过在清单中将应用注册为共享目标并处理 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onsharetargetactivated.aspx">Application.OnShareTargetActivated</a></strong> 事件来处理共享合约请求。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">针对结果启动应用</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269385.aspx">处理文件激活</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243292.aspx">接收数据</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>复制和粘贴。</strong> <br><br>在应用之间复制和粘贴文本及其他内容。</td>
<td align="left">借助 <strong>ClipboardManager</strong> 和 <strong>ClipData</strong> 类，可以使用<strong>剪贴板框架</strong>实现复制和粘贴。</td>
<td align="left">可以使用 <strong>UIPasteboard</strong>、<strong>UIMenuController</strong> 和 <strong>UIResponderStandardEditActions</strong> 实现复制和粘贴。</td>
<td align="left">许多默认 XAML 控件已经支持复制和粘贴。 你可以使用 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br205967">Windows.ApplicationModel.DataTransfer</a></strong> 中的 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx">DataPackage</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.aspx">Clipboard</a></strong> 类，自行实现复制和粘贴。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243291.aspx">复制和粘贴</a></td>
</tr>
<tr class="even">
<td align="left"><strong>拖放。</strong> <br><br>在应用之间拖放内容。</td>
<td align="left">拖放可通过使用 <strong>Android 拖/放框架</strong>在单个应用程序内部实现。</td>
<td align="left">iOS 不提供任何高级别的拖放 API。</td>
<td align="left">通过在应用中实现拖放，可以启用应用到应用、桌面到应用以及应用到桌面的拖放功能。 你可以借助 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx">AllowDrop</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx">CanDrag</a></strong> 属性以及 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx">DragOver</a></strong> 和 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx">Drop</a></strong> 事件，在 UIElement 类中实现拖放支持。<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt227651.aspx">拖放</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">软件设计</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>软件设计模式。</strong> <br><br>针对平台推荐的或适用于平台的模式。</td>
<td align="left">尽管 Beta 版“数据绑定框架”支持广泛使用 <strong>Model-View-ViewModel (MVVM)</strong> 模式，但并没有针对 Android 开发推荐或提供任何正式模式。 许多第三方文章和框架推荐使用 <strong>Model-View-Presenter (MVP)</strong> 和 <strong>MVVM</strong> 方法。</td>
<td align="left"><strong>Model-View-Controller (MVC)</strong> 是与 iOS 一起使用的常见模式，并且已集成到平台中。</td>
<td align="left">在针对 UWP 进行生成时，不要局限于某一特定模式。<br/><br/>你可以使用内置的<a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">数据绑定</a>模式以确保彻底分离数据和 UI 问题，并避免对随后会更新属性值的 UI 事件处理程序进行编码。<br/><br/>你可以通过利用第三方 MVVM 库（如 <a href="https://mvvmlight.codeplex.com/">MVVM 光工具包</a>），或者滚动自己的 MVVM 库并保持逻辑退出代码隐藏来扩展数据绑定，以遵循 <strong>Model-View-ViewModel (MVVM)</strong> 模式。<br/><br/><a href="https://msdn.microsoft.com/library/hh848246.aspx">MVVM 模式</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">模板 10 Visual Studio 项目模板</a></td>
</tr>
</tbody>
</table>
