---
title: "sys.sp_rda_deauthorize_db (Transact-SQL) | Microsoft Docs"
description: Learn how to use sys.sp_rda_deauthorize_db to remove authenticated connections between local Stretch-enabled databases and remote Azure databases.
ms.custom: ""
ms.date: 07/25/2022
ms.service: sql
ms.reviewer: randolphwest
ms.subservice: stored-procedures
ms.topic: "reference"
f1_keywords: 
  - "sys.sp_rda_deauthorize_db"
  - "sys.sp_rda_deauthorize_db_TSQL"
dev_langs: 
  - "TSQL"
helpviewer_keywords: 
  - "sys.sp_rda_deauthorize_db stored procedure"
ms.assetid: 2e362e15-2cd5-4856-9f0b-54df56b0866b
author: markingmyname
ms.author: maghan
---
# sys.sp_rda_deauthorize_db (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Removes the authenticated connection between a local Stretch-enabled database and the remote Azure database. Run **sp_rda_deauthorize_db**  when the remote database is unreachable or in an inconsistent state and you want to change query behavior for all Stretch-enabled tables in the database.  

> [!IMPORTANT]  
> Stretch Database is deprecated in [!INCLUDE [sssql22-md](../../includes/sssql22-md.md)]. [!INCLUDE [ssNoteDepFutureAvoid-md](../../includes/ssnotedepfutureavoid-md.md)]
 
 ![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## Syntax  
  
```  
sp_rda_deauthorize_db   
```  
  
## Return Code Values  
 0 (success) or >0 (failure)  
  
## Permissions  
 Requires db_owner permissions.  
  
## Remarks  
 After you run **sp_rda_deauthorize_db** , all queries against Stretch-enabled databases and tables fail. That is, the query mode is set to DISABLED. To exit this mode, do one of the following things.  
  
-   Run [sys.sp_rda_reauthorize_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) to reconnect to the remote Azure database. This operation automatically resets the query mode to LOCAL_AND_REMOTE, which is the default behavior for Stretch Database. That is, queries return results from both local and remote data.  
  
-   Run [sys.sp_rda_set_query_mode &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) with the LOCAL_ONLY argument to let queries continue to run against local data only.  
  
## See Also  
 [sys.sp_rda_set_query_mode &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md)   
 [sys.sp_rda_reauthorize_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md)   
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
