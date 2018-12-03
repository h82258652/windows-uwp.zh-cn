---
title: 更新的多人游戏邀请流程
description: 提供了更新的 Xbox Live 多人游戏邀请实现流程。
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏 2015
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8348591"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>更新的多人游戏邀请流程

由于听取了 Xbox One Beta 的反馈，从 Xbox One 恢复更新 24 起，多人游戏邀请的用户体验流程已经更改，且已于 2013 年 11 月 6 日发布。这是对**仅用户体验 (UX)** 的更改，不会从游戏角度影响任何行为或功能。 作品开发人员不需要进行任何代码更改。

## <a name="summary-of-changes"></a>更改摘要

-   初始邀请 toast 已从“加入我的派对”更改为“&lt;游戏标题&gt;我们一起玩吧”，而且现在有了一个按钮，允许用户启动游戏并直接跳转到游戏中。

-   当用户选择新的“&lt;游戏标题&gt;我们一起玩吧”选项时，“派对”应用默认不贴靠。 同时进行此更改是为了让用户可以直接跳转到游戏中。

-   发送方一边已经添加了新的 toast，显示“向游戏添加 \[*数字*\] 个好友”。 这明确了当游戏会话与用户的派对关联时将发出邀请。

详细的用户体验流程通过以下示例加以介绍。 每个表显示两个用户 David 和 Laura 的示例流程。 这些流程在每个列中显示，并列出现。 <b style="background-color: #FFFF00">突出显示的文本</b>显示了对以前的 UX 流程进行的调整。

## <a name="inviting-users-from-within-a-game"></a>从游戏内邀请用户

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
David 位于他正在玩的游戏的多人游戏大厅内。<p><br>David 选择“邀请好友”<b></b>。<p><br>David 选择 Laura。<p><br>Toast 弹出，指示邀请已发送。 | Laura 正在玩另一个游戏。
    </td>
    <td style="border-bottom:solid 1px #fff">
Laura 正在玩另一个游戏。
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Toast 弹出指示 David 发出的邀请，并<b style="background-color: #FFFF00">显示游戏名称和图标</b>。 （“派对”应用不自动贴靠。） <p><br>
在“通知中心”，<b style="background-color: #FFFF00">Laura 可以选择“启动并接受邀请”</b>、<b>接受邀请</b>或<b style="background-color: #FFFF00">拒绝邀请</b>。
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">情况 1：Laura 选择“启动并接受邀请”</b>（这是一个新选项）</td>
  </tr>
  <tr>
    <td>
Toast 弹出指示 Laura 已加入 David 的派对。             
    <p><br>
David 从多人游戏大厅开始游戏。                              
    <p><br>
    <b style="background-color: #FFFF00">Toast 弹出指示游戏邀请已发送给 Laura。</b>
    </td>
    <td>
游戏启动，“派对”应用不贴靠。
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>情况 2：Laura 选择“接受邀请”</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura 加入派对。</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">Toast 弹出指示游戏邀请已发送给 Laura。</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>Toast 弹出指示未为派对找到游戏。
    <p><br>
在“通知中心”，Laura 可以选择： <ul>
    <li>   <b>接受游戏邀请：</b>游戏启动。
    <li>   <b>拒绝游戏邀请：</b>没有游戏启动。Laura 仍位于派对中，并将收到后来的游戏邀请。         
    <li>   <b style="background-color: #FFFF00">离开派对：没有游戏启动。Laura 被从派对中删除。</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>游戏内邀请流程：在派对中，切换游戏

<table>
  <tr>
    <td>
一起玩游戏。
    <p><br>
David 切换到其他游戏并转到多人游戏菜单。
     <p><br>
Xbox 系统 UI 询问 David 是否要将他的派对切换到新游戏，他可以回答“是”<b></b>或“否”<b></b>。
    </td>
    <td style="text-align:top">
一起玩游戏。
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>情况 1：是</b>
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
派对进入新作品。
    <p><br>
从多人游戏大厅，David 开始游戏。
    <p><br>
    <b style="background-color: #FFFF00">Toast 弹出指示游戏邀请已发送给 Laura。
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>Toast 弹出指示未为派对找到游戏。
    <p><br>
从“通知中心”，Laura 可以选择： <ul>
     <li><b>接受游戏邀请</b>：新游戏启动 <li><b>拒绝游戏邀请：</b>没有游戏启动，但 Laura 仍位于派对中，并将收到后来的游戏邀请。
     <li><b style="background-color: #FFFF00"><b>离开派对：</b>没有游戏启动，Laura 被从派对中删除。</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>情况 2：否</b>
    </td>
  </tr>
  <tr>
    <td>
派对不进入新作品。
David 在多人游戏模式下玩游戏，没有派对成员。
David 仍位于派对内。
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>从“主页”发出的邀请流程

<table>
  <tr>
    <td>
David 正在浏览“主页”<b></b>，在他的“好友”<b></b>列表中，他看到 Laura 在线。
    <p><br>
David 选择邀请 Laura 加入他的派对。 Toast 弹出指示邀请已经发送。 “派对”应用为 David 贴靠。
    </td>
    <td>
Laura 正在玩游戏。
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Toast 弹出指示 David 已邀请 Laura 加入他的派对。
    <p><br>
在“通知中心”，Laura 可以选择“接受邀请”<b></b>。
    <p><br>
当 Laura 接受时，“派对”应用贴靠。                                                                         </td>
  </tr>
  <tr>
    <td>
Toast 弹出指示 Laura 已加入派对。
    <p><br>
David 和 Laura 讨论他们要玩什么游戏。 David 启动游戏并进入多人游戏模式。
    <p><br>
游戏将提供邀请好友的选项，或自动拉入派对成员。
    <p><br>
    <b style="background-color: #FFFF00">Toast 弹出指示游戏邀请已发出。</b>
    </td>
    <td>
Toast 弹出指示已为派对找到游戏。
    <p><br>
在“通知中心”，Laura 可以： <ul>
    <li>   <b>接受游戏邀请：</b>游戏启动 <li>   <b>拒绝游戏邀请：</b>没有游戏启动，Laura 仍位于派对中，并将收到后来的邀请。
    <li>   <b style="background-color: #FFFF00">离开派对：没有游戏启动，Laura 被从派对中删除。</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>有关“已发送游戏邀请”toast 的更多信息

**已发送游戏邀请** toast 将在第一次建立包含远程派对成员的游戏会话时出现。 发送给远程派对成员的后续邀请不生成此 toast。 当游戏对 **PullReservedPlayersAsync** 进行多次调用时，这可以防止用户被重复的**已发送游戏邀请** toast 刷屏。

**注意**最佳实践是将所有需要的好友添加为“保留”，然后只调用 **PullReservedPlayersAsync** 一次。
