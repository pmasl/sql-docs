---
title: "DBCC TRACEON (Transact-SQL)"
description: "DBCC TRACEON (Transact-SQL)"
author: rwestMSFT
ms.author: randolphwest
ms.date: "07/17/2017"
ms.service: sql
ms.subservice: t-sql
ms.topic: "language-reference"
f1_keywords:
  - "DBCC_TRACEON_TSQL"
  - "TRACEON"
  - "DBCC TRACEON"
  - "TRACEON_TSQL"
helpviewer_keywords:
  - "DBCC TRACEON statement"
  - "trace flags [SQL Server], enabling"
dev_langs:
  - "TSQL"
---
# DBCC TRACEON (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Enables the specified trace flags.
  
![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## Syntax  
  
```syntaxsql
DBCC TRACEON ( trace# [ ,...n ][ , -1 ] ) [ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## Arguments
*trace#*  
Is the number of the trace flag to turn on.  
  
*n*  
Is a placeholder that indicates multiple trace flags can be specified.  
  
-1  
Switches on the specified trace flags globally. This argument is required in Azure SQL Managed Instance. 
  
WITH NO_INFOMSGS  
Suppresses all informational messages.  
  
## Remarks  
On a production server, to avoid unpredictable behavior, we recommend that you only enable trace flags server-wide by using one of the following methods:
-   Use the **-T** command-line startup option of Sqlservr.exe. This is a recommended best practice because it makes sure that all statements will run with the trace flag enabled. These include commands in startup scripts. For more information, see [sqlservr Application](../../tools/sqlservr-application.md).  
-   Use DBCC TRACEON **(**_trace#_ [**,** ...*.n*]**,-1)** only while users or applications are not concurrently running statements on the system.  

Trace flags are used to customize certain characteristics by controlling how [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] operates. Trace flags, after they are enabled, remain enabled in the server until disabled by executing a DBCC TRACEOFF statement. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], there are two types of trace flags: session and global. Session trace flags are active for a connection and are visible only for that connection. Global trace flags are set at the server level and are visible to every connection on the server. To determine the status of trace flags, use DBCC TRACESTATUS. To disable trace flags, use DBCC TRACEOFF.
  
After turning on a trace flag that affects query plans, execute `DBCC FREEPROCCACHE;` so that cached plans are recompiled using the new plan-affecting behavior.

Azure SQL Managed Instance supports the following global Trace Flags: 460,2301,2389,2390,2453,2467,7471,8207,9389,10316, and 11024

## Result Sets  
 DBCC TRACEON returns the following result set (message):  
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## Permissions  
Requires membership in the **sysadmin** fixed server role.
  
## Examples  
The following example disables hardware compression for tape drivers, by switching on trace flag `3205`. This flag is switched on only for the current connection.
  
```sql  
DBCC TRACEON (3205);  
GO  
```  
  
The following example switches on trace flag `3205` globally.
  
```sql  
DBCC TRACEON (3205, -1);  
GO  
```  
  
The following example switches on trace flags `3205`, and `260` globally.
  
```sql  
DBCC TRACEON (3205, 260, -1);  
GO  
```  
  
## See Also  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
[DBCC TRACESTATUS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-tracestatus-transact-sql.md)  
[Trace Flags &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
[Enable plan-affecting SQL Server query optimizer behavior that can be controlled by different trace flags on a specific-query level](https://support.microsoft.com/kb/2801413)
  
  
