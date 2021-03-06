---
title: "Hints (Transact-SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "03/17/2017"
ms.prod: "sql-non-specified"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "database-engine"
ms.tgt_pltfrm: ""
ms.topic: "language-reference"
dev_langs: 
  - "TSQL"
helpviewer_keywords: 
  - "query optimizer [SQL Server], hints"
  - "hints [SQL Server], about hints"
  - "SELECT statement [SQL Server], hints"
  - "hints [SQL Server]"
  - "INSERT statement [SQL Server], hints"
  - "UPDATE statement [SQL Server], hints"
  - "DELETE statement [SQL Server], hints"
ms.assetid: 99412475-b0df-4264-9938-33a0b302b41a
caps.latest.revision: 27
author: "BYHAM"
ms.author: "rickbyh"
manager: "jhubbard"
---
# Hints (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Hints are options or strategies specified for enforcement by the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] query processor on SELECT, INSERT, UPDATE, or DELETE statements. The hints override any execution plan the query optimizer might select for a query.  
  
> [!CAUTION]  
>  Because the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] query optimizer typically selects the best execution plan for a query, we recommend that <join_hint>, <query_hint>, and <table_hint> be used only as a last resort by experienced developers and database administrators.  
  
 The following hints are described in this section:  
  
-   [Join Hints](../../t-sql/queries/hints-transact-sql-join.md)  
  
-   [Query Hints](../../t-sql/queries/hints-transact-sql-query.md)  
  
-   [Table Hint](../../t-sql/queries/hints-transact-sql-table.md)  
  
  