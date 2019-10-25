---
Description: 你可以设置应用在应用商店中应变为可用的准确日期和时间，从而为你提供更大的灵活性并为不同市场自定义日期。
title: 配置精确发布计划
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10，uwp，计划，发布日期，日期，启动
ms.localizationpriority: medium
ms.openlocfilehash: ec9ee00aaa350fc48185cc6328674ac2d8f62ea5
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690356"
---
# <a name="configure-precise-release-scheduling"></a>配置精确发布计划

使用 "[定价和可用性](set-app-pricing-and-availability.md)" 页上的 "**计划**" 部分，可以设置应用在应用商店中应变为可用的准确日期和时间，从而提供更高的灵活性以及为不同市场自定义日期的能力。

> [!NOTE]
> 尽管本主题指的是应用程序，但外接程序提交的版本计划使用相同的过程。

此外，还可以选择设置产品在商店中不再可用的日期。 请注意，这意味着在商店中不能再通过搜索或浏览找到该产品，但任何具有直接链接的客户都可以看到产品的商店列表。 如果他们已拥有该产品，或者他们有[促销代码](generate-promotional-codes.md)并使用 Windows 10 设备，则他们只能下载该产品。

默认情况下（除非已在 "[可见性](choose-visibility-options.md#discoverability)" 部分的 "存储" 选项中选择了 "**使此应用可供使用，但无法发现**" 选项），则当客户通过认证并完成发布时，你的应用将立即可供客户使用正在. 若要选择其他日期，请选择 "**显示选项**" 以展开此部分。

请注意，如果在 "[可见性](choose-visibility-options.md#discoverability)" 部分的 "存储" 选项中选择了 "**使此应用程序可供使用，但无法发现**" 选项之一，则你将无法在 "**计划**" 部分中配置日期，因为你的应用程序不会被释放到客户，因此没有要配置的发布日期。

> [!IMPORTANT]
> 您在 "计划" 部分中指定的日期仅适用于 Windows 10 上的客户。
>
>如果你以前发布的应用支持早期版本的操作系统，则你选择的任何 "**停止购置**" 日期将不会应用于这些客户;它们仍将能够获取应用（除非你在 "[可见性](choose-visibility-options.md#discoverability)" 部分中使用新选择提交更新，或者在 "**应用概述**" 页中选择 "**使应用不可用**"）。


## <a name="base-schedule"></a>基本计划

