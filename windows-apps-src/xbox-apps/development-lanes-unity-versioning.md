---
title&#58; Unity：对你的 UWP 项目进行版本控制 作者：JordanEllis6809
---

# Unity：对你的 UWP 项目进行版本控制

还没有使用 UWP 生成适用于 Xbox 的 Unity 游戏？  请先阅读[此处](development-lanes-unity.md)。

关于为何要将你生成的 UWP 目录的各个部分添加到版本控制有多种不同的原因，其中一个原因是为了添加依赖项（即 Xbox Live SDK）。  我们将在此教程中使用此方案作为示例，希望它能帮助你解决你的项目的个别需求。

***免责声明：我们将使用 Git 作为版本控制解决方案。  如果你的解决方案有所不同，概念应该仍然相通。***

若要刷新内存，这是我们的游戏 ***ScrapyardPhoenix*** 的目录的当前外观：

![生成目标文件夹](images/build-destination.png)

这是我们的 UWP 目录的外观：

![UWP 与解决方案](images/uwp-vs-solution.png)

在此目录中，我们只关心一个文件夹，即 ***ScrapyardPhoenix***（在此处插入你的游戏的名称）文件夹。  在版本控制中，可以忽略其他所有内容：

***不了解什么是 .gitignore 文件？  在[此处](https://git-scm.com/docs/gitignore)了解详细信息。***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

我们将从 `UWP/ScrapyardPhoenix` 文件夹内选择一些不同的文件和文件夹来添加到我们的版本控制。  首先我们来详细查看完整内容：

![UWP 生成目录](images/uwp-build-directory.png)  

## 文件夹  

`Assets` | 
            ***Include*** | 包含 Windows 应用商店图像。  
`Data`   | 
            ***Ignore*** | Unity 将你的项目编译到的位置（Scenes、Shaders、Scripts、Prefabs 等）。  
`Dependencies` | 
            ***Include*** | 此文件夹是 I 创建用于保留所有 UWP 依赖项（即 XboxLiveSDK.dll）的文件夹。  
`Properties` | 
            ***Include*** | 包含可由开发人员修改的更多高级设置。  
`Unprocessed` | 
            ***Ignore*** | 包含 Unity `.dll` 和 `.pdb` 文件。  

## 文件  

`App.cs` | 
            ***Include*** | 你的 UWP 应用程序的入口点。  它可以进行修改，并使用其他源文件进行扩展。  
`Package.appxmanifest` | 
            ***Include*** | 你的 AppX 的程序包清单。  
`project.json` | 
            ***Include*** | 描述你的 `*.csproj` 所依赖的 NuGet 程序包。  
`ScrapyardPhoenix.csproj` | 
            ***Include*** | 描述你的 UWP 生成目标。  如果你将其他依赖项添加到 UWP 项目，则此 `*.csproj` 文件将包含该信息。  
`ScrapyardPhoenix.csproj.user` | 
            ***Ignore*** | 此文件包含本地用户信息。

## 生成的 .gitignore

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

于是，现在你的团队成员将与你生成的 UWP 项目同步。  现在你可以向你的 UWP 项目随意添加其他资源、源和依赖项。

有关对 UWP 文件夹进行版本控制的一些更多示例可以在[这些示例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)中找到。

## 向 UWP 应用添加依赖项

向 UWP 应用程序添加依赖项的操作就像在任何其他 Visual Studio 项目中执行此操作一样。  此处唯一值得搞清楚的部分是要向解决方案中的哪个项目添加依赖项。  下面是显示哪一个项目的图像（突出显示）：

![UWP 解决方案](images/uwp-solution.png)

例如，***ScrapyardPhoenix（通用 Windows）***是你将添加对 Xbox Live SDK 的引用的项目。



<!--HONumber=Jul16_HO1-->


