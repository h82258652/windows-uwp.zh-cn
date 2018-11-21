---
author: Karl-Bridge-Microsoft
Description: User interactions in the Universal Windows Platform (UWP) are a combination of input and output sources (such as mouse, keyboard, pen, touch, touchpad, speech, Cortana, controller, gesture, gaze, and so on), along with various modes or modifiers that enable extended experiences (including mouse wheel and buttons, pen eraser and barrel buttons, touch keyboard, and background app services).
title: 交互入门
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9babc1f96b83123cef4bf103f4d13696697cc897
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7555742"
---
# <a name="interaction-primer"></a>交互入门

![Windows 输入类型](images/input-interactions/icons-inputdevices03.png)

通用 Windows 平台 (UWP) 中的用户交互组合了输入和输出源（例如鼠标、键盘、笔、触摸、触摸板、语音、**Cortana**、控制器、手势、注视等）以及支持扩展体验（包括鼠标滚轮和按钮、笔橡皮擦、筒状按钮、触摸键盘和后台应用服务）的各种模式或修饰符。

UWP 使用“智能”的上下文交互系统，在大多数情况下消除了单独处理应用接收的独特输入类型的需要。 这包括处理作为泛型指针类型的触摸、触摸板、鼠标和笔输入，以支持静态手势（例如点击或长按）、操作手势（例如滑动平移或呈现数字墨迹）。

当与特定的外形规格配对时，自行熟悉每个输入设备类型及其行为、功能和限制。 这可以帮助你决定平台控件和提示是足够用于应用，还是要求你提供自定义的交互体验。

## <a name="gaze"></a>凝视

对于 **Windows 10 2018 年 4 月更新**，我们使用眼睛和头部跟踪输入设备引入了对凝视输入的支持。 

> [!NOTE]
> **Windows 10 Fall Creators Update** 以及[目视控制](https://support.microsoft.com/en-us/help/4043921/windows-10-get-started-eye-control)中引入了对目视跟踪硬件的支持，这是一项内置功能，让你可以使用眼睛控制屏幕指针，使用屏幕键盘键入，并使用文本到语音转换与其他人交流。

### <a name="device-support"></a>设备支持

- 平板电脑
- 电脑和笔记本电脑

### <a name="typical-usage"></a>典型用法

根据用户眼睛的位置及移动，跟踪用户的凝视、注意和状态。 这一使用 UWP 应用并与之交互的新方式对于以下用户是特别有用的辅助技术：患有神经肌肉疾病（如 ALS），以及有其他受损肌肉或神经功能方面的残障。 凝视输入还为游戏（包括目标获取和跟踪）和传统的生产力应用程序、展台及其他交互式场景提供具有吸引力的机会，如传统输入设备（键盘、鼠标和触控）不可用或可能对释放用户双手以执行其他任务（如提购物袋）非常有用/有帮助的情况。

### <a name="more-info"></a>详细信息

[凝视交互和目视跟踪](gaze-interactions.md)

## <a name="surface-dial"></a>Surface Dial

对于 **Windows 10 周年更新**，我们引入了 Windows Wheel 输入设备类别。 Surface Dial 是此类设备中的第一项。

### <a name="device-support"></a>设备支持

- 平板电脑
- PC 和笔记本电脑

### <a name="typical-usage"></a>典型用法

借助基于旋转操作（或手势）的外形规格，Surface Dial 旨在成为对主设备输入进行补充或修改的多模态辅助输入设备。 在大多数情况下，用户使用其惯用手执行某个任务（如使用笔进行墨迹书写）期间，该设备由其非惯用手操控。

### <a name="more-info"></a>详细信息

[Surface Dial 设计指南](windows-wheel-interactions.md)

## <a name="cortana"></a>Cortana

在 windows 10， **Cortana**扩展性来处理来自用户的语音命令，并启动你的应用程序执行一个单独操作。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![Cortana](images/input-interactions/icons-cortana01.png)

### <a name="typical-usage"></a>典型用法

语音命令是在语音定义命令 (VCD) 文件中定义的单个发音，通过 **Cortana** 指向一个已安装的应用。 可在前台或后台启动该应用，具体取决于交互的级别和复杂程度。 例如，需要额外上下文或用户输入的语音命令最好在前台处理，而基本命令可以在后台处理。

通过集成应用的基本功能，并为用户提供中心入口点以便在无需直接打开应用的情况下完成大多数任务，**Cortana** 成为了应用和用户之间的联络人。 在大多数情况下，这可以为用户节省大量时间和精力。 有关详细信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)。

