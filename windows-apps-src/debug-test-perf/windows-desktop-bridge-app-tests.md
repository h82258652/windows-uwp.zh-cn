---
author: PatrickFarley
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: Windows 桌面桥应用测试
description: 使用桌面桥的内置测试以确保你的桌面应用进行了优化其转换为 UWP 应用。
ms.author: pafarley
ms.date: 12/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，应用认证
ms.localizationpriority: medium
ms.openlocfilehash: 96087d2a41eb443374d8cd9bda5608d6156f9173
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "3232409"
---
# <a name="windows-desktop-bridge-app-tests"></a>Windows 桌面桥应用测试

[桌面桥应用](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root)是 Windows 桌面应用程序转换为使用[桌面桥](https://developer.microsoft.com/en-us/windows/bridges/desktop)的通用 Windows 平台 (UWP) 应用。 转换后，将以面向 Windows 10 桌面版的 UWP 应用包（.appx 或 .appxbundle）的形式打包、维护和部署 Windows 桌面应用程序。

## <a name="required-versus-optional-tests"></a>必需测试与可选测试
对于 Windows 桌面桥应用的可选测试仅供参考，将不能用于在 Microsoft 应用商店载入过程中评估你的应用。 我们建议调查这些测试结果以生成质量更好的应用。 应用商店载入的整体通过/失败条件取决于必需测试，而不是这些可选测试。

## <a name="current-optional-tests"></a>当前可选测试

### <a name="1-digitally-signed-file-test"></a>1. 数字签名的文件测试 
**背景**  
此测试验证所有可移植可执行 (PE) 文件是否都包含有效签名。 数字签名文件的存在使用户可以了解软件是正版。

**测试详细信息**  
测试扫描包中的所有可移植可执行文件，并在其标头中检查签名。 建议对所有 PE 文件进行数字签名。 如果有任何 PE 文件未进行签名，则会生成警告。
 
**更正操作**  
始终建议使用数字签名文件。 有关详细信息，请参阅[代码签名简介](https://msdn.microsoft.com/en-us/library/ms537361(v=vs.85).aspx)。

### <a name="2-file-association-verbs"></a>2. 文件关联谓词 
**背景**  
此测试扫描包注册表以检查是否注册了任何文件关联谓词。 

**测试详细信息**  
可以使用各种通用 Windows 平台 (UWP) API 增强已转换的桌面应用。 此测试检查应用中的 UWP 二进制文件是否未调用非 UWP API。 UWP 二进制文件设置了 **AppContainer** 标志。

**更正操作**  
有关这些扩展以及如何正确使用它们的说明，请参阅[桌面到 UWP 桥：应用扩展](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-extensions)。 

### <a name="3-debug-configuration-test"></a>3. 调试配置测试
此测试验证 appx 不是调试版本。
 
**背景**  
要通过 Microsoft 应用商店的认证，应用必须不编译为调试，且不得引用可执行文件的调试版本。 此外，你必须生成优化代码才能使应用通过此测试。
 
**测试详细信息**  
测试应用，确保它不是调试版本并且未链接到任何调试框架。
 
**更正操作**  
* 提交到 Microsoft 应用商店之前，将应用编译为发行版本。
* 确保你安装了正确版本的 .NET Framework。
* 确保该应用未链接到框架的调试版本，并使用发布版本构建。 如果此应用包含 .NET 组件，请确保安装了正确的 .NET Framework 版本。

### <a name="4-package-sanity-test"></a>4. 程序包健全性测试
#### <a name="41-archive-files-usage"></a>4.1 存档文件的使用

**背景**  
此测试可帮助你生成更好的桌面桥应用以在 [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) 机器上运行。

**测试详细信息**  
此测试检查存档的文件内的所有可执行文件或自解压缩内容。 由于此类型的内容内包含的可执行文件在载入到 Windows 应用商店期间未签名，因此应用可能不会按预期在 Windows 10 S 系统上运行。
 
**更正操作**
* 考虑评估测试所标记的文件，以确定是否会影响到在 Windows 10 S 环境中运行的应用。
* 如果你的应用受到影响，从存档的文件中删除可执行文件，并且不使用自解压缩存档将可执行文件放在磁盘上。 这应该会阻止应用功能的丢失。

#### <a name="42-blocked-executables"></a>4.2 阻止的可执行文件

**背景**  
此测试可帮助你生成更好的桌面桥应用以在 [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) 机器上运行。 

**测试详细信息**  
该测试检查应用是否试图启动可执行文件，这在 Windows 10 S 系统上是受到限制的。 依赖于此功能的应用可能无法按预期在 Windows 10 S 系统上运行。 

**更正操作**  
* 确定来自测试的哪个已标记的条目表示不属于应用一部分的用于启动可执行文件的调用，并删除这些调用。 
* 如果已标记的文件是应用程序的一部分，你可以忽略警告。


## <a name="current-required-tests"></a>当前必需测试

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. 应用功能测试（特殊的使用功能）

**背景**  
专用许可范围专用于非常特定的场景。 仅允许公司帐户使用这些功能。 

**测试详细信息**  
验证应用是否在声明以下任何功能： 
* EnterpriseAuthentication
* SharedUserCertificates
* DocumentsLibrary

如果声明了其中任一功能，该测试将向用户显示警告。 

**更正操作**  
如果你的应用不需要特殊的使用功能，请考虑将其删除。 此外，使用这些功能应接受其他载入策略审查。

### <a name="2-app-manifest-resources-tests"></a>2. 应用清单资源测试 
#### <a name="21-app-resources-validation"></a>2.1 应用资源验证
如果应用部件清单 (manifest) 中声明的字符串或图像不正确，则应用可能未正确安装。 如果安装应用时出现了这些错误，应用的徽标或其他图像可能无法正确显示。    

**测试详细信息**  
检查应用部件清单 (manifest) 中定义的资源，确保它们是最新且有效的。

**更正操作**  
使用下表作为指南。

错误消息 | 备注
--------------|---------
图像 {image name} 定义 Scale 和 TargetSize 限定符；一次只能定义一个限定符。 | 可以自定义不同分辨率的图像。 在实际消息中，{image name} 包含有错误的图像名称。 确保每个图像都将 Scale 或 TargetSize 定义为限定符。 
图像 {image name} 不符合大小限制。  | 确保所有应用图像都符合相应的大小限制。 在实际消息中，{image name} 包含有错误的图像名称。 
程序包中缺少图像 {image name}。  | 缺少所需的图像。 在实际消息中，{image name} 包含缺少的图像名称。 
图像 {image name} 不是有效的图像文件。  | 确保所有应用图像都符合相应的文件格式类型限制。 在实际消息中，{image name} 包含无效的图像名称。 
图像“BadgeLogo”在位置 (x, y) 上具有一个无效的 ABGR 值 {value}。 像素必须为白色 (##FFFFFF) 或透明 (00######)  | 锁屏提醒徽标是显示在锁屏提醒通知旁边的图像，用于在锁屏上标识应用。 该图像必须是单色图像（它只能包含白色和透明像素）。 在实际消息中，{value} 在图像中包含无效的颜色值。 
图像“BadgeLogo”在位置 (x, y) 上具有一个对高对比度白色图像来说无效的 ABGR 值 {value}。 像素必须为 (##2A2A2A) 或更暗，或者透明 (00######)。  | 锁屏提醒徽标是显示在锁屏提醒通知旁边的图像，用于在锁屏上标识应用。 因为当处于高对比度白色背景中时锁屏提醒徽标出现在白色背景上，因此它必须是较暗版本的正常锁屏提醒徽标。 在高对比度白色背景中，锁屏提醒徽标只能包含比 (##2A2A2A) 暗或透明的像素。 在实际消息中，{value} 在图像中包含无效的颜色值。 
图像必须至少定义一个没有 TargetSize 限定符的变量。 它必须定义一个 Scale 限定符或者保持 Scale 和 TargetSize 为未指定状态，默认值为 Scale-100。  | 有关详细信息，请参阅有关[响应式设计](https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx)和[应用资源](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data)的指南。 
该程序包缺少一个“resources.pri”文件。  | 如果你在应用清单中包含可本地化的内容，请确保你的应用包包含有效的 resources.pri 文件。 
“resources.pri”文件必须包含一个其名称与程序包名称 {package full name} 相匹配的资源映射  | 如果清单发生更改并且 resources.pri 中的资源映射名称不再与清单中的程序包名称相匹配，你将遇到此错误。 在实际消息中，{package full name} 包含 resources.pri 必须包含的程序包名称。 为了解决此问题，你需要重新构建 resources.pri，而这样做的最简单方法就是重新构建应用包。 
“resources.pri”文件不得启用 AutoMerge。  | MakePRI.exe 支持一个名为 AutoMerge 的选项。 AutoMerge 的默认值为 off。 启用后，AutoMerge 在运行时将应用的语言包资源合并到一个 resources.pri 中。 我们不建议执行此操作适用于要通过 Microsoft 应用商店分发的应用。 通过 Microsoft 应用商店分发应用的 resources.pri 必须为应用的程序包的根目录中，并包含应用支持的所有语言参考。 
字符串 {string} 不符合 {number} 个字符的最大长度限制。  | 请参阅[应用包要求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)。 在实际消息中，{string} 替换为有错误的字符串并且 {number} 包含最大长度。 
字符串 {string} 不得包含前导空格/尾随空格。  | 应用部件清单 (manifest) 中元素的架构不允许前导空格或尾随空格字符。 在实际消息中，{string} 替换为有错误的字符串。 确保 resources.pri 中清单字段的任何本地化值都没有前导空格或尾随空格字符。 
字符串必须非空（长度大于零）  | 有关详细信息，请参阅[应用包要求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)。 
“resources.pri”文件中没有指定的默认资源。  | 有关详细信息，请参阅有关[应用资源](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data)的指南。 在默认版本配置中，Visual Studio 在生成捆绑包时仅在应用包中包含比例为 200 的图像资源，从而将其他资源放入资源包中。 确保你包含比例为 200 的图像资源或将项目配置为包含你拥有的资源。 
“resources.pri”文件中没有指定的资源值。  | 确保应用清单具有在 resources.pri 中定义的有效资源。 
图像文件 {filename} 必须小于 204800 字节。  | 减小指示图像的大小。 
{filename} 文件不能包含反向映射部分。  | 如果反向映射是在调用 makepri.exe 时在 Visual Studio 的 F5 调试期间生成的，通过在生成 pri 文件时运行 makepri.exe（不具有 /m 参数）可删除该反向映射。 



#### <a name="22-branding-validation"></a>2.2 品牌验证
**背景**  
桌面桥应用应该完整并且功能齐全。 使用默认图像（来自模板或 SDK 示例）的应用会带来很差的用户体验，且无法在应用商店目录中方便地标识。

**测试详细信息**  
该测试将验证应用使用的图像不是 SDK 示例或 Visual Studio 中的默认图像。 

**更正操作**  
将默认图像替换为更能区别和代表该应用的图像。

### <a name="3-package-compliance-tests"></a>3. 包合规性测试
#### <a name="31-app-manifest"></a>3.1 应用部件清单 (manifest)
测试应用部件清单 (manifest) 的内容，确保它的内容是正确的。

**背景**  
应用必须拥有格式正确的应用部件清单 (manifest)。

**测试详细信息**  
检查应用部件清单 (manifest) 以验证内容是否正确，如[应用包要求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)中所述。 此测试中会进行以下检查：
* **文件后缀名和协议**  
应用可能会声明它可以关联的文件类型。 大量不常见文件类型的声明会形成较差的用户体验。 此测试会限制可以与应用关联的文件扩展名的数量。
* **框架依赖关系规则**  
此测试强制要求应用声明对 UWP 的适当依赖关系。 如果存在不适当的依赖关系，该测试将失败。 如果应用面向的操作系统版本和框架依赖关系采用的操作系统版本不匹配，该测试将失败。 如果应用引用了任何“预览”版的框架 DLL，该测试也将失败。
* **进程间通信 (IPC) 验证**  
此测试强制要求桌面桥应用不在应用容器外部与桌面组件通信。 进程间通信仅适用于旁加载应用。 使用等效于 `DesktopApplicationPath` 的名称指定 [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) 的应用无法通过此测试。  

**更正操作**  
针对[应用包要求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)中所述的要求检查应用部件清单 (manifest)。


#### <a name="32-application-count"></a>3.2 应用程序计数
此测试验证应用包（.appx 应用程序包）中是否包含某个应用程序。 

**背景**  
此测试依据应用商店策略进行实现。 

**测试详细信息**  
此测试验证程序包中的 .appx 包总数是否小于 512，以及程序包中是否只存在一个“主”包。 它还验证程序包版本的修订号是否设置为 0。 

**更正操作**  
确保应用包和程序包满足**测试详细信息**中所述的要求。


#### <a name="33-registry-checks"></a>3.3 注册表检查
**背景**  
此测试检查应用程序是否安装或更新任何新的服务或驱动程序。

**测试详细信息**  
测试在 registry.dat 文件中查找特定注册表位置的更新，这些更新指示注册了新的服务或驱动程序。 如果应用在尝试安装驱动程序或服务，则测试会失败。  

**更正操作**  
检查失败并删除有问题的服务或驱动程序（如果不需要）。 当应用依赖于这些服务或驱动程序时，如果要载入到应用商店，则需要修订应用。


### <a name="4-platform-appropriate-files-test"></a>4. 平台的相应文件测试
安装混合二进制文件的应用可能崩溃或者不能正确运行，具体取决于用户的处理器体系结构。 

**背景**  
此测试扫描应用包中的二进制文件，确定是否存在体系结构冲突。 应用包不得包含无法在部件清单 (manifest) 指定的处理器体系结构上使用的二进制文件。 包含不受支持的二进制文件可能会导致应用发生崩溃，或造成应用包大小出现不必要的增加。 

**测试详细信息**  
在与应用包处理器体系结构声明交叉引用时，验证可移植可执行标头中每个文件的“位元”是否适当。 

**更正操作**  
遵循以下指南，确保你的应用包仅包含应用部件清单 (manifest) 中指定的体系结构支持的文件。 
* 如果应用的目标处理器体系结构为非特定处理器类型，则应用包不能包含 x86、x64 或 ARM 二进制文件或图像类型文件。
* 如果应用的目标处理器体系结构为 x86 处理器类型，则应用包必须仅包含 x86 二进制文件或图像类型文件。 如果应用包包含 x64 或 ARM 二进制文件或图像类型文件，将无法通过这项测试。
* 如果应用的目标处理器体系结构为 x64 处理器类型，则应用包必须包含 x64 二进制文件或图像类型文件。 请注意，在这种情况下，应用包还可以包含 x86 文件，但主要应用体验应使用 x64 二进制文件。 如果应用包包含 ARM 二进制文件或图像类型文件，或者*仅* 包含 x86 二进制文件或图像类型文件，将无法通过这项测试。
* 如果应用的目标处理器体系结构为 ARM 处理器类型，则应用包必须仅包含 ARM 二进制文件或图像类型文件。 如果应用包包含 x64 或 x86 二进制文件或图像类型文件，将无法通过这项测试。 

### <a name="5-supported-api-test"></a>5. 支持的 API 测试
检查应用是否使用了任何不兼容的 API。 

**背景**  
桌面桥应用可以利用一些旧版 Win32 API 以及现代 API（UWP 组件）。 此测试识别使用不支持 API 的托管二进制文件。
 
**测试详细信息**  
此测试检查应用中的所有 UWP 组件：
* 验证，应用包中的每个托管二进制文件均不依赖于通过检查二进制文件的导入地址表不支持用于 UWP 应用开发的 Win32 API。
* 验证应用包中的每个托管二进制文件是否均不依赖于批准的配置文件以外的功能。 

**更正操作**  
这可以通过确保应用编译为发行版本而不是调试版本来进行更正。 

> [!NOTE]
> 应用的调试版本将无法通过此测试，即使该应用使用仅[适用于 UWP 应用的 Api](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)。 检查错误消息，识别存在 API 不允许的 API 适用于 UWP 应用。 

> [!NOTE]
> 内置于该调试配置中的 c + + 应用将无法通过此测试，即使配置仅适用于 UWP 应用使用 Windows SDK 中的 Api。 有关详细信息，请参阅[UWP 应用中的 Windows Api 的替代项](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx)。

### <a name="6-user-account-control-uac-test"></a>6. 用户帐户控制 (UAC) 测试  

**背景**  
确保应用在运行时不需要用户帐户控制。

**测试详细信息**  
应用不能请求管理员提升或 UIAccess 每个 Microsoft Store 策略。 不支持提升的安全权限。 

**更正操作**  
应用必须作为交互用户来运行。 有关详细信息，请参阅 [UI 自动化安全概述](https://go.microsoft.com/fwlink/?linkid=839440)。

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Windows 运行时元数据验证
**背景**  
确保随应用发送的组件符合 UWP 类型系统。

**测试详细信息**  
此测试引发一些与正确类型使用相关的标志。

**更正操作**  
* **ExclusiveTo 属性**  
确保 UWP 类未实现标记为 ExclusiveTo 其他类的接口
* **一般元数据正确性**  
确保用于生成相应类型的编译器符合最新的 UWP 规范。
* **属性**  
确保 UWP 类中的所有属性都具有 `get` 方法（`set` 方法是可选的）。 对于所有属性，确保 `get` 方法返回的类型与 `set` 方法输入参数的类型匹配。
* **类型位置**  
确保所有 UWP 类型的元数据位于在应用包中具有最长命名空间匹配名称的 .winmd 文件中。
* **类型名称区分大小写**  
确保所有 UWP 类型在应用包中具有不区分大小写的唯一名称。 还要确保没有任何 UWP 类型名称在应用包中用作命名空间名称。
* **类型名称正确性**  
确保在全局命名空间或 Windows 顶级命名空间中没有 UWP 类型。
 

### <a name="8-windows-security-features-tests"></a>8. Windows 安全功能测试
更改默认的 Windows 安全保护可能增加客户的风险。 

#### <a name="81-banned-file-analyzer"></a>8.1 禁止的文件分析器
**背景**  
某些文件已使用重要安全性、可靠性或其他改进进行了更新。 Windows 桌面桥应用必须包含这些文件的最新版本，因为过期版本存在风险。 Windows 应用认证工具包会阻止这些文件，以确保所有应用都使用当前版本。

**测试详细信息**  
Windows 应用认证工具包中“对被禁止文件的检查”当前会对以下文件进行检查：
* *Bing.Maps.JavaScript\js\veapicore.js*  
当应用使用“Release Preview”版本的文件而不是最新官方版本时，此检查通常会失败。 

**更正操作**  
若要解决此问题，适用于 UWP 应用使用最新版本的[必应地图 SDK](http://go.microsoft.com/fwlink/p/?linkid=614880) 。

#### <a name="82-private-code-signing"></a>8.2 私有代码签名
测试应用包中是否存在私有代码签名二进制文件。 

**背景**  
私有代码签名文件应该保持私有，因为在泄露这些文件的事件中，它们可能会被恶意使用。 

**测试详细信息**  
在应用包中检查扩展名为 .pfx 或 .snk 的文件，这指示其中包含私有签名密钥。 

**更正操作**  
从包中删除任何私有代码签名密钥（例如 .pfx 和 .snk 文件）。


## <a name="related-topics"></a>相关主题

* [Microsoft Store 策略](https://msdn.microsoft.com/library/windows/apps/Dn764944)
