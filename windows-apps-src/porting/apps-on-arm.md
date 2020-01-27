---
title: ARM 기반 Windows 10
description: 이 문서에서는 환경 및 앱 ARM에서 실행되는 방식과 제한 사항이 무엇인지, 자세히 알아볼 수 있는 위치에 대한 개요를 제공합니다.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 항상 연결, ARM, ARM64, x86 에뮬레이션
ms.localizationpriority: medium
ms.openlocfilehash: 1a72fbaf4982f2a053298f10279eacba6a46d05d
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726000"
---
# <a name="windows-10-on-arm"></a>ARM 기반 Windows 10
원래 Windows 10 Mobile과 달리 원래 Windows 10은 x86 및 x64 프로세서 기반 PC에서만 실행할 수 있었습니다. 现在，Windows 10 桌面可以在使用秋季创建者更新或更新版本的 ARM64 处理器的计算机上运行。 ARM CPU 아키텍처의 절전 특징은 하루 종일 배터리 사용 시간을 유지하고 모바일 데이터 네트워크를 지원할 수 있게 합니다. 이러한 PC는 훌륭한 응용 프로그램의 호환성을 제공하고 기존 x86 win32 응용 프로그램을 수정하지 않고 실행할 수 있도록 합니다. 有关详细信息或演示，请查看 "始终连接的 PC" 的第[9 频道视频](https://channel9.msdn.com/Events/Build/2017/P4171)。

여기에서는 데스크톱 버전 ARM64(흔히 *AArch64*라고도 함) 프로세서 기반 Windows 10을 실행하는 PC를 짧게 칭하여 *ARM*이라고 부릅니다.  여기에서는 32비트 ARM 아키텍처(다른 설명서에서 일반적으로 *ARM*이라고 함)를 줄여 *ARM32*라는 용어를 사용합니다.

## <a name="apps-and-experiences-on-arm"></a>ARM 기반 앱 및 환경

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>기본 제공 Windows 10 환경, 앱 및 드라이버
Windows 10 的内置体验（例如，Edge、Cortana、开始菜单和资源管理器）都是本机的，并作为 ARM64 运行。 这还包括所有设备驱动程序，如图形、网络或硬盘。 这可确保你获得最佳用户体验，并以 Qualcomm Snapdragon 处理器的完全本机速度运行设备中的电池寿命。

### <a name="universal-windows-platform-uwp-apps"></a>UWP(유니버설 Windows 플랫폼) 앱
ARM 上的 Windows 10 从 Microsoft Store 运行所有 x86、ARM32 和 ARM64 [UWP 应用](../get-started/universal-application-platform-guide.md)。 ARM32 和 ARM64 应用在无需任何模拟的情况下运行，而 x86 应用在仿真下运行。 UWP 개발자인 경우 이 디바이스에 대한 최적의 사용자 환경을 제공하므로 앱에 대한 ARM 패키지를 제출할 수 있는지 확인하십시오. 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조하세요.

>[!NOTE]
> 若要生成 UWP 应用程序以本机方式面向 ARM64 平台，你必须安装有 Visual Studio 2017 版本15.9 或更高版本或 Visual Studio 2019。 有关详细信息，请参阅[此博客文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)。


>[!IMPORTANT]
> ARM 上的 Windows 10 支持从存储在 ARM64 设备上的 x86、ARM32 和 ARM64 UWP 应用。 当用户在 ARM64 设备上下载 UWP 应用时，操作系统将自动安装可用应用的最佳版本。 如果向应用商店提交应用的 x86、ARM32 和 ARM64 版本，操作系统会自动安装 ARM64 版本的应用。 如果只提交应用的 x86 和 ARM32 版本，操作系统将安装 ARM32 版本。 如果只提交 x86 版本的应用，操作系统会安装该版本并在仿真下运行它。 아키텍처에 대한 자세한 내용은 [앱 패키지 아키텍처](/windows/msix/package/device-architecture)를 참조하세요.

### <a name="win32-apps"></a>Win32 앱
除了 UWP 应用之外，ARM 上的 Windows 10 还可以运行未修改的 x86 Win32 应用，并具有良好的性能和无缝的用户体验，就像任何 PC 一样。 这些 x86 Win32 应用无需重新编译 ARM，甚至不能意识到它们是在 ARM 处理器上运行的。 请注意，不支持64位 x64 Win32 应用程序，但绝大多数应用程序都有可用的 x86 版本。  如果选择应用程序体系结构，只需选择32位 x86 版本即可在 ARM PC 上的 Windows 10 上运行该应用程序。

## <a name="in-this-section"></a>이 섹션의 내용
|항목 | 설명 |
|-----|-----|
|[ARM에서 x86 에뮬레이션이 작동하는 방식](apps-on-arm-x86-emulation.md)|x86 앱이 ARM에셔 에뮬레이션되는 방식을 설명하는 개요.|
|[ARM 기반 x86 앱의 문제 해결](apps-on-arm-troubleshooting-x86.md)|ARM을 기반으로 실행되는 경우 x86 앱 관련 일반적인 문제 및 이를 해결하는 방법. |
|[ARM 上的 ARM 应用疑难解答](apps-on-arm-troubleshooting-arm32.md)|在 ARM 上运行时，ARM32 和 ARM64 应用的常见问题，以及如何解决这些问题。 |
|[ARM의 프로그램 호환성 문제 해결사](apps-on-arm-program-compat-troubleshooter.md)|앱이 ARM에서 제대로 작동하지 않는 경우 호환성 설정 조정에 대한 지침. |

## <a name="related-topics"></a>관련 항목
|항목 | 설명 |
|-----|-----|
|[WDK를 사용하여 ARM64 드라이버 빌드](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)|ARM64 드라이버를 구축하기 위한 지침. |
| [在 ARM 上调试 x86 应用](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) | ARM 기반 x86 앱을 디버깅하기 위한 지침. |
