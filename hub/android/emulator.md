---
title: 从 Windows 运行 Android 设备或模拟器
description: 在 Android 设备或模拟器上从 Windows 测试应用，并使用 hyper-v 和 Windows 虚拟机监控程序平台（WHPX）启用虚拟化。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android，windows，模拟器，虚拟设备，设备设置，启用设备，开发人员，配置，虚拟化，visual studio，hyper-v，intel，haxm，amd，Windows 虚拟机监控程序平台，WHPX
ms.date: 04/28/2020
ms.openlocfilehash: c651661d573695902368ffa595ce5d3014791a9a
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255151"
---
# <a name="test-on-an-android-device-or-emulator"></a>在 Android 设备或仿真程序上测试

可以通过多种方法在 Windows 计算机上使用实际设备或仿真程序来测试和调试 Android 应用程序。 本指南概述了几个建议。

## <a name="run-on-a-real-android-device"></a>在实际 Android 设备上运行

若要在实际 Android 设备上运行应用，首先需要启用 Android 设备进行开发。 默认情况下，Android 上的开发人员选项已隐藏，因为版本4.2 和启用可能因 Android 版本而异。

### <a name="enable-your-device-for-development"></a>启用设备进行开发

对于运行最新版本 Android 9.0 + 的设备：

1. 使用 USB 电缆将设备连接到 Windows 开发计算机。 你可能会收到安装 USB 驱动程序的通知。
2. 在 Android 设备上打开 "**设置**" 屏幕。
3. 选择 "**关于手机**"。
4. 滚动到底部，点击 "**内部版本号**" 7 次，直到**你现在成为开发人员！** 可见。
5. 返回到上一个屏幕，选择 "**系统**"。
6. 选择 "**高级**"，滚动到底部，然后点击 "**开发人员选项**"。
7. 在 "**开发人员选项**" 窗口中，向下滚动以查找并启用 " **USB 调试**"。

对于运行旧版 Android 的设备，请参阅[设置设备以进行开发](https://docs.microsoft.com/xamarin/android/get-started/installation/set-up-device-for-development)。

### <a name="run-your-app-on-the-device"></a>在设备上运行应用

1. 在 Android Studio 工具栏上，从 "**运行配置**" 下拉菜单中选择应用。

    ![Android Studio 运行配置菜单](../images/android-run-config-menu.png)

2. 从 "**目标设备**" 下拉菜单中，选择要在其上运行应用的设备。

    ![Android Studio 目标设备菜单](../images/android-target-device-menu.png)

3. 选择 "运行▷"。 这会在连接的设备上启动应用。

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>使用模拟器在虚拟 Android 设备上运行应用

在 Windows 计算机上运行 Android 仿真程序的第一件事是，无论你的 IDE （Android Studio、Visual Studio 等）如何，都可以通过启用虚拟化支持来大幅提高模拟器性能。

### <a name="enable-virtualization-support"></a>启用虚拟化支持

使用 Android 仿真程序创建虚拟设备之前，建议通过打开 Hyper-v 和 Windows 虚拟机监控程序平台（WHPX）功能来启用虚拟化。 这将允许您的计算机处理器显著提高模拟器的执行速度。

> 若要运行 Hyper-v 和 Windows 虚拟机监控程序平台，你的计算机必须：
>
> * 提供4GB 的可用内存
> * 使用带有二级地址转换（SLAT）的64位 Intel 处理器或 AMD Ryzen CPU
> * 正在运行 Windows 10 build 1803 + （[检查生成 #](ms-settings:about)）
> * 已更新图形驱动程序（设备管理器 > 显示适配器 > 更新驱动程序）
>
> 如果计算机不满足此条件，则可以运行[INTEL HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows)或[AMD 虚拟机监控程序](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)。 有关详细信息，请参阅文章：[仿真程序性能的硬件加速](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration)或[Android Studio 模拟器文档](https://developer.android.com/studio/run/emulator)。

1. 通过打开命令提示符并输入以下命令，验证计算机的硬件和软件是否与 Hyper-v 兼容：`systeminfo`

    ![命令提示符中的 systeminfo.exe 的 hyper-v 要求](../images/systeminfo.png)

2. 在 Windows 搜索框（左下方）中，输入 "Windows 功能"。 从搜索结果中选择 **"打开或关闭 Windows 功能**"。

3. 出现**Windows 功能**列表后，滚动到 "查找**hyper-v** " （包括管理工具和平台）和**Windows 虚拟机监控程序平台**，确保选中此复选框以启用两者，然后选择 **"确定"**。

4. 出现提示时，重启计算机。

### <a name="emulator-for-native-development-with-android-studio"></a>具有 Android Studio 的本机开发的模拟器

生成和测试本机 Android 应用时，建议[使用 Android Studio](./native-android.md)。 在应用程序准备好进行测试后，可以通过以下方式生成并运行应用程序：

1. 在 Android Studio 工具栏上，从 "**运行配置**" 下拉菜单中选择应用。

    ![Android Studio 运行配置菜单](../images/android-run-config-menu.png)

2. 从 "**目标设备**" 下拉菜单中，选择要在其上运行应用的设备。

    ![Android Studio 目标设备菜单](../images/android-target-device-menu.png)

3. 选择 "运行▷"。 这将启动[Android Emulator](https://developer.android.com/studio/run/emulator)。

> [!TIP]
> 在模拟器设备上安装应用后，可以使用 "[应用更改](https://developer.android.com/studio/run#apply-changes)" 来部署某些代码和资源更改，而无需生成新的 APK。

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>用于通过 Visual Studio 进行跨平台开发的模拟器

有许多适用于 Windows 电脑的[Android 仿真程序选项](https://www.androidauthority.com/best-android-emulators-for-pc-655308/)。 建议使用 Google 的[android 模拟器](https://developer.android.com/studio/run/emulator)，因为它提供对最新 Android OS 映像和 Google Play 服务的访问权限。

### <a name="install-android-emulator-with-visual-studio"></a>安装 Android 仿真程序与 Visual Studio

1. 如果尚未安装，请下载[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)。 使用 Visual Studio 安装程序[修改工作负荷](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads)，并确保具有 **.net 工作负载的移动开发**。

2. 创建新项目。 [设置 Android Emulator](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/)后，可以使用[Android Device Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#requirements)创建、复制、自定义和启动各种 Android 虚拟设备。 从 "工具" 菜单中启动 Android Device Manager： **tools** > **Android** > **Android Device Manager**。

3. 打开 Android Device Manager 后，选择 " **+ 新建**" 以创建新设备。

4. 你将需要为设备命名，从下拉菜单中选择基本设备类型，选择处理器、操作系统版本以及虚拟设备的其他几个变量。 有关详细信息，请[Android Device Manager 主屏幕](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#main-screen)。

5. 在 Visual Studio 工具栏中，选择 "**调试**" （在应用程序启动后附加到在模拟器内运行的应用程序进程）或 "**发布**" 模式（禁用调试器）。 然后从 "设备" 下拉菜单中选择虚拟设备，然后选择 "**播放**" 按钮▷在模拟器中运行应用程序。

    ![Visual Studio 启动 Android Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>其他资源

- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)

- [添加 Windows Defender 排除项以提高性能](defender-settings.md)