### <a name="more-info"></a>详细信息

[Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)
 

## <a name="speech"></a>语音

语音是用户用于与应用程序交互的一种高效自然方式。 这是一种与应用程序通信的简单且准确的方式，并且可使用户提高效率并在各种情况下获取信息。

语音可以是补充输入类型，而在许多情况下则是主要输入类型，具体取决于用户设备。 例如，HoloLens 和 Xbox 之类的设备不支持传统输入类型（特定方案中的软件键盘除外）。 而对于大多数用户交互而言，它们依赖于语音输入和输出（通常与其他诸如注视和手势等非传统输入类型相结合）。

文本到语音转换（也称为 TTS 或语音合成）用于通知或指示用户。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![语音](images/input-interactions/icons-speech01.png)

### <a name="typical-usage"></a>典型用法

语音交互的模式有三种：

**自然语言**

自然语言是我们定期与人们进行口头上的交互的方式。 我们的语音随人以及环境的不同而不同，但一般都能理解。 在不理解的时候，我们通常使用不同的词语和语序来传达相同的想法。

与应用的自然语言交互与此类似：我们通过设备将应用当作一样来对其说话，期望它能够明白并且相应地做出反应。

自然语言是语音交互的最高级模式，可以通过 **Cortana** 实现和公开。

**命令和控件**

命令和控件使用文字命令激活控件和功能，例如单击某个按钮或选择某个菜单项。

因为命令和控件对成功的用户体验至关重要，所以通常不推荐单一输入类型。 语音通常是基于用户首选项或硬件功能的多个用户输入选项之一。

**听写**

最基本的语音输入方法。 将每个发音转换为文本。

听写通常在应用无需理解含义或意图的情况下使用。

### <a name="more-info"></a>详细信息

[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)
 

## <a name="pen"></a>笔

笔（或触笔）可用作像素精确的定位设备（如鼠标），并且是用于数字墨迹输入的最佳设备。

**注意**有两种类型的笔设备： 主动式和被动式。
  -   被动式笔中不包含电子组件，并且可以有效模仿手指的触摸输入。 它们要求能够识别基于触点压力的输入的基本设备屏幕。 因为用户在输入面上书写时经常把手放在上面，所以输入数据由于防误触失败会受到污染。
  -   主动式笔中包含电子组件，并且可以与复杂的设备屏幕结合使用，以向系统和应用提供更广泛的输入数据（包括悬停或邻近数据）。 防误触功能更加强大。

当我们在此谈到笔设备时，我们指的是可提供丰富输入数据且主要用于精确墨迹和定位交互的主动式笔设备。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT

![笔](images/input-interactions/icons-pen01.png)

### <a name="typical-usage"></a>典型用法

Windows 墨迹平台与笔设备相得益彰，提供了一种创建手写便笺、绘画和注释的自然方法。 该平台支持捕获通过数字化器输入的墨迹数据、生成墨迹数据、在输出设备上以墨迹笔划的形式呈现这些数据、管理墨迹数据以及执行手写识别。 除了在用户写字或绘图时捕获笔的空间运动，你的应用还可以收集信息（如压力、形状、颜色和不透明度），以便提供与使用钢笔、铅笔或画笔在纸张上绘图极其类似的用户体验。

笔输入和触控输入的不同之处在于，触摸可以通过对这些对象执行肢体手势（如轻扫、滑动、拖动和旋转等等），模拟在屏幕上直接操作 UI 元素。

应该提供特定于笔的 UI 命令或提示来支持这些交互。 例如，使用“上一页”和“下一页”（或 + 和 -）按钮，使用户可以翻阅网页内容，或者旋转对象、调整对象大小和对对象进行缩放。

### <a name="more-info"></a>详细信息

