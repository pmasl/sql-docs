---
title: "sys.sp_rda_reconcile_indexes (Transact-SQL) | Microsoft Docs"
description: Learn about sys.sp_rda_reconcile_indexes. See how to use this Transact-SQL stored procedure to queue a schema task to reconcile indexes on a remote table.
ms.custom: ""
ms.date: 07/25/2022
ms.service: sql
ms.reviewer: randolphwest
ms.subservice: stored-procedures
ms.topic: "reference"
f1_keywords: 
  - "sp_rda_reconcile_indexes"
  - "sp_rda_reconcile_indexes_TSQL"
dev_langs: 
  - "TSQL"
helpviewer_keywords: 
  - "sys.sp_rda_reconcile_indexes stored procedure"
ms.assetid: 96b31ab9-bf84-46d6-9990-81f5c51f885a
author: markingmyname
ms.author: maghan
---
# sys.sp_rda_reconcile_indexes (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Queues a schema task to reconcile indexes on the remote table. After this task finishes successfully, the remote table has the same indexes that exist on the local Stretch-enabled table.  

> [!IMPORTANT]  
> Stretch Database is deprecated in [!INCLUDE [sssql22-md](../../includes/sssql22-md.md)]. [!INCLUDE [ssNoteDepFutureAvoid-md](../../includes/ssnotedepfutureavoid-md.md)]
  
 If there is another task queued to reconcile indexes when you call **sp_rda_reconcile_indexes**, this stored procedure does not queue a duplicate task.  
  
 ![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## Syntax  
  
```  
  
sp_rda_reconcile_indexes [@objname = ] 'objname'  
  
```  
  
## Arguments  
 [@objname = ] *'objname'*  
 Is the qualified or non-qualified name of the Stretch-enabled table for which you want to reconcile indexes. Quotes are required only if you specify a qualified object.  
  
## Return Code Values  
 0 (success) or >0 (failure)  
  
## See Also  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
