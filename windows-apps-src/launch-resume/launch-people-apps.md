---
author: TylerMSFT
title: "启动“人脉”应用"
description: "本主题介绍了 ms-people URI 方案。 你的应用可以使用此 URI 方案来针对特定操作启动“人脉”应用。"
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 25abea87eaf374a1a8b5432c522d51bd7bbc1c1e
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-people-app"></a>启动“人脉”应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题介绍了 **ms-people:** URI 方案。 你的应用可以使用此 URI 方案来针对特定操作启动“人脉”应用。

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
<td align="left">允许其他应用通过搜索的结果页提供将启动“人脉”应用的搜索字符串。
<div class="alert">
**注意**  
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
**注意**  
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
**注意**  
<p>参数区分大小写。</p>
<p>参数的顺序无关紧要。</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
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
<td align="left">**SearchString**</td>
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
<td align="left">**ContactId**</td>
<td align="left"><p>可选。</p>
<p>联系人的联系人 ID。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>可选。</p>
<p>联系人的电话号码。</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**电子邮件**</td>
<td align="left"><p>可选。</p>
<p>联系人的电子邮件。</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>可选。</p>
<p>联系人的姓名。</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**联系人**</td>
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
<td align="left">**PhoneNumber**</td>
<td align="left"><p>可选。</p>
<p>联系人的电话号码。</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**电子邮件**</td>
<td align="left"><p>可选。</p>
<p>联系人的电子邮件。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>可选。</p>
<p>联系人的姓名。</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 

