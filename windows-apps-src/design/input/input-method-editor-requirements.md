---
Description: 开发自定义输入法编辑器 (IME) ，以帮助用户输入不能在标准键盘上轻松表示的语言的文本。
title: 输入法编辑器 (IME) 要求
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: ime、输入法编辑器、输入、交互
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ecf150973defb0a431fc7248181ddf648576ac77
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87840014"
---
# <a name="custom-input-method-editor-ime-requirements"></a>自定义输入法编辑器 (IME) 要求

这些指导原则和要求可帮助您开发自定义的输入法编辑器 (IME) ，以帮助用户输入不能在标准键盘上轻松表示的语言的文本。

有关 Ime 的概述，请参阅[输入法编辑器 (IME) ](input-method-editors.md)。

## <a name="default-ime"></a>默认 IME

用户可以选择其任何活动 Ime (**设置-> 时间 & 语言 > 语言 > 首选语言-> 语言包选项**) 为其首选语言的默认 IME。

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="首选语言设置":::

选择首选语言的 "语言选项" 设置屏幕上的默认键盘。

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="首选语言键盘":::

> [!Important]
> 我们不建议直接写入注册表来设置自定义 IME 的默认键盘。

## <a name="compatibility-requirements"></a>兼容性要求

下面是自定义 IME 的基本兼容性要求。

### <a name="ime-must-be-compatible-with-windows-apps"></a>IME 必须与 Windows apps 兼容

使用[文本服务框架 (TSF) ](/windows/win32/tsf/text-services-framework)来实现 ime。 以前，你可以选择使用[输入法管理器 (IMM32) ](/windows/win32/intl/input-method-manager)用于输入服务。 现在，系统会阻止使用输入法管理器实现的 Ime (IMM32) 。

当应用程序启动时，TSF 将为用户当前选择的 IME 加载 IME DLL。 加载 IME 后，它将受到与应用相同的应用容器限制。 例如，如果应用程序未在其清单中请求 Internet 访问权限，则 IME 无法访问 Internet。 此行为可确保 Ime 不会违反安全协定。

TSF 是应用程序和 IME 之间的中介。 TSF 向 IME 传达输入事件，并在用户选择字符后接收输入字符。

此行为与以前版本的 Windows 相同，但加载到 Windows 应用会影响 IME 的可能功能。

如果输入法需要在 Windows 应用和桌面应用之间提供不同的功能或 UI，请确保 TSF 加载的 DLL 检查要加载到的应用类型。 调用 IME 中的[ITfThreadMgrEx：： GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)方法，并检查 TF_TMF_IMMERSIVEMODE 标志，使 IME 根据结果触发不同的应用程序逻辑。

Windows 应用不支持表文本服务 (TTS) Ime。

> [!NOTE]
> 某些用于生成 TTS Ime 的工具会生成由 Windows 标记为恶意软件的 Ime。

### <a name="ime-must-be-compatible-with-the-system-tray"></a>IME 必须与系统托盘兼容

没有用于托管 IME 图标的语言栏。 相反，系统任务栏上会显示一个指示当前输入选项的输入标记。 输入指示器只显示 IME 标记图标，以指示当前正在运行的 IME。 此外，用户可以在 IME 标记图标的左侧显示一个 IME 模式图标，用户可以使用该图标来执行最常用的 IME 模式切换，例如打开或关闭 IME。

输入指示器仅显示兼容 Ime 的 IME 标记图标和模式图标。 不兼容的 Ime 不会在系统托盘中显示品牌图标和模式图标。 相反，输入指示器显示语言缩写，而不是 IME 标记图标。

将 IME 图标存储在 DLL 或 EXE 文件中，而不是单独的 .ico 文件中。 IME 图标的设计必须遵循以下 UI 设计指南部分中所述的指导原则。

### <a name="ime-branding-icon"></a>IME 标记图标

输入指示器在系统上注册输入法时，通过使用 IME 定义的资源 ID，从 IME DLL 中获取 IME 标记图标。

### <a name="ime-mode-icon"></a>IME 模式图标

某些输入法可能需要依赖于系统托盘上显示的输入指示器来显示 IME 模式图标。 在这种情况下，IME 使用 GUID_LBI_INPUTMODE 将 IME 模式图标传递到输入指示器。

