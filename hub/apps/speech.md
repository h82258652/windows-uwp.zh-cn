---
title: Windows 10 中的语音、语音和对话
description: 本页提供有关如何开始开发已启用语音的 Windows 应用程序的信息。
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10 中的语音，语音，语音，对话，win32 语音应用程序，UWP speech apps，WPF speech apps，WinForms speech apps
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 7ac8d782591ce8f3716e491714c4cbf241e80b6c
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726547"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10 中的语音、语音和对话

![语音英雄图像](images/hero-speech-composite-small.png)

语音可以是一种有效、自然和愉快的方式，用户可以使用鼠标、键盘、触摸、控制器或手势与 Windows 应用程序、补充甚至替换传统交互体验。

基于语音的功能（如语音识别、听写、语音合成（也称为文本到语音转换或 TTS）以及会话语音助手（如 Cortana 或 Alexa）可以提供可访问的、包含的用户体验，使用户能够使用你的应用程序可能无法满足其他输入设备的要求。

本页提供各种 Windows 开发框架如何为生成 Windows 应用程序的开发人员提供语音识别、语音合成和会话支持的相关信息。

## <a name="platform-specific-documentation"></a>플랫폼별 설명서

:::row:::
   :::column:::
      ![UWP(유니버설 Windows 플랫폼)](images/platform-uwp.png)

      **UWP(유니버설 Windows 플랫폼)**

      在适用于 Windows 10 应用程序和游戏的新式平台上，在任何 Windows 设备（包括电脑、手机、Xbox One、HoloLens 等）上生成启用了语音功能的应用，并将其发布到 Microsoft Store。

      [음성 조작](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)

      [음성 인식](https://docs.microsoft.com/windows/uwp/design/input/speech-recognition)

      [연속 받아쓰기](https://docs.microsoft.com/windows/uwp/design/input/enable-continuous-dictation)

      [语音合成](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis)

      [会话代理](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)

      [Cortana 语音命令](https://docs.microsoft.com/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Win32 플랫폼 앱](images/platform-win32.png)

      **Win32 플랫폼**

      使用此处提供的工具、信息、示例引擎和应用程序，为 Windows 桌面和 Windows Server 开发支持语音功能的应用程序。

      [Microsoft 语音平台-软件开发工具包（SDK）（版本11）](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [Microsoft Speech SDK，版本5。1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      XAML UI 모델 및 .NET Framework를 사용하여 관리형 Windows 애플리케이션용으로 설정된 플랫폼에서 접근성 지원 앱과 도구를 개발합니다.

      [.NET Framework 的系统语音编程指南](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Azure 语音服务](images/platform-azure-speech.png)

      **Azure 语音服务**

      通过 Azure 语音服务设计、构建和测试可访问的网站。

      [语音到文本](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [文本到语音转换](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [语音翻译](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [语音首次虚拟助手](https://docs.microsoft.com/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **旧功能**

      旧的、不推荐使用和/或不受支持的 Microsoft Speech 技术版本。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Microsoft 代理](https://docs.microsoft.com/windows/win32/lwef/microsoft-agent)

      [Microsoft Speech Application 软件开发工具包（SASDK）版本1。0](https://www.microsoft.com/download/details.aspx?id=2200)
   :::column-end:::
   :::column:::
      [Microsoft 语音 API （SAPI）5。3](https://docs.microsoft.com/previous-versions/windows/desktop/ms723627(v=vs.85))

      [Microsoft 语音 API （SAPI）5。4](https://docs.microsoft.com/previous-versions/windows/desktop/ee125663(v=vs.85))

      [必应语音识别控件](https://docs.microsoft.com/previous-versions/bing/speech/dn434583(v%3dmsdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>샘플

다양한 접근성의 특징과 기능을 보여 주는 Windows 샘플 전체를 다운로드하고 실행합니다.

:::row:::
   :::column:::
      [코드 샘플 브라우저](https://docs.microsoft.com/samples/browse/?term=speech)

      新示例浏览器（替换 MSDN 代码库）。
   :::column-end:::
   :::column:::
      [GitHub의 Windows 클래식 샘플](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      이러한 샘플에서는 Windows 및 Windows Server의 기능 및 프로그래밍 모델을 보여 줍니다. 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub의 UWP(유니버설 Windows 플랫폼) 코드 샘플](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      이러한 샘플에서는 Windows 10용 Windows SDK(소프트웨어 개발 키트)의 UWP(유니버설 Windows 플랫폼)에 대한 API 사용 패턴을 보여 줍니다.
   :::column-end:::
   :::column:::
      [XAML 컨트롤 갤러리](https://github.com/microsoft/Xaml-Controls-Gallery)

      이 앱에서는 흐름 디자인 시스템에서 지원되는 다양한 Xaml 컨트롤을 보여 줍니다.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>비디오

涵盖了如何构建合并语音交互的 Windows 应用程序的各种视频。

:::row:::
   :::column:::
      **Cortana 및 음성 플랫폼 세부 정보**
   :::column-end:::
   :::column:::
      **通用 Windows 应用中的 Cortana 扩展性**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>다른 리소스

:::row:::
   :::column:::
      **블로그 및 뉴스**

      Microsoft 语音领域的最新功能。
   :::column-end:::
   :::column:::
      **커뮤니티 및 지원**

      Windows 开发人员和用户的会面，并一起学习。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [뉴스](https://news.microsoft.com/?s=speech)

      [语音博客](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Windows 社区-语音](https://community.windows.com/search?q=speech)

      [Windows 语音开发人员论坛](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::
