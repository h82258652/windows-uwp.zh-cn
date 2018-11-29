---
title: 使用适用于 UWP 游戏的云服务
description: 了解有关实现云作为 UWP 游戏后端的详细信息。
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.date: 03/27/2018
ms.topic: article
keywords: windows 10, uwp, 游戏, 云服务
ms.localizationpriority: medium
ms.openlocfilehash: 12f9125b4a128b754fc90c48e6a289cec40a7f54
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8200307"
---
#  <a name="using-cloud-services-for-uwp-games"></a>使用适用于 UWP 游戏的云服务

Windows10 中的通用 Windows 平台 (UWP) 提供的一组 API 可用于开发跨 Microsoft 设备的游戏。 当开发跨平台和设备的游戏时，可以充分利用云后端来帮助你根据需求扩展游戏。

如果你在为自己的游戏寻找完整的云后端解决方案，请参阅[适用于游戏后端的软件即服务](#software-as-a-service-for-game-backend)。

##  <a name="what-is-cloud-computing"></a>什么是云计算？

云计算基于 Internet 按需使用 IT 资源和应用程序，来存储和处理设备的数据。 术语_云_是在 Internet 上提供大量资源（非本地资源）的一种比喻说法，可供你从任意位置进行访问。
云计算的本质是提供一种可使用资源和软件的全新方式。 用户不再需要事先购买整套产品和资源，只需要能够使用作为服务提供的平台、软件和资源即可。 云提供商通常根据其客户的使用情况或服务计划产品向他们收取费用。

##  <a name="why-use-cloud-services"></a>为什么要使用云服务？

使用适用于游戏的云服务的一个好处是，事先无需在物理硬件服务器方面投入资金，只需事后根据使用情况或服务计划支付费用即可。 它是有助于管理在开发全新游戏时所涉及风险的一种方法。 

另一个好处是，你的游戏可以接入大量云资源来实现可扩展性（有效管理大量同时在线玩家、频繁实时游戏计算或数据需求导致的任何突发峰值）。 这将全天候确保游戏性能的稳定状态。 此外，可以通过在全球任意一处的任意平台上运行的任何设备访问云资源，这意味着你可以向全球每一位用户提供你的游戏。

向你的游戏玩家呈现令人惊叹的游戏可玩性体验非常重要。 由于云中运行的游戏服务器独立于客户端更新，因此它们可以全面给予你对游戏环境的更多控制，同时确保环境安全。   还可以通过从不信任客户端并执行服务器端游戏逻辑，来通过云实现游戏玩法的一致性。 服务到服务连接还可以配置为允许更为集中的游戏体验；示例包括将游戏内购买链接到各种付款方式、通过不同游戏网络进行桥接以及将游戏内更新分享到热门社交媒体门户（如 Facebook 和 Twitter）。 

还可以使用专用云服务器创建大型的持久游戏世界、构建游戏玩家社区、收集并分析随时间推移的游戏玩家数据，以提升游戏可玩性，并对你的游戏盈利设计模型进行优化。

此外，可以使用云服务实现需要具备密集游戏数据管理功能的游戏，如具有多玩家异步机制的社交游戏。

##  <a name="how-game-companies-use-the-cloud-technology"></a>游戏公司以什么方式使用云技术

了解其他开发商在其游戏中实现云解决方案的方式。

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>开发商</th>
        <th>描述</th>
        <th>主要游戏方案</th>
        <th>了解详细信息</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent Games</a></td>
        <td><b>Tencent Games</b> 使用 Azure Service Fabric 开发了一项创新型解决方案，能够以服务形式提供传统电脑游戏。 这项云游戏解决方案采用“瘦客户端 + 厚云”模式，可在后台以微服务形式运行工作负荷。</td>
        <td>
            <ul>
                <li>传统电脑游戏会作为云游戏提供给世界各地的用户 <li>优化了游戏交付过程 <li>游戏功能被隔离为微服务，以降低复杂性、减少因依赖性而导致的重复工作负荷，并能够独立升级新功能 <li>下载小型安装包以播放最新的游戏内容（安装包大小从 GB 级降低到 MB 级） <li>降低了维护成本 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent Games 和 Microsoft 生成了云游戏解决方案</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">使用 Service Fabric 生成游戏：有关腾讯的实现方式的详细信息（视频）</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>光环 5：守护者</b>（通过 DocumentDB API）使用 Azure Cosmos DB 将<a href="https://www.halowaypoint.com/spartan-companies">光环：斯巴达连队</a>作为其社交游戏平台实现，选择 Azure Cosmos DB 的原因是其自动索引功能带来的速度和灵活性。</td>
        <td>
            <ul>
                <li>用于处理多玩家游戏玩法的组创建/管理的可扩展数据层 <li>游戏和社交媒体集成 <li>通过多种属性实时查询数据 <li>同步游戏玩法成就和统计数据 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">（通过 DocumentDB API）使用 Azure Cosmos DB 实现了社交游戏玩法</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games 制作的<b>崛起时代</b>，是一款可以在装有现代浏览器的设备上运行的大型多人在线 (MMO) 史诗般 3D 空间游戏。 因此可以在电脑、笔记本电脑、手机和其他移动设备上玩此款游戏，无需安装任何插件。该游戏使用 ASP.NET Core、HTML5、WebGL 和 Azure。</td>
        <td>
            <ul>
                <li>基于浏览器的跨平台游戏 <li>一个大型的持久开放世界 <li>处理密集型实时游戏玩法计算 <li>扩展玩家的数量 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">使用 Service Fabric 生成游戏：《崛起时代》MMO 游戏（视频）</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">使用 Azure Service Fabric 以微服务形式管理游戏组件（视频）</a> 
                <li><a href="https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">采访《崛起时代》开发人员（视频）</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.nextgames.com/">Next Games</a></td>
        <td>Next Games 是<b>行尸走肉：无人之地</b>视频游戏的制作者，该游戏改编自 AMC 的原创系列。 《行尸走肉》游戏后端使用的是 Azure。 这款游戏在开放的首个周末和第一周内达到 1000000 下载量，在美国 App Store 中成为 iPhone 和 iPad 免费应用第一名、12 个国家/地区中免费应用第一名和 13 个国家/地区中免费游戏第一名。
        </td>
        <td>
            <ul>
                <li>跨平台 <li>供多名玩家参与的回合制策略游戏 <li>扩展性能灵活自如 <li>玩家欺诈保护 <li>动态内容传递 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">我们的生成方式：利用 Azure 上的 Next Games 全球在线游戏平台（博客与视频）</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">《行尸走肉》（通过 DocumentDB API）使用 Azure Cosmos DB 来加快开发周期并且使游戏玩法的吸引力更胜一筹</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixel Squad</a></td>
        <td>Pixel Squad 使用 Unity 游戏引擎和 Azure 开发了<b>罪恶港湾</b>。 <b>罪恶港湾</b>是一款社交策略游戏，适用于 Android、iOS 和 Windows 平台。 这款游戏使用了 Azure Blob 存储、托管的 Azure Redis 缓存、负载平衡 IIS VM 阵列和 Microsoft 通知中心。 了解它们如何管理扩展以及如何处理 5000 名玩家同时在线时的玩家数量飙升情况。
        </td>
        <td>
            <ul>
                <li>跨平台 <li>多人在线游戏 <li>扩展玩家的数量 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">《罪恶港湾》大型多人在线游戏使用 Azure 云服务的方式</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>其他链接

* [Hitman 和 Azure：创建只有使用云才能实现的“难寻目标”等游戏功能](https://channel9.msdn.com/Series/Hitman)
* [Hitcents、Game Troopers 和 InnoSpark 的秘密武器 Azure](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [基于 Bizspark 计划使用 Azure 的游戏初创公司](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>如何设计云后端

制作方和游戏设计人员在讨论游戏中需要具备哪些游戏特性和功能的同时，最好一开始就考虑想要如何设计游戏基础架构。 如果想要开发适用于各种设备和跨不同主流平台的游戏，可以将 Azure 用作你的游戏后端。

### <a name="understanding-iaas-paas-or-saas"></a>了解 IaaS、PaaS 或 SaaS

首选，需要考虑最适合你的游戏的服务级别。 了解以下三类服务之间的差异可以帮助你确定构建后端所要采用的方法。

* [基础结构即服务 (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    基础结构即服务 (IaaS) 是一类通过 Internet 配置和管理的即时计算基础设施。 想象有许多台计算机可随时基于需求快速扩展和减少。 IaaS 可帮助你避免购买自己的物理服务器和其他数据中心基础设施所需的成本以及管理它们的复杂性。

* [平台即服务 (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    平台即服务 (PaaS) 与 IaaS 类似，但它还包括对诸如服务器、存储和网络连接等基础设施的管理。 因此，首先是不必购买物理服务器和数据中心基础设施，其次是也不需要购买和管理软件许可证、底层应用程序基础架构、中间件、开发工具或其他资源。

* [软件即服务 (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    软件即服务 (SaaS) 允许用户通过 Internet 连接和使用基于云的应用。 它提供了一个完整的软件解决方案，你可以从云服务商那里以即付即用方式购买该解决方案。  常见的示例包括电子邮件、日历和办公工具（如 Microsoft Office 365）。 你可以租用组织的应用，并且你的用户通常使用 Web 浏览器通过 Internet 连接到该应用。 所有基础结构、中间件、应用软件和应用数据都位于服务提供商的数据中心。 服务提供商将管理硬件和软件，并通过适当的服务协议来确保游戏和数据的可用性和安全性。 SaaS 允许你的组织以最小的前期成本快速启动并运行应用。


### <a name="design-your-game-infrastructure-using-azure"></a>使用 Azure 设计游戏的基础结构

以下是一些 Azure 云服务/产品适用于游戏的方式。 Azure 适用于 Windows、Linux 以及诸如 Ruby、Python、Java 和 PHP 等熟悉的开源技术。 有关详细信息，请参阅[面向游戏的 Azure](https://azure.microsoft.com/solutions/gaming/)。

| 要求                 | 活动方案                            | 产品供应                      | 产品功能                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| 在云中托管你的域     | 有效地响应 DNS 查询            | [Azure DNS](https://azure.microsoft.com/services/dns/) | 托管你的域以实现高性能和高可用性  |
| 登录，身份验证      | 对玩家登录信息和玩家身份进行验证  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | 通过多重身份验证单一登录到任何云和本地 Web 应用            | 
| 使用基础结构即服务 (IaaS) 的游戏      | 游戏托管在云中虚拟机上       | [Azure VM](https://azure.microsoft.com/services/virtual-machines/) | 从 1 扩展到数千个虚拟机实例以用作游戏服务器（内置虚拟网络连接和负载平衡）；本地系统的混合一致性           |
| 使用平台即服务模型 (PaaS) 的 Web 或手机游戏            | 游戏托管在托管平台上                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | 适用于网站或手机游戏的 PaaS（这意味着 Azure VM 与中间件/开发工具/BI/DB 管理相互配合）   |
| 可用性高的可缩放型 n 层云游戏，并且可以更好地控制操作系统 (PaaS)        | 游戏托管在托管平台上                | [Azure 云服务](https://azure.microsoft.com/services/app-service/) | 旨在支持可缩放、可靠且运行成本低廉的应用程序的 PaaS   |
| 用于提高性能和可用性的跨区域负载平衡 | 传送传入的游戏请求。 可以充当第一级负载平衡。       | [Azure 流量管理器](https://azure.microsoft.com/en-us/services/traffic-manager/) | 提供了多种自动故障转移选项，并且能够均衡地或通过加权值分配你的流量。 可以无缝地合并本地系统和云系统。 |
| 游戏数据的云存储       | 最新游戏数据存储在云中，并发送到客户端设备 | [Azure Blob 存储](https://azure.microsoft.com/services/storage/blobs/)| 可以存储的文件类型不受限制；对象存储适用于大量非结构化数据（如图像、音频、视频等）。  |
| 临时数据存储表| 游戏事务（游戏状态的变化）临时存储在表中 | [Azure 表存储](https://azure.microsoft.com/services/storage/tables/)| 可以根据游戏需求以灵活模式存储游戏数据 |
| 队列游戏事务/请求| 以队列形式处理游戏事务 | [Azure 队列存储](https://azure.microsoft.com/services/storage/queues/)| 队列会吸收意外的突发流量，并可以防止服务器因游戏中突然产生的大量请求而不堪重负。   |
| 可扩展的关系型游戏数据库| 关系型数据的结构化存储，如数据库中的游戏内事务 | [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)| SQL 数据库即服务（[比较 VM 上的 SQL](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/)）  |
| 可扩展分配的低延迟游戏数据库| 借助模式灵活性快速读、写以及查询游戏和玩家数据 | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| 低延迟 NoSQL 文档数据库即服务   |
| 借助 Azure 服务使用自己的数据中心 | 从你自己的数据中心检索游戏，并发送到客户端设备 | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | 使你的组织可以通过自己的数据中心提供 Azure 服务，以助你一臂之力  |
| 大数据块传输| 可以通过 Azure CDN 从最近的内容分发网络 (CDN) pop 位置向用户发送游戏图像、音频和视频等大型文件。    | [Azure 内容分发网络](https://azure.microsoft.com/services/cdn/) | Azure CDN 基于现代网络拓扑的大型集中式节点构建，用于处理突发的大数据流量和严重负载以显著提升速度和可用性，从而明显改善用户体验  |
| 低延迟               | 执行缓存以生成内含更多控制和保证隔离数据的快速运行且可扩展的游戏；还可用于改善游戏的配对功能。 | [Azure Redis 缓存](https://azure.microsoft.com/services/cache/) | 高吞吐量、一贯的低延迟数据访问，可增强快速运行且可扩展的 Azure 应用程序  |
| 高可扩展性，低延迟 | 以低延迟的读写处理游戏用户变幻莫测的数量 | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | 能够应对最复杂、低延迟、数据密集型情形并且进行可靠扩展，从而实现一次处理较多用户。 Service Fabric 使你无需创建无状态应用所需的单独存储或缓存，即可生成游戏。 |
| 能够每秒从设备中收集数百万个事件                         | 每秒从设备中记录数百万个事件 | [Azure 事件中心](https://azure.microsoft.com/services/event-hubs/) | 从游戏、网站、应用和设备的云级别遥测引入  |
| 游戏数据的实时处理  | 执行玩家数据的实时分析以提升游戏可玩性| [Azure 流分析](https://azure.microsoft.com/services/stream-analytics/) | 云中的实时流处理  |
| 开发预测的游戏玩法         | 基于玩家数据创建自定义的动态游戏玩法  | [Azure 机器学习](https://azure.microsoft.com/services/machine-learning/) | 完全托管的云服务，使你可以轻松生成、部署、和共享预测的分析解决方案  |
| 收集和分析游戏数据| 大规模并行处理来自关系型和非关系型数据库的数据 | [Azure 数据仓库](https://azure.microsoft.com/services/sql-data-warehouse/)| 弹性数据仓库即服务（内含企业类功能）   |
| 吸引用户以增加用法和保留| 从任何后端向任何平台发送定向推送通知以引起兴趣，并鼓励执行特定游戏操作 | [Azure 通知中心](https://azure.microsoft.com/services/notification-hubs/)| 快速推送广播可在所有主流平台上覆盖数百万移动设备 &mdash; 例如 iOS、Android、Windows、Kindle、百度。 可以在任何后端上托管你的游戏 &mdash; 例如云或本地。|
| 将媒体内容流式传输到你的本地和全球受众，同时保护你的内容| 广播优质游戏预告片并且可以从所有设备观看电影剪辑| [Azure 媒体服务](https://azure.microsoft.com/services/media-services/)| 利用集成内容分发网络功能点播和直播视频流。 使用一个播放器满足你的所有播放需求，包括内容保护和加密。| 
| 开发、分发你的移动应用以及对其进行 Beta 测试 | 测试和分发你的移动应用。 应用性能和用户体验管理。 | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| 将崩溃报告和用户指标与应用分发和用户反馈平台集成。 支持 Android、Cordova、iOS、OS X、Unity、Windows 和 Xamarin 应用。 此外，请考虑使用 [Visual Studio Mobile Center](https://www.visualstudio.com/vs/mobile-center/) &mdash; 这是兼具大量分析、崩溃报告、推送通知、应用分发等功能的应用任务控制中心。 |
| 创建市场营销活动以增加用法和保留  | 根据数据分析，向目标玩家发送推送通知以引起兴趣，并鼓励执行特定游戏操作 | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) - 将于 2018 年 3 月停用，目前仅适用于现有客户 |  增加所有主流平台（iOS、Android、Windows、Windows Phone）上的游戏时间和用户保留 |


##  <a name="startup-and-developer-resources"></a>初创公司和开发商资源

* [面向初创公司的 Microsoft](https://startups.microsoft.com)

    面向初创公司的 Microsoft 提供产品、技术和市场进入方面的权益来帮助初创公司成长。 其中一项权益包括获得 Azure 免费帐户。 你有 200 美元信用额度在 30 天内探索各项服务、在 12 个月内探索受欢迎的免费服务，并始终可以使用 25 个以上免费的服务。 有关详细信息，请参阅[使用 Azure 免费帐户将您的创业点子变为现实](https://azure.microsoft.com/free/startups/)。
    
* [开发人员计划](e2e.md#developer-programs)

    Microsoft 提供多个开发人员计划，如 [ID@Xbox](http://www.xbox.com/Developers/id) 和 [Xbox Live 创意者计划](https://developer.microsoft.com/games/xbox/xboxlive/creator)，可以帮助你开发和发布游戏。

## <a name="learning-resources"></a>学习资源

* //版本 2016：[CodeLabs &mdash; 使用 Microsoft Azure App Service 和 Microsoft SQL Azure 后端在 Unity 中保存游戏分数](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* 版本 2017年:[使用 Microsoft Azure 提供世界级游戏体验： 从 Halo、 杀手，等 WalkingDead （视频） 的游戏中汲取](https://channel9.msdn.com/Events/Build/2017/P4062)
* 可重复使用的一组构建块、项目、服务和最佳做法，旨在在 GitHub 上使用 Azure 支持常见游戏工作负荷：[Azure 上的游戏构建块](https://github.com/MicrosoftDX/nether)
* [Azure 上的游戏服务（视频）](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>工具和其他有用的链接

* [MSDN 论坛 &mdash; Azure 平台](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [基于云的负载测试工具](https://www.visualstudio.com/team-services/cloud-load-testing/)
* [SDK 和命令行工具](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>适用于游戏后端的软件即服务

[Playfab](https://playfab.com/) 当前为 8,000 万每月活跃玩家参与的超过 1,200 个实时游戏提供支持。 它是一个完整的后端平台，包括具有实时控制的完整堆栈 LiveOps。 

你可以使用 SDK 将此解决方案集成到移动电话、电脑或控制台游戏中。 有些 SDK 适用于所有受欢迎的游戏引擎和平台，包括 Android、iOS、Unreal、Unity 和 Windows。 要开始，请参阅[文档](https://api.playfab.com/)。

它提供诸如身份验证、玩家数据管理、多人游戏和实时分析这样的游戏服务来帮助你的游戏扩大用户群。 利用实时数据管道和 LiveOps 的能力，通过自定义的游戏内项目、活动和促销来吸引用户。 你还可以执行 A/B 测试、生成报告、发送推送通知，等等。 

我们正在不断创新并添加新功能。 有关详细信息，请参阅[功能](https://playfab.com/features/)，要了解定价信息，请参阅[基于你的需求的简单定价](https://playfab.com/pricing/)。

## <a name="related-links"></a>相关链接

* [Windows10 游戏开发指南](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [面向游戏的 Azure](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [面向初创公司的 Microsoft](https://startups.microsoft.com)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 
