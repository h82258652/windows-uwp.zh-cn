---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314871"
---
数据库系统的两个[常见选项](https://insights.stackoverflow.com/survey/2019#technology-_-databases)是[MongoDB](https://www.mongodb.com/what-is-mongodb)和[PostgreSQL](https://www.postgresql.org/about/)。 

MongoDB 是一种 NoSQL 文档数据库，旨在使用 JSON 并存储无架构的数据。 它适用于灵活性和非结构化数据、缓存实时分析和水平缩放。 

PostgreSQL （有时称为 Postgres）是一个 SQL 关系数据库，重点介绍扩展性和标准符合性。 它也可以处理 JSON，但通常更适合用于结构化数据、垂直缩放和符合 ACID 要求的需求，如电子商务和财务交易。

架构

**PostgreSQL**表 |列 |值 |记录.

**MongoDB （NoSQL）：** 集合 |项 |值 |文档.

您选择的数据库类型应取决于您将使用数据库的应用程序的类型。 建议您查找结构化和非结构化数据库的优点和缺点，并根据用例进行选择。 除了 PostgreSQL 和 MongoDB 以外，还有其他几个数据库系统需要考虑。