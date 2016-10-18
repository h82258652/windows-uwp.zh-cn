---
author: JordanEllis6809
title: "将 Unity 游戏引入 Xbox 上的 UWP"
description: "Xbox 上的 Unity UWP 开发。"
translationtype: Human Translation
ms.sourcegitcommit: ea3bea2e5d6de0e55615de701a69e90d81f0f553
ms.openlocfilehash: 73f701a2608c6ce8d10cab817683ada4e9eecc08

---

# 将 Unity 游戏引入 Xbox 上的 UWP


在此分步教程中，我们假定你已有一个采用 Unity 的游戏，随时可供生成和部署。

另请参阅[本教程的视频版本](https://www.youtube.com/watch?v=f0Ptvw7k-CE)。

想要对你的 Unity UWP 项目进行版本控制？ 请参阅[对你的 UWP 项目进行版本控制](development-lanes-unity-versioning.md)。

## 步骤 0：确保已正确安装 Unity

安装 Unity 时，必须选择以下组件：

![Unity 安装组件](images/unity-install-components.png)

## 步骤 1：生成 UWP 解决方案

在你的 Unity 游戏项目中，打开位“文件”-&gt;“生成设置”****的“生成设置”****窗口，然后转到“Windows 应用商店选项”菜单。

![“生成设置”窗口](images/build-settings.png)

请确保“SDK”****设置设为“通用 10”****，然后选择“生成”****按钮，这将启动询问目标文件夹的“文件资源管理器”窗口。 在你的项目的 **Assets** 目录旁边创建名为 **UWP** 的文件夹，然后选择该文件夹作为生成的目标文件夹。

![生成目标文件夹](images/build-destination.png)

Unity 现在已创建一个新的 Visual Studio 解决方案，我们将用它部署你的 UWP 游戏。

![UWP VS 解决方案](images/uwp-vs-solution.png)

## 步骤 2：部署你的游戏

打开 **UWP** 文件夹中新生成的解决方案，然后将目标平台更改为“x64”****。

![x64 生成平台](images/x64-build-platform.png)

既然你已拥有适用于你的游戏的 UWP Visual Studio 解决方案，[遵循这些步骤](getting-started.md)将使你可以成功地将你的游戏部署到零售 Xbox One！

## 步骤 3：修改和重新生成

如果对任何非脚本的内容进行了更改，为了使这些更改在游戏的 UWP 生成中显示，项目必须从编辑器重新生成（如 __步骤 1__ 中所述）。

## 对你的 UWP 项目进行版本控制

在一些常见的情况下，必须将此新生成的 UWP 目录的各个部分添加到版本控制。 例如，当你要将新的依赖项添加到 UWP 项目（例如 Xbox Live SDK）时。  我们在[对你的 UWP 项目进行版本控制](development-lanes-unity-versioning.md)中详细了解此示例。

## 另请参阅
- [将现有游戏移植到 Xbox](development-lanes-landing.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Aug16_HO4-->


