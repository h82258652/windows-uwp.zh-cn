---
title: "将你的 Unity 游戏引入 Xbox One"
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# 将你的 Unity 游戏引入 Xbox One

在此分步教程中，我们假定你已有一个采用 Unity 的游戏，随时可供生成和部署。

[本教程的视频版本。](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## 步骤 0：确保已正确安装 Unity

安装 Unity 时，必须选择以下组件：

![Unity 安装组件](images/unity-install-components.png)

## 步骤 1：生成 UWP 解决方案

在你的 Unity 游戏项目中，打开位于 `File -> Build Settings...` 的“生成设置”窗口，然后转到“Windows 应用商店选项”菜单，如下所示。

![“生成设置”窗口](images/build-settings.png)

确保 `SDK` 设置已设置为 `Universal 10`。 接下来按该菜单底部的“生成”按钮，这将启动要求选择目标文件夹的资源管理器窗口。 在你的项目的 `Assets` 目录中创建名为 `UWP` 的文件夹，然后选择该文件夹作为生成的目标文件夹。

![生成目标文件夹](images/build-destination.png)

Unity 现在已创建一个新的 Visual Studio 解决方案，我们将在下一步中用它来部署你的 UWP 游戏。

![UWP VS 解决方案](images/uwp-vs-solution.png)

## 步骤 2：部署你的游戏

打开 `Assets/UWP` 文件夹中新生成的解决方案。  打开后，将目标平台更改为 X64。

![x64 生成平台](images/x64-build-platform.png)

既然你已拥有适用于你的游戏的 UWP Visual Studio 解决方案，[遵循这些步骤](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)将使你可以成功地将你的游戏部署到零售 Xbox One！

## 开发人员说明

- 建议你在你的版本控制中忽略 UWP 文件夹。 如果你想要将其他 XAML 元素添加到你的项目中，并需要对 UWP 文件夹中的某些资源进行版本控制，请参阅[仅执行此操作的这些示例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)。

- 如果你在 Unity 中对你的游戏的生成中所包含的任何内容（脚本除外）进行了更改，则必须重新生成你的 UWP 解决方案，才可以在你下次部署时看到这些更改。 会发生这种情况，因为在 Unity 的生成步骤过程中，你的项目的所有资源会编译到一个资源文件中。 当 UWP 解决方案部署游戏时，它会引用该生成的资源文件。




<!--HONumber=Jun16_HO5-->


