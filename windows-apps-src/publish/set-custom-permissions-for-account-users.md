---
author: jnHs
Description: "为帐户用户设置自定义权限。"
title: "为帐户用户设置自定义权限"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 23d8c14bfdbfc05a1397fa67cb831d38ec092233
ms.lasthandoff: 02/08/2017

---

# <a name="set-custom-permissions-for-account-users"></a>为帐户用户设置自定义权限

向帐户添加用户时，可向他们提供[标准角色](manage-account-users.md#roles-and-permissions)，或者可以选择自定义权限，向用户提供相应的访问权限级别。 有些权限适用于整个帐户，有些权限可授予所有产品或限制为授予特定产品。 

若要使用自定义权限而非标准角色，可在添加或编辑用户帐户时在**角色**部分中单击**自定义权限**。 

> **注意**：无论是添加用户、组还是添加 Azure AD 应用程序，均可应用同一权限。

若要为用户启用某项权限，请将框切换为相应设置。 

![访问权限设置指南](images/permission_key.png)

- **无法访问**：用户没有所指示的权限。
- **只读**：用户有权访问与指示区域相关的功能，但不可更改。
- **读/写**：用户有权更改和查看与区域关联的功能。
- **混合**：无法直接选择此选项，但**混合**指示器将显示是否允许使用该权限的访问组合。 例如，如果向**所有产品**的**定价和可用性**授予**只读**访问权限，但随后向特定产品的**定价和可用性**授予**读/写**访问权限，则**所有产品**的**定价和可用性**指示器将显示为“混合”。 如果某些产品**无法访问**某项权限，但其他权限具有**读/写**和/或**只读**访问权限，这也同样适用。

对于某些权限（例如与查看分析数据相关的权限），仅可授予**只读**访问权限。 请注意，在当前实现中，某些权限的**只读**和**读/写**访问权限没有区别。 请查看每项权限的详细信息，了解**只读**和**读/写**访问权限授予的特定功能。

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
<tr><td align="left">    **帐户用户**                       </td><td align="left">  可查看在**管理用户**部分中添加到帐户的用户。          </td><td align="left">  可将用户添加到帐户，并更改**管理用户**部分中的现有用户。             </td></tr>
<tr><td align="left">    **帐户级别的广告性能报告** </td><td align="left">  可查看帐户级的[广告性能报告](advertising-performance-report.md#account-level-advertising-performance-report)。 （无法查看个别产品的广告性能报告，除非单独授予该权限。）       </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    **广告市场活动**                        </td><td align="left">  可查看在帐户中创建的[广告市场活动](create-an-ad-campaign-for-your-app.md)。      </td><td align="left">  可在帐户中创建、管理和查看[广告市场活动](create-an-ad-campaign-for-your-app.md)。          </td></tr>
<tr><td align="left">    **广告中介**                        </td><td align="left">  可查看帐户中所有产品的[广告中介配置](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx)。    </td><td align="left">  可查看和更改帐户中所有产品的[广告中介配置](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx)。        </td></tr>
<tr><td align="left">    **广告中介报告**                </td><td align="left">  可查看帐户中所有产品的[广告中介报告](ad-mediation-report.md)。    </td><td align="left">  不适用    </td></tr>
<tr><td align="left">    **广告性能报告**              </td><td align="left">  可查看帐户中所有产品的[广告性能报告](advertising-performance-report.md)。 （无法查看帐户级别的[广告性能报告](advertising-performance-report.md#account-level-advertising-performance-report)，除非单独授予该权限。）       </td><td align="left">  可查看帐户中所有产品的[广告性能报告](advertising-performance-report.md)。 （无法查看帐户级别的[广告性能报告](advertising-performance-report.md#account-level-advertising-performance-report)，除非单独授予该权限。）         </td></tr>
<tr><td align="left">    **广告单元**                            </td><td align="left">  可查看为帐户创建的[广告单元](monetize-with-ads.md)。    </td><td align="left">  可创建、管理和查看帐户[广告单元](monetize-with-ads.md)。             </td></tr>
<tr><td align="left">    **联盟广告**                       </td><td align="left">  可查看帐户中所有产品的[联盟广告](about-affiliate-ads.md)使用情况。    </td><td align="left">  可管理和查看帐户中所有产品的[联盟广告](about-affiliate-ads.md)使用情况。                </td></tr>
<tr><td align="left">    **联盟性能报告**      </td><td align="left">  可查看帐户中所有产品的[联盟性能报告](affiliates-performance-report.md)。   </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    **应用安装广告报告**             </td><td align="left">  可查看帐户中所有产品的[应用安装广告报告](app-install-ads-reports.md)。           </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    **社区广告**                       </td><td align="left">  可查看帐户中所有产品的免费[社区广告](about-community-ads.md)使用情况。          </td><td align="left">  可创建、管理和查看帐户中所有产品的免费[社区广告](about-community-ads.md)使用情况。               </td></tr>
<tr><td align="left">    **联系人信息**                        </td><td align="left">  可查看“帐户设置”部分中的[联系人信息](managing-your-profile.md)。        </td><td align="left">  可编辑和查看“帐户设置”部分中的[联系人信息](managing-your-profile.md)。            </td></tr>
<tr><td align="left">    **COPPA 合规性**                    </td><td align="left">  可查看帐户中所有产品的 [COPPA 合规性](monetize-with-ads.md#coppa-compliance)选择（指示产品是否面向年龄在 13 岁以下的儿童）。                                            </td><td align="left">  可编辑和查看帐户中所有产品的 [COPPA 合规性](monetize-with-ads.md#coppa-compliance) 选择（指示产品是否面向年龄在 13 岁以下的儿童）。         </td></tr>
<tr><td align="left">    **客户组**                     </td><td align="left">  可查看**客户**部分中的[客户组](create-customer-groups.md)（类别和外部测试版组）。      </td><td align="left">  可创建、编辑和查看**客户**部分中的[客户组](create-customer-groups.md)（类别和外部测试版组）。       </td></tr>
<tr><td align="left">    **新应用**                            </td><td align="left">  可查看新应用创建页面，但实际上无法在帐户中创建新应用。    </td><td align="left">  在帐户中预留新应用名称可[创建新应用](create-your-app-by-reserving-a-name.md)，并可创建新提交，然后将应用提交到应用商店。     </td></tr>
<tr><td align="left">    **新捆绑包**&nbsp;*                       </td><td align="left">  可查看新捆绑包创建页面，但实际上无法在帐户中创建新捆绑包。     </td><td align="left">  可创建产品的新捆绑包。          </td></tr>
<tr><td align="left">    **合作伙伴服务**&nbsp;*                  </td><td align="left">  可查看检索 XTokens 服务的安装证书。     </td><td align="left">  可管理和查看检索 XTokens 服务的安装证书。       </td></tr>
<tr><td align="left">    **付款帐户**                      </td><td align="left">  可在**帐户设置**中查看[付款信息](setting-up-your-payout-account-and-tax-forms.md#payout-account)。     </td><td align="left">  可在**帐户设置**中编辑和查看[付款信息](setting-up-your-payout-account-and-tax-forms.md#payout-account)。       </td></tr>
<tr><td align="left">    **付款摘要**                      </td><td align="left">  可查看[付款摘要](payout-summary.md)，访问并下载付款报告信息。       </td><td align="left">  可查看[付款摘要](payout-summary.md)，访问并下载付款报告信息。   </td></tr>
<tr><td align="left">    **依赖方**&nbsp;*                   </td><td align="left">  可查看依赖方，检索 XTokens。    </td><td align="left">  可管理和查看依赖方，检索 XTokens。     </td></tr>
<tr><td align="left">    **沙盒**&nbsp;*                         </td><td align="left">  可访问**沙盒**页，并查看帐户中的沙盒以及这些沙盒的任何适用配置。 无法查看每个沙盒的产品和提交，除非授予相应的产品级别权限。 </td><td align="left">  可访问**沙盒**页，并查看和管理帐户中的沙盒，包括创建和删除沙盒以及管理它们的配置。 无法查看每个沙盒的产品和提交，除非授予相应的产品级别权限。    </td></tr>
<tr><td align="left">    **税务配置文件**                         </td><td align="left">  可查看**帐户设置**中的[税务配置文件信息和表单](setting-up-your-payout-account-and-tax-forms.md#tax-forms)。     </td><td align="left">  可填写税单和更新**帐户设置**中的[税务配置文件信息](setting-up-your-payout-account-and-tax-forms.md#tax-forms)。     </td></tr>
<tr><td align="left">    **测试帐户**&nbsp;*                     </td><td align="left">  可查看测试 Xbox Live 配置的帐户。      </td><td align="left">  可创建、管理和查看测试 Xbox Live 配置的帐户。      </td></tr>
<tr><td align="left">    **Xbox 设备**                        </td><td align="left">  可在**帐户设置**部分查看为帐户启用的 Xbox 开发主机。       </td><td align="left">  可在**帐户设置**部分查看为帐户添加、删除和启用的 Xbox 开发主机。     </td></tr>
    </tbody>
    </table>

\* 标有星号 (*) 的权限可将访问权限授予不向所有帐户提供的功能。 如果帐户尚未启用这些功能，则选择这些权限不会有任何效果。   

## <a name="product-level-permissions"></a>产品级别的权限

本部分中的权限可授予帐户中所有产品，或者进行自定义，支持仅向一个或多个特定产品授予权限。 这些权限分为四个类别：**分析**、**盈利**、**发布**和 **Xbox Live**。 可展开这些类别，查看每个类别中的各个权限。 

若要向帐户中所有产品授予权限，请在标为**所有产品**的行中选择该权限（切换框，指示**只读**、**读/写**或**无法访问**）。 
 
> **使用技巧**：为**所有产品**选择的内容将应用到当前在帐户中的所有产品和在帐户中创建的任何未来产品。

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
    <tr><td align="left">    **客户反馈**    </td><td>    可查看产品的[分级](ratings-report.md)、[评论](reviews-report.md)和[反馈](feedback-report.md)报告。       </td><td>    不适用（若要响应反馈或评论，必须授予**联系客户**权限）   </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    **Xbox 分析** </td><td>    可查看产品的 Xbox 分析报告。 （注意：此报告尚不可用。）    </td><td>    不适用   </td><td>    不适用       </td><td>    不适用          </td></tr>
    <tr><td align="left">    **真实数据**   </td><td>    可查看产品的实时报告。       </td><td>    不适用   </td><td>    不适用     </td><td>    不适用                 </td></tr>
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
    <tr><td align="left">    **联系客户**  </td><td>    可查看[客户反馈响应](respond-to-customer-feedback.md)和[客户评论响应](respond-to-customer-reviews.md)，前提是还授予了**客户反馈**权限。 还可查看为产品创建的[目标通知](send-push-notifications-to-your-apps-customers.md)。    </td><td>    可[响应客户反馈](respond-to-customer-feedback.md)和[响应客户评论](respond-to-customer-reviews.md)，前提是还授予了**客户反馈**权限。 还可为产品[创建和发送目标通知](send-push-notifications-to-your-apps-customers.md)。                   </td><td>    不适用         </td><td>    不适用                          </td></tr>
    <tr><td align="left">    **实验**</td><td>    可查看产品的[实验（A/B 测试）](../monetize/run-app-experiments-with-a-b-testing.md)和实验数据。   </td><td>    可为产品创建、管理和查看[实验（A/B 测试）](../monetize/run-app-experiments-with-a-b-testing.md)，还可查看实验数据。     </td><td>    不适用  </td><td>    不适用                 </td></tr>
    <tr><td align="left">    **促销代码**     </td><td>    可查看产品及其加载项的[促销代码](generate-promotional-codes.md)订单和使用信息，并可查看使用情况信息。         </td><td>    可查看、管理和创建产品及其加载项的[促销代码](generate-promotional-codes.md)，还可查看使用信息。          </td><td>    不适用（对于父产品的设置适用于所有加载项）     </td><td>    不适用（对于父产品的设置适用于所有加载项）     </td></tr>
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
    <tr><td align="left">    **Xbox 服务配置**&nbsp;\*    </td><td>    可查看与成就、多人游戏、排行榜和产品的其他 Xbox Live 配置相关的设置。  </td><td>    可查看和编辑与成就、多人游戏、排行榜和产品的其他 Xbox Live 配置相关的设置。  </td><td>    不适用     </td><td>    不适用                      </td></tr>
    <tr><td align="left">    **应用频道**&nbsp;\*</td><td>    不适用  </td><td>    可通过 OneGuide 将促销视频频道发布到 Xbox 主机以供观看。  </td><td>  不适用 </td><td> 不适用 </td></tr>
</tbody>
</table>

\* 标有星号 (*) 的权限可将访问权限授予不向所有帐户提供的功能。 如果帐户尚未启用这些功能，则选择这些权限不会有任何效果。  

