---
description: "MSSQLSERVER_7916"
title: "MSSQLSERVER_7916 | Microsoft Docs"
ms.custom: ""
ms.date: "04/04/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: supportability
ms.topic: "reference"
helpviewer_keywords: 
  - "7916 (Database Engine error)"
ms.assetid: 9bac3536-de14-4e98-84c2-bde9a59ba0d1
author: MashaMSFT
ms.author: mathoma
---
# MSSQLSERVER_7916
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## Details  
  
| Attribute | Value |  
| :-------- | :---- |  
|Product Name|SQL Server|  
|Event ID|7916|  
|Event Source|MSSQLSERVER|  
|Component|SQLEngine|  
|Symbolic Name|DBCC2_REPAIR_RECORD|  
|Message Text|Repair: Deleted record for object ID O_ID, index ID I_ID, partition ID PN_ID, alloc unit ID A_ID (type TYPE), on page P_ID, slot S_ID. Indexes will be rebuilt.|  
  
## Explanation  
This is an information messages from REPAIR that indicates the specified record was deleted from the page.  
  
## User Action  
None  
  