你为基本计划做出的选择将适用于你的应用程序可用的所有市场，除非你稍后通过选择 "[自定义" 作为特定市场](#customize-the-schedule-for-specific-markets)来添加特定市场（或市场组）的日期。

你将在此处看到两个选项： "**发布**" 和 "**停止获取**"。 

## <a name="release"></a>发布

在 "**发布**" 下拉箭头中，可以设置希望应用在应用商店中可用的时间。 这意味着在应用程序中通过搜索或浏览可发现应用，并且客户可以查看其商店列表并获取应用。

>[!NOTE]
> 在应用程序发布并在应用商店中可用后，你将无法再选择**发布**日期（因为应用已发布）。

以下是可以为产品**发布**计划配置的选项：
- **尽快：在**认证和发布产品后，产品会立即发布。 这是默认选项。
- **在**：产品将在你选择的日期和时间发布。 另外还有两个选项：
   - **UTC**：你选择的时间将是协调世界时（UTC）的时间，因此应用程序可以在任何位置的同一时间发布。
   - **本地**：你选择的时间将是与市场相关联的每个时区中使用的。 （请注意，对于包含多个时区的市场，只会使用市场中的一个时区。 对于美国，使用东部时区。 此页面向下显示了时区的综合性列表。）
- **未计划**：应用商店中将无法使用该应用。 如果选择此选项，稍后可以通过创建新的提交并选择其他选项之一来使应用在应用商店中可用。


## <a name="stop-acquisition"></a>停止购置

在 "**停止获取**" 下拉列表中，你可以设置一个日期和时间，以允许新客户从存储中获取它或发现其列表。 如果你想要精确控制何时不再向新客户提供应用（例如，当你在多个应用间协调可用性时），这会很有用。

默认情况下，"**停止购置**" 设置为 "从不"。 若要更改**此项，请在下拉菜单**中选择 ""，然后按如上所述指定日期和时间。 在您选择的日期和时间，客户将无法再获取该应用程序。

必须了解的是，此选项的影响与选择 "**使此应用程序可发现但**在[可见性](choose-visibility-options.md#discoverability)" 部分中不可用，并选择 "**停止获取"：任何具有直接链接的客户都可以看到产品的存储列出，但只有在其之前拥有产品，或者有促销代码并使用 Windows 10 设备的情况**下，才能下载。 若要完全停止向新客户提供应用，请在 "应用概述" 页上单击 "**使应用不可用**"。 有关详细信息，请参阅[从应用商店中删除应用](guidance-for-app-package-management.md#removing-an-app-from-the-store)。

> [!TIP]
> 如果选择一个日期来**停止获取**，稍后决定要使该应用再次可用，则可以创建新的提交，并将**停止获取**更改回 "**从不**"。 发布更新的提交后，应用将再次变为可用。

## <a name="customize-the-schedule-for-specific-markets"></a>为特定市场自定义计划 

默认情况下，你选择的选项将应用于提供应用的所有市场。 若要自定义特定市场的价格，请单击 "**自定义特定市场**"。 此时将显示 "**市场选择**" 弹出窗口，其中列出了你选择使应用可用的所有市场。 如果你在[市场](define-pricing-and-market-selection.md)部分中排除了任何市场，则不会显示这些市场。 

若要为一个市场添加计划，请选择它，然后单击 "**保存**"。 然后，你将看到上文所述的相同**发布**和**停止收购**选项，但所做的选择仅适用于该市场。

要添加将应用于多个市场的计划，你将创建一个*市场组*。 为此，请选择想要包括的市场，然后输入组的名称。 （此名称仅供参考，任何客户都不可见。）例如，如果想要为北美创建市场组，则可以选择 "**加拿大**"、"**墨西哥**" 和 "**美国**"，然后将其命名为**北美**或所选的其他名称。 完成创建市场组后，单击 "**保存**"。 然后，你将看到上述相同的**发布**和**停止收购**选项，但你所做的选择将仅适用于该市场组。

若要为其他市场或其他市场组添加自定义计划，只需再次单击 "**自定义特定市场**" 并重复这些步骤。 若要更改市场组中包含的市场，请选择其名称。 若要删除市场组（或单个市场）的自定义计划，请单击 "**删除**"。

> [!NOTE]
> 市场不能属于**计划**部分中使用的多个市场组。 

## <a name="global-time-zones"></a>全局时区

下面是一个表格，其中显示了每个市场中使用了哪些特定时区，因此当你的提交使用当地时间（例如，在本地上午9点发布）时，可以找出在每个市场中发布的时间，特别适用于具有多个时间 z 的市场一种，如加拿大。

<a name="market--time-zone"></a>市场：时区
==================
阿富汗：（UTC + 04：30）喀布尔阿尔巴尼亚：（UTC + 01：00）萨拉热窝，斯科普里，华沙，萨格勒布阿尔及利亚：（UTC + 01：00）萨拉热窝，斯科普里，华沙，萨格勒布美洲萨摩亚：（utc + 01：00）萨摩亚安道尔：（UTC + 01：00）萨拉热窝，斯科普里，华沙，萨格勒布安哥拉：（UTC + 01：00）西部中央非洲安圭拉：（UTC-04:00）大西洋时间（加拿大）南极洲：（UTC + 12：00）奥克兰，惠灵顿安提瓜和巴布达：（UTC-04:00）大西洋时间（加拿大）阿根廷：（utc-03:00）市市，布宜诺斯艾利斯亚美尼亚：（UTC + 04：00）阿尔阿布扎比，马斯喀特 Aruba：（UTC-04:00）大西洋时间（加拿大）澳大利亚：（UTC + 10：00）堪培拉，墨尔本，悉尼奥地利：（UTC + 01：00）阿姆斯特丹，柏林，伯尔尼，罗马，斯德哥尔摩，维也纳阿塞拜疆：（UTC + 04：00）巴库巴哈马，：（utc-05:00）东部时间（美国 &）巴林：（UTC+ 04:00）阿尔阿布扎比，马斯喀特孟加拉国：（UTC + 06：00）达卡巴巴多斯：（UTC-04:00）大西洋时间（加拿大）白俄罗斯：（UTC + 03：00）明斯克：（utc + 01：00）布鲁塞尔，哥本哈根，马德里，巴黎伯利兹：（UTC-06:00）中部时间（美国 & 加拿大）贝宁：（UTC + 01：00）西亚非洲百慕大群岛：（UTC-04:00）大西洋时间（加拿大）不丹：（UTC + 06：00）达卡委内瑞拉玻利瓦尔共和国：（UTC-04:00）加拉加斯玻利维亚：（UTC-04:00）乔治敦，La 巴斯，马瑙斯，San Juan 博内尔岛，圣歇斯和萨巴岛：（UTC-04:00）大西洋时间（加拿大）波斯尼亚和黑塞哥维那：（UTC + 01：00）萨拉热窝，斯科普里，华沙，萨格勒布博茨瓦纳：（UTC + 01：00）中北部非洲布韦岛：（UTC + 00：00）蒙罗维亚，雷克雅未克巴西：（UTC-03:00）巴西利亚英属印度洋领地：（UTC + 06：00）达卡英属维尔京群岛：（UTC-04:00）大西洋时间（加拿大）文莱：（utc + 02：00）伊尔库茨克保加利亚：（utc + 02：00）蒙罗维亚，雷克雅未克布隆迪：（UTC + 02：00）哈拉雷，比勒陀利亚 CÃ́te 科特迪瓦：（UTC + 00：00）蒙罗维亚，雷克雅未克柬埔寨：（UTC + 07：00）曼谷，河内，雅加达喀麦隆：（UTC + 01：00）中非西部加拿大：（UTC-05:00）东部时间（美国 & 加拿大）佛得角：（UTC-01:00）佛得角
Cayman Islands: (UTC-05:00) Eastern Time (US & Canada) Central African Republic: (UTC+01:00) West Central Africa Chad: (UTC+01:00) West Central Africa Chile: (UTC-04:00) Santiago China: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Christmas Island: (UTC+07:00) Krasnoyarsk Cocos (Keeling) Islands: (UTC+06:30) Yangon (Rangoon) Colombia: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Comoros: (UTC+03:00) Nairobi Congo: (UTC+01:00) West Central Africa Congo (DRC): (UTC+01:00) West Central Africa Cook Islands: (UTC-10:00) Hawaii Costa Rica: (UTC-06:00) Central Time (US & Canada) Croatia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb CuraÃ§ao: (UTC-04:00) Cuiaba Cyprus: (UTC+02:00) Chisinau Czech Republic: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Denmark: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Djibouti: (UTC+03:00) Nairobi Dominica: (UTC-04:00) Atlantic Time (Canada) Dominican Republic: (UTC-04:00) Atlantic Time (Canada) Ecuador: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Egypt: (UTC+02:00) Chisinau El Salvador: (UTC-06:00) Central Time (US & Canada) Equatorial Guinea: (UTC+01:00) West Central Africa Eritrea: (UTC+03:00) Nairobi Estonia: (UTC+02:00) Chisinau Ethiopia: (UTC+03:00) Nairobi Falkland Islands (Islas Malvinas): (UTC-04:00) Santiago Faroe Islands: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Fiji: (UTC+12:00) Fiji Finland: (UTC+02:00) Helsinki, Kyiv, Riga, Sofia, Tallinn, Vilnius France: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris French Guiana: (UTC-03:00) Cayenne, Fortaleza French Polynesia: (UTC-10:00) Hawaii French Southern and Antarctic Lands: (UTC+05:00) Ashgabat, Tashkent Gabon: (UTC+01:00) West Central Africa Gambia, The: (UTC+00:00) Monrovia, Reykjavik Georgia: (UTC-05:00) Eastern Time (US & Canada) Germany: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Ghana: (UTC+00:00) Monrovia, Reykjavik Gibraltar: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Greece: (UTC+02:00) Athens, Bucharest Greenland: (UTC+00:00) Monrovia, Reykjavik Grenada: (UTC-04:00) Atlantic Time (Canada) Guadeloupe: (UTC-04:00) Atlantic Time (Canada) Guam: (UTC+10:00) Guam, Port Moresby Guatemala: (UTC-06:00) Central Time (US & Canada) Guernsey: (UTC+00:00) Monrovia, Reykjavik Guinea: (UTC+00:00) Monrovia, Reykjavik Guinea-Bissau: (UTC+00:00) Monrovia, Reykjavik Guyana: (UTC-04:00) Atlantic Time (Canada) Haiti: (UTC-05:00) Eastern Time (US & Canada) Heard Island and McDonald Islands: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Holy See (Vatican City): (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Honduras: (UTC-06:00) Central Time (US & Canada) Hong Kong SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Hungary: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Iceland: (UTC+00:00) Monrovia, Reykjavik India: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Indonesia: (UTC+07:00) Bangkok, Hanoi, Jakarta Iraq: (UTC+04:00) Abu Dhabi, Muscat Ireland: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Israel: (UTC+02:00) Jerusalem Italy: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Jamaica: (UTC-05:00) Eastern Time (US & Canada) Japan: (UTC+09:00) Osaka, Sapporo, Tokyo Jersey: (UTC+00:00) Monrovia, Reykjavik Jordan: (UTC+02:00) Chisinau Kazakhstan: (UTC+05:00) Ashgabat, Tashkent Kenya: (UTC+03:00) Nairobi Kiribati: (UTC+14:00) Kiritimati Island Korea: (UTC+09:00) Seoul Kuwait: (UTC+04:00) Abu Dhabi, Muscat Kyrgyzstan: (UTC+06:00) Astana Laos: (UTC+07:00) Bangkok, Hanoi, Jakarta Latvia: (UTC+02:00) Chisinau Lebanon: (UTC+02:00) Chisinau Lesotho: (UTC+02:00) Harare, Pretoria Liberia: (UTC+00:00) Monrovia, Reykjavik Libya: (UTC+02:00) Chisinau Liechtenstein: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Lithuania: (UTC+02:00) Chisinau Luxembourg: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Macao SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Macedonia, FYROM: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Madagascar: (UTC+03:00) Nairobi Malawi: (UTC+02:00) Harare, Pretoria Malaysia: (UTC+08:00) Kuala Lumpur, Singapore Maldives: (UTC+05:00) Ashgabat, Tashkent Mali: (UTC+00:00) Monrovia, Reykjavik Malta: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Man, Isle of: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Marshall Islands: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Martinique: (UTC-04:00) Atlantic Time (Canada) Mauritania: (UTC+00:00) Monrovia, Reykjavik Mauritius: (UTC+04:00) Port Louis Mayotte: (UTC+03:00) Nairobi Mexico: (UTC-06:00) Guadalajara, Mexico City, Monterrey Micronesia: (UTC+10:00) Guam, Port Moresby Moldova: (UTC+02:00) Chisinau Monaco: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Mongolia: (UTC+07:00) Krasnoyarsk Montenegro: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Montserrat: (UTC-04:00) Atlantic Time (Canada) Morocco: (UTC+01:00) Casablanca Mozambique: (UTC+02:00) Harare, Pretoria Myanmar: (UTC+06:30) Yangon (Rangoon) Namibia: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Nauru: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Nepal: (UTC+05:45) Kathmandu Netherlands: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna New Caledonia: (UTC+11:00) Solomon Is., New Caledonia New Zealand: (UTC+12:00) Auckland, Wellington Nicaragua: (UTC-06:00) Central Time (US & Canada) Niger: (UTC+01:00) West Central Africa Nigeria: (UTC+01:00) West Central Africa Niue: (UTC+13:00) Samoa Norfolk Island: (UTC+11:00) Solomon Is., New Caledonia Northern Mariana Islands: (UTC+10:00) Guam, Port Moresby Norway: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Oman: (UTC+04:00) Abu Dhabi, Muscat Pakistan: (UTC+05:00) Islamabad, Karachi Palau: (UTC+09:00) Osaka, Sapporo, Tokyo Palestinian Authority: (UTC+02:00) Chisinau Panama: (UTC-05:00) Eastern Time (US & Canada) Papua New Guinea: (UTC+10:00) Vladivostok Paraguay: (UTC-04:00) Asuncion Peru: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Philippines: (UTC+08:00) Kuala Lumpur, Singapore Pitcairn Islands: (UTC-08:00) Pacific Time (US & Canada) Poland: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Portugal: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Qatar: (UTC+04:00) Abu Dhabi, Muscat Reunion: (UTC+04:00) Port Louis Romania: (UTC+02:00) Chisinau ROW: (UTC-07:00) Mountain Time (US & Canada) Russia: (UTC+03:00) Moscow, St. Petersburg Rwanda: (UTC+02:00) Harare, Pretoria SÃ£o TomÃ© and PrÃ­ncipe: (UTC+00:00) Monrovia, Reykjavik Saint BarthÃ©lemy: (UTC+04:00) Yerevan Saint Helena, Ascension and Tristan da Cunha: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Saint Kitts and Nevis: (UTC-04:00) Atlantic Time (Canada) Saint Lucia: (UTC-04:00) Atlantic Time (Canada) Saint Martin (French Part): (UTC-04:00) Atlantic Time (Canada) Saint Pierre and Miquelon: (UTC-02:00) Mid-Atlantic - Old Saint Vincent and the Grenadines: (UTC-04:00) Atlantic Time (Canada) Samoa: (UTC+13:00) Samoa San Marino: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Saudi Arabia: (UTC+03:00) Kuwait, Riyadh Senegal: (UTC+00:00) Monrovia, Reykjavik Serbia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Seychelles: (UTC+04:00) Abu Dhabi, Muscat Sierra Leone: (UTC+00:00) Monrovia, Reykjavik Singapore: (UTC+08:00) Kuala Lumpur, Singapore Sint Maarten (Dutch Part): (UTC-04:00) Atlantic Time (Canada) Slovakia: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Slovenia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Solomon Islands: (UTC+11:00) Solomon Is., New Caledonia Somalia: (UTC+03:00) Nairobi South Africa: (UTC+02:00) Harare, Pretoria South Georgia and the South Sandwich Islands: (UTC-02:00) Mid-Atlantic - Old Spain: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Sri Lanka: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Suriname: (UTC-03:00) Cayenne, Fortaleza Svalbard and Jan Mayen: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Swaziland: (UTC+02:00) Harare, Pretoria Sweden: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Switzerland: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Taiwan: (UTC+08:00) Taipei Tajikistan: (UTC+05:00) Ashgabat, Tashkent Tanzania: (UTC+03:00) Nairobi Thailand: (UTC+07:00) Bangkok, Hanoi, Jakarta Timor-Leste: (UTC+09:00) Seoul Togo: (UTC+00:00) Monrovia, Reykjavik Tokelau: (UTC+13:00) Nuku'alofa Tonga: (UTC+13:00) Nuku'alofa Trinidad and Tobago: (UTC-04:00) Atlantic Time (Canada) Tunisia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Turkey: (UTC+03:00) Istanbul Turkmenistan: (UTC+05:00) Ashgabat, Tashkent Turks and Caicos Islands: (UTC-05:00) Eastern Time (US & Canada) Tuvalu: (UTC+12:00) Petropavlovsk-Kamchatsky - Old U.S. Minor Outlying Islands: (UTC+13:00) Samoa U.S. Virgin Islands: (UTC-04:00) Atlantic Time (Canada) Uganda: (UTC+03:00) Nairobi Ukraine: (UTC+02:00) Chisinau United Arab Emirates: (UTC+04:00) Abu Dhabi, Muscat United Kingdom: (UTC+00:00) Dublin, Edinburgh, Lisbon, London United States: (UTC-05:00) Eastern Time (US & Canada) Uruguay: (UTC-03:00) Brasilia Uzbekistan: (UTC+05:00) Ashgabat, Tashkent Vanuatu: (UTC+11:00) Solomon Is., New Caledonia Vietnam: (UTC+07:00) Bangkok, Hanoi, Jakarta Wallis and Futuna: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Western Sahara (Disputed): (UTC+00:00) Dublin, Edinburgh, Lisbon, London Yemen: (UTC+04:00) Abu Dhabi, Muscat Zambia: (UTC+02:00) Harare, Pretoria Zimbabwe: (UTC+02:00) Harare, Pretoria
