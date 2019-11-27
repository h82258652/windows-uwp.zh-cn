---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314871"
---
[MongoDB](https://www.mongodb.com/what-is-mongodb) 和 [PostgreSQL](https://www.postgresql.org/about/) 是数据库系统的两个[热门选择](https://insights.stackoverflow.com/survey/2019#technology-_-databases)。 

MongoDB 是一种 NoSQL 文档数据库，用于处理 JSON 和存储无架构数据。 它适用于实现灵活性和处理非结构化数据、缓存实时分析以及进行水平缩放。 

PostgreSQL（有时称为 Postgres）是一种 SQL 关系数据库，着重于扩展性和标准符合性。 现在，它也可以处理 JSON，但通常更适合处理结构化数据、进行垂直缩放以及符合 ACID 的需求，例如电子商务和金融交易。

架构：

PostgreSQL  ：表 | 列 | 值 | 记录。

MongoDB (NoSQL)  ：集合 | 键 | 值 | 文档。

选择的数据库类型应取决于将使用该数据库的应用程序的类型。 我们建议你查看结构化和非结构化数据库的优点和缺点，并根据用例进行选择。 除了 PostgreSQL 和 MongoDB，还可以考虑其他几种数据库系统。