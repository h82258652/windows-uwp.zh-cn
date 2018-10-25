---
title: 跨平台游玩游戏入门
author: KevinAsgari
description: 了解如何开发在电脑和 Xbox One 主机上运行的跨平台游玩游戏。
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 跨平台游玩, 随处游戏
ms.localizationpriority: medium
ms.openlocfilehash: 14f6e895ed98804fa965ee6d9ef6cadcde6f220f
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5477551"
---
# <a name="get-started-with-cross-play-games"></a>跨平台游玩游戏入门

## <a name="summary"></a>摘要

随着 Windows 10 的发布，游戏开发人员将能够发布在 Xbox One（作为 XDK 游戏）和 Windows 10（作为 UWP 游戏）上运行的单个产品。 在某些情况下，开发人员需要为这些游戏启用跨平台游玩，其中其游戏的 Xbox One 和 Windows 10 版本会跨 Xbox Live 服务（例如，多人游戏、游戏保存、成就等）得到统一。 要启动跨平台游玩，这些游戏会在游戏的 XDK 和 UWP 版本中共享单个主题作品 ID 和 Xbox Live 服务配置。

引入 XDK+UWP 今日游戏需要 4 个主要步骤：

1.  在 Windows 开发人员中心中创建 UWP 产品

2.  在 XDP 中创建 XDK 游戏，选择你要在其中共享 XBL 配置的平台

3.  将 UWP 产品信息绑定到 XDP 中的 XDK 产品

4.  通过 XDP 配置和发布 Xbox Live

本文档的目的是进一步详细说明这 4 个步骤，让 Xbox 员工尽可能轻松地引入 XDK+UWP 跨平台游玩游戏

## <a name="terminology"></a>术语

### <a name="scenario-terms"></a>场景术语

1.  跨平台游玩：在多个平台上发布一个游戏，但共享单个 Xbox 主题作品 ID 和服务配置。 最终结果是，游戏的两个版本共享同样的 Xbox Live 配置 - 成就、排行榜、游戏保存、多人游戏等。

2.  Windows 开发人员中心：你可以在此门户中预定用于今天的 UWP 开发的应用标识和设置 UWP 的 Xbox Live 配置。

3.  XDP：现存的 Xbox 开发人员门户，可引入、配置和发布 Xbox One XDK 和 SRA 游戏，还将看到对引入、配置和发布 XDK+UWP 跨平台游玩游戏的额外使用。

### <a name="identity-terms"></a>标识条款

1.  主题作品 ID：这是 Xbox 主题作品 ID，用于将每个游戏标识到 Xbox Live 中。 映射到单个产品的主题作品 ID，可能会跨越多个平台。

2.  服务配置 ID (SCID)：每个 Xbox 主题作品（由主题作品 ID 标识）都有相应的服务配置 ID（即 SCID）。 与主题作品交互时，此 ID 允许 Xbox Live 唯一标识要使用的规则/配置。

3.  包系列名称 (PFN)：这是分配给在开发人员中心中创建的每个产品的标识。 将 UWP 绑定到此开发人员中心产品的标识后，它将采用此 PFN。 PFN 是唯一的产品标识符，可能会跨越多个平台。 PFN 与 Xbox 主题作品 ID 的比例为 1:1。

4.  MSA 应用 ID：也称为 MSA 客户端 ID，这是由 MSA 在开发人员中心中在产品创建时间分配的另一个应用标识。 此标识有助于 Microsoft 服务识别你的应用。 MSA 应用与 PFN 的比例（以及相应地与 Xbox 主题作品 ID 的比例）为 1:1。

## <a name="scenario-overview"></a>场景概述

### <a name="what-is-cross-play"></a>什么是跨平台游玩？

Windows 10 展示型体验；跨平台游玩是 Xbox One 和电脑之间的跨设备游戏，游戏会在设备版本之间共享单个 Xbox Live 配置，从而呈现跨设备多人游戏、成就和排行榜以及游戏保存等场景。

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>跨平台游玩有哪些利弊？

如果你想让游戏的 XDK 和 UWP 版本进行以下操作，跨平台游玩可能是正确的方法：

-   采用至少一种多人游戏模式参与跨设备的多人游戏（Xbox One 与电脑）

