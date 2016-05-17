---
author: mtoepke
title: Windows 10 游戏开发指南
description: 开发通用 Windows 平台 (UWP) 游戏的资源和信息的端到端指南。
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
---

# Windows 10 游戏开发指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

欢迎使用 Windows 10 游戏开发指南！

本指南提供开发通用 Windows 平台 (UWP) 游戏所需的资源和信息的端到端集合。

## 通用 Windows 平台 (UWP) 游戏开发简介


当创建 Windows 10 游戏时，你将有机会在手机、电脑和 Xbox One 上接触到世界范围内的数百万玩家。 凭借 Windows 上的 Xbox、Xbox Live、跨设备多人游戏、令人惊叹的游戏社区以及诸如通用 Windows 平台 (UWP) 和 DirectX 12 等强大的新功能， Windows 10 游戏令所有年龄和流派的玩家都感到兴奋不已。 新的通用 Windows 平台 (UWP) 通过适用于手机、电脑和 Xbox One 的常用 API 以及为每种设备体验定制游戏的工具和选项，可跨所有 Windows 10 设备为游戏提供兼容性。

本指南提供可在你开发游戏时提供帮助的端到端的信息和资源集合。 每个部分均按照游戏开发阶段进行组织，因此你在需要时就知道在哪查找信息。

