---
title: "使用 T-SQL 创建 Azure SQL 数据库或将其移入弹性池 | Microsoft 文档"
description: "使用 T-SQL 在弹性池中创建 Azure SQL 数据库。 或使用 T-SQL 将数据库移入和移出池。"
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: sharded databases pool; how to
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 17ec1ad8218824048179e32ae5059b6a6d77a367


---
# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>使用 Transact-SQL 监视和管理弹性数据库池
> [!div class="op_single_selector"]
> * [Azure 门户](sql-database-elastic-pool-manage-portal.md)
> * [PowerShell](sql-database-elastic-pool-manage-powershell.md)
> * [C#](sql-database-elastic-pool-manage-csharp.md)
> * [T-SQL](sql-database-elastic-pool-manage-tsql.md)
>  

使用 [Create Database（Azure SQL 数据库）](https://msdn.microsoft.com/library/dn268335.aspx)和 [Alter Database（Azure SQL 数据库）](https://msdn.microsoft.com/library/mt574871.aspx)命令可以创建数据库并将它们移入和移出弹性池。 必须先有弹性池才可以使用这些命令。 这些命令只影响数据库。 无法使用 T-SQL 命令更改新池的创建和池属性（例如最小和最大 eDTU）的设置。

## <a name="create-a-new-database-in-an-elastic-pool"></a>在弹性池中创建新数据库
结合 SERVICE_OBJECTIVE 选项使用 CREATE DATABASE 命令。   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

弹性池中的所有数据库都都继承弹性池的服务层（基本、标准、高级）。 

## <a name="move-a-database-between-elastic-pools"></a>在弹性池之间移动数据库
结合 MODIFY 使用 ALTER DATABASE 命令，并将 SERVICE\_OBJECTIVE 选项设置为 ELASTIC\_POOL；将名称设置为目标池的名称。

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>将数据库移入弹性池
结合 MODIFY 使用 ALTER DATABASE 命令，并将 SERVICE\_OBJECTIVE 选项设置为 ELASTIC_POOL；将名称设置为目标池的名称。

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>将数据库移出弹性池
使用 ALTER DATABASE 命令并将 SERVICE_OBJECTIVE 设置为某个性能级别（S0、S1 等）。

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>列出弹性池中的数据库
使用 [sys.database\_service\_objectives 视图](https://msdn.microsoft.com/library/mt712619)列出弹性池中的所有数据库。 登录到 master 数据库可查询该视图。

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>获取池的资源使用情况数据
使用 [sys.elastic\_pool\_resource\_stats 视图](https://msdn.microsoft.com/library/mt280062.aspx)来检查逻辑服务器上弹性池的资源使用情况统计信息。 登录到 master 数据库可查询该视图。

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>获取弹性数据库的资源使用情况
使用 [sys.dm\_ db\_ resource\_stats 视图](https://msdn.microsoft.com/library/dn800981.aspx)或 [sys.resource \_stats 视图](https://msdn.microsoft.com/library/dn269979.aspx)来检查弹性池中数据库的资源使用情况统计信息。 此过程类似于查询任何单一数据库的资源使用情况。

## <a name="next-steps"></a>后续步骤
创建弹性数据库池后，你可以通过创建弹性作业来管理池中的弹性数据库。 弹性作业可以用来根据池中数据库的数目来运行 T-SQL 脚本。 有关详细信息，请参阅[弹性数据库作业概述](sql-database-elastic-jobs-overview.md)。 

请参阅[使用 Azure SQL 数据库进行扩展](sql-database-elastic-scale-introduction.md)：使用弹性数据库工具扩展、移动数据、查询或创建事务。




<!--HONumber=Nov16_HO3-->

