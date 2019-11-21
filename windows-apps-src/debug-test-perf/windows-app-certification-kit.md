---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 应用认证工具包
description: 若要为你的应用创造在 Microsoft Store 中发布的最佳机会或令其通过 Windows 认证，请在提交应用进行认证之前先在本地进行验证和测试。 本主题显示了如何安装并运行 Windows 应用认证工具包。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, app certification
ms.localizationpriority: medium
ms.openlocfilehash: 4772edb9c99426396b7fa3a8734e2f45391c3a0f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257836"
---
# <a name="windows-app-certification-kit"></a>Windows 应用认证工具包



To get your app [Windows Certified](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) or prepare it for [publication to the Microsoft Store](https://docs.microsoft.com/windows/uwp/publish/app-submissions), you should validate and test it locally first. This topic shows you how to install and run the [Windows App Certification Kit](https://msdn.microsoft.com/en-US/windows/apps/bg127575) to ensure your app is safe and efficient.

## <a name="prerequisites"></a>必备条件

测试通用 Windows 应用的先决条件：

-   You must install and run Windows 10.
-   You must install [Windows App Certification Kit version 10]( https://go.microsoft.com/fwlink/p/?LinkID=309666), which is included in the Windows Software Development Kit (SDK) for Windows 10.
-   必须[启用设备进行开发](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
-   必须将要测试的 Windows 应用部署到计算机。

**A note about in-place upgrades**

安装更高版本的 [Windows 应用认证工具包]( https://go.microsoft.com/fwlink/p/?LinkID=309666)将替换已安装在该计算机上的所有早期版本的工具包。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>以交互方式使用 Windows 应用认证工具包验证 Windows 应用

1.  从“开始”菜单中，搜索“应用”、找到“Windows 工具包”，然后单击“Windows 应用认证工具包”。

2.  从“Windows 应用认证工具包”中，选择你希望执行的验证类别。 例如，如果你要验证 Windows 应用，请选择“验证 Windows 应用”。

    你可能会直接浏览到要测试的应用，或从 UI 的列表选择应用。 首次运行 Windows 应用认证工具包时，UI 将列出已安装在计算机上的所有 Windows 应用。 对于任何后续的运行，UI 将显示已验证的最新 Windows 应用。 如果未列出要测试的应用，则可以单击“我的应用未列出”来获取系统上安装的所有应用的完整列表。

3.  在已输入或选定要测试的应用后，单击“下一步”。

4.  在下一屏幕中，你将看到与正在测试的应用类型相对应的测试工作流。 如果列表中的某一测试灰显，则表示该测试不适用于你的环境。 例如，如果你正在 Windows 7 上测试 Windows 10 应用，则只有静态测试才能应用到工作流。 Note that the Microsoft Store may apply all tests from this workflow. 选择要运行的测试，然后单击“下一步”。

    Windows App 认证工具包开始验证该应用。

5.  测试后，在提示符处输入要保存测试报告的文件夹的位置。

    Windows 应用认证工具包将创建一个 HTML 及一个 XML 报告并将它保存在此文件夹中。

6.  打开报告文件并查看测试结果。

> [!NOTE]
> If you're using Visual Studio, you can run the Windows App Certification Kit when you create your app package. 请参阅[打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)以了解操作方法。

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>从命令行使用 Windows 应用认证工具包验证 Windows 应用

> [!IMPORTANT]
> The Windows App Certification Kit must be run within the context of an active user session.

1.  在命令窗口中，导航到包含 Windows 应用认证工具包的目录。

    **Note**   The default path is C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\.

2.  按此顺序输入以下命令，以测试已安装在你的测试计算机上的应用：

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    或者，你也可以使用以下命令（如果未安装应用）。 Windows App 认证工具包将打开相应程序包，然后应用适当的测试工作流：

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  在测试完成后，打开名为 `[report file name]` 的报告文件并查看测试结果。

**Note**  The Windows App Certification Kit can be run from a service, but the service must initiate the kit process within an active user session and cannot be run in Session0.

**Note**   For more info about the Windows App Certification Kit command line, enter the command `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>使用低能耗电脑进行测试

Windows 应用认证工具包的性能测试阈值基于低能耗电脑的性能。

执行测试的计算机的属性会影响测试结果。 To determine if your app’s performance meets the [Microsoft Store Policies](https://docs.microsoft.com/legal/windows/agreements/store-policies), we recommend that you test your app on a low-power computer, such as an Intel Atom processor-based computer with a screen resolution of 1366x768 (or higher) and a rotational hard drive (as opposed to a solid-state hard drive).

随着低能耗计算机的发展，其性能特征可能会随时间的推移而改变。 Refer to the most current [Microsoft Store Policies](https://docs.microsoft.com/legal/windows/agreements/store-policies) and test your app with the most current version of the Windows App Certification Kit to make sure that your app complies with the latest performance requirements.

## <a name="related-topics"></a>相关主题

* [Windows App Certification Kit tests](windows-app-certification-kit-tests.md)
* [Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 

 




