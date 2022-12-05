---
description: "Cursor Concurrency (ODBC)"
title: "Cursor Concurrency (ODBC) | Microsoft Docs"
ms.custom: ""
ms.date: "03/06/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: native-client
ms.topic: "reference"
helpviewer_keywords: 
  - "concurrency [ODBC]"
  - "cursors [ODBC], concurrency"
  - "ODBC cursors, concurrency"
ms.assetid: 68228ece-cbf1-4f19-bfdc-053884c1af48
author: markingmyname
ms.author: maghan
monikerRange: ">=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"
---
# Cursor Concurrency (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cursor operations, like cursor types, are affected by the concurrency options set by the application. Concurrency options are set using the SQL_ATTR_CONCURRENCY option of [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). The concurrency types are:  
  
-   Read-only (SQL_CONCUR_READONLY)  
  
-   Values (SQL_CONCUR_VALUES)  
  
-   Row version (SQL_CONCUR_ROWVER)  
  
-   Lock (SQL_CONCUR_LOCK)  
  
## See Also  
 [Cursor Properties](../../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
  