[笔设计指南](https://msdn.microsoft.com/library/windows/apps/dn456352)
 

## <a name="touch"></a>触摸

借助触摸，可以将一个或多个手指组成的肢体手势用作一种备用输入法（类似于鼠标或笔）来模拟 UI 元素的直接操作（如平移、旋转、调整大小或移动），也可以将其用作一种补充输入法来修改其他输入的各个方面（例如涂抹使用笔绘制的笔划墨迹）。 当用户在屏幕上与元素进行交互时，此类可触知体验可以为用户提供更自然的真实感觉。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT

![触摸](images/input-interactions/icons-touch01.png)

### <a name="typical-usage"></a>典型用法

触控输入的支持区别很大，具体取决于设备。

一些设备根本不支持触摸，一些设备单个触摸点，而其他设备支持多个触摸点（两个或以上触点）。

大多数支持多点触控输入的设备通常 可以识别十个独特的并发触点。

Surface Hub 设备可以识别 100 个独特的并发触摸点。

一般而言，触摸：

-   适用于单个用户，除非与 Microsoft 团队设备（例如 Surface Hub）一起使用，因为这些设备强调合作。
-   对设备方向没有限制。
-   用于所有交互，包括文本输入（触摸键盘）和墨迹书写（应用配置）。

### <a name="more-info"></a>详细信息

[触摸设计指南](https://msdn.microsoft.com/library/windows/apps/hh465370)
 

## <a name="touchpad"></a>触摸板

触摸板将间接的多点触摸输入和定位设备（如鼠标）的精确输入结合起来。 这种结合使触摸板既适用于触摸优化的 UI，也适用于效率应用的较小目标。

### <a name="device-support"></a>设备支持

-   PC 和笔记本电脑
-   IoT

![触摸板](images/input-interactions/icons-touchpad01.png)

### <a name="typical-usage"></a>典型用法

触摸板通常支持一组触摸手势，提供类似于通过触摸直接操作对象和 UI 的支持。

由于触摸板支持这种聚合的交互体验，我们还建议提供鼠标风格的 UI 命令或提示，而不只是依赖于触摸输入支持。 提供特定于触摸板的 UI 命令或提示来支持这些交互。

应该提供特定于鼠标的 UI 命令或提示来支持这些交互。 例如，使用“上一页”和“下一页”（或 + 和 -）按钮，使用户可以翻阅网页内容，或者旋转对象、调整对象大小和对对象进行缩放。

### <a name="more-info"></a>详细信息

[触摸板设计指南](https://msdn.microsoft.com/library/windows/apps/dn456353)
 

## <a name="keyboard"></a>键盘

键盘是主要的文本输入设备。对于残障人士，或者认为键盘是与应用交互的最快和最有效方法的用户而言，键盘通常非常重要。

使用[适用于手机的 Continuum](http://go.microsoft.com/fwlink/p/?LinkID=699431)，一种新体验兼容 windows 10 移动版设备，用户可以将手机连接鼠标和键盘来使手机像笔记本电脑一样工作。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![键盘](images/input-interactions/icons-keyboard01.png)

### <a name="typical-usage"></a>典型用法

用户可以通过硬件键盘，以及屏幕键盘 (OSK) 和触摸键盘这两个软件键盘与通用 Windows 应用交互。

OSK 是一种可视软件键盘，你可以借助触摸、鼠标、笔/触笔或其他定位设备（不需要触摸屏）来使用屏幕键盘代替物理键盘键入和输入数据。 OSK 是针对没有物理键盘的系统提供的，或者是为行动有障碍而无法使用传统物理输入设备的用户而提供的。 OSK 可模拟硬件键盘的大部分功能（如果不是全部功能）。

触摸键盘是一种借助触摸屏输入来输入文本的可视软件键盘。 触摸键盘不可以代替 OSK，因为它仅用于文本输入（它不模拟硬件键盘），并且仅在文本字段或其他可编辑的文本控件获得焦点时显示。 触摸键盘不支持应用或系统命令。

**注意**OSK 的优先级高于触摸键盘时，它不会显示 OSK 是否存在。

一般而言，键盘：

-   适用于单个用户。
-   对设备方向没有限制。
-   用于文本输入、导航、玩游戏以及辅助功能。
-   始终可用，无论是主动式还是被动式。

### <a name="more-info"></a>详细信息

[键盘设计指南](https://msdn.microsoft.com/library/windows/apps/hh972345)
 

## <a name="mouse"></a>鼠标

鼠标最适合用于效率应用或用户交互需要将像素级精度用于定位和命令的高密度 UI。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT

![鼠标](images/input-interactions/icons-mouse01.png)

### <a name="typical-usage"></a>典型用法

可以结合使用各种键盘键（Ctrl 、Shift、Alt 等）来修改鼠标输入。 这些键可以与鼠标左键、鼠标右键、滚轮按钮和 X 按钮结合使用，以形成一个扩展的、针对鼠标而优化的命令集。 （某些 Microsoft 鼠标设备有两个其他按钮，称为 X 按钮，通常用于在 Web 浏览器中向前和向后导航。）

与笔输入类似，鼠标输入和触摸输入的不同之处在于，触摸可以通过对这些对象执行肢体手势（如轻扫、滑动、拖动和旋转等等）模拟在屏幕上直接操作 UI 元素。

应该提供特定于鼠标的 UI 命令或提示来支持这些交互。 例如，使用“上一页”和“下一页”（或 + 和 -）按钮，使用户可以翻阅网页内容，或者旋转对象、调整对象大小和对对象进行缩放。

### <a name="more-info"></a>详细信息

[鼠标设计指南](https://msdn.microsoft.com/library/windows/apps/dn456351)
 

## <a name="gesture"></a>手势

手势是在控制应用程序或与应用程序进行交互时，作为输入识别的任何形式的用户动作。 手势采用多种形式：从只使用一只手在屏幕上指定内容，到特定的习得型动作，到使用整个正文的长按拉伸的连续动作。 在设计自定义手势时请小心，因为其含义可能因地域和文化而异。

### <a name="device-support"></a>设备支持

-   PC 和笔记本电脑
-   IoT
-   Xbox
-   HoloLens

![笔势](images/input-interactions/icons-gesture01.png)

### <a name="typical-usage"></a>典型用法

静态手势事件在交互完成之后引发。

- 静态手势事件包括 Tapped、DoubleTapped、RightTapped 和 Holding。

操作手势事件表明一个持续的交互。 它们在用户触摸元素时开始引发，一直持续到用户抬起手指或者操作取消时。

- 操作事件包括多点触控交互（例如，缩放、平移或旋转）和使用惯性和速度数据的交互（例如拖动）。 （操作事件提供的信息并不标识交互，而是提供诸如位置、转换增量和速度的数据。）

- 诸如 PointerPressed 和 PointerMoved 的指针事件会提供每个触摸接触的低级别详细信息，包括指针动作以及区分按下和释放事件的能力。

由于 Windows 支持聚合的交互体验，我们还建议提供鼠标风格的 UI 命令或提示，而不只是依赖于对触摸输入的支持。 例如，使用“上一页”和“下一页”（或 + 和 -）按钮，使用户可以翻阅网页内容，或者旋转对象、调整对象大小和缩放对象。


## <a name="gamepadcontroller"></a>游戏板/控制器

游戏板/控制器是专门用来玩游戏的极其专业的设备。 但是，它也用于模拟基本键盘输入，并可提供非常类似于键盘的 UI 导航体验。

### <a name="device-support"></a>设备支持

-   PC 和笔记本电脑
-   IoT
-   Xbox

![控制器](images/input-interactions/icons-controller01.png)

### <a name="typical-usage"></a>典型用法

玩游戏和与专用控制台交互。


## <a name="multiple-inputs"></a>多个输入

通过尽可能适应更多的用户和设备，以及设计应用并使其可与尽可能多的输入类型（手势、语音、触摸、触摸板、鼠标和键盘）结合使用，可使灵活性、可用性和辅助功能最大化。

### <a name="device-support"></a>设备支持

-   手机和平板手机
-   平板电脑
-   PC 和笔记本电脑
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![多个输入](images/input-interactions/icons-inputdevices03-vertical.png)

### <a name="typical-usage"></a>典型用法

正如人们在彼此交流时会结合使用语音和手势，在与应用交互时也会用到多种类型和模式的输入。 但是，这些组合交互需要尽可能直观和自然，因为它们也可能创造非常令人困惑的体验。





 

 