若要开始操作，[游戏开发资源](#resources)部分提供有关文档、程序和其他有助于创建游戏的资源的高级调查。

本指南在其他 Windows 10 游戏开发资源和材料可用时会进行更新。

## 游戏开发资源


从文档到开发人员计划、论坛、博客和示例，游戏开发之路上有很多资源可提供帮助。 以下是在开发 Windows 10 游戏时要了解的资源综述。

> **注意** Xbox One 开发和精选 Windows 10 游戏功能（例如 Xbox Live 服务）通过诸如 ID@Xbox 和 Microsoft Studios 等计划进行管理。 本指南涵盖大范围资源，因此你可能会发现有些资源无法访问，具体取决于所属计划或特定的开发角色。 示例是解析为 developer.xboxlive.com、forums.xboxlive.com、xdi.xboxlive.com 或游戏开发人员网络 (GDN) 的链接。 有关与 Microsoft 合作的信息，请参阅[开发人员计划](#programs)

 

### 游戏开发文档

在本指南中，你可以查找指向相关文档的深层链接：根据任务、技术和游戏开发阶段进行组织。 为了让你全面了解可用内容，以下是 Windows 10 游戏开发的主要文档门户。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 开发人员中心主要门户</td>
        <td>[Windows 开发人员中心](https://dev.windows.com)</td>
    </tr>
    <tr>
        <td>开发 Windows 应用</td>
        <td>[开发 Windows 应用](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>通用 Windows 平台应用开发</td>
        <td>[Windows 10 应用的操作方法指南](https://msdn.microsoft.com/library/windows/apps/mt244352)</td>
    </tr>
    <tr>
        <td>UWP 游戏的操作方法指南</td>
        <td>[游戏和 DirectX](index.md) </td>
    </tr>
    <tr>
        <td>DirectX 参考和概述</td>
        <td>[DirectX 图形和游戏](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Xbox Live 文档</td>
        <td>[Xbox Live SDK](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Xbox One 开发人员文档 (GDN)</td>
        <td>[Xbox One XDK 文档](https://developer.xboxlive.com/platform/development/documentation/Pages/home.aspx)</td>
    </tr>
    <tr>
        <td>Xbox One 开发人员白皮书 (GDN)</td>
        <td>[白皮书](https://developer.xboxlive.com/platform/development/education/Pages/WhitePapers.aspx)</td>
    </tr>     
</table>


### 开发人员计划

Microsoft 提供多个开发人员计划，可帮助你开发和发布 Windows 游戏。 若要在 Windows 应用商店中发布游戏，你需要在 Windows 开发人员中心上创建开发者帐户。 其他计划可能会引起你的兴趣，具体取决于游戏和工作室需要，并可以创造 Xbox One 开发和 Xbox Live 集成的机遇。

### Windows 开发人员中心

在 Windows 开发人员中心中注册开发人员帐户是发布 Windows 游戏的第一步。 开发人员帐户让你可以保留游戏名称和将免费或付费游戏提交到适用于所有 Windows 设备的 Windows 应用商店。 使用开发人员帐户管理游戏和游戏内产品、获取详细分析，以及支持全世界玩家创建绝佳体验的服务。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>注册开发人员帐户</td>
        <td>[准备好注册了吗？](https://msdn.microsoft.com/library/windows/apps/bg124287)</td>
    </tr>
</table>  


### ID@Xbox

ID@Xbox 计划可帮助符合资格的游戏开发人员自行在 Windows 和 Xbox One 上发布游戏。 如果你想开发适用于 Xbox One 的应用，或将诸如玩家分数、成就和排行榜等 Xbox Live 功能添加到 Windows 10 游戏，请注册 ID@Xbox。 成为一名 ID@Xbox 开发人员，获取所需的工具和支持，来发挥你的创造力并最大可能地取得成功。 在申请 ID@Xbox 之前，请在 Windows 开发人员中心注册开发人员帐户。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox 开发人员计划</td>
        <td>[独立的 Xbox One 开发人员计划](http://go.microsoft.com/fwlink/p/?LinkID=526271)</td>
    </tr>
    <tr>
        <td>ID@Xbox 消费者站点</td>
        <td>[ID@Xbox](http://www.idatxbox.com/)</td>
    </tr>
</table>


### DirectX 提前访问计划

想要获取提前预览版的 Direct3D 12 API 更改和想要在论坛上提供反馈的专业游戏开发人员可以加入 DirectX 提前访问计划。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>注册 DirectX 12 提前访问计划</td>
        <td>[DirectX 提前访问计划](http://1drv.ms/1dgelm6)</td>
    </tr>
</table>


### Xbox 工具和中间件

Xbox 工具和中间件计划给使用游戏工具和中间件的专业开发人员颁发 Xbox 开发工具包许可证。 同意加入该计划的开发人员可以将其 Xbox XDK 技术共享和分配到其他已获得授权的 Xbox 开发人员。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>联系工具和中间件计划</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>


### 游戏示例

有许多 Windows 10 游戏和应用示例可以帮助你了解 Windows 10 游戏功能和快速开始开发游戏。 会定期开发和发布更多示例，所以不要忘记时不时返回到示例门户，查看一下新增内容。 你还可以[查看](https://help.github.com/articles/watching-repositories/) GitHub 存储库以接收有关更改和新增内容的通知。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台应用示例</td>
        <td>[Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples)</td>
    </tr>
    <tr>
        <td>Direct3D 12 图形示例</td>
        <td>[DirectX-Graphics-Samples](https://github.com/Microsoft/DirectX-Graphics-Samples)</td>
    </tr>
    <tr>
        <td>Direct3D 11 第一人称游戏示例</td>
        <td>[使用 DirectX 创建一款简单的 UWP 游戏](tutorial--create-your-first-metro-style-directx-game.md)</td>
    </tr>
    <tr>
        <td>Direct2D 自定义图像效果示例</td>
        <td>[D2DCustomEffects](http://go.microsoft.com/fwlink/p/?LinkId=620531)</td>
    </tr>
    <tr>
        <td>Direct2D 渐变网格示例</td>
        <td>[D2DGradientMesh](http://go.microsoft.com/fwlink/p/?LinkId=620532)</td>
    </tr>
    <tr>
        <td>Direct2D 照片调整示例</td>
        <td>[D2DPhotoAdjustment](http://go.microsoft.com/fwlink/p/?LinkId=620533)</td>
    </tr>
    <tr>
        <td>Xbox One 游戏示例 (GDN)</td>
        <td>[示例](https://developer.xboxlive.com/platform/development/education/Pages/Samples.aspx)</td>
    </tr>
    <tr>
        <td>Windows 8 游戏示例（MSDN 代码库）</td>
        <td>[Windows 应用商店游戏示例](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft)</td>
    </tr>
    <tr>
        <td>JavaScript 和 HTML5 游戏示例</td>
        <td>[JavaScript 和 HTML5 触摸游戏示例](https://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)</td>
    </tr>      
</table>


### 开发人员论坛

若要提问和回答游戏开发问题以及联系游戏开发社区，开发人员论坛是一个不错的选择。 论坛也是不错的资源，从中可以查找开发人员过去遇到的并已解决的难题的现有答案。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 应用开发人员论坛</td>
        <td>[Windows 应用商店和应用论坛](https://social.msdn.microsoft.com/Forums/home?category=windowsapps)</td>
    </tr>
    <tr>
        <td>UWP 应用开发人员论坛</td>
        <td>[开发通用 Windows 平台应用](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop)</td>
    </tr>

    <tr>
        <td>Desktop applications developer forums</td>
        <td>[Windows desktop applications forums](https://social.msdn.microsoft.com/Forums/home?category=windowsdesktopdev)</td>
    </tr>
    <tr>
        <td>DirectX Windows Store games (archived forum posts)</td>
        <td>[Building Windows Store games with DirectX (archived)](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx)</td>
    </tr>
    <tr>
        <td>Windows 10 managed partner developer forums</td>
        <td>[XBOX Developer Forums: Windows 10](http://aka.ms/win10devforums)</td>
    </tr>
    <tr>
        <td>DirectX early access program forums</td>
        <td>[DirectX 12 forum](http://directx12forum.azurewebsites.net/index.php)</td>
    </tr>
</table>


### 开发人员博客

开发人员博客是另一种获取有关游戏开发的最新信息的绝佳资源。 你可以查找有关新功能、实现细节、最佳实践、体系结构背景等的文章。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>生成适用于 Windows 的应用博客</td>
        <td>[生成适用于 Windows 的应用](http://blogs.windows.com/buildingapps/)</td>
    </tr>
    <tr>
        <td>Windows 10（博客文章）</td>
        <td>[Windows 10 中的文章](http://blogs.windows.com/blog/tag/windows-10/)</td>
    </tr>
    <tr>
        <td>Visual Studio 工程团队博客</td>
        <td>[Visual Studio 博客](http://blogs.msdn.com/b/visualstudio/)</td>
    </tr>
    <tr>
        <td>Visual Studio 开发人员工具博客</td>
        <td>[开发人员工具博客](http://blogs.msdn.com/b/developer-tools/)</td>
    </tr>
    <tr>
        <td>Somasegar 的开发人员工具博客</td>
        <td>[Somasegar 的博客](http://blogs.msdn.com/b/somasegar/)</td>
    </tr>
    <tr>
        <td>DirectX 开发人员博客</td>
        <td>[DirectX 开发人员博客](http://blogs.msdn.com/b/directx)</td>
    </tr>
    <tr>
        <td>DirectX 12 简介（博客文章）</td>
        <td>[DirectX 12](http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ 工具团队博客</td>
        <td>[Visual C++ 团队博客](http://blogs.msdn.com/b/vcblog/)</td>
    </tr>
    <tr>
        <td>ID@Xbox 开发人员博客</td>
        <td>[ID@Xbox 开发人员博客](http://www.idatxbox.com/category/developer-blog/)</td>
    </tr>
</table>
 

## 概念和计划


在概念和计划阶段，你需确定游戏的外观以及将其创造出来需要使用的技术和工具。

### 游戏开发技术概述

在开始开发适用于 UWP 的游戏时，你可以使用适用于图形、输入、音频、网络、实用工具和库的多个选项。

如果你已经确定好将在游戏中使用的所有技术，那真是太好了！ 如果没有，[适用于 UWP 应用的游戏技术](game-development-platform-guide.md)指南很好地概述了许多可用技术， 并且强烈建议你阅读该指南，以便可帮助你了解各种选项以及如何组合使用它们。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 游戏技术调查</td>
        <td>[适用于 UWP 应用的游戏技术](game-development-platform-guide.md)</td>
    </tr>
</table>
 

这三个 GDC 2015 视频清楚概述了 Windows 10 游戏开发和 Windows 10 游戏体验。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 10 游戏开发概述（视频）</td>
        <td>[开发适用于 Windows 10 的游戏](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Windows 10 游戏体验（视频）</td>
        <td>[Windows 10 上的游戏消费者体验](http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10)</td>
    </tr>
    <tr>
        <td>Microsoft 生态系统上的游戏（视频）</td>
        <td>[Microsoft 生态系统上游戏的未来](http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem)</td>
    </tr>
</table>
 

### 选择图形技术和编程语言

有多种编程语言和图形技术可在 Windows 10 游戏中使用。 采用何种路径具体取决于你正在开发的游戏类型、你的开发工作室的经验和偏好，以及你的游戏的特定功能要求。 你是使用 C#、C++ 或 JavaScript？ 还是使用 DirectX、XAML 或 HTML5？

### DirectX

对于最高性能的 2D 和 3D 图形以及多媒体，可选择 Microsoft DirectX。 Windows 10 中的新功能 Direct3D 12 可提供了类似于控制台 API 的强大功能，而且比以往更快更高效。 你的游戏可以充分利用现代图形硬件，还可以展示更多的对象、更丰富的场景以及增强的游戏效果。 Direct3D 12 可以在 Windows 10 电脑和 Xbox One 上提供优化的图形。 如果你想要使用 Direct3D 11 中熟悉的图形管道，仍然可以从添加到 Direct3D 11.3 的新呈现功能和优化功能中获益。 如果你是一名倾向于基于 Win32 的桌面版 Windows API 开发人员，你仍然可以在 Windows 10 中使用该选项。

DirectX 中的广泛功能和深度平台集成可为要求极高的游戏提供所需的功能和性能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 游戏操作方法指南</td>
        <td>[游戏和 DirectX](index.md)</td>
    </tr>
    <tr>
        <td>DirectX 概述和参考</td>
        <td>[DirectX 图形和游戏](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 编程指南和参考</td>
        <td>[Direct3D 12 图形](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
</table>
 

### XAML

XAML 是一种易于使用的声明性 UI 语言，它具有一些便捷的功能，如动画、情节提要、数据绑定、可缩放的基于矢量的图形、自动调整大小和场景图。 XAML 非常适用于游戏 UI、菜单、子画面和 2D 图形。 若要简化 UI 布局，则可以使用 XAML，因为它与诸如 Expression Blend 和 Microsoft Visual Studio 等设计和开发工具兼容。 XAML 通常与 C# 结合使用，但在 C++ 是你的首选语言时，或者你的游戏对 CPU 要求较高时，C++ 也是一个不错的选择。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML 平台概述</td>
        <td>[XAML 平台](https://msdn.microsoft.com/library/windows/apps/mt228259)</td>
    </tr>
    <tr>
        <td>XAML UI 和控件</td>
        <td>[控件、布局和文本](https://msdn.microsoft.com/library/windows/apps/mt228348)</td>
    </tr>
</table>
 

### HTML 5

超文本标记语言 (HTML) 是一种常见的 UI 标记语言，适用于网页、应用和胖客户端。 Windows 游戏可将 HTML5 用作功能完备的呈现图层，以便提供 HTML 的熟悉功能、对通用 Windows 平台的访问权限，并支持诸如 AppCache、Web Worker、Canvas、拖放、异步编程和 SVG 等现代 Web 功能。 在后台，HTML 呈现将充分利用 DirectX 硬件加速的功能，使你即使不编写任何额外的代码，也仍然可以从 DirectX 的性能中获益。 如果你擅长于 Web 开发、打算移植 Web 游戏或者想要使用与其他选项相比更易获取的语言和图形图层，则 HTML5 是一个不错的选择。 HTML5 与 JavaScript 结合使用，但也可用于调用使用 C# 或 C++/CX 创建的组件。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HTML5 和文档对象模型信息</td>
        <td>[HTML 和 DOM 参考](https://msdn.microsoft.com/library/windows/apps/br212882.aspx)</td>
    </tr>
    <tr>
        <td>HTML5 W3C 建议</td>
        <td>[HTML5](http://go.microsoft.com/fwlink/p/?linkid=221374)</td>
    </tr>
</table>
 

### 组合呈现技术

Microsoft DirectX 图形基础结构 (DXGI) 通过多种图形技术提供互操作性和兼容性。 对于高性能图形，你可以将 XAML 与 DirectX 结合使用，具体操作方式是：将 XAML 用于菜单和其他简单 UI，而将 DirectX 用于呈现复杂的 2D 和 3D 场景。 DXGI 还可在 Direct2D、Direct3D、DirectWrite、DirectCompute 和 Microsoft 媒体基础之间提供兼容性。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 图形基础结构编程指南和参考</td>
        <td>[DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)</td>
    </tr>
    <tr>
        <td>组合 DirectX 和 XAML</td>
        <td>[DirectX 和 XAML 互操作](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

### C++

C++/CX 是一种高性能、低开销的语言，可提供速度、兼容性和平台访问的强大组合。 利用 C++/CX，可轻松使用 Windows 10 中所有出色的游戏功能，包括 DirectX 和 Xbox Live。 你还可以重复使用现有的 C++ 代码和库。 C++/CX 可创建快捷的本机代码，该代码不会使垃圾回收产生开销，可确保你的游戏拥有出色的性能和低功耗，从而可以延长电池使用时间。 将 C++/CX 与 DirectX 或 XAML 结合使用，或使用这两者的组合来创建游戏。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C++/CX 参考和概述</td>
        <td>[Visual C++ 语言参考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ 编程指南和参考</td>
        <td>[Visual Studio 2015 中的 Visual C++](https://msdn.microsoft.com/library/60k1461a.aspx)</td>
    </tr>
</table>
 

### C#

C#（读作“C sharp”）是一种现代创新型语言，它简单、功能强大、类型安全，而且面向对象。 C# 在保持 C 语言的亲切和直观风格的同时，还支持应用的快速开发。 尽管 C# 易于使用，但它还具有许多高级语言功能，如多态性、委派、lambdas、关闭、迭代方法、协变以及语言集成查询 (LINQ) 表达式。 如果你面向 XAML、想要快速开始开发你的游戏或者之前有过 C# 经验，则 C# 是一个不错的选择。 C# 主要与 XAML 结合使用，因此如果你想要使用 DirectX，请改为选择 C++，或者编写你游戏的一部分作为与 DirectX 交互的 C++ 组件。 或者考虑使用 [Win2D](https://github.com/Microsoft/Win2D)，它是适用于 C# 和 C++ 的即时模式 Direct2D 图形库。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# 编程指南和参考</td>
        <td>[C# 语言参考](https://msdn.microsoft.com/library/kx37x362.aspx)</td>
    </tr>
</table>
 

### JavaScript

JavaScript 是一种动态脚本语言，广泛用于现代 Web 应用程序和胖客户端应用程序。

Windows JavaScript 应用可以采用一种简单而又直观的方式访问通用 Windows 平台中的强大功能 – 作为面向对象的 JavaScript 类的方法和属性。 如果你来自于 Web 开发环境、已熟悉 JavaScript 或者想要使用 HTML5、CSS、WinJS 或 JavaScript 库，则对于你的游戏而言，JavaScript 是一个不错的选择。 如果你面向 DirectX 或 XAML，请改为选择 C# 或 C++/CX。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>JavaScript 和 Windows 运行时参考</td>
        <td>[JavaScript 参考](https://msdn.microsoft.com/library/windows/apps/jj613794)</td>
    </tr>
</table>


### 使用 Windows 运行时组件合并语言

借助通用 Windows 平台，可轻松将采用不同语言编写的组件结合使用。 使用 C++、C# 或 Visual Basic 创建 Windows 运行时组件，然后通过 JavaScript、C#、C++ 或 Visual Basic 调用这些组件。 采用你所选定的语言编写你游戏的部分程序，是一个不错的方法。 借助组件，你也可以使用仅提供特定语言版本的外部库，并使用你已编写的传统代码。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>如何创建 Windows 运行时组件</td>
        <td>[创建 Windows 运行时组件](https://msdn.microsoft.com/library/windows/apps/hh441572.aspx)</td>
    </tr>
</table>


### 你的游戏应该使用哪个版本的 DirectX？

在为游戏选择 DirectX 时，你需要决定要使用哪个版本：是 Microsoft Direct3D 12 还是 Microsoft Direct3D 11。

Direct3D 11.3 是使用熟悉的 Direct3D 编程模型的低级别图形 API。 Direct3D 11 仍是适用于通用 Windows 应用的选项，并且你将可以访问在 Direct3D 11.3 中添加的新的呈现和优化功能。

Direct3D 12 是 Windows 10 中的新增功能，并且引入了新的管道编程模型。 Direct3D 12 更靠近硬件、使用的抽象也更少，并且能让游戏更好地控制资源的使用。 Direct3D 12 拥有更加出色的 CPU、GPU 和电源性能。

如果你拥有使用 Direct3D 11 编写的现有引擎，并且还没有准备好跳跃到 Direct3D 12， 可以使用 Direct3D 11 on 12 实现一些性能改进并开始转换到 Direct3D 12。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>选择 Direct3D 12 或 Direct3D 11</td>
        <td>[什么是 Direct3D 12？](https://msdn.microsoft.com/library/windows/desktop/dn899228)</td>
    </tr>
    <tr>
        <td>Direct3D 11 概述</td>
        <td>[Direct3D 11 图形](https://msdn.microsoft.com/library/windows/desktop/ff476080)</td>
    </tr>
    <tr>
        <td>Direct3D 11 on 12 概述</td>
        <td>[Direct3D 11 on 12](https://msdn.microsoft.com/library/windows/desktop/dn913195)</td>
    </tr>
</table>


### 桥、游戏引擎和中间件

根据游戏需要，使用桥、游戏引擎或中间件可以节省开发和测试时间及资源。 以下是一些可以帮助你决定是否有适合你的桥、游戏引擎和中间件的概述和资源。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>适用于 Windows 10 的桥和游戏引擎（博客文章）</td>
        <td>[将代码移植到快速发展的 Windows 10 应用商店的更多方法](http://blogs.windows.com/buildingapps/2015/09/17/more-ways-to-bring-your-code-to-fast-growing-windows-10-store/)</td>
    </tr>
    <tr>
        <td>使用中间件开发游戏（视频）</td>
        <td>[使用中间件加速 Windows 应用商店游戏的开发](https://channel9.msdn.com/Events/Build/2013/3-187)</td>
    </tr>
    <tr>
        <td>Visual Studio、Unity、Unreal 以及 Cocos2d（博客文章）</td>
        <td>[用于游戏开发的 Visual Studio：与 Unity、Unreal Engine 和 Cocos2d 的新合作关系](http://blogs.msdn.com/b/somasegar/archive/2015/04/17/visual-studio-for-game-development-new-partnerships-with-unity-unreal-engine-and-cocos2d.aspx)</td>
    </tr>
    <tr>
        <td>游戏中间件简介（博客文章）</td>
        <td>[游戏开发中间件 - 它是什么？ 我需要它吗？](http://blogs.msdn.com/b/wsdevsol/archive/2014/05/02/game-development-middleware-what-is-it-do-i-need-it.aspx)</td>
    </tr>
</table>
 

### 通用 Windows 平台桥

通用 Windows 平台桥是将现有应用或游戏移植到 UWP 的技术。 桥是快速开始开发 UWP 游戏的绝佳方法。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 桥</td>
        <td>[将代码移植到 Windows](https://dev.windows.com/bridges/)</td>
    </tr>
    <tr>
        <td>面向 iOS 的 Windows 桥</td>
        <td>[将 iOS 应用移植到 Windows](https://dev.windows.com/bridges/ios)</td>
    </tr>
    <tr>
        <td>适用于 .NET 和 Win32 的 Windows 桥（“Project Centennial”）预览版</td>
        <td>[Windows 开发人员预览计划](http://go.microsoft.com/fwlink/p/?LinkID=624543)</td>
    </tr>
</table>
 

### Unity

Unity 5 是备受赞誉的下一代开发平台，用于创建 2D 和 3D 游戏以及交互式体验。 Unity 5 提供全新的艺术功能、增强的图形功能以及改进的开发效率。

根据 [Unity 路线图](https://unity3d.com/unity/roadmap)，对 DirectX 12 的支持将在 Unity 的以后版本中推出。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unity 游戏引擎</td>
        <td>[Unity - 游戏引擎](http://unity3d.com/)</td>
    </tr>
    <tr>
        <td>获取 Unity 5</td>
        <td>[获取 Unity](http://unity3d.com/get-unity)</td>
    </tr>
    <tr>
        <td>Unity 5.2 中的通用 Windows 平台应用支持（博客文章）</td>
        <td>[Unity 5.2 中的 Windows 10 通用平台应用](http://blogs.unity3d.com/2015/09/09/windows-10-universal-apps-in-unity-5-2/)</td>
    </tr>
    <tr>
        <td>Windows 的 Unity 文档</td>
        <td>[Unity 手册 / Windows](http://docs.unity3d.com/Manual/Windows.mdl)</td>
    </tr>
    <tr>
        <td>将 Unity 游戏发布为通用 Windows 平台应用（视频）</td>
        <td>[如何将你的 Unity 游戏发布为 UWP 应用](https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app)</td>
    </tr>
    <tr>
        <td>使用 Unity 制作 Windows 游戏和应用（视频）</td>
        <td>[借助 Unity 制作 Windows 游戏和应用](https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity)</td>
    </tr>
    <tr>
        <td>使用 Visual Studio 的 Unity 游戏开发（视频系列）</td>
        <td>[将 Unity 与 Visual Studio 2015 结合使用](http://go.microsoft.com/fwlink/?LinkId=722359)</td>
    </tr>
</table>
 

### Havok

Havok 模块化的工具和技术套件可帮助游戏创建者达到交互式和沉浸式体验的新级别。 Havok 支持高度真实的物理特性、交互模拟和令人惊叹的电影制作技术。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok 网站</td>
        <td>[Havok](http://www.havok.com/)</td>
    </tr>
    <tr>
        <td>Havok 工具套件</td>
        <td>[Havok 产品概述](http://www.havok.com/products/)</td>
    </tr>
    <tr>
        <td>Havok 支持论坛</td>
        <td>[Havok](https://software.intel.com/forums/havok/)</td>
    </tr>
</table>
 

### Cocos2d

Cocos2d-X 是支持生成 UWP 游戏的跨平台开源游戏开发引擎和工具套件。 从版本 3 开始，还添加了 3D 功能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td>[什么是 Cocos2d-X？](http://www.cocos2d-x.org/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x 程序员指南</td>
        <td>[Cocos2d-x 程序员指南 v3.8](http://www.cocos2d-x.org/programmersguide/)</td>
    </tr>
    <tr>
        <td>Windows 10 上的 Cocos2d-x（博客文章）</td>
        <td>[在 Windows 10 上运行 Cocos2d-x](https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x Windows 应用商店游戏（视频）</td>
        <td>[使用 Cocos2d-x 生成适用于 Windows 设备的游戏](http://www.microsoftvirtualacademy.com/training-courses/build-a-game-with-cocos2d-x-for-windows-devices)</td>
    </tr>
</table>


### Unreal Engine

Unreal Engine 4 是面向所有类型的游戏和开发人员推出的一整套游戏开发工具。 对于要求非常高的控制台和电脑游戏，Unreal Engine 已在全球开发人员中得到广泛的采用。 订阅了 Unreal Engine 4 的 [DirectX 12 提前访问计划](#dxeap)的成员，将可以访问支持 DirectX 12 的 Unreal Engine 4.4 开发项目。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unreal Engine 概述</td>
        <td>[什么是 Unreal Engine 4](https://www.unrealengine.com/what-is-unreal-engine-4)</td>
    </tr>
</table>
 

### 中间件和合作伙伴

可以提供解决方案的其他中间件和引擎合作伙伴有很多，具体取决于游戏开发需要。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 开发人员中心游戏合作伙伴</td>
        <td>[开发人员中心合作伙伴（游戏）](https://devcenterpartners.windows.com/directory#filter=gaming)</td>
    </tr>
    <tr>
        <td>Windows 开发人员中心合作伙伴</td>
        <td>[开发人员中心合作伙伴](https://devcenterpartners.windows.com/directory)</td>
    </tr>
</table>
 

### 移植游戏

如果你拥有一款现有游戏，有很多可用的资源和指南可以帮助你将游戏快速移植到 UWP。 为快速启动移植工作，你也可以考虑使用[通用 Windows 平台桥](#uwp_bridges)

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>将 Windows 8 应用移植到通用 Windows 平台应用</td>
        <td>[从 Windows 运行时 8.x 移动到 UWP](https://msdn.microsoft.com/library/windows/apps/mt238322)</td>
    </tr>
    <tr>
        <td>将 Windows 8 应用移植到通用 Windows 平台应用（视频）</td>
        <td>[将 8.1 应用移植到 Windows 10](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21)</td>
    </tr>
    <tr>
        <td>将 iOS 应用移植到通用 Windows 平台应用</td>
        <td>[从 iOS 移动到 UWP](https://msdn.microsoft.com/library/windows/apps/mt238320)</td>
    </tr>
    <tr>
        <td>将 Silverlight 应用移植到通用 Windows 平台应用</td>
        <td>[从 Windows Phone Silverlight 移动到 UWP](https://msdn.microsoft.com/library/windows/apps/mt238323)</td>
    </tr>
    <tr>
        <td>从 XAML 或 Silverlight 移植到通用 Windows 平台应用（视频）</td>
        <td>[将应用从 XAML 或 Silverlight 移植到 Windows 10](https://channel9.msdn.com/Events/Build/2015/3-741)</td>
    </tr>
    <tr>
        <td>将 Xbox 游戏移植到通用 Windows 平台应用</td>
        <td>[从 Xbox One 移植到 Windows 10 UWP](https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)</td>
    </tr>
    <tr>
        <td>从 DirectX 9 移植到 DirectX 11</td>
        <td>[从 DirectX 9 移植到通用 Windows 平台 (UWP)](porting-your-directx-9-game-to-windows-store.md)</td>
    </tr>
    <tr>
        <td>从 Direct3D 11 移植到 Direct3D 12</td>
        <td>[从 Direct3D 11 移植到 Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709)</td>
    </tr>
    <tr>
        <td>从 OpenGL ES 移植到 Direct3D 11</td>
        <td>[从 OpenGL ES 2.0 移植到 Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)</td>
    </tr>
    <tr>
        <td>使用 ANGLE 将 OpenGL ES 移植到 Direct3D 11</td>
        <td>[ANGLE](http://go.microsoft.com/fwlink/p/?linkid=618387)</td>
    </tr>
    <tr>
        <td>UWP 中的经典 Windows API 等效内容</td>
        <td>[通用 Windows 平台 (UWP) 应用中的 Windows API 替代项](https://msdn.microsoft.com/library/windows/apps/hh464945)</td>
    </tr>
</table>


## 原型和设计


现在已确定要创建的游戏类型以及用于生成游戏的工具和图形技术，可以随时开始进行设计和原型制作。 其核心是，你的游戏是通用 Windows 平台应用，所以你将从这里开始。

### 通用 Windows 平台 (UWP) 简介

Windows 10 引入通用 Windows 平台 (UWP)，该平台在 Windows 10 设备上提供常用 API 平台。 UWP 发展和扩展了 Windows 运行时模型，而使其成为一致、统一的核心。 面向 UWP 的游戏可以调用所有设备公用的 WinRT API。 因为 UWP 提供有保证的核心 API 层，所以你可以选择创建一个将在 Windows 10 设备上安装的应用包。 并且如果你需要，你的游戏仍可以调用特定于运行游戏的设备的 API（包括一些经典的 Win32 和 .NET Windows API）。

UWP 的目标是拥有：

-   一个核心操作系统
-   一个应用程序平台
-   一个游戏社交网络
-   一个应用商店
-   一条引入路径

以下是详细讨论通用 Windows 平台应用的出色指南，建议你阅读这些指南以帮助你了解该平台。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台应用简介</td>
        <td>[什么是通用 Windows 平台应用？](https://msdn.microsoft.com/library/windows/apps/dn726767)</td>
    </tr>
    <tr>
        <td>UWP 概述</td>
        <td>[UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)</td>
    </tr>
</table>
 

### UWP 开发入门

可轻松快速地完成通用 Windows 平台应用开发的准备工作。 以下指南将指导你分步完成该过程。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 开发入门</td>
        <td>[Windows 应用入门](https://dev.windows.com/getstarted)</td>
    </tr>
    <tr>
        <td>开始 UWP 开发</td>
        <td>[准备工作](https://msdn.microsoft.com/library/windows/apps/dn726766)</td>
    </tr>
</table>

如果你是 UWP 编程的“完全初学者”，并考虑在游戏中使用 XAML（请参阅[选择图形技术和编程语言](#choosing_technology)）， [面向完全初学者的 Windows 10 开发](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)视频系列是不错的起点。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>使用 XAML 进行 Windows 10 开发的初学者指南（视频系列）</td>
        <td>[面向完全初学者的 Windows 10 开发](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)</td>
    </tr>
    <tr>
        <td>宣布推出使用 XAML 的 Windows 10 完全初学者系列（博客文章）</td>
        <td>[面向完全初学者的 Windows 10 开发](http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/)</td>
    </tr>
</table>

### UWP 开发概念

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>通用 Windows 平台应用开发概述</td>
        <td>[开发 Windows 应用](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>UWP 中的网络编程概述</td>
        <td>[网络和 Web 服务](https://msdn.microsoft.com/library/windows/apps/mt280378)</td>
    </tr>
    <tr>
        <td>在游戏中使用 Windows.Web.HTTP 和 Windows.Networking.Sockets</td>
        <td>[游戏网络](work-with-networking-in-your-directx-game.md)</td>
    </tr>
    <tr>
        <td>UWP 中的异步编程概念</td>
        <td>[异步编程](https://msdn.microsoft.com/library/windows/apps/mt187335)</td>
    </tr>
</table>
 

### 进程周期管理

进程周期管理（或称应用生命周期）介绍通用 Windows 平台应用可以转换的各种激活状态。 你的游戏不仅可以激活、暂停、恢复或终止，还可以通过多种方式在这些状态中转换。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>处理应用生命周期转换</td>
        <td>[应用生命周期](https://msdn.microsoft.com/library/windows/apps/mt243287)</td>
    </tr>
    <tr>
        <td>使用 Microsoft Visual Studio 触发应用转换</td>
        <td>[如何在 Visual Studio 中为 Windows 应用商店应用触发暂停、恢复和后台事件](https://msdn.microsoft.com/library/hh974425.aspx)</td>
    </tr>
</table>
 

### 设计游戏用户体验

出色的游戏源自于获得灵感的设计。

游戏与应用共享一些通用的用户界面元素和设计原则，不过游戏通常具有独特的外观且以用户体验为设计目标。 当游戏设计充分考虑了以下两方面时，你的游戏必然会取得成功：游戏应何时使用经过测试的用户体验以及应何时打破常规进行创新？ 你为游戏所选择的呈现技术（DirectX、XAML、HTML5 或这三者的任意组合）可能会影响实现细节，但在大多数情况下，你所应用的设计准则不受该选择的影响。

除用户体验设计之外，诸如级别设计、速度、全局设计和其他方面等游戏玩法设计本身就是一种艺术形式：这由你和你的团队决定，本开发指南中未对其进行介绍。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 设计基础知识和指南</td>
        <td>[设计 UWP 应用](https://dev.windows.com/design)</td>
    </tr>
    <tr>
        <td>设计应用生命周期状态</td>
        <td>[启动、暂停和恢复的用户体验指南](https://msdn.microsoft.com/library/windows/apps/dn611862)</td>
    </tr>
    <tr>
        <td>面向多个设备外形规格（视频）</td>
        <td>[为 Windows Core World 设计游戏](http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World)</td>
    </tr>
</table>
 

### 颜色指南和调色板

在游戏中遵循一致的颜色指南，有助于美化外观、辅助导航，并让玩家清楚地了解菜单和 HUD 功能。 一致的游戏元素（如警告、危害、XP 和成就）颜色可使 UI 更简洁明了，同时减少对显式标签的需求。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>颜色指南</td>
        <td>[最佳做法：颜色](https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip)</td>
    </tr>
</table>
 

### 版式

合理使用版式可为游戏带来许多方面的改进，包括 UI 布局、导航、可读性、氛围、品牌以及玩家沉浸式体验。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>版式指南</td>
        <td>[最佳实践：版式](http://go.microsoft.com/fwlink/?LinkId=535007)</td>
    </tr>
</table>
 

### UI 地图

UI 地图是一个游戏导航布局，在其中菜单以流程图的形式呈现。 UI 地图可帮助所有参与其中的利益相关方了解游戏的界面和导航路径，并且可在开发周期的早期便揭露出潜在的障碍和死角。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UI 地图指南</td>
        <td>[最佳实践：UI 地图](http://go.microsoft.com/fwlink/?LinkId=535008)</td>
    </tr>
</table>
 

### DirectX 开发

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 上的 DirectX 游戏开发</td>
        <td>[游戏和 DirectX](index.md)</td>
    </tr>
    <tr>
        <td>与 UWP 应用模型的 DirectX 交互</td>
        <td>[应用对象和 DirectX](about-the-metro-style-user-interface-and-directx.md)</td>
    </tr>
    <tr>
        <td>DirectX 概述和参考</td>
        <td>[DirectX 图形和游戏](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 编程指南和参考</td>
        <td>[Direct3D 12 图形](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>DirectX 12 基础（视频）</td>
        <td>[电源更优，性能更佳：DirectX 12 上的游戏](http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12)</td>
    </tr>
</table>
 

DirectX 工具包、DirectX 纹理处理库和 DirectXMesh 几何图形处理库提供用于 DirectX 开发的纹理、网格、子画面以及其他实用工具功能和帮助程序类。 相比于自己实现这些功能，这些库可以节省大量时间和精力。 尽管主要是为 Direct3D 11 实现，但这些库的其中一部分也可用于 Direct3D 12。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>获取 DirectX 工具包 (DirectX 11)</td>
        <td>[DirectXTK](http://go.microsoft.com/fwlink/?LinkId=248929)</td>
    </tr>
    <tr>
        <td>获取 DirectX 纹理处理库 (DirectX 11)</td>
        <td>[DirectXTex](http://go.microsoft.com/fwlink/?LinkId=248926)</td>
    </tr>
    <tr>
        <td>获取 DirectXMesh 几何图形处理库</td>
        <td>[DirectXMesh](http://go.microsoft.com/fwlink/?LinkID=324981)</td>
    </tr>
    <tr>
        <td>DirectXTK 中的 Direct3D 12 支持（博客文章）</td>
        <td>[对 DirectX 12 的支持](https://github.com/Microsoft/DirectXTK/issues/2)</td>
    </tr>
</table>


## 生产


你的工作室现在完全参与并转向生产周期，并且整个团队都分配了工作。 通过优化、重构和扩展原型，使其成为完整的游戏。

### 通知和动态磁贴

磁贴是游戏在“开始”菜单上的表示形式。 磁贴和通知可以吸引玩家的注意力，即使他们当前并未玩你的游戏也是如此。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>开发磁贴和锁屏提醒</td>
        <td>[磁贴、锁屏提醒和通知](https://msdn.microsoft.com/library/windows/apps/mt185606)</td>
    </tr>
    <tr>
        <td>动态磁贴和通知演示示例</td>
        <td>[通知示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)</td>
    </tr>
    <tr>
        <td>自适应磁贴模板（博客文章）</td>
        <td>[自适应磁贴模板 — 架构和文档](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx)</td>
    </tr>
    <tr>
        <td>设计磁贴和锁屏提醒</td>
        <td>[磁贴和锁屏提醒指南](https://msdn.microsoft.com/library/windows/apps/hh465403)</td>
    </tr>
    <tr>
        <td>交互开发动态磁贴模板的 Windows 10 应用</td>
        <td>[通知可视化工具](https://www.microsoft.com/store/apps/9nblggh5xsl1)</td>
    </tr>
</table>
 

### 启用应用内产品 (IAP) 购买

IAP（应用内产品）是供玩家在游戏中购买的补充项。 IAP 可以是新的加载项、游戏级别、项目或玩家可能喜欢的任何其他内容。 如果使用得当，则 IAP 可以在改进游戏体验的同时，提供收入。 通过 Windows 开发人员中心仪表板定义和发布游戏 IAP， 并支持使用游戏代码进行应用内购买。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>持久型应用内产品</td>
        <td>[启用应用内产品购买](https://msdn.microsoft.com/library/windows/apps/mt219684)</td>
    </tr>
    <tr>
        <td>可消费应用内产品</td>
        <td>[启用可消费应用内产品购买](https://msdn.microsoft.com/library/windows/apps/mt219683)</td>
    </tr>
    <tr>
        <td>应用内产品详细信息和提交</td>
        <td>[IAP 提交](https://msdn.microsoft.com/library/windows/apps/mt148551)</td>
    </tr>
    <tr>
        <td>监视游戏的 IAP 销售和统计数据</td>
        <td>[IAP 购置报告](https://msdn.microsoft.com/library/windows/apps/mt148538)</td>
    </tr>
</table>
 
### 调试和性能监视工具

Windows Performance Toolkit (WPT) 包含各种性能监控工具，这些工具可生成有关 Windows 操作系统和应用程序的详细性能概况。 该工具包在监视内存使用量和改善游戏性能方面尤其有用。 Windows Performance Toolkit 包含在 Windows 10 SDK 和 Windows ADK 中。 该工具包包含两个独立的工具：Windows Performance Recorder (WPR) 和 Windows Performance Analyzer (WPA)。 另外一个用于生成转储文件来调查游戏崩溃的有用工具是 ProcDump，它是 [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default) 的一部分。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>从 Windows 10 SDK 获取 Windows Performance Toolkit (WPT)</td>
        <td>[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)</td>
    </tr>
    <tr>
        <td>从 Windows ADK 获取 Windows Performance Toolkit (WPT)</td>
        <td>[Windows ADK](https://msdn.microsoft.com/windows/hardware/dn913721.aspx)</td>
    </tr>
    <tr>
        <td>使用 Windows Performance Analyzer 对无响应 UI 问题进行疑难解答（视频）</td>
        <td>[使用 WPA 进行关键路径分析](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer)</td>
    </tr>
    <tr>
        <td>使用 Windows Performance Recorder 诊断内存使用量和泄露（视频）</td>
        <td>[内存占用和泄漏](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks)</td>
    </tr>
    <tr>
        <td>获取 ProcDump</td>
        <td>[ProcDump](https://technet.microsoft.com/sysinternals/dd996900)</td>
    </tr>
    <tr>
        <td>了解如何使用 ProcDump（视频）</td>
        <td>[配置 ProcDump 以创建转储文件](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK)</td>
    </tr>
</table>

### 高级 DirectX 技术和概念

部分 DirectX 开发可能需做到细致入微，所以会很复杂。 当你在生产中需要深入了解 DirectX 引擎的细节或调试性能难题时， 本部分中的资源和信息可起到帮助作用。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>优化图形和性能（视频）</td>
        <td>[DirectX 12 高级图形和性能](http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance)</td>
    </tr>
    <tr>
        <td>DirectX 图形调试（视频）</td>
        <td>[使用 DirectX 工具解决游戏的图形难题](http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools)</td>
    </tr>
    <tr>
        <td>Direct3D 12 编程指南</td>
        <td>[Direct3D 12 编程指南](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>组合 DirectX 和 XAML</td>
        <td>[DirectX 和 XAML 互操作](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

Microsoft DirectX 产品团队提供了一系列有关 DirectX 12 开发的有深度的视频。 它们介绍有关资源绑定、演示模式、调试、资源障碍和其他许多 DirectX 12 概念的详细信息。 此系列有时还会有嘉宾主持人。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>图形和 DirectX 12 开发视频（YouTube 频道）</td>
        <td>[Microsoft DirectX 12 和图形教育](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
    <tr>
        <td>Direct3D 12 资源和堆管理（视频）</td>
        <td>[DirectX 12 中的堆和资源](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
</table>
 

### 全球化和本地化

针对 Windows 平台开发全球通用的游戏，并了解内置于 Microsoft 热门产品中的国际功能。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>为全球市场准备你的游戏</td>
        <td>[面向全球受众进行开发的指南](https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx)</td>
    </tr>
    <tr>
        <td>将语言、文化与技术桥接在一起</td>
        <td>[语言约定和标准 Microsoft 术语的联机资源](http://www.microsoft.com/Language/Default.aspx)</td>
    </tr>
</table>


## 提交和发布游戏


以下指南和信息可帮助你尽可能顺利地完成发布和提交过程。

### 打包和上载

你将使用新的统一 Windows 开发人员中心仪表板发布和管理游戏程序包。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 开发人员中心应用发布</td>
        <td>[发布 Windows 应用](https://dev.windows.com/publish)</td>
    </tr>
    <tr>
        <td>对你的游戏分级（博客文章）</td>
        <td>[使用 IARC 系统分配年龄分级的单一工作流](https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/)</td>
    </tr>
    <tr>
        <td>打包游戏</td>
        <td>[打包 UWPDirectX 游戏](package-your-windows-store-directx-game.md)</td>
    </tr>
    <tr>
        <td>以第三方开发人员身份打包游戏（博客文章）</td>
        <td>[在不使用发布者的应用商店帐户访问权限的情况下创建可上载的程序包](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/)</td>
    </tr>
    <tr>
        <td>上载游戏和控制游戏版本</td>
        <td>[上载应用包](https://msdn.microsoft.com/library/windows/apps/mt148542)</td>
    </tr>
</table>
 

### 策略和认证

不要让认证问题延迟游戏发布。 以下是需要注意的策略和常见认证问题。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 应用商店应用开发人员协议</td>
        <td>[应用开发人员协议](https://msdn.microsoft.com/library/windows/apps/hh694058)</td>
    </tr>
    <tr>
        <td>在 Windows 应用商店中发布应用的策略</td>
        <td>[Windows 应用商店策略](https://msdn.microsoft.com/library/windows/apps/dn764944)</td>
    </tr>
    <tr>
        <td>如何避免一些常见的应用认证问题</td>
        <td>[避免常见的认证失败](https://msdn.microsoft.com/library/windows/apps/jj657968)</td>
    </tr>
</table>
 

### 应用商店清单 (StoreManifest.xml)

应用商店清单 (StoreManifest.xml) 是一种可选的配置文件，可包含在应用包中。 应用商店清单提供不属于 AppxManifest.xml 文件的其他功能。 例如，如果目标设备不具备指定的最低 DirectX 功能级别或指定的最小系统内存，你可以使用应用商店清单阻止游戏安装。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>应用商店清单架构</td>
        <td>[应用商店清单架构 (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335)</td>
    </tr>
</table>
 

## 游戏生命周期管理


完成开发并交付游戏并不意味着“游戏结束”。 你可能完成了一个版本的开发，但你在市场中的游戏之路才刚刚开始。 你会想要监视使用情况和错误报告、回复用户反馈以及发布游戏更新。

### Windows 开发人员中心分析和推广

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 开发人员中心分析</td>
        <td>[分析](https://msdn.microsoft.com/library/windows/apps/mt148522)</td>
    </tr>
    <tr>
        <td>回复客户评论</td>
        <td>[回复客户评论](https://msdn.microsoft.com/library/windows/apps/mt148546)</td>
    </tr>
    <tr>
        <td>推广游戏的方法</td>
        <td>[推广你的应用](https://dev.windows.com/store-promotion)</td>
    </tr>
</table>
 

### Visual Studio Application Insights

Visual Studio Application Insights 提供关于发布的游戏的性能、遥测和使用情况分析。 Application Insights 可帮助你检测和解决游戏发布后出现的问题、持续监视和改善使用情况，并了解玩家如何继续与游戏交互。 Application Insights 的工作原理是向你的应用添加一个 SDK，它会将遥测数据发送到 [Azure 门户](http://portal.azure.com/)

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>应用程序性能和使用情况分析</td>
        <td>[Visual Studio Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)</td>
    </tr>
    <tr>
        <td>在 Windows 应用中启用 Application Insights</td>
        <td>[适用于 Windows Phone 和应用商店应用的 Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/)</td>
    </tr>
</table>
 

### 创建和管理内容更新

若要更新发布的游戏，请提交带有更高版本号的新应用包。 程序包通过提交和认证后将自动作为更新向客户提供。

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>更新和控制游戏版本</td>
        <td>[程序包版本编号](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
    <tr>
        <td>游戏程序包管理指南</td>
        <td>[应用包管理指南](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
</table>


## 将 Xbox Live 添加到游戏


> **注意** Xbox Live 开发通过 ID@Xbox 和 Microsoft Studios 等计划进行管理。 本指南涵盖范围广泛的资源，你可能会发现有些资源无法访问，具体取决于你所参与的计划或特定的开发角色。 示例是解析为 developer.xboxlive.com、forums.xboxlive.com、xdi.xboxlive.com 或游戏开发人员网络 (GDN) 的链接。 有关与 Microsoft 合作的信息，请参阅[开发人员计划](#programs)

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>下载最新的 Xbox Live SDK</td>
        <td>[Xbox Live SDK](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>将 Xbox Live 添加到通用 Windows 平台应用</td>
        <td>[操作方法：将 Xbox Live SDK 添加到通用 Windows 平台 (UWP) 应用](http://aka.ms/xsapi2uwp)</td>
    </tr>
    <tr>
        <td>使用 Xbox Live 的游戏的要求</td>
        <td>[Windows 10 Xbox Live 的 Xbox 要求](http://go.microsoft.com/fwlink/?LinkId=533217)</td>
    </tr>
    <tr>
        <td>Xbox Live 游戏开发概述（视频）</td>
        <td>[使用适用于 Windows 10 的 Xbox Live 进行开发](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10)</td>
    </tr>
    <tr>
        <td>跨平台比赛（视频）</td>
        <td>[Xbox Live 多玩家：介绍跨平台比赛和游戏服务](http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay)</td>
    </tr>
    <tr>
        <td>Fable Legends 中的跨设备玩法（视频）</td>
        <td>[Fable Legends：使用 Xbox Live 进行跨设备游戏](http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live)</td>
    </tr>
    <tr>
        <td>Xbox Live 统计数据和成就（视频）</td>
        <td>[在 Xbox Live 中利用基于云的用户统计数据和成就的最佳实践](http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live)</td>
    </tr>
</table>
 

## 其他资源

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>独立的游戏开发（视频）</td>
        <td>[独立开发人员的新机遇](http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers)</td>
    </tr>
    <tr>
        <td>多核移动设备的注意事项（视频）</td>
        <td>[多核移动设备中持续的游戏性能](http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices)</td>
    </tr>
    <tr>
        <td>开发 Windows 10 桌面游戏（视频）</td>
        <td>[Windows 10 的电脑游戏](http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10)</td>
    </tr>
</table>



 

 

 


<!--HONumber=May16_HO2-->


