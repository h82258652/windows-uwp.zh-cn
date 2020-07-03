---
description: 了解如何开发 UWP 应用。
title: 开发 UWP 应用
keywords: UWP 应用开发 线程处理 异步平台概述 门户 开发 开发人员
ms.date: 03/29/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 86fa9fb7f2cea7f190640b02fdcf219c3376115d
ms.sourcegitcommit: 6cdba316bdbd85a2429259ebfb59ff94440e234a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85882849"
---
# <a name="develop-uwp-apps"></a>开发 UWP 应用

用于创建适用于 Windows 10 的 UWP 应用的操作方法文章和代码。

:::row:::
    :::column:::
        <a href="/windows/uwp/get-started/universal-application-platform-guide">
            <img src="https://docs.microsoft.com//media/hubs/windows/win_developer-uwp.svg" alt="UWP overview" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/windows/uwp/get-started/universal-application-platform-guide">通用 Windows 平台概述</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">说明 UWP 是什么、它的工作方式以及它所提供的功能。</p>
    :::column-end:::
    :::column:::
        <a href="/windows/uwp/porting/index">
            <img src="https://docs.microsoft.com/media/illustrations/teams-fast-track.svg" alt="Porting guide" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/windows/uwp/porting/index">移植指南</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">将现有的 Windows 窗体、WPF、Android 或 iOS 应用带到 UWP 中。</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <a href="/windows/uwp/get-started/universal-application-platform-guide" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">                 
                            <img src="https://docs.microsoft.com//media/hubs/windows/win_developer-uwp.svg" alt="UWP overview"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Overview of the Universal Windows Platform</h3>
                        <p>An explanation of what UWP is, how it works, and the features it provides.</p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
    <li>
        <a href="/windows/uwp/porting/index" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">                
                            <img src="https://docs.microsoft.com/media/illustrations/teams-fast-track.svg" alt="Porting guide" />
                        </div>
                    </div>                
                    <div class="cardText">
                        <h3>Porting guide</h3>
                        <p>Bring your existing Windows Forms, WPF, Android, or iOS app to UWP. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>                 
</ul> -->

## <a name="api-reference"></a>API 参考

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/uwp/api">Windows UWP 命名空间</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">构成 Windows 运行时且按命名空间组织的类、结构、接口、方法、属性和事件。</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/uwp/schemas/">UWP 架构</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">通用 Windows 平台 (UWP) 应用的文件和 XML 架构规范。</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <a href="/uwp/api" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Windows UWP namespaces</h3>
                        <p>The classes, structures, interfaces, methods, properties, and events that make up the Windows Runtime, organized by namespace.</p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
    <li>
        <a href="/uwp/schemas/" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Schemas for UWP</h3>
                        <p>File and XML schema specifications for Universal Windows Platform (UWP) apps. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>                 
</ul> -->

