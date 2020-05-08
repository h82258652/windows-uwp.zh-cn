---
title: 通过更新 Defender 设置提高性能速度
description: 了解如何通过更新 Windows Defender 设置以排除检查指定的文件类型，从而提高性能速度和生成时间。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android，windows，windows defender，设置，配置，排除，% USERPROFILE%，devenv，性能，速度，生成，gradle
ms.date: 04/28/2020
ms.openlocfilehash: d818c4f568698a121fb7051ec5e6e2d246bff924
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255131"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>更新 Windows Defender 设置以提高性能

本指南介绍如何设置 Windows Defender 安全设置中的排除项，以提高你的生成时间和 Windows 计算机的总体性能速度。

## <a name="windows-defender-overview"></a>Windows Defender 概述

在 Windows 10 版本1703及更高版本中， [Windows Defender 防病毒](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus)应用程序是 windows 安全的一部分。 Windows Defender 旨在利用内置的实时防护来防范病毒、勒索软件、间谍软件和其他安全威胁，使你的电脑保持安全。

**但是**，在开发 Android 应用程序时，Windows Defender 的实时保护还会大幅降低文件系统访问和生成速度。

在 Android 生成过程中，将在您的计算机上创建许多文件。 启用防病毒实时扫描后，每次在防病毒扫描该文件时创建新文件时，生成过程都将暂停。

幸运的是，Windows Defender 能够排除你知道在防病毒扫描过程中保护的文件、项目目录或文件类型。

> [!WARNING]
> 若要确保你的计算机不受恶意软件的攻击，你不应完全禁用实时扫描或 Windows Defender 防病毒软件。

## <a name="add-exclusions-to-windows-defender"></a>向 Windows Defender 添加排除项

若要改善 Android 生成速度，请在[Windows Defender 安全中心](windowsdefender://)添加排除项，方法是：

1. 选择 Windows 菜单的 "**开始**" 按钮
2. 输入**Windows 安全性**
3. 选择**病毒和威胁防护**
4. 选择 "安全 **& 威胁防护设置**" 下的 "**管理设置**"
5. 滚动到**排除**标题，并选择 "**添加或删除排除**项"
6. 选择 " **+ 添加排除**"。 然后，需要选择要添加的排除项是**文件**、**文件夹**、**文件类型**还是**进程**。

![Windows Defender 添加排除屏幕快照](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>建议排除项

以下列表显示了建议从 Windows Defender 实时扫描中添加的每个 Android Studio 目录的默认位置：

- Gradle 缓存：`%USERPROFILE%\.gradle`
- Android Studio 项目：`%USERPROFILE%\AndroidStudioProjects`
- Android SDK：`%USERPROFILE%\AppData\Local\Android\SDK`
- Android Studio 系统文件：`%USERPROFILE%\.AndroidStudio<version>\system`

如果未使用 Android Studio 设置的默认位置，或者如果已从 GitHub 下载项目（例如），则这些目录位置可能不适用于你的项目。 请考虑将排除添加到当前 Android 开发项目的目录，无论其位于何处。

你可能想要考虑的其他排除项包括：

- Visual Studio 开发环境过程：`devenv.exe`
- Visual Studio 生成过程：`msbuild.exe`
- JetBrains 目录：`%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

有关添加防病毒扫描排除项的详细信息，包括如何自定义组策略受控环境的目录位置，请参阅 Android Studio 文档的 "[防病毒影响](https://developer.android.com/studio/intro/studio-config#antivirus-impact)" 部分。

> [!Note]
> Daniel Knoodle 已设置 GitHub 存储库，其中包含建议的脚本来添加[适用于 Visual Studio 2017 的 Windows Defender 排除](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10)项。

## <a name="additional-resources"></a>其他资源

- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)

- [添加 Windows Defender 排除项以提高性能](./defender-settings.md)

- [启用虚拟化支持以提高模拟器性能](./emulator.md#enable-virtualization-support)
