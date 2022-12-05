---
description: "Version Number"
title: "Version Number | Microsoft Docs"
ms.custom: ""
ms.date: "01/19/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: connectivity
ms.topic: conceptual
helpviewer_keywords: 
  - "version number supported [ODBC]"
  - "interoperability [ODBC], version number supported"
ms.assetid: 6eccacdf-b837-4b66-bd48-ba31771acecb
author: David-Engel
ms.author: v-davidengel
---
# Version Number
There are several versions of ODBC, each with different features. An application determines which ODBC version the Driver Manager and a particular driver support by calling **SQLGetInfo** with the SQL_ODBC_VER and SQL_DRIVER_ODBC_VER options.
