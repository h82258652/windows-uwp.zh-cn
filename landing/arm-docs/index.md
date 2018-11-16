---
layout: LandingPage
description: 本页提供了有关你可以开始开发 ARM64 win32 和 UWP 应用的信息。
title: 基于 ARM 的 Windows 10
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: 基于 ARM、 ARM、 构建 win32 ARM64 应用构建 ARM64 驱动程序的 Windows 10
ms.openlocfilehash: 83f2a0d03040a682e6965558174294fe27e21bfb
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7111421"
---
# <a name="windows-10-on-arm"></a>基于 ARM 的 Windows 10
在 ARM 处理器受支持的电脑上运行 Windows 10。 本页提供了有关你可以了解有关平台的详细信息并开始开发应用的信息。 我们还鼓励你通过使用在页面底部的链接提供你的反馈。

## <a name="introductory-videos"></a>入门视频
观看并了解如何在 ARM 上运行 Windows 10。

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>生成 ARM64 Win32 c + + 应用</h3><p>了解如何安装适用于 Visual Studio ARM64 工具。 然后我们将指导你完成创建和编译的新的 ARM 64 项目的步骤。</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>面向开发人员的 ARM 上生成 2018 Windows 10</h3><p>了解有关 Windows 10 on ARM 设备，如何神奇的 x86 模拟的工作原理，以及最后如何提交和 ARM 上生成适用于 Windows 10 的应用。 我们将介绍如何生成 ARM64 的桌面应用和 UWP 的应用。</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>Windows 社区站立与古柯 Gallo</h3><p>更深入了解如何在 ARM64 上, 运行 Windows 10 和熟悉应用和此平台上的体验。</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>基于 ARM 的了解 Windows 10
获取通过查看这些资源了解平台。

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
                    <p class="x-hidden-focus">请查看文档，了解基础知识。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="主题大约 x86 模拟" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>了解如何 x86 模拟的工作原理</h3>
                    <p class="x-hidden-focus">了解有关此密钥功能基于 ARM 的 Windows 10 的所有问题。</p>
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
在 ARM 上定制你的应用到 Windows 10 开始菜单和那里充分利用可用的功能。  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="生成 ARM64 应用" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>生成 ARM64 sdk 的新应用</h3>
                    <p class="x-hidden-focus">请查看其中我们指导你完成你的应用编译为 ARM64 本机在基于 ARM 的 Windows 10 上运行的这篇博客文章。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="Arm32 应用疑难解答" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>在 ARM 上的 UWP 应用</h3>
                    <p class="x-hidden-focus">请按照本指南来设置你针对成功的通用 Windows 平台 (UWP) 应用。</p>                    
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
                    <p class="x-hidden-focus">获取你在基于 ARM 的 Windows 10 上运行顺畅的代码。</p>
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
                    <p class="x-hidden-focus">将你的驱动程序重新编译为 ARM64。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="疑难解答 x86 应用" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>x86 应用在 ARM 上</h3>
                    <p class="x-hidden-focus">开发你的 x86 应用在 ARM 上的 Windows 10 上执行其最。</p>
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

## <a name="let-us-know-if-you-have-feedback"></a>让我们知道是否你有的反馈
通过利用你和现有客户反馈，我们正在不断提升我们的产品。 如果你有想法、 堵塞问题，或只是想要如何出色共享是你的体验时，这些链接将帮助你。

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>使用反馈中心</h3>
                <p>我们没有错过某些内容？ 你是否拥有一个好主意？ 让我们知道在反馈中心中。</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>报告错误</h3>
                <p>在我们的平台中找到 bug？ 发送电子邮件至我们的详细信息。</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>提供文档反馈</h3>
                <p>你已使用我们的文档中找到问题？ 你是否要我们来创造更清楚内容？ 在我们的文档 GitHub 存储库上创建一个问题。</p>
            </div>
        </a>
    </li>
</ul>