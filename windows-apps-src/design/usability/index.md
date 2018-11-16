---
description: 了解如何使你的应用针对全世界用户都具有包容性和辅助性。
keywords: uwp 应用辅助功能, 全球化, 设计包容应用, 辅助功能应用要求
title: UWP 应用中的可用性 - Windows 应用开发
author: mijacobs
layout: LandingPage
template: detail.hbs
ms.author: mijacobs
ms.date: 10/18/2017
ms.topic: landing-page
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: 80f57ba29d31da0b4d49b620e916e39d4ef69842
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6847245"
---
# <a name="usability-for-uwp-apps"></a>UWP 应用中的可用性



这些较小的优化额外关注了细节方面，可将良好的用户体验转换为一次能满足全球用户需求的真正具有包容性的用户体验。

本部分中的设计和编码说明可以帮助你使你的 UWP 应用更具包容性，方法是添加辅助功能、支持全球化和本地化、允许用户自定义其体验，以及随时为用户提供帮助。


## <a name="accessiblity"></a>辅助功能

辅助功能是指应用可供那些对使用常规用户界面存在限制（阻碍或阻止他们使用）的用户使用。 对于某些情况，辅助功能要求是由法律强制实施的。 但是，无论法律是否有规定，最好都解决辅助功能问题，以尽可能扩大应用的受众范围。

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">辅助功能概述</a></b> <br/> 本文概述了与 UWP 应用的辅助功能方案相关的概念和技术。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">设计非独占软件</a></b><br/>了解如何改进适用于 Windows 10 的通用 Windows 平台 (UWP) 应用的非独占设计。  以辅助功能为中心来设计和生成非独占软件。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">开发非独占 Windows 应用</a></b><br/> 本文是开发辅助 UWP 应用的路线图。</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">辅助功能测试</a> </b><br/>为确保 UWP 应用为辅助应用所要遵循的测试过程。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">应用商店中的辅助功能</a></b><br/>介绍有关在 Microsoft Store 中将 UWP 应用声明为辅助应用的要求。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">辅助功能清单</a></b><br/>提供了可帮助你确保 UWP 应用为辅助应用的清单。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">公开基本的辅助功能信息</a></b><br/>基本的辅助功能信息通常按照名称、角色和值进行分类。 本主题介绍可帮助应用公开辅助技术所需的基本信息的代码。</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">键盘辅助功能</a></b><br/>如果应用未提供良好的键盘访问，则盲人用户或行动不便的用户在使用该应用时会存在困难，或者可能根本无法使用该应用。</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">高对比度主题</a></b><br/>介绍了为确保 UWP 应用在高对比度主题处于活动状态时可供使用所需的步骤。 </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">辅助文本要求</a></b><br/>本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。 本主题还讨论了 UWP 应用中的文本元素可以具有的 Microsoft UI 自动化角色，以及图形中文本的最佳做法。</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">要避免的辅助功能做法</a></b><br/>列出创建辅助的 UWP 应用时应避免的做法。</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">自定义自动化对等</a></b><br/>介绍 UI 自动化的自动化对等概念以及如何为自己的自定义 UI 类提供自动化支持。</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">控件模式和接口</a></b><br/>列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>


## <a name="globalization-and-localization"></a>全球化和本地化

Windows 的使用遍及世界各地，用户的语言、区域及文化背景各不相同。 用户讲各种不同的语言，位于不同的国家和地区。 某些用户会说多种语言。 因此，应用在涉及语言、区域和文化系统设置的多种组合的配置上运行。 通过使用*全球化*和*本地化*对应用进行设计使其更具有适应性，拓展其潜在市场。

<a href="../globalizing/globalizing-portal.md">全球化和本地化门户</a>

## <a name="app-settings"></a>应用设置

借助应用设置，用户可以自定义你的应用，从而可以针对其个人需求和偏好来优化它。 通过提供适当的设置并妥善存储它们，可使用户体验更加出色。

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">指南</a></b><br/>有关创建和显示应用设置的最佳做法。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">存储和检索应用数据</a></b><br/>如何存储和检索本地、漫游和临时应用数据。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>


## <a name="in-app-help"></a>应用内帮助
无论你设计的应用有多好，某些用户仍将需要一些额外帮助。

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">应用帮助指南</a></b><br/>应用程序可能会很复杂，而为用户提供有效的帮助可大幅改善他们的体验。
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">说明性 UI</a></b><br/>有时，向用户解释应用中他们不熟悉的功能（例如特定的触摸交互）会很有用。 在这些情况下，你需要通过 UI 向用户显示说明，以便他们可以发现并使用可能错过的功能。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">应用内帮助</a></b><br/>在大多数情况下，最好在用户选择查看时在应用程序内显示帮助。 在创建应用内帮助时，请考虑以下指南。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">外部帮助</a></b><br/>在大多数情况下，最好在用户选择查看时在应用程序内显示帮助。 在创建应用内帮助时，请考虑以下指南。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul>

