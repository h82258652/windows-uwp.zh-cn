---
author: QuinnRadich
title: "选择 UWP 版本"
description: "在 Microsoft Visual Studio 中编写 UWP 应用时，可以选择要面向的版本。 了解不同的 UWP 版本之间的区别，以及如何在新项目和现有项目中配置你的选择。"
redirect_url: ../updates-and-versions/choose-a-uwp-version/
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f

---

# 选择 UWP 版本

**此页面已重新定位到 ../updates-and-versions/choose-a-uwp-version/**

在 Microsoft Visual Studio 中编写 UWP 应用时，可以选择要面向的版本。 了解不同的 UWP 版本之间的区别，以及如何在新项目和现有项目中配置你的选择。

## 每个 UWP 版本中有何区别？

Windows 10 的每个连续版本中都提供了适用于 UWP 的全新和更改的 API。 有关哪个版本中添加了哪些功能的特定信息，请参阅 [Windows 10 中面向开发人员的新增功能](../whats-new/windows-10-version-1607.md)。

有关枚举所有设备系列及其版本和所有 API 合约及其版本的参考主题，请参阅[设备系列](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx)和 [API 合约](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx)。

## 选择要用于你的应用的版本

在 Visual Studio 的“新的通用 Windows 项目”****对话框中，可以选择一个版本作为“目标版本”****和“最低版本”****。

* **目标版本**。 这将在你的项目文件中设置 *TargetPlatformVersion* 设置。 还可确定你的应用包清单中的 *TargetDeviceFamily@MaxVersionTested* 属性的值。 所选值指定你的项目面向的 UWP 平台版本（以及适用于你的应用的 API 集），因此我们建议你选择最新版本。 有关你的应用包清单的详细信息以及手动配置 TargetDeviceFamily 的一些指南，请参阅 [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903)。
* **最低版本**。 这将在你的项目文件中设置 *TargetPlatformMinVersion* 设置。 还可确定你的应用包清单中的 *TargetDeviceFamily@MinVersion* 属性的值。 所选值指定你的项目所适用的 UWP 平台的最低版本。

请注意，你在声明应用于从“最低版本”****到“目标版本”****范围中适用的任何 Windows 版本。 如果这两个版本为同一版本，则无需执行任何特殊操作。 如果版本不同，请注意以下一些事项。

* 在代码中，可随意（即无需条件检查）调用“最低版本”****指定的版本中存在的任何 API。
* 使用“目标版本”****的值标识用于编译项目的所有参考（合约 WinMD）。 但是这些参考使你通过对 API 的调用编译代码，这些 API 并不一定存在于你已声明支持的设备上（通过“最低版本”****）。 因此，在“最低版本”****后引入的任何 API 都需要通过自适应代码调用。 有关自适应代码的详细信息，请参阅[通用 Windows 平台 (UWP) 应用指南](universal-application-platform-guide.md)。


<!--HONumber=Aug16_HO5-->