-   共享用户可以在两台设备上使用的单个游戏保存

-   具有一组成就和玩家分数/挑战/排行榜，用户可以针对两个设备叠加增长

如果你想进行以下操作，跨平台游玩可能不是正确的方法：

-   在任意和所有多人游戏模式下，你想阻止游戏的 Xbox One 玩家参与设备中的多人游戏

-   你想让 Xbox One 和电脑游戏保存保持独立（可能出于安全或信任原因）

-   你希望游戏的 Xbox One 和电脑版本有单独的玩家分数（即购买 Xbox One 和电脑的用户可以在每个平台获得 1000 玩家分数，而不是共享的 1000 分）

一般情况下，跨平台游玩会将大部分值添加到：

-   强调游戏的 Xbox One 和电脑版本之间的连续性的免费游戏/Xbox Play Anywhere 游戏

-   以 Xbox One 和电脑之间的跨设备多人游戏为特征的游戏

**注意**：跨平台游玩适用于同时发布了游戏的 XDK 和 UWP 版本的新游戏，以及已经提供了 XDK，但正在添加 UWP 版本的游戏。

### <a name="what-are-the-restrictions-of-cross-play"></a>跨平台游玩有哪些限制？

在 Xbox One 支持 UWP 游戏之前，跨平台游玩游戏需要游戏的 XDK（适用于 Xbox One 主机）和 UWP（适用于 Windows 10 电脑版）版本。

所有 XDK + UWP 跨平台游玩游戏都会有一些重要的限制：

1.  **XDK 主题作品必须引入到 XDP 中**。 凭借服务配置和主线发布体验，不需要配备开发人员中心即可支持基于 XDK 的主题作品。

2.  **游戏的 XDK 和 UWP 版本均可使用在 XDP 中创建的单个服务配置**。 我们已经向 XDP 添加了新功能，允许游戏在其 XDK 和 UWP 版本之间共享单个服务配置。 仍然需要在开发人员中心上为程序包/目录发布 UWP 版本，但所有服务配置均可在 XDP 中发布。

3.  **不能在 XDP 和开发人员中心之间拆分服务配置**。 XDP 和开发人员中心不能感知对方的存在 – 在一个中发布会覆盖另一个中的现有发布。 这可能会无法修复地破坏服务配置并创建糟糕的用户体验（使成就消失、丢失游戏保存等），因此，我们需要在 XDP 中为 XDK+UWP 跨平台游玩游戏完成所有服务配置。

### <a name="create-your-uwp-product-in-the-windows-dev-center"></a>在 Windows 开发人员中心中创建 UWP 产品

通过遵循[将 Xbox Live 添加到新的或现有的 UWP 项目](get-started-with-visual-studio-and-uwp.md)指南，在 Windows 开发人员中心上创建 UWP 产品

## <a name="setup-your-xdk-product-in-xdp"></a>在 XDP 中设置你的 XDK 产品

既然你已经创建了 UWP，那么应该准备好在 XDP 中设置 XDK 产品了。 如果你还没有 XDK 作品，则必须创建一个。

### <a name="create-your-xdp-product"></a>创建 XDP 产品

使用帐户管理器在 XDP 中创建新产品下你的发布者 ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher))。

在 XDP 中创建产品时，确保滚动到 UI 左侧部分的底部选择你的平台。 检查每个你**希望某天**在其上使用 Xbox Live 跨平台游玩集成发布游戏的平台。

选择平台后，请指定游戏的资源访问类型（最有可能是 XDK 主题作品）和此产品的预期发布机制。


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>更新你的 XDP 产品平台

如果在 XDP 中已经有现有的 XDK 产品，则需要将其更新，以支电脑平台。 要执行此操作，请在产品上导航到“产品设置”&gt;“平台类型”。

![](../images/ingesting_crossplay_games_xdp/image10.png)

在此页上，选择你要支持的平台（选项有：Xbox One、阈值电脑和 Windows Mobile）。 选好后，请选择“提交平台”按钮。

更改将立即实现（无需发布服务配置、目录或二进制文件即可生效）。 注意，此配置会跨越沙盒 - 游戏的每个沙盒无法拥有不同的平台类型。

### <a name="enter-your-msa-app-id"></a>输入 MSA 应用 ID。

