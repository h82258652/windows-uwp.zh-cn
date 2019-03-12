---
layout: LandingPage
description: 本页提供了开始开发 ARM64 win32 和 UWP 应用时所需的信息。
title: 基于 ARM 的 Windows 10
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: '基于 ARM 的 Windows 10, ARM, 构建 win32 ARM64 应用, 构建 ARM64 驱动程序'
---

# <a name="windows-10-on-arm"></a>基于 ARM 的 Windows 10
Windows 10 在由 ARM 处理器提供动力的电脑上运行。 本页还提供了有关详细了解此平台以及开始开发应用的信息。 我们还鼓励你使用页面底部的链接来提供反馈。

## <a name="introductory-videos"></a>介绍性视频
观看并了解 Windows 10 如何在 ARM 上运行。

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>构建 ARM64 Win32 C++ 应用</h3><p>了解如何安装 ARM64 tools for Visual Studio。 然后，我们将引导你完成创建并编译新的 ARM 64 项目的步骤。</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>Build 2018：基于 ARM 的 Windows 10：面向开发人员的主题</h3><p>了解 ARM 设备上的 Windows 10，以及神奇的 x86 仿真的工作原理，最后了解如何针对基于 ARM 的 Windows 10 提交和构建应用。 我们将展示如何构建适用于桌面设备和 UWP 的 ARM64 应用。</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>Kevin Gallo 主持的 Windows Community Standup</h3><p>深入了解 Windows 10 如何在 ARM64 上运行，并了解此平台上的应用和体验。</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>了解基于 ARM 的 Windows 10
通过查看以下资源来了解此平台。

<ul class="cardsF panelContent cols cols2">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm" title="入门" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Get started icon" src="/media/common/i_get-started.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>基于 ARM 的 Windows 10 入门</h3>
                    <p class="x-hidden-focus">查看此文档来了解基础知识。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="有关 x86 仿真的主题" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>了解 x86 仿真的工作原理</h3>
                    <p class="x-hidden-focus">了解基于 ARM 的 Windows 10 的这一关键功能的所有信息。</p>
                </div>
            </div>
        </div>
    </li>
    <!--<li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.msdn.microsoft.com/harip/" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="" src="/media/common/i_blog.svg?branch=master" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Read the Kernel blog</h3>
                    <p class="x-hidden-focus">Get a deep understanding of the Windows by reading articles that are written by the creators of the kernel.</p>
                </div>
            </div>
        </div>
    </li>-->
</ul>

## <a name="developing-for-windows-10-on-arm"></a>针对基于 ARM 的 Windows 10 进行开发
开始定制你的应用，使之适应基于 ARM 的 Windows 10，并充分利用该系统提供的功能。  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="构建 ARM64 应用" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>使用 SDK 构建 ARM64 应用</h3>
                    <p class="x-hidden-focus">查看此博客文章，我们在其中引导你将应用编译为 ARM64 应用，以便以原生方式在基于 ARM 的 Windows 10 上运行。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="arm32 应用疑难解答" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM 上的 UWP 应用</h3>
                    <p class="x-hidden-focus">按照此指南设置通用 Windows 平台 (UWP) 应用以确保成功。</p>                    
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/debugger/debugging-arm64" title="调试 ARM64 应用" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="Debugging on ARM icon" src="/media/common/i_debug.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>在 ARM 上进行调试</h3>
                    <p class="x-hidden-focus">使你的代码在基于 ARM 的 Windows 10 上平稳运行。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/develop/building-arm64-drivers" title="构建 ARM64 驱动程序" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Building ARM64 drivers icon" src="/media/common/i_drivers.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>使用 WDK 构建 ARM64 驱动程序</h3>
                    <p class="x-hidden-focus">针对 ARM64 重新编译驱动程序。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="x86 应用疑难解答" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM 上的 x86 应用</h3>
                    <p class="x-hidden-focus">开发 x86 应用并使之在基于 ARM 的 Windows 10 上发挥最佳性能。</p>
                </div>
            </div>
        </div>
    </li>
</ul>

<!--## Other videos
<ul class="cols cols4">
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
</ul>-->

## <a name="let-us-know-if-you-have-feedback"></a>让我们知道你是否有反馈
我们持续利用你和我们的现有客户提供的反馈来改进我们的产品。 如果你有某个主意，被某个问题难住，或者只是想分享你的经历有多棒，这些链接将对你有所帮助。

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>使用反馈中心</h3>
                <p>我们漏掉了某些内容？ 你有很棒的主意？ 请在反馈中心内让我们知道。</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>报告 bug</h3>
                <p>在我们的平台中发现了 bug？ 请通过电子邮件向我们发送详细信息。</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>提供文档反馈</h3>
                <p>发现了与我们的文档相关的问题？ 希望我们使某些内容更为清晰？ 请在我们的文档 GitHub 存储库上创建问题。</p>
            </div>
        </a>
    </li>
</ul>