---
title: Web 服务
description: 了解如何创建您的应用程序的 Web 服务
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 web 服务
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600012"
---
# <a name="set-up-web-services-in-udc"></a>设置 UDC 中的 Web 服务

> [!WARNING]
> 以下文章是有关ID@Xbox和托管的合作伙伴开发人员仅由于 Web 服务配置受到限制。 与信赖方帐户级别权限授予 web 服务配置才可供开发人员。 如果还没有帐户级权限与开发帐户管理器 （坝） 以获得帮助的控件。

发布者可以创建 Web 服务，如果用户想要自定义应用程序/标题与 Xbox Live 服务进行交互的方式。 Web 服务是发布服务器级别配置，并可由任何标题在由发布者拥有的单一登录配置一个沙盒中调用。

若要定义 Web 服务的原因：

1. 它提供单一登录到 Xbox Live 用户-使你的 web 服务为 Xbox Live 用户提供单一登录，需要配置为信赖方的 Xbox Live。 配置通过这种方式，到 Xbox Live 身份验证的用户将自动进行身份验证到您的服务而无需重新输入一组不同的凭据。
2. 如果您的产品将使用其中一个 web 服务以使到 Xbox Live 服务的调用进行服务到服务调用你的服务从 Xbox Live 服务-，直接或代表单个用户，需要业务合作伙伴证书。

1. ## <a name="create-a-web-service"></a>创建 Web 服务

1. 转到[合作伙伴中心仪表板](https://partner.microsoft.com/dashboard/windows/overview)  
2. 单击要访问的页的右上角的齿轮状图标**设置**下拉列表。
3. 在下拉列表中，选择**开发人员设置**。
4. 在左侧导航栏中，展开选项**Xbox Live** ，然后选择**Web 服务**。

![web services gif ](../../images/dev-center/web-services/web-services.gif)

5. 在 Web 服务页上，单击**新的 Web 服务**。
6. 输入 Web 服务的名称并选择所需的访问类型。  
    * 遥测访问使你的服务来检索任何游戏的游戏的遥测数据。
    * 应用程序通道访问，拥有该服务的权限以编程方式发布在通过 OneGuide 扭转控制台上使用的应用程序通道的媒体提供程序。
7. 单击“保存”

此时，已定义服务和 Xbox Live 已注意到该服务存在。 具体取决于创建 web 服务的原因，你将需要配置信赖 Parties(Single Sing-On) 或业务合作伙伴 Certificates(Service-to-service calls)。  

## <a name="configure-relying-party"></a>配置信赖方

Web 服务需要为其配置为信赖方的 Xbox live 以便为 Xbox Live 用户提供单一登录体验 – 到 Xbox Live 身份验证的用户将自动向进行身份验证 web 服务而无需重新输入不同的凭据集。 若要实现此目的，必须在 Xbox 服务和 web 服务之间建立信任。 作为信赖方配置的一部分使用的一组声明 （如玩家代号、 设备类型标题 ID） 用于强制实施此信任关系。 这是 Xbox Live 和 web 服务，以帮助自动对用户进行身份验证之间交换的信息。

### <a name="create-a-relying-party"></a>创建信赖方

1. 转到合作伙伴中心仪表板  
2. 单击要访问的页的右上角的齿轮状图标**设置**下拉列表。
3. 在下拉列表中，选择**开发人员设置**。
4. 在左侧导航栏中，展开选项**Xbox Live** ，然后选择**信赖方**。
5. 在信赖方页上，单击上**新的信赖方**。
6. 为信赖方按以下格式输入的 URI: *example.com*。
7. 选择要用来确保安全的信赖方服务的加密类型。
8. 如果选择在上一步中的共享密钥对称加密，请单击**生成新密钥**以获取新的共享的密钥。 安全地保存此密钥在屏幕上按照的说明。
9. 输入**令牌的生存时间**以小时为单位。
10. 下**声明**，下拉列表中提供了你的信赖方服务可以使用进行身份验证的声明的列表。 选择你想要使用的所有声明。 所选的声明将出现在下拉列表中的下方。 默认情况下，将在该空间中填充一些标准的声明。
11. 单击**保存**完成后。  

## <a name="configure-a-business-partner-certificate"></a>配置业务合作伙伴证书

如果您的产品将使用其中一个 web 服务调用到 Xbox Live 服务，直接或代表单个用户，你将需要业务合作伙伴证书。

### <a name="generate-a-business-partner-certificate"></a>生成业务合作伙伴证书

已成功创建 Web 服务后，请继续执行以下步骤。  

1. 在 Web 服务页上，找到你想要将与业务合作伙伴证书关联的 web 服务。
2. 选择**生成证书**针对所选的 web 服务的链接。
3. 单击**显示选项**旁边生成新证书。 这会显示应使用管理员特权从 PowerShell 运行命令。
4. 运行所有一个命令后的其他应已成功为提供 Base64 编码 blob。 这是公共密钥。 从 PowerShell 复制公钥并将其粘贴在 CSP Blob 的占位符。
5. 单击**下载**并按照说明将证书绑定的页上。
    1. 使用同一台计算机用于生成的公钥。
    2. 在 PowerShell 中运行此命令： *mmc.exe*
    3. 选择文件并选择添加/删除管理单元。
    4. 选择证书并选择添加。 请确保证书管理单元中选择计算机帐户，然后单击完成，并单击确定。
    5. 打开 Personal\Certificate 存储区。
    6. 右键单击证书并选择所有任务并都选择导入。
    7. 选择从 UDC 下载的证书。
    8. 在 UI 中的证书已导入后右键单击并选择所有任务并都选择导出。
    9. 执行导出向导，并确保选择要导出的证书私钥。
    10. 完成导出向导。