---
title: Xbox Live 登录疑难解答
description: 了解如何解决 Xbox Live 登录问题。
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 登录, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: c2a3ff76ab1ecb457085be777b53474cd5f8fe81
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8872671"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Xbox Live 登录疑难解答

有几个问题可能会导致难以登录。  如果你遵循[适用于 UWP 游戏的 Visual Studio 入门](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)中的步骤，那么你可以最小化遇到任何异常错误的可能性。

## <a name="common-issues"></a>常见问题

### <a name="sandbox-problems"></a>沙盒问题

一般来说，你自己应该熟悉沙盒概念以及其与 Xbox Live 的关系。  你可以在 [Xbox Live 沙盒](../../xbox-live-sandboxes.md)指南中了解详细信息。

简单来说，沙盒将在零售版本之前强制执行内容隔离和访问控制。  对你的开发沙盒无访问权限和用户无法对游戏执行任何读取或写入操作。  你还可以为不同的沙盒发布各种服务配置变体，以便进行测试。

下面将讨论需要注意的沙盒相关注意事项：

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>开发人员帐户不能访问适用于运行时访问的沙盒

* 测试帐户 （也称为开发帐户） 或经授权的开发人员帐户必须用于登录到正在开发中的游戏。  请确保你尝试使用其中一个登录或在 XDP 上创建其他测试帐户[https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts)。 在合作伙伴中心上的 xbox live 相关联的开发人员帐户，你可以授权[https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* 确保帐户有权访问你的游戏发布到沙盒。  在 XDP 中创建的测试帐户继承了创建它们的 XDP 帐户的权限

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>你的设备未位于正确的沙盒上

必须将你正在开发的设备设置为开发沙盒。  在 Xbox One 上，你可以使用 *Xbox One 管理器*设置你的沙盒。  对于 Windows 10 桌面版，你可以使用位于 Xbox Live SDK 安装的“工具”目录中的 SwitchSandbox.cmd 脚本。

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>你的游戏服务配置未发布到正确的开发沙盒。

请确保已将你的游戏服务配置发布到开发沙盒中。  你无法在某个游戏的给定开发沙盒中登录到 Xbox Live，除非该游戏已发布到相同沙盒。  请参阅 [XDP 文档](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig)，了解有关该操作的信息。 你可以读取[合作伙伴中心文档](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration)，了解如何发布你的合作伙伴中心配置。

### <a name="ids-configured-incorrectly"></a>ID 配置不正确

配置游戏需要使用多个 ID。  你可以在[适用于 UWP 游戏的 Visual Studio 入门](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)和[跨平台联机游戏入门](../../get-started-with-partner/get-started-with-cross-play-games.md)中了解更多信息，具体取决于创建的游戏类型。

需要注意的事项如下：

* 确保你的应用 ID 正确输入到 XDP 或合作伙伴中心
* 确保将 PFN 正确输入到 XDP 或合作伙伴中心
* 如[将 Xbox Live 添加到新的或现有的 UWP 项目](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)指南所述，仔细检查你是否已在 Visual Studio 项目所在的相同目录中创建 xboxservices.config。
* 确保 appxmanifest 中的“程序包标识符”是正确的。  这是显示在合作伙伴中心的"程序包/标识符/名称"中的应用标识部分。

### <a name="title-id-or-scid-not-configured-correctly"></a>游戏 ID 或 SCID 未正确配置

* 对于 UWP 游戏，必须在 xboxservices.config 文件中将你的游戏 ID 和 SCID 设为正确的值。  另请确保将此文件的格式正确设为 UTF8。  你可以在[适用于 UWP 游戏的 Visual Studio 入门](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)中了解详细信息。 xboxservices.config 文件需区分大小写。
* 对于 XDK 游戏，可以在 package.appxmanifest 中设置这些值。
* 你可以在 Xbox Live SDK 的示例目录中查看 UWP 和 XDK 游戏配置的示例。

## <a name="test-using-the-xbox-app"></a>使用 Xbox 应用测试

如果你开发的是 UWP 应用程序，则可使用 Xbox 应用调试部分问题：

1. 使用 SwitchSandbox.cmd 脚本将设备的沙盒设置为开发沙盒
2. 打开 Xbox 应用并使用具有相同沙盒访问权限的测试帐户登录。

如果成功登录，则它会验证是否已在设备上正确设置沙盒以及你的测试帐户是否具有该沙盒的访问权限。

如果仍遇到登录错误，则可能是你的服务配置未发布到沙盒或者你的 xboxservices.config 未正确设置。 xboxservices.config 文件需区分大小写。

## <a name="debug-based-on-error-code"></a>基于错误代码的调试

以下为登录时可能出现的部分错误代码以及调试这些错误代码时可采取的步骤。  会出现以下屏幕截图中所示的错误代码。

![0x8015DC12 登录错误屏幕截图](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003 应用程序已禁用或未正确配置

1. 请尝试删除你的 PFX 文件。

![解决方案资源管理器中的 pfx 文件](../../images/troubleshooting/pfx_file.png)

如果你未登录到 Visual Studio 使用用于预配合作伙伴中心中的应用的 Microsoft 帐户，Visual Studio 将自动生成签名 pfx 文件基于你的个人 Microsoft 帐户或域帐户。 构建 appx 程序包时，Visual Studio 将使用自动生成的 pfx 签署该程序包并更改 package.appxmanifest 中的程序包“发布者”部分。 因此，生成的位（尤其是，appxmanifest.xml）所具有的程序包标识符与你预期使用的程序包标识符不同。 

2. 仔细检查你的 package.appxmanifest 在合作伙伴中心中设置为相同应用程序标识为你的游戏。 你可以右键单击项目并选择“应用商店”->“将应用与应用商店关联...”，如以下屏幕截图中所示。 或者，手动编辑你的 package.appxmanifest。 请参阅[适用于 UWP 游戏的 Visual Studio 入门](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)，了解详细信息。

![与 Microsoft Store 相关联](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>0x8015DC12 内容隔离错误

总体而言，这意味着设备或用户无法访问指定游戏。

1. 这可能意味着你未使用测试帐户尝试登录或者你的测试帐户没有所登录沙盒的访问权限。 请仔细检查[XDP 文档](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx)中创建测试帐户的说明，并[合作伙伴中心文档。](../../xbox-live-test-accounts.md) 如果需要创建一个新的测试帐户有权访问相应沙盒。

你可能需要删除 Windows 10 中的旧帐户，可通过转至“开始”菜单中的“设置”并转至“帐户”来进行删除。

2. 仔细检查你的游戏是否已发布到尝试使用的沙盒。 请有关如何执行此操作的信息，参阅[XDP 文档](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig)或[合作伙伴中心文档](../../xbox-live-service-configuration.md#sandbox-ids)。

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 意外或未知游戏

仔细检查 XDP 中的应用程序 ID 设置和开发人员中心绑定。 你可以查看[将 Xbox Live 支持添加到新的或现有的 Visual Studio UWP](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)中的相关说明。

### <a name="0x87dd000e-title-not-authorized"></a>0x87DD000E 游戏未授权

仔细检查你的设备是否已设置为正确的开发沙盒以及用户是否具有该沙盒的访问权限。 请参阅[使用 Xbox 应用测试](#test-xbox-app)部分，了解与如何使用 Xbox 应用进行验证相关的详细信息。

如果这仍未解决问题，则再检查“开发人员中心绑定”和“应用 ID”的设置是否如上所述。

如果出现的错误在此处并未加以介绍，请参阅 xbox::services::xbox_live_error_code 文档中的错误列表，获取与错误代码相关的详细信息。 还可参阅 XSAPI 中包括的 errors.h。

尝试完所有这些方法后，如果仍无法登录游戏，请在[论坛](http://forums.xboxlive.com)中发布支持线程或联系你的 DAM。