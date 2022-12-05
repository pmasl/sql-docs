---
title: High availability
titleSuffix: Azure SQL Database and SQL Managed Instance
description: Learn about the Azure SQL Database and SQL Managed Instance service high availability capabilities and features
author: AbdullahMSFT
ms.author: amamun
ms.reviewer: wiassaf, mathoma, emlisa
ms.date: 11/28/2022
ms.service: sql-db-mi
ms.subservice: high-availability
ms.topic: conceptual
ms.custom:
  - "sqldbrb=2"
  - "references_regions"
monikerRange: "= azuresql || = azuresql-db || = azuresql-mi"
---

# High availability for Azure SQL Database and SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

This article describes high availability in Azure SQL Database and Azure SQL Managed Instance. 

Zone-redundant configuration is currently in preview for SQL Managed Instance, and only available for the Business Critical service tier. 

## Overview

The goal of the high availability architecture in Azure SQL Database and SQL Managed Instance is to guarantee that your database is up and running minimum of 99.99% of time without worrying about the impact of maintenance operations and outages. For more information regarding specific SLA for different tiers, refer to [SLA for Azure SQL Database](https://azure.microsoft.com/support/legal/sla/azure-sql-database) and SLA for [Azure SQL Managed Instance](https://azure.microsoft.com/support/legal/sla/azure-sql-sql-managed-instance/). 

Azure automatically handles critical servicing tasks, such as patching, backups, Windows and Azure SQL upgrades, and unplanned events such as underlying hardware, software, or network failures. When the underlying database in Azure SQL Database is patched or fails over, the downtime is not noticeable if you [employ retry logic](develop-overview.md#resiliency) in your app. SQL Database and SQL Managed Instance can quickly recover even in the most critical circumstances ensuring that your data is always available.

The high availability solution is designed to ensure that committed data is never lost due to failures, that maintenance operations do not affect your workload, and that the database will not be a single point of failure in your software architecture. There are no maintenance windows or downtimes that should require you to stop the workload while the database is upgraded or maintained.

There are two high availability architectural models:

- **Standard availability model** that is based on a separation of compute and storage.  It relies on high availability and reliability of the remote storage tier. This architecture targets budget-oriented business applications that can tolerate some performance degradation during maintenance activities.
- **Premium availability model** that is based on a cluster of database engine processes. It relies on the fact that there is always a quorum of available database engine nodes. This architecture targets mission-critical applications with high IO performance, high transaction rate and guarantees minimal performance impact to your workload during maintenance activities.

SQL Database and SQL Managed Instance both run on the latest stable version of the SQL Server database engine and Windows operating system, and most users would not notice that upgrades are performed continuously.

## Basic, Standard, and General Purpose service tier locally redundant availability

The Basic, Standard, and General Purpose service tiers use the standard availability architecture for both serverless and provisioned compute. The following figure shows four different nodes with the separated compute and storage layers.

![Separation of compute and storage](./media/high-availability-sla/general-purpose-service-tier.png)

The standard availability model includes two layers:

- A stateless compute layer that runs the `sqlservr.exe` process and contains only transient and cached data, such as TempDB, model databases on the attached SSD, and plan cache, buffer pool, and columnstore pool in memory. This stateless node is operated by Azure Service Fabric that initializes `sqlservr.exe`, controls health of the node, and performs failover to another node if necessary.
- A stateful data layer with the database files (.mdf/.ldf) that are stored in Azure Blob Storage. Azure Blob Storage has built-in data availability and redundancy feature. It guarantees that every record in the log file or page in the data file will be preserved even if `sqlservr.exe` process crashes.

Whenever the database engine or the operating system is upgraded, or a failure is detected, Azure Service Fabric will move the stateless `sqlservr.exe` process to another stateless compute node with sufficient free capacity. Data in Azure Blob storage is not affected by the move, and the data/log files are attached to the newly initialized `sqlservr.exe` process. This process guarantees 99.99% availability, but a heavy workload may experience some performance degradation during the transition since the new `sqlservr.exe` process starts with cold cache.

## General Purpose service tier zone redundant availability

Zone-redundant configuration for the General Purpose service tier is offered for both serverless and provisioned compute. This configuration utilizes [Azure Availability Zones](/azure/availability-zones/az-overview)  to replicate databases across multiple physical locations within an Azure region. By selecting zone-redundancy, you can make your new and existing serverless and provisioned general purpose single databases and elastic pools resilient to a much larger set of failures, including catastrophic datacenter outages, without any changes of the application logic.

Zone-redundant configuration for the General Purpose tier has two layers:  

- A stateful data layer with the database files (.mdf/.ldf) that are stored in ZRS(zone-redundant storage). Using [ZRS](/azure/storage/common/storage-redundancy) the data and log files are synchronously copied across three physically isolated Azure availability zones.
- A stateless compute layer that runs the sqlservr.exe process and contains only transient and cached data, such as TempDB, model databases on the attached SSD, and plan cache, buffer pool, and columnstore pool in memory. This stateless node is operated by Azure Service Fabric that initializes sqlservr.exe, controls health of the node, and performs failover to another node if necessary. For zone-redundant serverless and provisioned General Purpose databases, nodes with spare capacity are readily available in other Availability Zones for failover.

The zone-redundant version of the high availability architecture for the General Purpose service tier is illustrated by the following diagram:

![Zone redundant configuration for General Purpose](./media/high-availability-sla/zone-redundant-for-general-purpose.png)

> [!IMPORTANT]
> - For General Purpose tier the zone-redundant configuration is Generally Available in the following regions: West Europe, North Europe, West US 2, France Central, East US 2 & East US. This is in preview in the following regions: Southeast Asia, Australia East, Japan East, and UK South. 
> - For zone redundant availability, choosing a [maintenance window](maintenance-window.md) other than the default is currently available in [select regions](maintenance-window.md#azure-region-support). 
> - Zone-redundant configuration is only available in SQL Database when Gen5 hardware is selected. Zone-redundant configuration is currently in preview for SQL Managed Instance, and only available for the Business Critical service tier. 


## Premium and Business Critical service tier locally redundant availability

Premium and Business Critical service tiers use the Premium availability model, which integrates compute resources (`sqlservr.exe` process) and storage (locally attached SSD) on a single node. High availability is achieved by replicating both compute and storage to additional nodes creating a three to four-node cluster.

![Cluster of database engine nodes](./media/high-availability-sla/business-critical-service-tier.png)

The underlying database files (.mdf/.ldf) are placed on the attached SSD storage to provide very low latency IO to your workload. High availability is implemented using a technology similar to SQL Server [Always On availability groups](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). The cluster includes a single primary replica that is accessible for read-write customer workloads, and up to three secondary replicas (compute and storage) containing copies of data. The primary node constantly pushes changes to the secondary nodes in order and ensures that the data is persisted to at least one secondary replica before committing each transaction. This process guarantees that if the primary node crashes for any reason, there is always a fully synchronized node to fail over to. The failover is initiated by the Azure Service Fabric. Once the secondary replica becomes the new primary node, another secondary replica is created to ensure the cluster has enough nodes (quorum set). Once failover is complete, Azure SQL connections are automatically redirected to the new primary node.

As an extra benefit, the premium availability model includes the ability to redirect read-only Azure SQL connections to one of the secondary replicas. This feature is called [Read Scale-Out](read-scale-out.md). It provides 100% additional compute capacity at no extra charge to off-load read-only operations, such as analytical workloads, from the primary replica.

## Premium and Business Critical service tier zone redundant availability 

By default, the cluster of nodes for the premium availability model is created in the same datacenter. With the introduction of [Azure Availability Zones](/azure/availability-zones/az-overview), SQL Database can place different replicas of the Business Critical database to different availability zones in the same region. To eliminate a single point of failure, the control ring is also duplicated across multiple zones as three gateway rings (GW). The routing to a specific gateway ring is controlled by [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) (ATM). Because the zone-redundant configuration in the Premium or Business Critical service tiers does not create additional database redundancy, you can enable it at no extra cost. By selecting a zone-redundant configuration, you can make your Premium or Business Critical databases resilient to a much larger set of failures, including catastrophic datacenter outages, without any changes to the application logic. You can also convert any existing Premium or Business Critical databases or pools to the zone-redundant configuration.

Because the zone-redundant databases have replicas in different datacenters with some distance between them, the increased network latency may increase the commit time and thus impact the performance of some OLTP workloads. You can always return to the single-zone configuration by disabling the zone-redundancy setting. This process is an online operation similar to the regular service tier upgrade. At the end of the process, the database or pool is migrated from a zone-redundant ring to a single zone ring or vice versa.

> [!IMPORTANT]
> This feature is not available in SQL Managed Instance. In SQL Database, when using the Business Critical tier, zone-redundant configuration is only available when the standard-series (Gen5) hardware is selected. For up to date information about the regions that support zone-redundant databases, see [Services support by region](/azure/availability-zones/az-region).

The zone-redundant version of the high availability architecture is illustrated by the following diagram:

![Diagram of the zone-redundant high availability architecture.](./media/high-availability-sla/zone-redundant-business-critical-service-tier.png)

> [!IMPORTANT]
> - This feature is currently in preview for SQL Managed Instance, and only available on the Business Critical service tier. In SQL Database, when using the Business Critical tier, zone-redundant configuration is only available when the Gen5 hardware is selected. For up to date information about the regions that support zone-redundant databases, see [Services support by region](/azure/availability-zones/az-region).
> - For zone redundant availability, choosing a [maintenance window](./maintenance-window.md) other than the default is currently available in [select regions](maintenance-window.md#azure-region-support). 

> [!IMPORTANT]
> For zone redundant availability, choosing a [maintenance window](/azure/azure-sql/database/maintenance-window) other than the default is currently available in [select regions](/azure/azure-sql/database/maintenance-window?view=azuresql&preserve-view=true#azure-region-support). 

During preview, zone redundancy for SQL Managed Instance is available in the Business Critical service tier and supported in the following regions: 

- Brazil South
- East US
- Japan East
- Norway East
- UAE North
- South Africa North
- Australia East
- Korea Central

## Hyperscale service tier locally redundant availability

The Hyperscale service tier architecture is described in [Distributed functions architecture](./service-tier-hyperscale.md#distributed-functions-architecture) and is only currently available for SQL Database, not SQL Managed Instance.

![Hyperscale functional architecture](./media/high-availability-sla/hyperscale-architecture.png)

The availability model in Hyperscale includes four layers:

- A stateless compute layer that runs the `sqlservr.exe` processes and contains only transient and cached data, such as non-covering RBPEX cache, TempDB, model database, etc. on the attached SSD, and plan cache, buffer pool, and columnstore pool in memory. This stateless layer includes the primary compute replica and optionally a number of secondary compute replicas that can serve as failover targets.
- A stateless storage layer formed by page servers. This layer is the distributed storage engine for the `sqlservr.exe` processes running on the compute replicas. Each page server contains only transient and cached data, such as covering RBPEX cache on the attached SSD, and data pages cached in memory. Each page server has a paired page server in an active-active configuration to provide load balancing, redundancy, and high availability.
- A stateful transaction log storage layer formed by the compute node running the Log service process, the transaction log landing zone, and transaction log long-term storage. Landing zone and long-term storage use Azure Storage, which provides availability and [redundancy](/azure/storage/common/storage-redundancy) for transaction log, ensuring data durability for committed transactions.
- A stateful data storage layer with the database files (.mdf/.ndf) that are stored in Azure Storage and are updated by page servers. This layer uses data availability and [redundancy](/azure/storage/common/storage-redundancy) features of Azure Storage. It guarantees that every page in a data file will be preserved even if processes in other layers of Hyperscale architecture crash, or if compute nodes fail.

Compute nodes in all Hyperscale layers run on Azure Service Fabric, which controls health of each node and performs failovers to available healthy nodes as necessary.

For more information on high availability in Hyperscale, see [Database High Availability in Hyperscale](./service-tier-hyperscale.md#database-high-availability-in-hyperscale).

## Hyperscale service tier zone redundant availability

Enabling this configuration ensures zone-level resiliency through replication across Availability Zones for all Hyperscale layers. By selecting zone-redundancy, you can make your Hyperscale databases resilient to a much larger set of failures, including catastrophic datacenter outages, without any changes to the application logic. All Azure regions that have [Availability Zones](/azure/availability-zones/az-overview#azure-regions-with-availability-zones) support zone redundant Hyperscale database.

Consider the following limitations: 

- Zone redundant configuration can only be specified during database creation. This setting cannot be modified once the resource is provisioned. Use [Database copy](database-copy.md), [point-in-time restore](recovery-using-backups.md#point-in-time-restore), or create a [geo-replica](active-geo-replication-overview.md) to update the zone redundant configuration for an existing Hyperscale database. When using one of these update options, if the target database is in a different region than the source or if the database backup storage redundancy from the target differs from the source database, the [copy operation](database-copy.md#database-copy-for-azure-sql-hyperscale) will be a size of data operation.
- Only standard-series (Gen5) hardware is supported.
- Named replicas are not currently supported.
- Zone redundancy cannot currently be specified when migrating an existing database from another Azure SQL Database service tier to Hyperscale.
- At least 1 high availability compute replica and the use of zone-redundant or geo-zone-redundant backup storage is required for enabling the zone redundant configuration for Hyperscale.

> [!IMPORTANT]
> For zone redundant availability, choosing a [maintenance window](maintenance-window.md) other than the default is currently available in [select regions](maintenance-window.md#azure-region-support).

### Create a zone redundant Hyperscale database

Use [Azure PowerShell](/powershell/azure/install-az-ps) or the [Azure CLI](/cli/azure/update-azure-cli) to create a zone redundant Hyperscale database. Confirm you have the latest version of the API to ensure support for any recent changes. 

# [Azure PowerShell](#tab/azure-powershell)

Specify the `-ZoneRedundant` parameter to enable zone redundancy for your Hyperscale database by using Azure PowerShell. The database must have at least 1 high availability replica and zone-redundant backup storage must be specified.

To enable zone redundancy using Azure PowerShell, use the following example command: 

```powershell
New-AzSqlDatabase -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "Database01" `
    -Edition "Hyperscale" -HighAvailabilityReplicaCount 1 -ZoneRedundant -BackupStorageRedundancy Zone -RequestedServiceObjectiveName HS_Gen5_2'
```



# [Azure CLI](#tab/azure-cli)

Specify the `-zone-redundant parameter` to enable zone redundancy for your Hyperscale database by using the Azure CLI. The database copy must have at least 1 high availability replica and zone-redundant backup storage.

To enable zone redundancy using the Azure CLI, use the following example command: 

```azurecli
az sql db create -g mygroup -s myserver -n mydb -e Hyperscale -f Gen5 –-ha-replicas 1 –-zone-redundant -–backup-storage-redundancy Zone --capacity 2
```

* * *

### Create a zone redundant Hyperscale database by creating a geo-replica

To make an existing Hyperscale database zone redundant, use Azure PowerShell or the Azure CLI to create a zone redundant Hyperscale database using active geo-replication.  The geo-replica can be in the same or different region as the existing Hyperscale database. 

# [Azure PowerShell](#tab/azure-powershell)

Specify the `-ZoneRedundant` parameter to enable zone redundancy for your Hyperscale database secondary. The secondary database must have at least 1 high availability replica and zone-redundant backup storage must be specified. 

To create your zone redundant database using Azure PowerShell, use the following example command: 

```powershell
New-AzSqlDatabaseSecondary -ResourceGroupName "myResourceGroup" -ServerName $sourceserver -DatabaseName "databaseName" -PartnerResourceGroupName "myPartnerResourceGroup" -PartnerServerName $targetserver -PartnerDatabaseName "zoneRedundantCopyOfMySampleDatabase" -ZoneRedundant -BackupStorageRedundancy Zone -HighAvailabilityReplicaCount 1
```


# [Azure CLI](#tab/azure-cli)

Specify the `-zone-redundant parameter` to enable zone redundancy for your Hyperscale database secondary. The secondary database must have at least 1 high availability replica and zone-redundant backup storage. 

To enable zone redundancy using the Azure CLI, use the following example command: 

```azurecli
az sql db replica create -g mygroup -s myserver -n originalDb --partner-server newDb -–ha-replicas 1 -–zone-redundant -–backup-storage-redundancy Zone
```

* * *

### Create a zone redundant Hyperscale database by creating a database copy

To make an existing Hyperscale database zone redundant, use Azure PowerShell or the Azure CLI to create a zone redundant Hyperscale database using database copy.  The database copy can be in the same or different region as the existing Hyperscale database. 

# [Azure PowerShell](#tab/azure-powershell)

Specify the `-ZoneRedundant` parameter to enable zone redundancy for your Hyperscale database copy. The database copy must have at least 1 high availability replica and zone-redundant backup storage must be specified. 

To create your zone redundant database using Azure PowerShell, use the following example command: 

```powershell
New-AzSqlDatabaseCopy -ResourceGroupName "myResourceGroup" -ServerName $sourceserver -DatabaseName "databaseName" -CopyResourceGroupName "myCopyResourceGroup" -CopyServerName $copyserver -CopyDatabaseName "zoneRedundantCopyOfMySampleDatabase" -ZoneRedundant -BackupStorageRedundancy Zone 
```


# [Azure CLI](#tab/azure-cli)

Specify the `-zone-redundant parameter` to enable zone redundancy for your Hyperscale database copy. The database copy must have at least 1 high availability replica and zone-redundant backup storage. 

To enable zone redundancy using the Azure CLI, use the following example command: 

```azurecli
az sql db copy --dest-name "CopyOfMySampleDatabase" --dest-resource-group "myResourceGroup" --dest-server $targetserver --name "<databaseName>" --resource-group "<resourceGroup>" --server $sourceserver -–ha-replicas 1 -–zone-redundant -–backup-storage-redundancy Zone
```

* * *

## Master database zone redundant availability

In Azure SQL Database, a [server](./logical-servers.md) is a logical construct that acts as a central administrative point for a collection of databases. At the server level, you can administer logins, Azure Active Directory authentication, firewall rules, auditing rules, threat detection policies, and auto-failover groups. Data related to some of these features, such as logins and firewall rules, is stored in the master database. Similarly, data for some DMVs, for example [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database), is also stored in the master database.

When a database with a zone-redundant configuration is created on a logical server, the master database associated with the server is automatically made zone-redundant as well. This ensures that in a zonal outage, applications using the database remain unaffected because features dependent on the master database, such as logins and firewall rules, are still available. Making the master database zone-redundant is an asynchronous process and will take some time to finish in the background. 

When none of the databases on a server are zone-redundant, or when you create an empty server, then the master database associated with the server is **not zone-redundant**.

You can use Azure PowerShell or the Azure CLI or the [REST API](/rest/api/sql/2021-11-01-preview/databases/get) to check the `ZoneRedundant` property for the master database: 

# [Azure PowerShell](#tab/azure-powershell)

Use the following example command to check the value of "ZoneRedundant" property for master database.

```powershell
Get-AzSqlDatabase -ResourceGroupName "myResourceGroup" -ServerName "myServerName" -DatabaseName "master"
```

# [Azure CLI](#tab/azure-cli)

Use the following example command to check the value of "ZoneRedundant" property for master database.

```azurecli
az sql db show --resource-group "myResourceGroup" --server "myServerName" --name master
```

---

## Accelerated Database Recovery (ADR)

[Accelerated Database Recovery (ADR)](../accelerated-database-recovery.md) is a database engine feature that greatly improves database availability, especially in the presence of long running transactions. ADR is currently available for Azure SQL Database, Azure SQL Managed Instance, and Azure Synapse Analytics.



## Testing application fault resiliency

High availability is a fundamental part of the SQL Database and SQL Managed Instance platform that works transparently for your database application. However, we recognize that you may want to test how the automatic failover operations initiated during planned or unplanned events would impact an application before you deploy it to production. You can manually trigger a failover by calling a special API to restart a database, an elastic pool, or a managed instance. In the case of a zone-redundant serverless or provisioned General Purpose database or elastic pool, the API call would result in redirecting client connections to the new primary in an Availability Zone different from the Availability Zone of the old primary. So in addition to testing how failover impacts existing database sessions, you can also verify if it changes the end-to-end performance due to changes in network latency. Because the restart operation is intrusive and a large number of them could stress the platform, only one failover call is allowed every 15 minutes for each database, elastic pool, or managed instance.

A failover can be initiated using PowerShell, REST API, or Azure CLI:

|Deployment type|PowerShell|REST API| Azure CLI|
|:---|:---|:---|:---|
|Database|[Invoke-AzSqlDatabaseFailover](/powershell/module/az.sql/invoke-azsqldatabasefailover)|[Database failover](/rest/api/sql/databases/failover)|[az rest](/cli/azure/reference-index#az-rest) may be used to invoke a REST API call from Azure CLI|
|Elastic pool|[Invoke-AzSqlElasticPoolFailover](/powershell/module/az.sql/invoke-azsqlelasticpoolfailover)|[Elastic pool failover](/javascript/api/@azure/arm-sql/elasticpools)|[az rest](/cli/azure/reference-index#az-rest) may be used to invoke a REST API call from Azure CLI|
|SQL Managed Instance|[Invoke-AzSqlInstanceFailover](/powershell/module/az.sql/Invoke-AzSqlInstanceFailover/)|[SQL Managed Instance - Failover](/rest/api/sql/managed%20instances%20-%20failover/failover)|[az sql mi failover](/cli/azure/sql/mi/#az-sql-mi-failover) may be used to invoke a REST API call from Azure CLI|

> [!IMPORTANT]
> The Failover command is not available for readable secondary replicas of Hyperscale databases.

## Conclusion

Azure SQL Database and Azure SQL Managed Instance feature a built-in high availability solution, that is deeply integrated with the Azure platform. It is dependent on Service Fabric for failure detection and recovery, on Azure Blob storage for data protection, and on Availability Zones for higher fault tolerance (as mentioned earlier in document not applicable to Azure SQL Managed Instance yet). In addition, SQL Database and SQL Managed Instance use the Always On availability group technology from the SQL Server instance for replication and failover. The combination of these technologies enables applications to fully realize the benefits of a mixed storage model and support the most demanding SLAs.

## Next steps

- Learn about [Azure Availability Zones](/azure/availability-zones/az-overview)
- Learn about [Service Fabric](/azure/service-fabric/service-fabric-overview)
- Learn about [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview)
- Learn [How to initiate a manual failover on SQL Managed Instance](../managed-instance/user-initiated-failover.md)
- For more options for high availability and disaster recovery, see [Business Continuity](business-continuity-high-availability-disaster-recover-hadr-overview.md)