创建 XDP 产品后，请转到产品的“产品设置”页，输入以前创建的 MSA 应用 ID。 可在 XDP 中，通过单击产品任务栏最左侧的“状态框”实现“产品设置”，如下所示。

![](../images/ingesting_crossplay_games_xdp/image11.png)

到达“产品设置”页后，请选择“应用程序 ID 设置”部分。 在此区域中，你可以输入检索到的 MSA 应用 ID 并将其填入“应用程序 ID”字段中，如下所示。

![](../images/ingesting_crossplay_games_xdp/image12.png)

**你****无需输入姓名和发布者属性**，**尤其是不应使用页面上的“获取应用程序 ID”链接**，因为你已经有需要在此字段中输入的 MSA 应用 ID，且不需要再为应用程序生成一个新的。

在“应用程序 ID”字段中输入你的 MSA 应用 ID 后，单击“提交应用程序 ID 设置”按钮。 这会使用 Xbox Live 安全保存你的 MSA 应用 ID 信息 - 无论你何时发出从 UWP 检索 XToken 的请求，其中包含的主题作品声明都会映射到此 XDP 产品（只要 UWP 正确使用了你在 Windows 开发人员中心创建的 AppX 清单标识！）

### <a name="enter-the-dev-center-pfn-into-xdp"></a>将开发人员中心 PFN 输入到 XDP 中

虽然上述步骤足以对 UWP 游戏进行身份验证和通过在 XDP 中创建和发布的服务配置使用 Xbox Live，某些 Xbox Live 功能（例如多人游戏邀请）需要感知 UWP 游戏的 PFN 才能正确运行。

要执行此操作，请导航到“产品设置”&gt;“开发人员中心绑定”

![](../images/ingesting_crossplay_games_xdp/image13.png)

在此页上，输入 UWP 应用的 PFN（如在 4.1.1 部分中检索到的），然后选择“保存”按钮。

此配置不会立即实现。 它会通过发布到沙盒的未来的服务配置实现。 因此，此信息会沙盒化，并需要发布到每个沙盒才可用。

### <a name="flag-your-app-for-xbox-cert-in-the-dev-center"></a>在开发人员中心标记应用以备 Xbox 认证

如今，将游戏识别为支持 Xbox Live 以便进行 Xbox 认证的过程需要一些手动干预。 使用版本管理器在开发人员中心中将你的应用标记为支持 Xbox Live，以便进行智能认证检测。

### <a name="update-your-uwp-game-in-the-xbox-app"></a>在 Xbox 应用中更新 UWP 游戏

通常情况下，Xbox 应用会使用 UWP 游戏的所有自动生成的主题作品 ID 来提供 Xbox 应用内的 Xbox Live 体验。 为了让 UWP 游戏正确使用在 Xbox 应用中由 XDP 生成的主题作品 ID，在**提交 UWP 游戏进行发布之前**，需要在 Windows 开发人员中心进行数据更新。

要执行此操作，请联系你的 DAM，并告知他们你要为主题作品名称更新 Xbox 应用主题作品 ID。  请务必为 UWP 包括在 XDP 中创建的主题作品 ID（在“产品设置”&gt;“产品详细信息”下的 XDP 中可见）和 Windows 10 的 URL（在“应用管理”&gt;“应用标识”下的“开发人员中心”中可见）。

## <a name="configure-xbox-live-in-xdp"></a>在 XDP 中配置 Xbox Live

### <a name="service-configuration"></a>服务配置

正确配置和绑定 XDP 和开发人员中心产品后，你可以像往常设置 XDK 主题作品那样，随意设置 XDP 中的共享 Xbox Live 配置。

**提醒** – 在任何情况下，绑定 XDK + UWP 游戏都不应通过开发人员中心启用、配置或发布 Xbox Live 服务配置。 未遵循本指南可能会永久损害游戏的 Xbox Live 配置。

### <a name="catalog-configuration"></a>目录配置

对于 XDK+UWP 游戏，目录配置需要设置两次：在 XDP 中为 XDK 设置一次，还要在开发人员中心为 UWP 设置一次。

