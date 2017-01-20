---
author: QuinnRadich
title: "选择 UWP 版本"
description: "在 Microsoft Visual Studio 中编写 UWP 应用时，可以选择要面向的版本。 了解不同的 UWP 版本之间的区别，以及如何在新项目和现有项目中配置你的选择。"
translationtype: Human Translation
ms.sourcegitcommit: 7e59551f528e23e497122f822fbfc09ec3086cdc
ms.openlocfilehash: 58e58c212cb6efd76f4afb8d3b6c7a3c5ddf215e

---

# <a name="choose-a-uwp-version"></a>选择 UWP 版本

在 Microsoft Visual Studio 中编写 UWP 应用时，可以选择要面向的版本。 目前，仅支持以下三种版本。

| 版本 | 说明 |
| --- | --- |
| 版本 14393（周年版本） | 这是最新版本的 Windows 10，已于 2016 年 7 月发布。 此版本中的一些突出功能如下： </br> \* **Windows Ink：**新的 InkCanvas 和 InkToolbar 控件。 </br> \* **Cortana API：**使用新的 Cortana 操作将 Cortana 支持与应用的特殊功能集成。 </br> \* **Windows Hello：**Microsoft Edge 现在支持 Windows Hello，使 Web 开发人员能够访问生物识别身份验证。 </br> 有关此版本的 Windows 中添加的这些功能以及许多其他功能的信息，请访问[开发人员中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或[面向开发人员的 Windows 10 中的新增功能](../whats-new/windows-10-version-1607.md)上的详细信息页面。  |
| 版本 10586 | 此版本的 Windows 10 已于 2015 年 11 月发布。 突出功能包括引入了用于在 Microsoft Edge 中进行视频通信的 ORTC（对象实时通信）和用于支持应用使用 Windows Hello 人脸身份验证的提供者 API。 [有关此版本中引入的功能的详细信息。](../whats-new/windows-10-version-1511.md) |
| 版本 10240 | 这是 Windows 10 的初始发行版本，已于 2015 年 7 月发布。 [有关此版本中引入的功能的详细信息。](../whats-new/windows-10-version-1507.md) |

我们强烈建议新的开发人员和针对普通受众编写代码的开发人员始终使用最新版本的 Windows (14393)。 编写企业应用的开发人员应着重考虑支持较旧的**最低版本**。

## <a name="whats-different-in-each-uwp-version"></a>每个 UWP 版本中有何区别？

Windows 10 的每个连续版本中都提供了适用于 UWP 的全新和更改的 API。 有关哪个版本中添加了哪些功能的特定信息，请参阅 [Windows 10 中面向开发人员的新增功能](../whats-new/windows-10-version-1607.md)。

有关枚举所有设备系列及其版本和所有 API 合约及其版本的参考主题，请参阅[设备系列](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx)和 [API 合约](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx)。

## <a name="choose-which-version-to-use-for-your-app"></a>选择要用于你的应用的版本

在 Visual Studio 的“新的通用 Windows 项目”对话框中，可以选择一个版本作为“目标版本”和“最低版本”。

* **目标版本**。 这将在你的项目文件中设置 *TargetPlatformVersion* 设置。 还可确定你的应用包清单中的 *TargetDeviceFamily@MaxVersionTested* 属性的值。 所选值指定你的项目面向的 UWP 平台版本（以及适用于你的应用的 API 集），因此我们建议你选择最新版本。 有关你的应用包清单的详细信息以及手动配置 TargetDeviceFamily 的一些指南，请参阅 [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903)。
* **最低版本**。 这将在你的项目文件中设置 *TargetPlatformMinVersion* 设置。 还可确定你的应用包清单中的 *TargetDeviceFamily@MinVersion* 属性的值。 所选值指定你的项目所适用的 UWP 平台的最低版本。

请注意，你在声明应用于从“最低版本”到“目标版本”范围中适用的任何 Windows 版本。 如果这两个版本为同一版本，则无需执行任何特殊操作。 如果版本不同，请注意以下一些事项。

* 在代码中，可随意（即无需条件检查）调用“最低版本”指定的版本中存在的任何 API。
* 确保在运行“最低版本”的设备上测试你的代码，以便它无需仅在“目标版本”中的 API 即可正常运行。
* 使用“目标版本”的值标识用于编译项目的所有参考（合约 WinMD）。 但是这些参考使你通过对 API 的调用编译代码，这些 API 并不一定存在于你已声明支持的设备上（通过“最低版本”）。 因此，在“最低版本”后引入的任何 API 都需要通过自适应代码调用。 有关自适应代码的详细信息，请参阅[通用 Windows 平台 (UWP) 应用指南](../get-started/universal-application-platform-guide.md)。



<!--HONumber=Dec16_HO1-->


