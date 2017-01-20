---
author: normesta
Description: "此中心主题涉及关于 Windows 信息保护 (EDP) 与文件、缓冲区、剪贴板、网络、后台任务以及锁屏下的数据保护有何关联的完整开发人员蓝图。"
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Windows 信息保护 (WIP)"
translationtype: Human Translation
ms.sourcegitcommit: 8a0ce7282ffaf76bcea94eec5ec3e3ceb99aa5ac
ms.openlocfilehash: 81bd77d0153c17c80ccdce77332ed57fb751c8ec

---

# <a name="windows-information-protection-wip"></a>Windows 信息保护 (WIP)


  __注意__ Windows 信息保护 (WIP) 策略可以应用于 Windows 10 版本 1607。

WIP 通过强制执行组织定义的策略来保护属于组织的数据。 如果你的应用包括在这些策略中，则你的应用生成的所有数据都将受到策略限制。 本主题帮助你生成可更流畅地强制执行这些策略的应用，而不对用户的个人数据造成任何影响。

## <a name="first-what-is-wip"></a>首先，什么是 WIP？

WIP 是支持组织的移动设备管理 (MDM) 和移动应用程序管理 (MAM) 系统的台式计算机、笔记本电脑、平板电脑和手机上的一组功能。

WIP 和 MDM 一起使组织可以更好地控制如何处理组织所管理的设备上的数据。 有时，用户上班时携带设备，但不在组织的 MDM 中注册设备。  在这些情况下，组织可以使用 MAM 更好地控制在用户在设备上安装的特定业务线应用中处理数据的方式。

通过使用 MDM 或 MAM，管理员可以标识允许哪些应用访问属于组织的文件，以及用户是否可以从这些文件复制数据，然后将该数据粘贴到个人文档中。

它的工作原理如下。 用户将他们的设备注册到组织的移动设备管理 (MDM) 系统中。 管理组织中的管理员使用 Microsoft Intune 或 System Center Configuration Manager (SCCM) 定义一个策略，然后将其部署到注册的设备。

如果不需要用户注册其设备，管理员将使用其 MAM 系统定义和部署适用于特定应用的策略。 当用户安装其中的任意应用时，他们将收到关联的策略。

该策略标识可以访问企业数据的应用（称为策略的*允许列表*）。 这些应用可以在剪贴板上或通过“共享”合约访问企业保护的文件、虚拟专用网 (VPN) 和企业数据。 此策略还定义管理数据的规则。 例如，是否可以从企业所有的文件复制数据，然后复制到非企业所有的文件中。

如果用户从组织的 MDM 系统中取消注册其设备，或卸载已被组织的 MAM 系统标识的应用，则管理员可以远程擦除设备中的企业数据。

![WIP 生命周期](images/wip-lifecycle.png)

> **阅读关于 WIP 的详细信息** <br>
* [介绍 Windows 信息保护](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [使用 Windows 信息保护 (WIP) 保护企业数据](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

如果你的应用在允许列表上，则你的应用生成的所有数据都将受到策略限制。 这意味着，如果管理员吊销用户对企业数据的访问权限，则这些用户将失去对你的应用所生成的所有数据的访问权限。

如果你的应用仅设计用于企业用途，则这没问题。 但是，如果你的应用创建用户认为属于其个人的数据，你将希望*启发*你的应用以便智能地辨别企业和个人数据。 我们调用此类型的应用*应用启发式*，因为它可以在保留用户个人数据的完整性时流畅地强制执行企业策略。

## <a name="create-an-enterprise-enlightened-app"></a>创建企业启发式应用

使用 WIP API 启发你的应用，然后将你的应用声明为企业启发式应用。

如果你的应用用于组织和个人用途，请启发该应用。

如果你希望流畅地处理策略元素的强制执行，请启发你的应用。

例如，如果策略允许用户将企业数据粘贴到个人文档中，则可以使用户在粘贴数据前无需响应同意对话框。 同样，你可以提供自定义信息对话框以响应这些类型的事件。

如果你已准备好启发你的应用，请参阅以下指南之一：

**对于使用 C 生成的通用 Windows 平台 (UWP) 应用#**

[生成使用企业和个人数据的启发式应用](wip-dev-guide.md)。

**对于使用 C++ 生成的桌面应用**

[生成使用企业和个人数据的启发式应用 (C++)](http://go.microsoft.com/fwlink/?LinkId=822192)。


## <a name="create-non-enlightened-enterprise-app"></a>创建非启发式企业应用

如果创建的业务线 (LOB) 应用不用于个人用途，则无需启发应用。

### <a name="windows-desktop-apps"></a>Windows 桌面应用
你无需启发 Windows 桌面应用，但应该测试应用，确保它在策略下正常运行。 例如，启动应用，使用，然后从 MDM 中取消注册设备。 然后，确保应用可以重新启动。 如果对应用程序操作至关重要的文件已加密，则应用程序可能无法启动。 另外，查看与应用交互的文件，确保应用不会意外加密用户的个人文件。 这可能会包括元数据文件、图像和其他内容。

测试应用后，将此标志添加到资源文件或项目中，然后重新编译应用。

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
虽然 MDM 策略不需要标志，但 MAM 策略需要标志。

### <a name="uwp-apps"></a>UWP 应用

如果你希望你的应用包括在 MAM 策略中，则你应该启发它。 部署到 MDM 下的设备的策略不需要它，但如果你将应用分发给组织消费者，那么很难确定他们将使用哪种类型的策略管理系统。 若要确保应用可在两种类型的策略管理系统（MDM 和 MAM）中运行，你应该启发应用。






 



<!--HONumber=Dec16_HO2-->