## <a name="articles"></a>文章

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">应用类型</h3>
        <a href="/windows/uwp/apps-for-education/">教育应用</a><br/>
        <a href="/windows/uwp/enterprise/">企业应用</a><br/>
        <a href="/windows/uwp/gaming/">游戏和 DirectX 应用</a><br/>
        <a href="/microsoft-edge/progressive-web-apps">渐进式 Web 应用</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">应用 UI</h3>
        <a href="https://developer.microsoft.com/windows/apps/design">有关控件、布局、版式、动画、可用性和 UI 设计，请参阅“设计和 UI”部分。</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">通信</h3>
        <a style="display:block" href="/windows/uwp/app-to-app/">应用到应用的通信</a><br/>
        <a style="display:block" href="/windows/uwp/networking/">网络和 Web 服务</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">数据和文件</h3>
        <a href="/windows/uwp/audio-video-camera/">音频、视频和相机</a><br/>
        <a href="/windows/uwp/data-access/" style="display:block" >数据访问</a><br/>
        <a href="/windows/uwp/data-binding/"style="display:block" >数据绑定</a><br/>
        <a href="/windows/uwp/files/" style="display:block" >文件、文件夹和库</a><br/>
        <a href="/windows/uwp/machine-learning/">机器学习</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">部署</h3>
        <a href="/windows/uwp/updates-and-versions/choose-a-uwp-version">选择 UWP 版本</a><br/>
        <a href="/windows/uwp/debug-test-perf/">调试、测试和性能</a><br/>
        <a href="/windows/uwp/monetize/">盈利、参与度和 Microsoft Store 服务</a><br/>
        <a href="/windows/uwp/packaging/">打包应用</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">平台</h3>
        <a href="/windows/uwp/cpp-and-winrt-apis/">C++/WinRT</a><br/>
        <a href="/windows/uwp/launch-resume/">启动、恢复和后台任务</a><br/>
        <a href="/windows/uwp/security/">安全性</a><br/>
        <a href="/windows/uwp/threading-async/">线程和异步编程</a><br/>
        <a href="/windows/uwp/composition/visual-layer">可视化层</a><br/>
        <a href="/windows/uwp/updates-and-versions/application-development-for-windows-as-a-service">Windows 即服务</a><br/>
        <a href="/windows/uwp/winrt-components/">Windows 运行时组件</a><br/>
        <a href="/windows/uwp/xaml-platform/">XAML 平台</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">人物和地点</h3>
        <a href="/windows/uwp/contacts-and-calendar/">联系人、我的人脉和日历</a><br/>
        <a href="/windows/uwp/maps-and-location/">地图与位置</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">外设、传感器和电源</h3>
        <a href="/windows/uwp/contacts-and-calendar/">概述</a><br/>
        <a href="/windows/uwp/devices-sensors/enable-device-capabilities">启用设备功能</a><br/>
        <a href="/windows/uwp/devices-sensors/pair-devices">设备配对</a><br/>
        <a href="/windows/uwp/devices-sensors/point-of-service">服务点</a><br/>
        <a href="/windows/uwp/devices-sensors/sensors">传感器</a><br/>
        <a href="/windows/uwp/devices-sensors/printing-and-scanning">打印</a><br/>
        <a href="/windows/uwp/devices-sensors/3d-printing">3D 打印</a><br/>
        <a href="/windows/uwp/devices-sensors/nfc">NFC</a><br/>
        <a href="/windows/uwp/devices-sensors/get-battery-info">电池信息</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">移植</h3>
        <a href="/windows/uwp/porting/">概述</a><br/>
        <a href="/windows/uwp/porting/wpsl-to-uwp-root">从 Windows Phone Silverlight 到 UWP</a><br/>
        <a href="/windows/uwp/porting/w8x-to-uwp-root">从 Windows 运行时 8.x 到 UWP</a><br/>
        <a href="/windows/uwp/porting/desktop-to-uwp-root">桌面桥</a><br/>
        <a href="/windows/uwp/porting/desktop-to-uwp-migrate">在桌面应用和 UWP 之间共享代码</a><br/>
        <a href="/windows/uwp/porting/android-ios-uwp-map">针对 Android 和 iOS 开发人员的概念映射</a><br/>
        <a href="/windows/uwp/porting/ios-to-uwp-root">从 iOS 移到 UWP</a><br/>
        <a href="/microsoft-edge/progressive-web-apps">将 Web 应用转换为 PWA</a><br/>
        <a href="/windows/uwp/porting/apps-on-arm">基于 ARM 的 Windows 10</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">进程和线程处理</h3>
        <a href="/windows/uwp/launch-resume/">启动、恢复和后台任务</a><br/>
        <a href="/windows/uwp/threading-async/">线程和异步编程</a><br/><br/><br/>
    :::column-end:::
:::row-end:::


 ## <a name="samples-and-tools"></a>示例和工具

 :::row:::
    :::column:::
        <a href="https://developer.microsoft.com/windows/samples">
            <img src="https://docs.microsoft.com/media/illustrations/sql-database-develop.svg" alt="Samples" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="https://developer.microsoft.com/windows/samples">示例</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">了解如何通过试用这些示例构建适用于 Windows 的出色应用。 这些示例将向你展示功能的工作原理并帮助你开始构建自己的 UWP 应用。</p>
    :::column-end:::
    :::column:::
        <a href="https://developer.microsoft.com/windows/downloads">
            <img src="https://docs.microsoft.com/media/illustrations/sql-get-started-download.svg" alt="Developer tools" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="https://developer.microsoft.com/windows/downloads">开发人员工具</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">获取 Visual Studio 2019、Windows 10 SDK 和其他开发人员工具。</p>
    :::column-end:::
:::row-end:::
