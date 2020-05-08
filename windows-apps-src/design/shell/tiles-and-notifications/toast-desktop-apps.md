---
Description: 发现桌面 Win32 应用用于发送 toast 通知的不同选项
title: 来自桌面应用的 Toast 通知
label: Toast notifications from desktop apps
template: detail.hbs
ms.date: 05/01/2018
ms.topic: article
keywords: windows 10，uwp，win32，桌面，toast 通知，desktop bridge，.msix，稀疏包，用于发送 toast，com 服务器，com 激活器，com，虚假 com，无 com，无 com，send toast 的选项
ms.localizationpriority: medium
ms.openlocfilehash: 020cdeb1aaddac7fe879e91d18e258aea1b387ea
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970982"
---
# <a name="toast-notifications-from-desktop-apps"></a>来自桌面应用的 Toast 通知

桌面应用（包括打包的[.msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview)应用、使用[稀疏包](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)获取包标识和经典非打包 Win32 应用的应用）可以像 Windows 应用应用一样发送交互式 toast 通知。 但是，由于激活方案不同，存在一些不同的桌面应用选项。

在本文中，我们将介绍你可以用于在 Windows 10 上发送 toast 通知的选项。 每个选项完全支持…

* 在操作中心中保持
* 可以从弹出窗口和操作中心内激活
* 可在 EXE 未运行时激活

## <a name="all-options"></a>所有选项

下表说明了用于在桌面应用内支持 toast 的选项，以及对应的受支持的功能。 你可以使用此表来选择最适合你的情形的选项。<br/><br/>

| 选项 | 视觉对象 | 操作 | 输入 | 在进程内激活 |
| -- | -- | -- | -- | -- |
| [COM 激活器](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [无 COM / 存根 CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>首选选项 - COM 激活器

这是适用于桌面应用程序的首选选项，并且支持所有通知功能。 不必担心使用“COM 激活器”；我们有 [C#](send-local-toast-desktop.md) 和 [C++ 应用](send-local-toast-desktop-cpp-wrl.md)的资源库，让使用此选项变得很简单，即使你之前从未编写过 COM 服务器。<br/><br/>

| 视觉对象 | 操作 | 输入 | 在进程内激活 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

使用 COM 激活器选项，你可以在应用中使用以下通知模板和激活类型。<br/><br/>

| 模板和激活类型 | .MSIX/稀疏包 | 经典 Win32 |
| -- | -- | -- |
| ToastGeneric 前台 | ✔️ | ✔️ |
| ToastGeneric 后台 | ✔️ | ✔️ |
| ToastGeneric 协议 | ✔️ | ✔️ |
| 旧版模板 | ✔️ | ❌ |

> [!NOTE]
> 如果将 COM 激活器添加到现有的 .MSIX/稀疏包应用，前台/背景和旧通知激活现在会激活 COM 激活器而不是命令行。

若要了解如何使用此选项，请参阅[从桌面 c # 应用发送本地 toast 通知](send-local-toast-desktop.md)或[通过桌面 c + + WRL 应用发送本地 toast 通知](send-local-toast-desktop-cpp-wrl.md)。


## <a name="alternative-option---no-com--stub-clsid"></a>备用选项 - 无 COM / 存根 CLSID

如果你无法实现 COM 激活器，这是一个备用选项。 但是，将牺牲一些功能，如输入支持（toast 上的文本框）和在进程内激活。<br/><br/>

| 视觉对象 | 操作 | 输入 | 在进程内激活 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

使用此选项，如果你支持经典 Win32，那么你能够使用的通知模板和激活类型将非常有限，如下所示。<br/><br/>

| 模板和激活类型 | .MSIX/稀疏包 | 经典 Win32 |
| -- | -- | -- |
| ToastGeneric 前台 | ✔️ | ❌ |
| ToastGeneric 后台 | ✔️ | ❌ |
| ToastGeneric 协议 | ✔️ | ✔️ |
| 旧版模板 | ✔️ | ❌ |

对于打包的[.msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview)应用程序和使用[稀疏包](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)的应用程序，只需像在 UWP 应用程序中一样发送 toast 通知。 当用户点击你的 toast 时，你的应用将使用你在 toast 中指定的启动参数启动命令行。

对于经典 Win32 应用，应设置 AUMID，以便你可以发送 toast，然后还应在快捷方式上指定 CLSID。 它可以是任何随机的 GUID。 不要添加 COM 服务器/激活器。 你正在添加“存根”COM CLSID，这将导致操作中心保留通知。 请注意，你只能使用协议激活 toast，因为存根 CLSID 将中断任何其他 toast 激活的激活。 因此，你必须更新你的应用以支持协议激活，并让 toast 协议激活你自己的应用。


## <a name="resources"></a>资源

* [从桌面 C# 应用发送本地 toast 通知](send-local-toast-desktop.md)
* [从桌面 C++ WRL 应用发送本地 toast 通知](send-local-toast-desktop-cpp-wrl.md)
* [toast 内容文档](adaptive-interactive-toasts.md)
