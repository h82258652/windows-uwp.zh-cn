---
title: 共享命名对象
description: 本主题说明如何在通用 Windows 平台（UWP）应用程序与 Win32 应用程序之间共享命名对象。
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 95bbecd85b1dfa6f6e12766c082f3338de549677
ms.sourcegitcommit: 2d375e1c34473158134475af401532cc55fc50f4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888569"
---
# <a name="sharing-named-objects"></a>共享命名对象

本主题说明如何在通用 Windows 平台（UWP）应用程序与 Win32 应用程序之间共享命名对象。

## <a name="named-objects-in-packaged-applications"></a>打包的应用程序中的命名对象

[命名对象](/windows/win32/sync/object-names)提供了一种简单的方法来共享对象句柄。 在进程创建命名对象后，其他进程可以使用该名称来调用相应的函数来打开该对象的句柄。 命名对象通常用于[线程同步](/windows/win32/sync/interprocess-synchronization)和[进程间通信](/windows/uwp/communication/interprocess-communication)。

默认情况下，打包应用程序只能访问已创建的命名对象。 为了与打包应用程序共享命名对象，必须在创建对象时设置权限，并且在打开对象时必须限定名称。

## <a name="creating-named-objects"></a>创建命名对象

命名对象是使用相应的 `Create` API 创建的：

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

所有这些 Api 共享一个 `LPSECURITY_ATTRIBUTES` 参数，调用方可以使用该参数指定[访问控制列表（acl）](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85))来控制哪些进程可以访问对象。 若要与打包应用程序共享命名对象，在创建命名对象时，必须在 Acl 中授予权限。

安全标识符（Sid）代表 Acl 中的标识。 每个打包的应用程序都有自己的基于其包系列名称的 SID。 可以通过将包系列名称传递到[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)来生成打包应用程序的 SID。

> [!NOTE]
> 在开发时，可以通过 Visual Studio 中的包清单编辑器、通过 Microsoft Store[发布的应用](/windows/uwp/publish/view-app-identity-details)程序或已安装的应用程序的[add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令来找到包系列名称。

[此示例](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples)演示了 ACL 命名对象所需的基本模式。 若要使用打包应用程序共享命名对象，请为每个应用程序生成一个[EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w)结构：

* `grfAccessMode = GRANT_ACCESS`
* 基于对象和预期用途 `grfAccessPermissions =` 适当的权限
    * [一般访问权限](/windows/win32/secauthz/generic-access-rights)
    * [同步对象安全和访问权限](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [文件映射安全和访问权限](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =` 从[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)获取的 SID

通过使用打包应用程序的 `EXPLICIT_ACCESS` 规则填充 `Create` 调用中的 `LPSECURITY_ATTRIBUTES` 参数，你可以授予对这些应用程序的访问权限，以打开命名对象。

> [!NOTE]
> Win32 应用程序可以访问由打包的应用程序创建的所有命名对象，前提是它们在[打开](#opening-named-objects)时限定对象名称。 不需要授予访问权限。

## <a name="opening-named-objects"></a>打开命名对象

通过将名称传递到相应的 `Open` API 来打开命名对象：

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

打包的应用程序创建的命名对象是在应用程序的命名空间（也称为命名对象路径）中创建的。 打开打包应用程序创建的命名对象时，对象名称必须以创建应用程序的命名对象路径为前缀。

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath)将根据其 SID 返回打包应用程序的命名对象路径。 可以通过将包系列名称传递到[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)来生成打包应用程序的 SID。

> [!NOTE]
> 在开发时，可以通过 Visual Studio 中的包清单编辑器、通过 Microsoft Store[发布的应用](/windows/uwp/publish/view-app-identity-details)程序或已安装的应用程序的[add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令来找到包系列名称。

打开打包应用程序创建的命名对象时，请使用格式 `<PATH>\<NAME>`：

* 将 `<PATH>` 替换为创建应用程序的命名对象路径。
* 将 `<NAME>` 替换为对象名称。

> [!NOTE]
> 只有在打包的应用程序创建了对象时，才需要使用 `<PATH>` 作为对象名称的前缀。 不需要对 Win32 应用程序创建的命名对象进行限定，不过，在[创建](#creating-named-objects)这些对象时仍必须授予访问权限。

## <a name="remarks"></a>备注

默认情况下，将隔离打包应用程序中的命名对象，以保留安全性并确保对应用程序生命周期事件（如挂起和终止）的支持。 跨应用程序共享命名对象会引入严格的绑定和版本控制约束，并要求每个应用程序都可以恢复到其他应用程序的生命周期。 由于这些原因，建议仅在同一发布者之间共享应用程序之间的命名对象。