对于 XDP 配置，设置过程与一般的 XDK 产品完全相同。 对于开发人员中心配置，可在[此处](https://dev.windows.com/en-us/publish)查找更多详细步骤。

<table>
  <tr>
    <td>
      对于仅 UWP 游戏，只需要设置有限的一组目录配置。 特别是，需要为仅 UWP 游戏填写营销信息，因为服务配置会为配置 Xbox Live 游戏的任意 XDP 使用此处输入的字符串。 此外，应将可用性设置为“不可用”，确保如果/当发布游戏的目录信息时，游戏绝不会显示在 Xbox One 目录中。
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>二进制文件配置

对于 XDK+UWP 游戏，二进制文件配置需要设置两次：在 XDP 中为 XDK 设置一次，还要在开发人员中心为 UWP 设置一次。

对于 XDP 配置，设置过程与一般的 XDK 产品完全相同。 对于开发人员中心配置，可在[此处](https://dev.windows.com/en-us/publish)查找更多详细步骤。

<table>
  <tr>
    <td>
      对于仅 UWP 游戏，无需在 XDP 中进行二进制文件配置。 你可以完全跳过此步骤。
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>在 XDP 中发布

一般来说，发布 XDK + UWP 游戏与分别在 XDP 中发布 XDK 主题作品和在开发人员中心发布 UWP 的过程相同。 因此，下面将重点介绍制作跨平台游玩游戏的独特过程和注意事项。

### <a name="dev-sandbox-publishing"></a>开发人员沙盒发布

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>没有适用于 Microsoft Store 的等效开发人员沙盒目录

尽管 XDP 允许你将目录和二进制文件发布到 Xbox One 目录的开发人员沙盒版本，但 Microsoft Store 目录不支持沙盒。 因此，在开发人员沙盒中测试 UWP 需要你旁加载该 UWP 并直接运行。 这不会影响 Xbox Live 测试，但可能会改变你的标准测试过程。

<table>
  <tr>
    <td>
      对于仅 UWP 游戏，仍需要发布 XDK 主题作品目录信息以取消阻止服务配置发布，虽然仅 UWP 仅游戏不存在 Xbox One 目录。
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>CERT 沙盒发布

### <a name="coordination-between-xdp-publishing-and-dev-center-publishing"></a>XDP 发布和开发人员中心发布之间的协调

在 XDP 中，从 CERT 移动到 RETAIL 是两个独立的显式操作。 但是，在开发人员中心，提交过程会自动移动游戏，使其通过认证并到达零售。 因此，请遵循操作顺序。

当你准备好转到认证时，应按顺序遵循以下步骤：

1.  在 XDP 中，将你的 XDK 产品发布到 CERT（包括目录、二进制文件和服务配置）

2.  为你的 UWP 产品启动开发人员中心提交

    1.  **务必在“发布日期”字段中选择“手动发布此应用”或“不早于 \[日期\]”。** 如果未执行此操作，UWP 游戏可能会不经干预就自动发布到零售。

<table>
  <tr>
    <td>
      对于仅 UWP 游戏，在启动开发人员中心提交之前，仍需要将目录和服务配置发布到 XDP 中的 CERT，即使你没有正在发布的 XDK 主题作品二进制文件。
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>RETAIL 沙盒发布

### <a name="coordination-between-xdp-publishing-and-dev-center-publishing"></a>XDP 发布和开发人员中心发布之间的协调

完成 XDK 主题作品的认证，且 UWP 已离开认证并准备好发布后，你可以将应用发布到 RETAIL。 同样，请务必按照特定顺序遵循此过程，确保 XDK 主题作品和 UWP 保持对齐。

1.  准备就绪后，将 XDP 中的 XDK 产品发布到 RETAIL（包括目录、二进制文件和服务配置）

2.  如果之前选择了“手动发布此应用”，请在开发人员中心中的产品的认证页上，选择“立即发布”，如果之前选择了“不早于 \[日期\]”，则等待发布发生。

完成这些步骤后，应该将你的 XDK 主题作品和 UWP 游戏发布到全世界，并在 RETAIL 中进行共享服务配置。 恭喜你！

<table>
  <tr>
    <td>
      对于仅 UWP 游戏，在开发人员中心中完成发布之前，仍需要将目录和服务配置发布到 XDP 中的 RETAIL，否则，发布的 UWP 将无法访问 Xbox Live。
    </td>
  </tr>
</table>
