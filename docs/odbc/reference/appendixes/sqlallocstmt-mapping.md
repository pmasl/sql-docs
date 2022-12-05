---
description: "SQLAllocStmt Mapping"
title: "SQLAllocStmt Mapping | Microsoft Docs"
ms.custom: ""
ms.date: "01/19/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: connectivity
ms.topic: reference
helpviewer_keywords: 
  - "mapping deprecated functions [ODBC], SQLAllocStmt"
  - "SQLAllocStmt function [ODBC], mapping"
ms.assetid: a2449dbb-1b6c-4b49-81b9-ebdddd4442fd
author: David-Engel
ms.author: v-davidengel
---
# SQLAllocStmt Mapping
When an application calls **SQLAllocStmt** through an ODBC *3.x* driver, the call to:  
  
```  
SQLAllocStmt(hdbc, phstmt)  
```  
  
 is mapped to **SQLAllocHandle** by the Driver Manager in the driver as follows:  
  
```  
SQLAllocHandle(SQL_HANDLE_STMT, InputHandle, OutputHandlePtr)  
```  
  
 with *InputHandle* set to *hdbc* and *OutputHandlePtr* set to *phstmt*.
