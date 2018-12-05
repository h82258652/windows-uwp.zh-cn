---
title: 启动“人脉”应用
description: 本主题介绍了 ms-people URI 方案。 你的应用可以使用此 URI 方案来针对特定操作启动“人脉”应用。
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46a297c3a611882724b18242d1c6272c3345ffc2
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691489"
---
# <a name="launch-the-people-app"></a>启动“人脉”应用

本主题介绍 **ms-people:** URI 方案。 你的应用可以使用此 URI 方案来针对特定操作启动“人脉”应用。

## <a name="ms-people-uri-scheme-reference"></a>ms-people: URI 方案引用

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">结果</th>
<th align="left">URI 方案</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">允许其他应用启动“人脉”应用主页。</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">允许其他应用启动“人脉”应用设置页。</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">允许其他应用通过搜索的结果页提供会启动“人脉”应用的搜索字符串。
<div class="alert">
<p>参数区分大小写。</p>
<p>如果未正确输入语法或缺少搜索字符串值，默认行为是返回未经过任何筛选的联系人的完整列表。</p>
</div>
<div>
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">如果找到该联系人，将启动到现有的联系人卡片。 或者，如果未找到联系人，将启动到临时的联系人卡片。 如果未提供输入参数，我们将启动带有联系人列表的“人脉”应用。
<div class="alert">
<p>参数区分大小写。</p>
<p>参数的顺序无关紧要。</p>
<p>如果存在多个匹配，我们将返回匹配的第一个联系人。</p>
</div>
<div> 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">启动到“人脉”应用内的保存联系人页面，以使用提供的电话号码或电子邮件地址保存给定的联系人。
<div class="alert">
<p>参数区分大小写。</p>
<p>参数的顺序无关紧要。</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
<tr class="even">
<td align="left">启动以在“人脉”应用中添加新联系人页面，用于保存给定联系人。
<div class="alert"><p>使用 <a href="https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a> 打开“保存新联系人”页面。 使用 <strong>LaunchUriAsync</strong> 将只启动“人脉”应用的主页。</p>
<p>参数区分大小写。</p>
<p>参数的顺序无关紧要。</p>
<p>你可以使用受支持参数的任意组合。</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savecontacttask?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesearch-parameter-reference"></a>ms-people:search: 参数引用

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">说明</th>
<th align="left">示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>可选。</p>
<p>联系人搜索信息的搜索字符串。</p>
<p>电话号码或联系人姓名。</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peopleviewcontact-parameter-reference"></a>ms-people:viewcontact: 参数引用

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">说明</th>
<th align="left">示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>可选。</p>
<p>联系人的联系人 ID。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>可选。</p>
<p>联系人的电话号码。</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>电子邮件</b></td>
<td align="left"><p>可选。</p>
<p>联系人的电子邮件。</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>可选。</p>
<p>联系人的姓名。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>联系人</b></td>
<td align="left"><p>可选。</p>
<p>Contact 对象。</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavetocontact-parameter-reference"></a>ms-people:savetocontact: 参数引用

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">说明</th>
<th align="left">示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>可选。</p>
<p>联系人的电话号码。</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>电子邮件</b></td>
<td align="left"><p>可选。</p>
<p>联系人的电子邮件。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>可选。</p>
<p>联系人的姓名。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>ms-people:savecontacttask: 参数引用

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">说明</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Company</b></td>
<td align="left"><p>可选。</p>
<p>联系人的公司名称。</p></td>

</tr>
<tr class="even">
<td align="left"><b>FirstName</b></td>
<td align="left"><p>可选。</p>
<p>联系人的名字。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>可选。</p>
<p>家庭地址所在城市。</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>可选。</p>
<p>家庭地址所在国家/地区。</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>可选。</p>
<p>家庭地址所在州。</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>可选。</p>
<p>家庭地址所在街道。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>可选。</p>
<p>家庭地址的邮政编码。</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomePhone</b></td>
<td align="left"><p>可选。</p>
<p>联系人的家庭电话。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>可选。</p>
<p>联系人的职务。</p></td>
</tr>

<tr class="even">
<td align="left"><b>LastName</b></td>
<td align="left"><p>可选。</p>
<p>联系人的姓氏。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>MiddleName</b></td>
<td align="left"><p>可选。</p>
<p>联系人的中间名。</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>可选。</p>
<p>联系人的移动电话号码。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Nickname</b></td>
<td align="left"><p>可选。</p>
<p>联系人的昵称。</p></td>
</tr>

<tr class="even">
<td align="left"><b>Notes</b></td>
<td align="left"><p>可选。</p>
<p>关于联系人的备注。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>可选。</p>
<p>联系人的其他电子邮件。</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>可选。</p>
<p>联系人的个人电子邮件。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffix</b></td>
<td align="left"><p>可选。</p>
<p>联系人的后缀。</p></td>
</tr>

<tr class="even">
<td align="left"><b>Title</b></td>
<td align="left"><p>可选。</p>
<p>联系人的职务。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Website</b></td>
<td align="left"><p>可选。</p>
<p>联系人的网站。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>可选。</p>
<p>工作地址所在城市。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>可选。</p>
<p>工作地址所在国家/地区。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>可选。</p>
<p>工作地址所在州。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>可选。</p>
<p>工作地址所在街道。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>可选。</p>
<p>工作地址的邮政编码。</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkEmail</b></td>
<td align="left"><p>可选。</p>
<p>联系人的工作电子邮件。</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkPhone</b></td>
<td align="left"><p>可选。</p>
<p>联系人的工作电话号码。</p></td>
</tr>
</tbody>
</table>
