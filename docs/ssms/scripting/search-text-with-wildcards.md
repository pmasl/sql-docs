---
title: "Search Text with Wildcards"
description: 'Learn how to use wildcards in the "Find what" field of a Find and Replace dialog box to specify a pattern to be matched.'
ms.custom: seo-lt-2019
ms.date: "03/01/2017"
ms.service: sql
ms.subservice: ssms
ms.reviewer: ""
ms.topic: conceptual
f1_keywords:
  - "vswildcardsbuilder"
helpviewer_keywords: 
  - "searches [SQL Server Management Studio], wildcards"
  - "Query Editor [SQL Server Management Studio], wildcard searches"
  - "wildcard options [SQL Server Management Studio]"
ms.assetid: 449600f8-cc87-4b3f-878a-59c158a88a40
author: markingmyname
ms.author: maghan
monikerRange: ">=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"
---
# Search Text with Wildcards
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  The following expressions can replace characters or digits in the **Find what** field of the [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Find and Replace** dialog box.  
  
#### To search using wildcards  
  
1.  To enable the use of wildcards in the **Find what** field during Quick Find, **Find in Files**, **Quick Replace**, or **Replace in Files** operations, select the **Use** option under **Find Options** and then choose **Wildcards**.  
  
2.  The triangular **Reference List** button next to the **Find what** field then becomes available. Click this button to display a list of the available wildcards. When you choose any item from the **Reference List**, it is inserted into the **Find what** string.  
  
 The following table describes the wildcards available in the **Reference List**.  
  
|Expression|Syntax|Description|  
|----------------|------------|-----------------|  
|Any single character|?|Matches any single character.|  
|Any single digit|#|Matches any single digit. For example, 7# matches numbers that include 7 followed by another number, such as 71, but not 17.|  
|Characters not in set|[! ]|Matches any one character that is not specified in the set.|  
|One or more characters|*|Matches any one or more characters. For example, new* matches any text that includes "new", such as newfile.txt.|  
|Set of characters|[ ]|Matches any one of the characters specified in the set.|  
  
## See Also  
 [Search and Replace](./search-and-replace.md)   
 [Search Text with Regular Expressions](./search-text-with-regular-expressions.md)