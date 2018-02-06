---
author: jnHs
Description: Set custom permissions for account users.
title: "为帐户用户设置自定义权限"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 用户角色, 用户权限, 自定义角色, 用户访问权限, 自定义权限, 标准角色"
ms.localizationpriority: high
ms.openlocfilehash: 1fdde4be606abae849ff3350d27afbbced157f75
ms.sourcegitcommit: 446fe2861651f51a129baa80791f565f81b4f317
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2018
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>为帐户用户设置角色或自定义权限

[将用户添加到开发人员中心帐户](add-users-groups-and-azure-ad-applications.md)时，将需要指定他们的帐户中有哪些访问权限。 可以通过将适用于整个帐户的[标准角色](#roles)分配给用户以实现此目的，或者可以选择[自定义权限](#custom)，向用户提供相应的访问权限级别。 有些自定义权限适用于整个帐户，有些权限限制于一个或多个特定产品（如果你愿意，可授予所有产品）。

> [!NOTE] 
> 无论是添加用户、组还是添加 Azure AD 应用程序，均可应用同一角色和权限。

在确定应用什么角色或权限时，请注意： 
-   用户（包括组和 Azure AD 应用程序）将能够使用与其分配角色关联的权限访问整个开发人员中心帐户，除非[自定义权限](#custom)并分配[产品级别的权限](#product-level-permissions)，这样他们只能使用特定的应用和/或加载项。
-   可以通过选择多个角色或使用自定义权限授予想要授予的访问权限，允许用户、组或 Azure AD 应用程序访问多个角色的功能。
-   具有某个角色（或自定义权限集）的用户还可加入具有其他角色（或权限集）的组。 在该情况下，用户可访问与组和个人帐户关联的所有功能。

> [!TIP]
> 本主题特定于 Windows 应用开发人员计划。 有关硬件开发人员计划中用户角色的信息，请参阅[管理用户角色](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles)。


<span id="roles" />
## <a name="assign-roles-to-account-users"></a>将角色分配给帐户用户

默认情况下，从将用户、组或 Azure AD 应用程序添加至开发人员中心帐户时起，即有一组标准角色可供选择。 每个角色都有一组特定的权限，以便在帐户内执行某些功能。 

除非选择定义[自定义权限](#custom)（方法为选中**自定义权限**），否则添加到帐户的每个用户、组或 Azure AD 应用程序都必须分配以下标准角色中的至少一个角色。 

> [!NOTE]
> 该帐户的**所有者**是第一个使用 Microsoft 帐户创建它的人员（而不是任何通过 Azure AD 添加的用户）。 此帐户所有者是唯一具有该帐户的完整访问权限的人员，其中包括删除应用、创建和编辑所有帐户用户以及更改所有财务和帐户设置的功能。 


| 角色                 | 描述              |
|----------------------|--------------------------|
| 管理员              | 具有对该帐户的完整访问权限，除了更改税收和付款设置。 这包括在开发人员中心中管理用户，但请注意，在 Azure AD 租户中创建和删除用户的功能取决于帐户在 Azure AD 中的权限。 即，如果用户分配了管理员角色，但在组织的 Azure AD 中没有全局管理员权限，他们将无法创建新用户或从目录中删除用户（尽管他们可以更改用户的开发人员中心角色）。 <p> 注意，如果开发人员中心帐户与多个 Azure AD 租户关联，则管理员无法查看完整的用户详细信息（包括名字、姓氏、密码恢复电子邮件，以及他们是否为 Azure AD 全局管理员），除非使用拥有该用户所在租户的全局管理员权限的帐户登录该租户。 不过，管理员可以在与开发人员中心帐户关联的任何租户中添加和删除用户。 |
| 开发人员            | 可以上传程序包并提交应用和加载项，并且可以查看[使用情况报告](usage-report.md)获取遥测详细信息。 无法查看财务信息或帐户设置。   |
| 业务参与者 | 可查看[运行状况](health-report.md)和[使用情况](usage-report.md)报告。 无法创建或提交产品、更改帐户设置或查看财务信息。                                         |
| 财务参与者  | 可查看[付款报告](payout-summary.md)、财务信息和购置报告。 无法对应用、加载项或帐户设置进行任何更改。                                                                                                                                   |
| 营销人员             | 可以[回复客户评论](respond-to-customer-reviews.md)和查看非财务[分析报告](analytics.md)。 无法对应用、加载项或帐户设置进行任何更改。      |

下表显示了每个角色（和帐户所有者）可用的一些特定功能。

|                                 |    帐户所有者                 |    管理员                       |    开发人员                     |    业务参与者    |    财务参与者    |    营销人员                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    购置报告           |    可以查看                      |    可以查看                      |     无法访问                    |     无法访问              |    可以查看               |    无法访问                     |
|    反馈报告/响应    |    可以查看和发送反馈    |    可以查看和发送反馈    |    可以查看和发送反馈    |     无法访问              |     无法访问             |    可以查看和发送反馈    |
|    运行状况报告                |    可以查看                      |    可以查看                      |    可以查看                      |    可以查看                |     无法访问             |    无法访问                     |
|    使用情况报告                 |    可以查看                      |    可以查看                      |    可以查看                      |    可以查看                |     无法访问             |    无法访问                     |
|    付款帐户               |    可以更新                    |    无法访问                     |    无法访问                     |    无法访问               |    可以查看               |    无法访问                     |
|    税务配置文件                  |    可以更新                    |    无法访问                     |    无法访问                     |    无法访问               |    可以查看               |    无法访问                     |
|    付款摘要               |    可以查看                      |    无法访问                     |    无法访问                     |    无法访问               |    可以查看               |    无法访问                     |

如果没有合适的标准角色，或者希望限制访问特定应用和/或加载项，可选中**自定义权限**，将自定义权限授予用户，如下所述。


<span id="custom" />
## <a name="assign-custom-permissions-to-account-users"></a>将自定义权限分配至帐户用户

若要分配自定义权限而非标准角色，可在添加或编辑用户帐户时在**角色**部分中单击**自定义权限**。 

若要为用户启用某项权限，请将框切换为相应设置。 

![访问权限设置指南](images/permission_key.png)

- **无法访问**：用户没有所指示的权限。
- **只读**：用户有权访问与指示区域相关的功能，但无法进行更改。 
- **读/写**：用户有权更改和查看与区域关联的功能。
- **混合**：无法直接选择此选项，但**混合**指示器将显示是否允许使用该权限的访问组合。 例如，如果向**所有产品**的**定价和可用性**授予**只读**访问权限，但随后向特定产品的**定价和可用性**授予**读/写**访问权限，则**所有产品**的**定价和可用性**指示器将显示为“混合”。 如果某些产品**无法访问**某项权限，但其他权限具有**读/写**和/或**只读**访问权限，这也同样适用。

对于某些权限（例如与查看分析数据相关的权限），仅可授予**只读**访问权限。 请注意，在当前实现中，某些权限的**只读**和**读/写**访问权限没有区别。 请查看每项权限的详细信息，了解**只读**和/或**读/写**访问权限授予的特定功能。

下表介绍了有关每项权限的特定详细信息。

## <a name="account-level-permissions"></a>帐户级权限

此部分中权限不限于特定产品。 向这些权限授予访问权限可支持用户访问整个帐户。

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">权限名称</th>
    <th align="left">只读</th>
    <th align="left">读取/写入</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    **帐户设置**                    </td><td align="left">  可查看**帐户设置**部分中的所有页面，包括[联系人信息](managing-your-profile.md)。       </td><td align="left">  可查看**帐户设置**部分中的所有页面。 可更改[联系人信息](managing-your-profile.md)和其他页面，但无法更改付款帐户或税务配置文件（除非单独授予该权限）。            </td></tr>
<tr><td align="left">    **帐户用户**                       </td><td align="left">  可在**用户**部分查看已添加到帐户的用户。          </td><td align="left">  可将用户添加到帐户，并在**用户**部分更改现有的用户。             </td></tr>
<tr><td align="left">    **帐户级别的广告性能报告** </td><td align="left">  可查看帐户级的[广告性能报告](advertising-performance-report.md)。      </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    **广告市场活动**                        </td><td align="left">  可查看在帐户中创建的[广告市场活动](create-an-ad-campaign-for-your-app.md)。      </td><td align="left">  可在帐户中创建、管理和查看[广告市场活动](create-an-ad-campaign-for-your-app.md)。          </td></tr>
<tr><td align="left">    **广告中介**                        </td><td align="left">  可查看帐户中所有产品的[广告中介配置](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx)。    </td><td align="left">  可查看和更改帐户中所有产品的[广告中介配置](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx)。        </td></tr>
<tr><td align="left">    **广告中介报告**                </td><td align="left">  可查看帐户中所有产品的[广告中介报告](ad-mediation-report.md)。    </td><td align="left">  不适用    </td></tr>
<tr><td align="left">    **广告性能报告**              </td><td align="left">  可查看帐户中所有产品的[广告性能报告](advertising-performance-report.md)。       </td><td align="left">  不适用         </td></tr>
<tr><td align="left">    **广告单元**                            </td><td align="left">  可查看为帐户创建的[广告单元](in-app-ads.md)。    </td><td align="left">  可创建、管理和查看帐户[广告单元](in-app-ads.md)。             </td></tr>
<tr><td align="left">    **联盟广告**                       </td><td align="left">  可查看帐户中所有产品的[联盟广告](about-affiliate-ads.md)使用情况。    </td><td align="left">  可管理和查看帐户中所有产品的[联盟广告](about-affiliate-ads.md)使用情况。                </td></tr>
<tr><td align="left">    **联盟性能报告**      </td><td align="left">  可查看帐户中所有产品的[联盟性能报告](affiliates-performance-report.md)。   </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    **应用安装广告报告**             </td><td align="left">  可查看[广告活动报告](promote-your-app-report.md)。           </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    **社区广告**                       </td><td align="left">  可查看帐户中所有产品的免费[社区广告](about-community-ads.md)使用情况。          </td><td align="left">  可创建、管理和查看帐户中所有产品的免费[社区广告](about-community-ads.md)使用情况。               </td></tr>
<tr><td align="left">    **联系人信息**                        </td><td align="left">  可查看“帐户设置”部分中的[联系人信息](managing-your-profile.md)。        </td><td align="left">  可编辑和查看“帐户设置”部分中的[联系人信息](managing-your-profile.md)。            </td></tr>
<tr><td align="left">    **COPPA 合规性**                    </td><td align="left">  可查看帐户中所有产品的 [COPPA 合规性](in-app-ads.md#coppa-compliance)选择（指示产品是否面向年龄在 13 岁以下的儿童）。                                            </td><td align="left">  可编辑和查看帐户中所有产品的 [COPPA 合规性](in-app-ads.md#coppa-compliance) 选择（指示产品是否面向年龄在 13 岁以下的儿童）。         </td></tr>
<tr><td align="left">    **客户组**                     </td><td align="left">  可查看**客户**部分中的[客户组](create-customer-groups.md)（类别和外部测试版组）。      </td><td align="left">  可创建、编辑和查看**客户**部分中的[客户组](create-customer-groups.md)（类别和外部测试版组）。       </td></tr>
<tr><td align="left">    **新应用**                            </td><td align="left">  可查看新应用创建页面，但实际上无法在帐户中创建新应用。    </td><td align="left">  在帐户中预留新应用名称可[创建新应用](create-your-app-by-reserving-a-name.md)，并可创建新提交，然后将应用提交到应用商店。     </td></tr>
<tr><td align="left">    **新捆绑包**&nbsp;*                       </td><td align="left">  可查看新捆绑包创建页面，但实际上无法在帐户中创建新捆绑包。     </td><td align="left">  可创建产品的新捆绑包。          </td></tr>
<tr><td align="left">    **合作伙伴服务**&nbsp;*                  </td><td align="left">  可查看检索 XTokens 服务的安装证书。     </td><td align="left">  可管理和查看检索 XTokens 服务的安装证书。       </td></tr>
<tr><td align="left">    **付款帐户**                      </td><td align="left">  可在**帐户设置**中查看[付款信息](setting-up-your-payout-account-and-tax-forms.md#payout-account)。     </td><td align="left">  可在**帐户设置**中编辑和查看[付款信息](setting-up-your-payout-account-and-tax-forms.md#payout-account)。       </td></tr>
<tr><td align="left">    **付款摘要**                      </td><td align="left">  可查看[付款摘要](payout-summary.md)，访问并下载付款报告信息。       </td><td align="left">  可查看[付款摘要](payout-summary.md)，访问并下载付款报告信息。   </td></tr>
<tr><td align="left">    **依赖方**&nbsp;*                   </td><td align="left">  可查看依赖方，检索 XTokens。    </td><td align="left">  可管理和查看依赖方，检索 XTokens。     </td></tr>
<tr><td align="left">    **光盘请求**&nbsp;*                   </td><td align="left">  可查看游戏光盘请求。    </td><td align="left">  可生成并查看游戏光盘请求。     </td></tr>
<tr><td align="left">    **沙盒**&nbsp;*                         </td><td align="left">  可访问**沙盒**页，并查看帐户中的沙盒以及这些沙盒的任何适用配置。 无法查看每个沙盒的产品和提交，除非授予相应的产品级别权限。 </td><td align="left">  可访问**沙盒**页，并查看和管理帐户中的沙盒，包括创建和删除沙盒以及管理它们的配置。 无法查看每个沙盒的产品和提交，除非授予相应的产品级别权限。    </td></tr>
<tr><td align="left">    **税务配置文件**                         </td><td align="left">  可查看**帐户设置**中的[税务配置文件信息和表单](setting-up-your-payout-account-and-tax-forms.md#tax-forms)。     </td><td align="left">  可填写税单和更新**帐户设置**中的[税务配置文件信息](setting-up-your-payout-account-and-tax-forms.md#tax-forms)。     </td></tr>
<tr><td align="left">    **测试帐户**&nbsp;*                     </td><td align="left">  可查看测试 Xbox Live 配置的帐户。      </td><td align="left">  可创建、管理和查看测试 Xbox Live 配置的帐户。      </td></tr>
<tr><td align="left">    **Xbox 设备**                        </td><td align="left">  可在**帐户设置**部分查看为帐户启用的 Xbox 开发主机。       </td><td align="left">  可在**帐户设置**部分查看为帐户添加、删除和启用的 Xbox 开发主机。     </td></tr>
    </tbody>
    </table>

\* 标有星号 (*) 的权限可将访问权限授予不向所有帐户提供的功能。 如果帐户尚未启用这些功能，则选择这些权限不会有任何效果。   


## <a name="product-level-permissions"></a>产品级别的权限

本部分中的权限可授予帐户中所有产品，或者进行自定义，支持仅向一个或多个特定产品授予权限。 

产品级别权限分为四个类别：**分析**、**盈利**、**发布**和 **Xbox Live**。 可展开这些类别，查看每个类别中的各个权限。 还可以选择启用一个或多个特定产品的**所有权限**。

若要向帐户中每个产品授予权限，请在标为**所有产品**的行中选择该权限（切换框，指示**只读**或**读/写**）。 
 
> [!TIP]
> 为**所有产品**选择的内容将应用到当前在帐户中的所有产品和在帐户中创建的任何未来产品。 若要防止将权限应用到未来产品，则分别选择所有产品，而不是选择**所有产品**。

在**所有产品**行下，将看到帐户中每个产品都列在独立一行上。 若仅为特定产品授予权限，请在该产品行中选择该权限。

每个加载项与**所有加载项**行一起列在父产品下的单独行中。 为**所有加载项**选择的内容将应用到该产品当前所有的加载项和为该产品创建的任何未来加载项。

请注意，无法为加载项设置某些权限。 这是因为它们不适用于加载项（例如**客户反馈**权限）或是因为在父产品级别授予的权限适用于该产品的所有加载项（例如**促销代码**）。 但请注意，适用于加载项的任何权限必须单独设置；加载项不会继承为父产品选择的内容。 例如，如果希望允许用户选择加载项的定价和可用性范围，则需要为该加载项（或**所有加载项**）启用**定价和可用性**权限，无论是否授予了父产品的**定价和可用性**权限。 


### <a name="analytics"></a>分析

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **购置**     </td><td>    可查看产品的[购置](acquisitions-report.md)和[加载项购置](add-on-acquisitions-report.md)报告。        </td><td>    不适用    </td><td>    不适用（父产品设置包括“加载项购置报告”）        </td><td>    不适用                         </td></tr>
    <tr><td align="left">    **使用情况** </td><td>    可查看产品的[使用情况报告](usage-report.md)。     </td><td>    不适用       </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    **运行状况** </td><td>    可查看产品的[运行状况报告](health-report.md)。    </td><td>    不适用     </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    **客户反馈**    </td><td>    可查看产品的[评论](reviews-report.md)和[反馈](feedback-report.md)报告。       </td><td>    不适用（若要响应反馈或评论，必须授予**联系客户**权限）   </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    **Xbox 分析** </td><td>    可查看产品的 Xbox 分析报告。 （注意：此报告尚不可用。）    </td><td>    不适用   </td><td>    不适用       </td><td>    不适用          </td></tr>
    <tr><td align="left">    **真实数据**   </td><td>    可查看产品的实时报告。 （注意：该报告目前仅通过[开发人员中心会员计划](dev-center-insider-program.md)提供。）      </td><td>    不适用   </td><td>    不适用     </td><td>    不适用                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>盈利

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **促销代码**     </td><td>    可查看产品及其加载项的[促销代码](generate-promotional-codes.md)订单和使用信息，并可查看使用情况信息。         </td><td>    可查看、管理和创建产品及其加载项的[促销代码](generate-promotional-codes.md)，还可查看使用信息。          </td><td>    不适用（对于父产品的设置适用于所有加载项）     </td><td>    不适用（对于父产品的设置适用于所有加载项）     </td></tr>
    <tr><td align="left">    **定向优惠**     </td><td>    可查看产品的[定向优惠](use-targeted-offers-to-maximize-engagement-and-conversions.md)。         </td><td>    可查看、管理和创建产品的[定向优惠](use-targeted-offers-to-maximize-engagement-and-conversions.md)。          </td><td>    不适用     </td><td>    不适用      </td></tr>
    <tr><td align="left">    **联系客户**  </td><td>    可查看[客户反馈响应](respond-to-customer-feedback.md)和[客户评论响应](respond-to-customer-reviews.md)，前提是还授予了**客户反馈**权限。 还可查看为产品创建的[目标通知](send-push-notifications-to-your-apps-customers.md)。    </td><td>    可[响应客户反馈](respond-to-customer-feedback.md)和[响应客户评论](respond-to-customer-reviews.md)，前提是还授予了**客户反馈**权限。 还可为产品[创建和发送目标通知](send-push-notifications-to-your-apps-customers.md)。                   </td><td>    不适用         </td><td>    不适用                          </td></tr>
    <tr><td align="left">    **实验**</td><td>    可查看产品的[实验（A/B 测试）](../monetize/run-app-experiments-with-a-b-testing.md)和实验数据。   </td><td>    可为产品创建、管理和查看[实验（A/B 测试）](../monetize/run-app-experiments-with-a-b-testing.md)，还可查看实验数据。     </td><td>    不适用  </td><td>    不适用                 </td></tr>

    </tbody>
    </table>

### <a name="publishing"></a>发布 

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **定价和可用性**  </td><td>    可查看产品提交的[定价和可用性](set-app-pricing-and-availability.md)页面。     </td><td>    可查看和编辑产品提交的[定价和可用性](set-app-pricing-and-availability.md)页面。 </td><td>    可查看加载项提交的[定价和可用性](set-add-on-pricing-and-availability.md)页面。   </td><td>    可查看和编辑加载项提交的[定价和可用性](set-add-on-pricing-and-availability.md)页面。          </td></tr>
    <tr><td align="left">    **属性**   </td><td>    可查看产品提交的[属性](enter-app-properties.md)页面。      </td><td>    可查看和编辑产品提交的[属性](enter-app-properties.md)页面。       </td><td>    可查看加载项提交的[属性](enter-add-on-properties.md)页面。     </td><td>    可查看和编辑加载项提交的[属性](enter-add-on-properties.md)页面。               </td></tr>
    <tr><td align="left">    **年龄分级**    </td><td>    可查看产品提交的[年龄分级](age-ratings.md)页面。       </td><td>    可查看和编辑产品提交的[年龄分级](age-ratings.md)页面。    </td><td>    * 可查看加载项提交的“年龄分级”页面。          </td><td>    * 可查看和编辑加载项提交的“年龄分级”页面。       </td></tr>
    <tr><td align="left">    **程序包**        </td><td>    可查看产品提交的[程序包](upload-app-packages.md)页面。  </td><td>    可查看和编辑产品提交的[程序包](upload-app-packages.md)页面，包括上传程序包。     </td><td>    * 可查看加载项提交的设备系列目标和程序包（如果适用）。   </td><td>    * 可查看和编辑加载项提交的设备系列目标，包括上传程序包（如果适用）。             </td></tr>
    <tr><td align="left">    **应用商店一览**  </td><td>    可查看产品提交的[应用商店一览页面](create-app-store-listings.md)。  </td><td>    可查看和编辑产品提交的[应用商店一览页面](create-app-store-listings.md)，并可添加不同语言的新应用商店一览。     </td><td>    可查看加载项提交的[应用商店一览页面](create-add-on-store-listings.md)。            </td><td>    可查看和编辑加载项提交的[应用商店一览页面](create-add-on-store-listings.md)，并可添加不同语言的应用商店一览。                 </td></tr>
    <tr><td align="left">    **应用商店提交**     </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。           </td><td>    可将产品提交到应用商店，并查看认证报告。 包含全新和更新的提交。 </td><td>如果“无法访问”设置为“只读”，则将授予此权限。     </td><td>    可将加载项提交到应用商店，并查看认证报告。 包含全新和更新的提交。</td></tr>
    <tr><td align="left">    **创建新提交**       </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。        </td><td>    可为产品创建新[提交](app-submissions.md)。  </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。   </td><td>    可为加载项创建新[提交](add-on-submissions.md)。        </td></tr>
    <tr><td align="left">    **新加载项**    </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。 </td><td>    可为产品[创建新加载项](set-your-add-on-product-id.md)。 </td><td>    不适用    </td><td>    不适用        </td></tr>
    <tr><td align="left">    **预留名称**   </td><td>    可查看产品的[管理应用名称](manage-app-names.md)页面。</td><td>    可查看和编辑产品的[管理应用名称](manage-app-names.md)页面，包括预留其他名称和删除预留的名称。 </td><td>   * 可查看加载项的预留名称。    </td><td>   * 可查看和编辑加载项的预留名称。          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **应用频道**&nbsp;\*</td><td>    不适用  </td><td>    可通过 OneGuide 将促销视频频道发布到 Xbox 主机以供观看。  </td><td>  不适用 </td><td> 不适用 </td></tr>
    <tr><td align="left">    **服务配置**&nbsp;\*    </td><td>    可查看与成就、多人游戏、排行榜和产品的其他 Xbox Live 配置相关的设置。  </td><td>    可查看和编辑与成就、多人游戏、排行榜和产品的其他 Xbox Live 配置相关的设置。  </td><td>    不适用     </td><td>    不适用                      </td></tr>
</tbody>
</table>

\* 标有星号 (*) 的权限可将访问权限授予不向所有帐户提供的功能。 如果帐户尚未启用这些功能，则选择这些权限不会有任何效果。  
