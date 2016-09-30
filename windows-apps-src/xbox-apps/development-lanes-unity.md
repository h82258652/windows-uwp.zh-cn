---
title&#58; Unity：将你的游戏引入 Xbox One 作者：JordanEllis6809 
---

# Unity：将你的游戏引入 Xbox One

在此分步教程中，我们假定你已有一个采用 Unity 的游戏，随时可供生成和部署。

[本教程的视频版本。](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

想要对你的 Unity UWP 项目进行版本控制？  请[查看此处](development-lanes-unity-versioning.md)。

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

既然你已拥有适用于你的游戏的 UWP Visual Studio 解决方案，[遵循这些步骤](https://msdn.microsoft.com/windows/uwp/xbox-apps/getting-started)将使你可以成功地将你的游戏部署到零售 Xbox One！

## 步骤 3：修改和重新生成

如果对任何非脚本的内容进行了更改，为了使这些更改在游戏的 UWP 生成中显示，项目必须从编辑器重新生成（如 __步骤 1__ 中所述）。

## 对你的 UWP 项目进行版本控制

在一些常见的情况下，必须将此新生成的 UWP 目录的各个部分添加到版本控制。  例如，当你要将新的依赖项添加到 UWP 项目（即 Xbox Live SDK）时。  我们将在[此处](development-lanes-unity-versioning.md)详细回顾此示例。



<!--HONumber=Jul16_HO2-->


