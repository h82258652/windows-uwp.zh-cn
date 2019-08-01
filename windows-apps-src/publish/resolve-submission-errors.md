---
Description: 如果将应用提交到应用商店后遇到错误，必须解决这些错误才能继续进行认证过程。
title: 解决提交错误
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 63bb2ae4d79266a28cf92434e0018dbaedd6a73f
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682701"
---
# <a name="resolve-submission-errors"></a>解决提交错误

如果将应用提交到应用商店后遇到错误，必须解决这些错误才能继续进行[认证过程](the-app-certification-process.md)。 错误消息将指示问题所在以及为解决该问题而需要执行的操作。 下面是一些可帮助你解决这些错误的其他信息。

## <a name="uwp-apps"></a>UWP 应用

如果你正在提交 UWP 应用, 则在预处理过程中, 如果你的包文件不是由 Visual Studio 为存储生成的 msixupload 或 .appxupload 文件, 则可能会出现错误。 请确保按照在创建应用包文件时[使用 Visual Studio 打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)中的步骤进行操作, 并且仅在提交的 "[包](upload-app-packages.md)" 页上上传 msixupload 或 .appxupload 文件, 而不是 .msix/appx 或 .msixbundle/.appxbundle.

如果显示了编译错误，请确保能够在发布模式中成功生成应用程序。 有关详细信息，请参阅 [.NET 本机内部编译器错误](https://go.microsoft.com/fwlink/p/?LinkID=613098)。

## <a name="desktop-application"></a>桌面应用程序

如果打算提交包含 Win32 和 UWP 二进制文件的包, 请确保使用 Visual Studio 2017 Update 4 和更高版本中提供的 Windows 打包项目创建该包。 如果使用 UWP 项目模板创建包, 可能无法将该包提交到存储区或将其旁加载到其他电脑上。 即使包成功发布, 它也可能会以意外的方式在用户的电脑上运行。 有关详细信息, 请参阅[使用 Visual Studio 打包应用 (台式计算机)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)。

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 3.x 和更早版本

> [!IMPORTANT]
> 从2018年10月31日起, 新创建的产品不能包含针对 Windows Phone 3.x 或更早版本的包。 有关详细信息, 请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

在预处理过程中检测到 Windows Phone 程序包的问题时，你可能会看到**错误 2001**。 在大多数情况下，需要重新生成你的应用包来更正错误。 完成后，请在提交的[程序包](upload-app-packages.md)页面上将旧程序包替换为新程序包，然后再单击“提交到应用商店”  。

有多种错误可能会导致此错误。 查看下面的列表来确定哪一种情形可能适用于你的程序包。

-   **包中的一个或多个程序集的模糊处理不正确:** 使用其他工具执行模糊处理, 或删除模糊处理。 编译进程将优化混淆的程序集，但有时以不受支持的方式使用修改 MSIL 的工具来混淆某些程序集将会导致错误。
-   **应用中的一个或多个方法的大小超过了 IL 的 256 KB:** 将有问题的方法重构为较小的函数。 对于程序集中方法的 MSIL 的大小，可以通过使用 ILDASM 工具来确定。
-   **一个或多个程序集的强名称签名验证失败:** 当使用与程序集元数据中所需的密钥不同的密钥执行强名称签名时, 通常会发生此错误。 使用正确的密钥签名或删除强名称签名。
-   **包包含混合模式 (带有托管代码和本机代码) 程序集:** Windows Phone 不支持混合模式程序集。 从程序包删除混合模式程序集并重新提交应用。
-   **Windows Phone 8.1 XAP 或 appx/.appxbundle 程序集无效:** 请确保 winmd 文件至少具有一个公共入口点。 如有需要，可以使用任何反编译程序应用程序查看代码并检查公共入口点。

在提交应用后，你可能会看到的另一个错误是**错误 1300**。 当一个或多个程序集（或整个程序包）已预编译时会发生此错误。 若要解决此问题，请在 Microsoft Visual Studio 中重新编译应用包，然后提交新生成的程序包。

## <a name="nameidentity-errors"></a>名称/标识错误

如果看到一条错误消息, **指出包中找到的名称不是保留的应用程序名称之一。请保留应用名称, 并/或使用此语言**的正确应用名称更新你的包, 原因可能是你在包中输入的名称不正确。 如果你使用的应用名称未在合作伙伴中心保留, 也会出现此错误。 通常，你可以遵循以下步骤来解决此错误：

- 转到应用[应用标识](view-app-identity-details.md)页（在**应用管理**下），确认应用是否分配了标识。 如果没有，将看到创建一个标识的选项。 需要保留一个应用名称才能创建该标识。 请确保这是已在程序包中使用过的名称。
- 如果应用已经具有标识，可能仍然需要保留要在程序包中使用的名称。 在**应用管理**下，单击[管理应用名称](manage-app-names.md)。 输入你想要使用的名称，然后单击“保留应用名称”  。

> [!IMPORTANT]
>  如果想要使用的名称不可用，可能是其他应用已经保留了该名称。 如果应用已经使用该名称发布，或者如果认为你有权使用它，请[联系支持人员](https://go.microsoft.com/fwlink/p/?LinkId=331509)。  

 

 