将 IME 模式图标传递到系统任务栏上的输入指示器时，IME 模式图标的默认大小为16x16 像素。 UI 缩放遵循 DPI。

将 IME 模式图标传递到安全桌面) 中的 UAC (用户帐户控制时，IME 模式图标的默认大小为20x20 像素。 UAC 上的 IME 模式图标的 UI 缩放在 PPI 之后。

## <a name="ime-must-work-in-app-container"></a>IME 必须在应用容器中运行

某些 IME 函数在应用容器中受到影响。

- **字典文件**-通常，ime 具有只读字典文件，用于将用户输入映射到特定字符。 若要从应用容器内部访问这些文件，IME 必须将它们放在程序文件或 Windows 目录下。 默认情况下，可以从应用容器读取这些目录，因此 Ime 可以访问存储在这些位置的字典文件。 如果 IME 必须将字典文件存储在其他位置，则必须显式操作[访问控制列表 (ACL) ](/windows/win32/secauthz/access-control-lists)字典文件，以允许从应用容器访问。
- **Internet 更新**-如果你的 IME 需要使用来自 Internet 的数据更新其字典，则无法在应用程序容器中可靠地执行此操作，因为不会始终允许 Internet 访问。 相反，IME 应运行单独的桌面进程，该进程负责使用 Internet 上的数据更新字典文件。
- **动态学习**-如果在具有 Internet 访问权限的应用容器中运行了 ime，则 ime 可以与之通信的终结点没有限制。 在这种情况下，IME 可以使用云服务器来提供实时学习服务。 某些 Ime 会在用户键入内容时，动态下载和上传用户输入。 由于不能在应用容器中保证 Internet 访问，因此不一定会允许这样做。
- 在**进程之间共享信息**-ime 可能需要在不同应用容器中的应用之间共享有关用户输入首选项的数据。 使用 web 服务在应用之间共享数据。

> [!Important]
> 如果尝试绕过应用容器安全规则，则 IME 可能被视为恶意软件并被阻止。

## <a name="ime-and-touch-keyboard"></a>IME 和触摸键盘

IME 必须确保其候选面板的 UI 以及其他 UI 元素不会绘制在触摸键盘的下方。 触摸键盘与所有应用显示在 z 顺序较高的频带中，IME UI 与活动的应用显示在同一 z 顺序带区中。 因此，触摸键盘可以重叠并隐藏 IME UI。 在大多数情况下，应用应调整其窗口大小，以用于触控键盘。 如果应用没有调整大小，IME 仍可使用[InputPane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) API 来获取触摸键盘的位置。 IME 查询[Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location)属性，或为触摸键盘的显示和隐藏事件注册处理程序。 每次用户点击编辑字段时都会引发 Show 事件，即使当前显示的是触摸键盘也是如此。 IME 可以使用此 API 来获取触碰 (或其他) 的 UI 之前的触摸键盘所用的屏幕空间，并重排 Ime UI，以避免在触摸键盘下绘制。

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>指定首选触摸键盘布局

IME 可以指定要使用的触摸键盘布局，并且启用了 IME 来处理触摸优化的布局。 此功能仅限于朝鲜语、日语、简体中文和繁体中文输入语言的 Ime。

触摸键盘支持7个布局，其中三个为经典布局，其中的四个为触控优化布局。 经典布局的外观和行为与物理键盘相同。

这三种经典布局均用于在不同形式中输入繁体中文：

- 基于拼音的输入
- 仓颉输入
- 大易输入

除了经典布局，每个韩语、日语、简体中文和繁体中文输入语言都有一个触摸优化的布局。

若要使用此功能，IME 必须实现[ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)接口，该接口由 IME 使用文本服务框架[ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) API 导出。

如果 IME 不支持 ITfFnGetPreferredTouchKeyboardLayout 接口，则使用 IME 会使触控键盘显示的语言使用默认的经典布局。

如果输入法需要将其中一种经典布局设置为首选布局，则除了支持 ITfFnGetPreferredTouchKeyboardLayout 和 ITfFunctionProvider 接口外，IME 不需要额外的工作。 但 IME 中需要额外的工作才能使用触摸优化布局，下一部分将对此进行介绍。

### <a name="touch-optimized-layout"></a>触摸优化布局

用于朝鲜语、日语、简体中文和繁体中文输入语言的触摸优化键盘在转换模式下显示了不同的输入法和 IME。 触摸键盘上有一个用于将 IME 转换模式设置为 "开" 或 "关" 的键，但键盘的 IME 模式也可能随着编辑控件中的焦点更改而更改。

日语、简体中文和繁体中文输入语言的触摸优化键盘包含 IME 用来浏览候选页的键或键。 对于日语和简体中文，候选页键显示在触摸优化布局上。 对于繁体中文，前一个和下一个候选页有单独的键。

按下这些键时，触摸键盘将调用[SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)函数，以将以下 Unicode 专用区域字符发送到突出输入的应用程序，IME 可以截获和操作：

- **下一页 (0xF003) ** -在按下了用于日语和简体中文的触摸优化键盘上时，或在繁体中文的触摸优化键盘上按下页面键时发送。
- **上一页 (0xF004) ** -在按下候选页键的同时按下了用于日语和简体中文的触摸优化键盘上的 Shift 键时，或在繁体中文上按下了触摸优化键盘时发送。

这些字符将作为 Unicode 输入发送。 下一段详细说明了如何在文本服务框架输入法接收的关键事件接收器通知期间提取字符信息。 不会在任何标头文件中定义这些字符值，因此需要在代码中定义这些字符值。

若要截获键盘输入，IME 必须注册为 key 事件接收器。 对于通过使用 SendInput 函数生成的 Unicode 输入， [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink)回调的 WPARAM 参数 (OnKeyDown，OnKeyUp，OnTestKeyDown，OnTestKeyUp) 始终包含虚拟密钥 VK_PACKET，并且不会直接标识该字符。

实现以下调用序列以访问字符：

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>IME 搜索集成

通过搜索约定向用户提供搜索功能，并与搜索窗格集成。

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="搜索窗格和 IME 建议":::<br/>
*搜索窗格和 IME 建议*

搜索窗格是用户在所有应用中执行搜索的中心位置。 对于 IME 用户，Windows 提供了独特的搜索体验，允许兼容 Ime 与 Windows 集成，以实现更高的效率和可用性。

如果用户使用与搜索兼容的 IME 键入内容，则有两个主要优点：

- IME 与搜索体验之间的无缝交互。 IME 在搜索框下内联显示，无需 occluding 搜索建议。 用户可以使用键盘在搜索框、IME 转换候选项和搜索建议之间无缝导航。
- 更快地访问应用程序提供的相关结果和建议。 应用有权访问所有当前的转换候选项，以提供更多相关建议。 为了更好地确定搜索建议的优先级，按相关性顺序向应用程序提供转换。 用户只需键入拼音即可查找并选择他们想要的结果，而无需转换。

如果符合以下条件，则 IME 与集成的搜索体验兼容：

- 与 Windows 样式 shell 兼容。
- 实现 TSF UILess 模式 Api。 有关详细信息，请参阅[UILess 模式概述](/windows/win32/tsf/uiless-mode-overview)。
- 实现 TSF 搜索集成 Api、 [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)和[ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)。

当在 "搜索" 窗格中激活时，将在 UIless 模式下放置兼容的 IME 并且无法显示其 UI。 相反，它会将候选转换发送到 Windows，这会在内联候选项列表控件中显示它们，如前面的屏幕截图所示。

此外，IME 发送应用于运行当前搜索的候选项。 这些候选项可以与转换候选项相同，也可以针对搜索进行定制。

优秀搜索候选项满足以下条件：

- 无前缀重叠。 例如，北京大学 and北京是冗余的，因为其中一个是另一个的前缀。
- 无冗余候选项。 任何多余的候选项不适合搜索，因为它不能帮助筛选结果。 例如，任何与北京大学匹配的结果也会与北京匹配。
- 无预测候选项，仅转换。 例如，如果用户键入 "北"，则 IME 可以将返回为候选项，但不能返回北京大学。 通常，预测候选人过于严格。

不符合条件的 Ime 与其他控件的搜索显示方式不兼容，并且无法利用 UI 集成和搜索候选项。 应用仅在用户完成撰写后接收查询。

当支持搜索协定的应用收到查询时，该查询事件包含一个 "queryTextAlternatives" 数组，该数组包含所有已知的替代项，该数组由最相关的 (可能) 到最不)  (不太相关。

如果提供了替代方法，则应用程序应将每个替代项视为查询，并返回所有匹配项的结果。 应用程序的行为应如同用户同时发出了多个查询，实际上是向提供结果的服务发出了 "或" 查询。 出于性能方面的考虑，应用通常会限制与最相关备选方案的5到20之间的匹配。

## <a name="ui-design-guidelines"></a>UI 设计准则

所有 Ime 都必须遵循[设计和代码 Windows 应用](/windows/uwp/design/)中所述的用户体验指导原则。

### <a name="dont-use-sticky-windows"></a>不要使用粘滞窗口

IME 窗口应仅在需要时显示，并且它们不应始终可见。 当用户不需要键入时，IME windows 不应显示。 IME 窗口不应为全屏窗口。 IME windows 不应相互重叠。 Windows 应采用 Windows 样式设计，并遵循 UI 缩放。

### <a name="ime-icons"></a>IME 图标

有两种类型的 IME 图标：品牌图标和模式图标。 所有 IME 图标都必须仅设计为黑白颜色。 新的 IME 图标借用系统托盘图标 glyphic 的外观。 此样式已经创建，因此所有语言都可以使用它来补充 familial 外观，同时也区分开来。

IME 图标的文件格式是 .ICO。 您必须提供以下图标大小。

- 16x16 像素
- 20x20 像素
- 24x24 像素
- 32x32 像素
- 40x40 像素
- 48x48 像素

确保所有分辨率中都提供了带 alpha 通道的32位图标。

IME 品牌图标由一个白色框定义，其中，新式字样中呈现的印刷标志符号。 每个语言组都选择每个定义标志符号。 标志符号为黑色。 此框包含以黑色为50% 的不透明度的 "1" 像素的外部笔划。 "新" 版本由框左上角的圆角定义。

IME 模式图标由新式字样中的白色排字标志符号定义，其中包含以50% 的不透明度表示的黑色1像素的外部笔划。

| 图标 | 描述 |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="繁体中文 ChangeJie 的 IME 图标示例。"::: | 繁体中文 ChangeJie 的 IME 图标示例。 |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="繁体中文 New ChangeJie 的输入法标记图标示例。"::: | 繁体中文 ChangeJie 的 IME 图标示例。 |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="中文模式图标"::: | 示例输入法模式图标。 |

### <a name="owned-window"></a>拥有的窗口

若要显示候选 UI，IME 必须将其窗口设置为拥有窗口，以便它可以显示当前正在运行的应用程序。 使用[ITfContextView：： GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)方法检索要拥有的窗口。 如果 GetWnd 返回错误或 NULLHWND，则调用[GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus)函数。

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>IME 候选窗口与浅消除曲面交互

弹出式窗口的消除模型称为 "轻型消除"，因为用户可以轻松关闭此类窗口。 为了使 Ime 在 Windows 交互模型中正常工作，IME Windows 必须参与浅色消除模型。

为了参与浅色消除模型，IME 必须使用[NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent)函数或类似函数引发三个新的 Windows 事件。 这些新事件包括：

- **EVENT_OBJECT_IME_SHOW** -IME 变为可见时引发此事件。
- **EVENT_OBJECT_IME_HIDE** -隐藏 IME 时引发此事件。
- **EVENT_OBJECT_IME_CHANGE** -IME 移动或更改大小时引发此事件。

### <a name="declaring-compatibility"></a>声明兼容性

Ime 通过使用[ITfCategoryMgr：： RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory)为其 IME 注册类别 GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT 来声明它们是兼容的。

### <a name="set-the-default-ime-mode-to-on"></a>将默认 IME 模式设置为打开

我们为 Ime 提供了更好的 UX。

## <a name="dpi-scaling-support-for-desktop-applications"></a>DPI 缩放支持桌面应用程序

增强的 DPI 缩放支持支持查询每个桌面进程的已声明 DPI 感知级别，以确定是否需要缩放 UI。 在多监视器方案中，Windows 会针对每个监视器上的不同 DPI 设置适当地缩放 UI。

由于 IME 在每个应用程序进程的上下文中运行，因此不应为 IME 声明 DPI 感知级别。 这可确保 IME 在当前进程的 DPI 感知级别下运行。

若要确保所有 IME UI 元素与正在运行的进程的 UI 元素具有缩放性，则必须相应地响应不同的 DPI 值。

> [!NOTE]
> 为了确保使用新的桌面应用程序进行奇偶校验，IME 应支持每个监视器– DPI 感知，但不应声明级别本身。 系统在每个方案中确定适当的缩放要求。

有关 DPI 缩放对桌面应用程序的支持的详细信息，请参阅[高 dpi](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)。

## <a name="ime-installation"></a>IME 安装

如果使用 Microsoft Visual Studio 生成输入法，请使用第三方安装程序（如 InstallShield Flexera Software）为 IME 创建安装体验。

以下步骤演示如何使用 InstallShield 为 IME DLL 创建安装项目。

- 安装 Visual Studio。
- 启动 Visual Studio。
- 在 "**文件**" 菜单上，指向 "**新建**"，然后选择 "**项目**"。 此时将打开 "**新建项目**" 对话框。
- 在左窗格中，导航到 "**模板 > 其他项目类型 >" 安装和部署**"，单击"**启用 InstallShield 受限版本**"，然后单击 **" 确定 "**。 按照安装说明进行操作。
- 重启 Visual Studio。
-  ( .sln) 文件中打开 IME 解决方案。
- 在解决方案资源管理器中，右键单击解决方案，指向 "**添加**"，然后选择 "**新建项目**"。 此时将打开 "**添加新项目**" 对话框。
- 在左侧树视图控件中，导航到 "**模板 > 其他项目类型 >" InstallShield 受限版本**"。
- 在中心窗口中，单击 " **InstallShield 受限版本项目**"。
- 在 "**名称**" 文本框中，键入 "SetupIME"，然后单击 **"确定**"。
- 在 "**项目助手**" 对话框中，单击 "**应用程序信息**"。
- 填写公司名称和其他字段。
- 单击 "**应用程序文件**"。
- 在左窗格中，右键单击 " **[INSTALLDIR]** " 文件夹，然后选择 "**新建文件夹**"。 将文件夹命名为 "插件"。
- 单击 "**添加文件**"。 导航到 IME DLL，并将其添加到 "**插件**" 文件夹中。 为 IME 词典重复此步骤。
- 右键单击 IME DLL，然后选择 "**属性**"。 此时将打开 "**属性**" 对话框。
- 在 "**属性**" 对话框中，单击 " **COM & .net 设置**" 选项卡。
- 在 "**注册类型**" 下，选择 "**自注册**"，然后单击 **"确定"**。
- 生成解决方案。 IME DLL 生成后，InstallShield 创建一个 setup.exe 文件，使用户能够在 Windows 上安装 IME。

若要创建自己的安装体验，请在安装过程中调用[ITfInputProcessorProfileMgr：： RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile)方法来注册 IME。 请勿直接写入注册表项。

如果 IME 必须在安装后立即使用，请调用[InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) ，以使用 psz 参数的以下格式将 IME 添加到启用用户的输入方法：

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>IME 辅助功能

实现以下约定，使 Ime 符合辅助功能要求，并与讲述人合作。 若要使候选列表可访问，你的 Ime 必须遵循此约定。

- 对于预测候选项列表，候选项列表必须具有等于 "IME_Candidate_Window" 的**UIA_AutomationIdPropertyId**或 "IME_Prediction_Window"。
- 当候选项列表出现并消失时，它会分别引发**UIA_MenuOpenedEventId**和**UIA_MenuClosedEventId**类型的事件
- 当当前选定的候选项发生更改时，候选项列表将引发**UIA_SelectionItem_ElementSelectedEventId**。 所选元素的属性应**UIA_SelectionItemIsSelectedPropertyId**等于**TRUE**。
- 候选列表中每一项的**UIA_NamePropertyId**必须是候选项的名称。 还可以通过**UIA_HelpTextPropertyId**提供其他信息来消除候选项的歧义。

## <a name="related-topics"></a>相关主题

- [输入法编辑器 (IME)](input-method-editors.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [辅助功能](/windows/uwp/design/accessibility/accessibility)
