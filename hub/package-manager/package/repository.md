---
title: 将清单提交到存储库
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4c1a8ab3c6a2cc697729fb5551e686a465bf6a0c
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83825118"
---
# <a name="submit-your-manifest-to-the-repository"></a>将清单提交到存储库

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

创建描述应用程序的[程序包清单](manifest.md)后，即可将清单提交到 Windows 程序包管理器存储库。 这是一个面向公众的存储库，其中包含 **winget** 工具可以访问的清单的集合。 若要提交清单，请将其上传到 GitHub 上的开源 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 存储库。

通过提交拉取请求将新清单添加到 GitHub 存储库后，一个自动化过程会验证你的清单文件，并通过检查来确保此程序包不是已知的恶意程序包。 如果验证成功，则会将你的程序包添加到面向公众的 Windows 程序包管理器存储库，以便 **winget** 客户端工具可以发现它。 请注意开源 GitHub 存储库中的清单与面向公众的 Windows 程序包管理器存储库中的清单之间的区别。

> [!IMPORTANT]
> Microsoft 保留以任何理由拒绝某个提交的权利。

## <a name="third-party-repositories"></a>第三方存储库

目前没有已知的第三方存储库。 Microsoft 正在与多个合作伙伴合作开发协议或 API 来启用第三方存储库。

## <a name="manifest-validation"></a>清单验证

当你将清单提交到 GitHub 上的 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 存储库时，系统会自动验证并评估你的清单，以确保 Windows 生态系统的安全。 还可以手动查看清单。

## <a name="how-to-submit-your-manifest"></a>如何提交清单

若要将清单提交到存储库，请执行以下步骤。

### <a name="step-1-validate-your-manifest"></a>步骤 1：验证清单

**winget** 工具提供了 [validate](..\winget\validate.md) 命令来确认是否正确创建了清单。 若要验证清单，请使用此命令。

```CMD
winget validate \<manifest-file>
```

如果验证失败，请根据错误来查找行号并进行更正。 验证清单后，即可将其提交到存储库。

### <a name="step-2-clone-the-repository"></a>步骤 2：克隆存储库

接下来，创建存储库的分叉并克隆它。

1. 在浏览器中转到 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)，然后单击“分叉”。
    ![分叉的图片](images\fork.png)

2. 在命令行环境（例如 Windows 命令提示符或 PowerShell）中，使用以下命令克隆你的分叉。
    ```CMD
    git clone \<your-fork-name>
    ```

 3. 如果要进行多次提交，请创建一个分支而不是分叉。 我们目前只允许每次提交一个清单文件。
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>步骤 3:将清单添加到本地存储库

必须将清单文件添加到采用以下文件夹结构的存储库中：

**manifests** / **publisher** / **application** / **version.yaml**

* **manifests** 文件夹是存储库中所有清单的根文件夹。
* **publisher** 文件夹是发布软件的公司的名称。 例如，**Microsoft**。
* **application** 文件夹是应用程序或工具的名称。 例如，**VSCode**。
* **version.yaml** 是清单的文件名。 此文件名必须设置为应用程序的当前版本。 例如，**1.0.0.yaml**。

>[!IMPORTANT]
> 清单中的 `Id` 值必须与清单文件夹路径中的发布者和应用程序名称相匹配，并且清单中的 `version` 值必须与文件名中的版本相匹配。 有关详细信息，请参阅[创建程序包清单](manifest.md#tips-and-best-practices)。

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>步骤 4：将清单提交到远程存储库

你现在已准备就绪，可以将新清单推送到远程存储库了。

1. 使用 `add` 命令来准备提交。
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. 使用 `commit` 命令来提交更改，并提供有关此提交的信息。
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. 使用 `push` 命令将更改推送到远程存储库。
    ```CMD
    `git push`
    ```

### <a name="step-5-create-a-pull-request"></a>步骤 5：创建拉取请求

推送更改后，返回到 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 并创建拉取请求，将分叉或分支合并到**主**分支。

![“拉取请求”选项卡的图片](images\pull-request.png)

## <a name="validation-process"></a>验证过程

当你创建拉取请求时，会启动一个自动化过程，该过程会验证清单并处理你的拉取请求。 我们会向你的拉取请求添加标签，方便你跟踪进度。

### <a name="submission-expectations"></a>对提交的预期

将应用程序提交到 Windows 程序包管理器存储库时，所有提交都应能够正常完成。 下面是对提交的一些预期：

* 清单符合[架构要求](manifest.md#manifest-contents)。
* 清单中的所有 URL 都通向安全的网站。
* 安装程序和应用程序中没有病毒。
* 对于管理员和非管理员，应用程序都可以正确地安装和卸载。
* 安装程序支持非交互模式。
* 所有清单条目都准确且没有误导性。
* 安装程序直接来自发布者的网站。

### <a name="pull-request-labels"></a>拉取请求标签

在验证过程中，我们会将一系列标签应用于拉取请求以传达进度信息。

* **Needs: author feedback**：提交发生故障。 我们会将拉取请求重新分配给你。 如果你在 10 天内未解决此问题，我们会关闭拉取请求。
* **Manifest-Validation-Error**：提交的清单包含语法错误。
* **URL-Validation-Error**：提交中的一个或多个 URL 未能通过 [SmartScreen](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview) 验证。
* **Binary-Validation-Error**：提交的应用程序安装程序未能通过病毒扫描测试，或存在哈希不匹配情况。
* **Pull-Request-Error**：拉取请求存在问题。 例如，文件夹结构未采用[必需格式](#step-3-add-your-manifest-to-the-local-repository)。
* **Validation-Error**：提交的应用程序未能通过常规验证测试。
* **Validation-Installation-Error**：提交的应用程序未能通过安装测试。
* **Validation-Uninstall-Error**：提交的应用程序未能通过卸载测试。
* **Validation-Virus-Scan-Error**：提交的应用程序未能通过病毒扫描测试。
* **Azure-Pipeline-Passed**：清单已完成第一个验证部分。 在此步骤之后，你的拉取请求会分配给我们的测试团队进行最终验证。
* **Validation-Completed**：验证已完成，系统会合并你的拉取请求。
