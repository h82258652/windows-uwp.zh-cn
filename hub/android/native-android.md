---
title: Windows 上的本机 Android 开发
description: 开始在 Windows 上开发 Android 本机应用。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、android studio、visual studio、c + + android 游戏、windows defender、模拟器、虚拟设备、安装、java、kotlin
ms.date: 04/28/2020
ms.openlocfilehash: 31cdd5124c659b5d35a7e8192db1e87821b891f4
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255221"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Windows 上的本机 Android 开发入门

本指南将帮助你开始使用 Windows 创建本机 Android 应用程序。 如果希望使用跨平台解决方案，请参阅[Windows 上的 Android 开发概述](./overview.md)，了解一些选项的简要概述。

创建本机 Android 应用程序的最直接的方法是在[Java 或 Kotlin](#java-or-kotlin)中使用 Android Studio，不过，如果你有特定目的，也可以[使用 c 或 c + + 进行 Android 开发](#use-c-or-c-for-android-game-development)。 Android Studio SDK 工具可将代码、数据和资源文件编译为存档 Android 包 apk 文件。 一个 APK 文件包含 Android 应用程序的所有内容，是安装了 Android 的设备用于安装应用程序的文件。

## <a name="install-android-studio"></a>安装 Android Studio

Android Studio 是 Google 的 Android 操作系统的官方集成开发环境。 [下载适用于 Windows 的最新版本的 Android Studio](https://developer.android.com/studio)。

- 如果下载了 .exe 文件（推荐），请双击以启动它。
- 如果下载了 .zip 文件，解压缩 ZIP，将 android-studio 文件夹复制到 Program Files 文件夹中，然后打开 android-studio > bin 文件夹并启动 studio64 （适用于64位计算机）或工作室（适用于32位计算机）。

按照中的设置向导 Android Studio 并安装它所建议的任何 SDK 包。 当新工具和其他 api 变得可用时，Android Studio 将使用弹出窗口通知你，或通过选择 "**帮助** > " "**检查更新**" 来检查更新。

## <a name="create-a-new-project"></a>创建新项目

选择 "**文件** > " "**新建** > " "**新建项目**"。

在 "**选择你的项目**" 窗口中，你将能够在这些模板之间进行选择：

- **基本活动**：使用应用栏、浮动操作按钮和两个布局文件创建一个简单的应用：一个用于活动，另一个用于分隔文本内容。

- **空活动**：使用示例文本内容创建一个空活动和单个布局文件。

- **底部导航活动**：为活动创建标准底部导航栏。 请参阅 Google 的内容设计准则中的[底部导航组件](https://material.io/guidelines/components/bottom-navigation.html)。

- 详细了解如何在 Android Studio 文档中[选择活动模板](https://developer.android.com/studio/projects/templates#SelectTemplate)。

模板通常用于向新的和现有的应用模块中添加活动。 例如，若要为应用程序的用户创建登录屏幕，请使用[登录活动模板](https://developer.android.com/studio/projects/templates#LoginActivity)添加一个活动。

> [!NOTE]
> Android 操作系统基于**组件**的概念，并使用术语**活动**和**意向**来定义交互。 **活动**表示用户可执行的单个重点任务。 **活动**提供了一个窗口，用于基于**视图**类使用类生成用户界面。 Android 操作系统中的**活动**有一个生命周期，由六个回调`onCreate()`集定义：、 `onStart()`、 `onResume()`、 `onPause()` `onStop()`、和。 `onDestroy()` 活动组件使用**意向**对象彼此交互。 意向定义要启动的活动，或说明要执行的操作的类型（系统为你选择适当的活动，甚至可以来自不同的应用程序）。 详细了解 Android 文档中的[活动](https://developer.android.com/reference/android/app/Activity)、[活动生命周期](https://developer.android.com/guide/components/activities/activity-lifecycle)和[意向](https://developer.android.com/reference/android/content/Intent.html)。

### <a name="java-or-kotlin"></a>Java 或 Kotlin

**Java**变成了1991中的一种语言，它是由 Sun Microsystems 开发的，但现在归 Oracle 所有。 它已成为世界上最大的支持社区之一的最受欢迎且功能强大的编程语言之一。 Java 是基于类和面向对象的，旨在尽可能少地实现依赖关系。 语法类似于 C 和 c + +，但它的低级别设施比其中任何一个少。

**Kotlin**是第一次在2011中发布为一种新的开源语言，并已作为 Android Studio Java 的一种替代方法，因为2017。 在5月2019，Google 公布了 Kotlin，因为它是适用于 Android 应用开发人员的首选语言，因此，尽管这是一种较新的语言，但它也具有强大的支持社区，并且已被识别为一种速度最快的编程语言。 Kotlin 是跨平台、静态类型化的，并且设计为与 Java 完全互操作。

Java 更广泛地用于更广泛的应用程序，并提供了 Kotlin 不具备的某些功能，例如选中的异常、不属于类的基元类型、静态成员、非私有字段、通配符类型和三元运算符。 Kotlin 专门为 Android 设计和推荐。 它还提供了 Java 不具备的某些功能，例如，由类型系统控制的空引用、没有原始类型、固定数组、适当的函数类型（而不是 Java 的 SAM 转换）、使用不带通配符的站点差异、智能强制转换等。 Kotlin 文档[更深入地介绍了 Java 的比较](https://kotlinlang.org/docs/reference/comparison-to-java.html)。

### <a name="minimum-api-level"></a>最低 API 级别

需要确定应用程序的最低 API 级别。 这将确定应用程序将支持的 Android 版本。 更低的 API 级别更早，因此通常支持更多设备，但更高的 API 级别较高，因此提供更多功能。

![Android Studio 最小 API 选择屏幕](../images/android-minimum-api-selection.png)

选择 "**帮助我选择**链接"，以打开一个比较图表，其中显示了与平台版本版本关联的设备支持分发和关键功能。

![Android Studio 最小 API 比较屏幕](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>即时应用支持和 Androidx 项目

你可能会注意到一个复选框，可**支持即时应用**，另一个复选框用于在项目创建选项中**使用 androidx 项目**。 不检查*即时应用支持*，并选中 " *androidx* " 作为建议默认值。

Google Play**即时应用**提供一种方法，让用户无需安装即可试用应用或游戏。 可以通过 Play Store、Google 搜索、社交网络和共享链接的任何位置来呈现这些即时应用。 通过选中 "**支持即时应用**" 框，你会要求 Android Studio 将 Google Play 即时开发 SDK 与你的项目结合使用。 若要详细了解[Google Play 即时应用](https://developer.android.com/topic/google-play-instant)以及如何[创建可立即启用的应用捆绑包](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)，请参阅 Android 文档。

**AndroidX 项目**代表 android 支持库的新版本，并提供跨 android 版本的向后兼容性。 AndroidX 提供了一个一致的命名空间，该命名空间从所有可用包的字符串 AndroidX 开始。

> [!NOTE]
> AndroidX 现在是默认库。 若要取消选中此框并使用之前的支持库，需要删除最新 Android Q SDK。 有关说明，请参阅[取消选中使用](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts)StackOverflow 上的 Androidx 项目，但首先请注意，以前的支持库包已映射到相应的 Androidx 包中。 若要获取所有旧类的完整映射并生成新的项目，请参阅[迁移到 AndroidX](https://developer.android.com/jetpack/androidx/migrate)。

## <a name="project-files"></a>项目文件

"Android Studio**项目**" 窗口包含以下文件（请确保从下拉菜单中选择了 "Android" 视图）：

**应用 > java > myfirstapp > MainActivity**

应用的主活动和入口点。 当你生成并运行应用时，系统会启动此活动的实例并加载其布局。

**应用 > res > 布局 > activity_main .xml**

定义活动用户界面（UI）的布局的 XML 文件。 它包含文本为 "Hello World" 的 TextView 元素

**应用 > 清单 > Androidmanifest.xml**

描述应用及其每个组件的基本特征的清单文件。

**Gradle 脚本 > Gradle**

对于每个应用模块，都有两个文件的名称分别为： "项目：我的第一个应用"、整个项目和 "模块：应用"。 新项目最初将只有一个模块。 使用模块的 build. 文件可控制 Gradle 插件生成应用的方式。 有关详细信息，请参阅 Android 文档[配置生成](https://developer.android.com/studio/build/index)一文。

## <a name="use-c-or-c-for-android-game-development"></a>使用 C 或 c + + 进行 Android 游戏开发

Android 操作系统旨在支持以 Java 或 Kotlin 编写的应用程序，这些应用程序可以从系统体系结构中嵌入的工具中获益。 许多系统功能（如 Android UI 和意向处理）仅通过 Java 接口公开。 在某些情况下，你可能想要**通过 Android 本机开发工具包（NDK）使用 C 或 c + + 代码**，但有一些相关的挑战。 游戏开发是一个示例，因为游戏通常使用 OpenGL 或 Vulkan 中编写的自定义呈现逻辑，并受益于专门针对游戏开发的大量 C 库。 使用 C 或 c + +*可能*还有助于将额外性能从设备中弹出，以实现低延迟或运行计算密集型应用程序，如物理学模拟。 不过，此 NDK**不适用于大多数初学者的 Android 编程人员**。 除非你有使用 NDK 的特定目的，否则建议不要使用 Java、Kotlin 或某个[跨平台框架](./overview.md)。

若要创建具有 C/c + + 支持的新项目，请执行以下操作：

- 在 Android Studio 向导的 "**选择项目**" 部分中，选择 "*本机 c + +*" 项目类型。 选择 "**下一步**"，完成剩余字段，然后再次选择 "**下一步**"。

- 在向导的 "**自定义 c + + 支持**" 部分中，可以通过 " **c + + 标准**" 字段自定义项目。 使用下拉列表选择要使用的 c + + 标准化。 选择**工具链默认值**将使用默认的 CMake 设置。 选择“完成”  。

- Android Studio 创建新项目后，可以在 "**项目**" 窗格中找到一个**cpp**文件夹，其中包含本机源文件、标头、用于 CMake 或 ndk 构建的生成脚本，以及预生成的包含在项目中的库。 还可以在`native-lib.cpp` `src/main/cpp/`文件夹中找到一个示例 c + + 源文件，该文件提供了一个`stringFromJNI()`返回字符串 "Hello from c + +" 的简单函数。 此外，你将在生成本机库所需[`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html)的模块根目录中找到 CMake 生成脚本。

若要了解详细信息，请参阅 Android 文档主题：[将 C 和 c + + 代码添加到你的项目](https://developer.android.com/studio/projects/add-native-code)。 有关示例，请参阅 GitHub 上的[Android Studio c + + 集成存储库的 ANDROID NDK 示例](https://github.com/android/ndk-samples)。 若要在 Android 上编译并运行 c + + 游戏，请使用[Google Play 游戏服务 API](https://developers.google.com/games/services/cpp/gettingStartedAndroid)。

## <a name="design-guidelines"></a>设计指南

设备用户希望应用程序以某种方式查看和行为 .。。无论是轻扫或点击还是使用语音控件，用户都将对应用程序的外观和使用方式保持特定的期望。 这些期望应保持一致，以便减少混乱和不满。 Android 提供这些平台和设备期望的指南，其中结合了用于视觉和导航模式的 Google 材料设计基础，以及兼容性、性能和安全性的质量指导原则。

在[Android 设计文档](https://developer.android.com/design)中了解详细信息。

### <a name="fluent-design-system-for-android"></a>适用于 Android 的流畅设计系统

Microsoft 还提供了设计指南，旨在提供跨整个 Microsoft 移动应用组合的无缝体验。

[适用于 android 的流畅设计系统](https://www.microsoft.com/design/fluent/#/android)设计和构建本机 Android 的自定义应用程序，但仍然是唯一熟知的。

- [Sketch 工具包](https://aka.ms/fluenttoolkits/android/sketch)
- [Figma 工具包](https://aka.ms/fluenttoolkits/android/figma)
- [Android 字体](https://fonts.google.com/specimen/Roboto)
- [Android 用户界面指南](https://developer.android.com/design/)
- [适用于 Android 应用程序图标的准则](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>其他资源

- [Android 应用程序基础](https://developer.android.com/guide/components/fundamentals)

- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)

- [添加 Windows Defender 排除项以提高性能](defender-settings.md)

- [启用虚拟化支持以提高模拟器性能](emulator.md#enable-virtualization-support)
