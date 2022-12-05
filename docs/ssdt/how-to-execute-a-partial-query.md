---
title: Execute a Partial Query
description: Get help debugging a section of a complex query. Use the Transact-SQL Editor to highlight a specific script segment and execute it as a single query.
ms.service: sql
ms.subservice: ssdt
ms.topic: conceptual
ms.assetid: af04ab37-6cbb-4185-9382-e5922fa5b1df
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 02/09/2017
---

# How to: Execute a Partial Query

The Transact\-SQL Editor allows you to highlight a specific segment of the script and execute it as a single query. This makes it easy for you to debug sections of complex queries.  
  
> [!WARNING]  
> The following procedure uses entities created in previous procedures in the [Connected Database Development](../ssdt/connected-database-development.md) and [Project-Oriented Offline Database Development](../ssdt/project-oriented-offline-database-development.md) sections.  
  
## To partially execute a query  
  
1. In **SQL Server Object Explorer**, double-click **PerishableFruits** under **Views** to open it in Transact\-SQL editor.  
  
2. Highlight the `SELECT p.Id, p.Name FROM dbo.Product p` segment in the code, right-click and select **Execute Query**.  
  
3. Notice that all the rows with the specified fields in the `Products` table are returned in the **Results** pane.