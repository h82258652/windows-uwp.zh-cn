---
title: 针对 Windows 上的 Android 开发做出本机响应
description: 开始使用 Windows 上的 Xamarin 本机开发 Android 应用。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android，windows，响应本机，模拟器，展览，地铁捆绑程序，终端
ms.date: 04/28/2020
ms.openlocfilehash: db49e3ed12fee8e7ced7680e305a84afb89f32a2
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255231"
---
# <a name="get-started-developing-for-android-using-react-native"></a>开始使用响应本机开发 Android

本指南将帮助你开始在 Windows 上使用 "响应本机"，以创建可在 Android 设备上工作的跨平台应用。

## <a name="overview"></a>概述

响应本机是 Facebook 创建的[开源](https://github.com/facebook/react-native)移动应用程序框架。 它用于开发适用于 Android、iOS、Web 和 UWP 的应用程序（Windows），该应用程序提供本机 UI 控件和对本机平台的完全访问权限。 使用响应本机需要了解 JavaScript 基础知识。

## <a name="get-started-with-react-native-by-installing-required-tools"></a>通过安装必需的工具开始响应本机入门

1. [安装 Visual Studio Code](https://code.visualstudio.com) （或所选的代码编辑器）。

2. [安装适用于 Windows 的 Android Studio](https://developer.android.com/studio)。 默认情况下，Android Studio 安装最新 Android SDK。 响应本机要求 Android 6.0 （Marshmallow） SDK 或更高版本。 建议使用最新的 SDK。

3. 为 Java SDK 和 Android SDK 创建环境变量路径：
    - 在 Windows search 菜单中输入 "编辑系统环境变量"，这将打开 "**系统属性**" 窗口。
    - 选择 "**环境变量 ...** "，然后选择 "**用户变量**" 下的 "**新建 ...** "。
    - 输入变量名称和值（path）。 Java 和 Android Sdk 的默认路径如下所示。 如果已选择特定位置以安装 Java 和 Android Sdk，请确保相应地更新变量路径。
        - JAVA_HOME： C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME： C:\Users\username\AppData\Local\Android\Sdk

    ![添加环境变量路径的屏幕截图](../images/add-environmental-variable-path.png)

4. [安装 NodeJS For Windows](https://nodejs.org/en/)如果要使用多个项目和 NodeJS 版本，则可能需要考虑使用[适用于 Windows 的 Node 版本管理器（nvm）](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 。 建议为新项目安装最新的 LTS 版本。

> [!NOTE]
> 你可能还需要考虑安装和使用适用于你喜欢的命令行接口（CLI）的[Windows 终端](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)以及用于[版本控制的 Git](https://git-scm.com/downloads)。 [JAVA JDK](https://www.oracle.com/java/technologies/javase-downloads.html)随 Android Studio v2.0 + 一起打包，但如果需要单独从 Android Studio 中更新 JDK，请使用[Windows x64 安装程序](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)。

## <a name="create-a-new-project-with-react-native"></a>创建新项目，并对其进行响应

1. 使用 npm 从 Windows 命令提示符、PowerShell、 [Windows 终端](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)或 VS Code 中的集成终端（查看 > 集成终端）安装[展览 CLI](https://docs.expo.io/versions/latest/)命令行实用工具。

    ```powershell
    npm install -g expo-cli
    ```

2. 使用展览创建在 iOS、Android 和 web 上运行的响应本机应用。 你将需要在项目模板之间进行选择，其中包括**空白**、**空白（TypeScript）**、**选项卡**（使用响应导航的示例屏幕）、**最小**或**最小（TypeScript）**。

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > 如果你使用`npx create-react-native-app`，则仍将运行，但除了使用展览外，还可以获得[额外的好处](https://github.com/react-native-community/discussions-and-proposals/issues/23)。

3. 打开新的 "我的新应用程序" 目录：

    ```powershell
    cd my-new-app
    ```

4. 若要运行项目，请输入以下命令。 这会在默认 internet 浏览器中打开一个 localhost 窗口，其中显示了节点地铁捆绑程序。 它还会在你的命令行和地铁捆绑程序浏览器窗口中显示一个 QR 码。 * 可以使用命令： `npm start`或。 `npm run android`

     ```powershell
    expo start
    ```

    ![浏览器中的地铁捆绑程序屏幕截图](../images/metro-bundler.png)

5. 若要查看 Android 设备上运行的项目，需要先使用 Android 设备上[的 Google Play 商店安装 "博览会客户端应用](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US)"。 安装了博览会客户端应用后，在设备上打开它，并选择 "**扫描 QR 码**"。 注册 QR 码后，你将能够在你的浏览器中的设备上和 localhost 上运行的地铁捆绑程序窗口中看到包生成。

6. 若要查看 Android 模拟器上运行的项目，首先需要打开 Android Studio，然后创建并启动虚拟设备。 **Tools** > **AVD Manager** > **[+ 创建虚拟设备 ...](https://developer.android.com/studio/run/managing-avds#createavd)** 创建虚拟设备后，在 Android 虚拟设备管理器的 "**操作**" 列下选择 "启动" 按钮▷，开始模拟设备。 打开虚拟设备后，返回到在 internet 浏览器窗口中运行的地铁捆绑程序窗口，并选择左侧列中的 "Android 设备/模拟器上运行"。 你会看到一个弹出窗口，让你知道地铁捆绑程序 "正在尝试打开模拟器 ..."然后，查看在您的仿真 Android 设备中打开的用户应用程序，并在完成下载 JavaScript 捆绑后，您将看到您的响应本机应用程序已显示。 （如果遇到问题，请[查看博览会 Android 模拟器文档](https://docs.expo.io/workflow/android-studio-emulator/)。）

7. 打开响应本机项目，开始使用应用。 你应该会在设备上或你的 Android Emulator 中看到，在运行的应用中自动更新你的更改。

8. 尝试将登陆页面视图文本更改为 "Hello World！"。 可以在所选的 IDE 中执行此操作。 （建议 VS Code 或 Android Studio。）登录页面文件将因你选择的模板而异。 它可以是`App.js`、 `App.tsx`或`HomeScreen.js`。

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. 尝试添加映像。 首先，您需要在应用程序中创建一个文件夹，该文件夹的 "android" 和 "ios" 文件夹相同，我们将其称为 "映像"。 在该文件夹中放置一个图像`your-image.png` ，例如。 使用以下格式引用图像，并将其设置为高度和宽度样式。

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> 如果要添加对响应本机应用程序的支持，使其作为 Windows 10 应用程序运行，请参阅[Windows 的响应本机入门](https://microsoft.github.io/react-native-windows/docs/getting-started)文档。

## <a name="additional-resources"></a>其他资源

- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)

- [添加 Windows Defender 排除项以提高性能](defender-settings.md)

- [启用虚拟化支持以提高模拟器性能](emulator.md#enable-virtualization-support)
