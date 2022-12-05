---
description: "Transaction Support in DBMSs"
title: "Transaction Support in DBMSs | Microsoft Docs"
ms.custom: ""
ms.date: "01/19/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: connectivity
ms.topic: conceptual
helpviewer_keywords: 
  - "interoperability [ODBC], transaction support"
  - "transactions [ODBC], DBMS support"
ms.assetid: 0fc2ae34-4748-4120-9fc3-bb28c8ed867e
author: David-Engel
ms.author: v-davidengel
---
# Transaction Support in DBMSs
Some databases, especially desktop databases such as dBASE, Paradox, and Btrieve, do not support transactions. Even among databases that support transactions, there is variation in what kinds of SQL statements can be in a transaction. For more information, see the SQL_TXN_CAPABLE option in the [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) function description.
