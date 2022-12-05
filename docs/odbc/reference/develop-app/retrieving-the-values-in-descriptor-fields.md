---
description: "Retrieving the Values in Descriptor Fields"
title: "Retrieving the Values in Descriptor Fields | Microsoft Docs"
ms.custom: ""
ms.date: "01/19/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: connectivity
ms.topic: conceptual
helpviewer_keywords: 
  - "descriptors [ODBC], retrieving or setting field values"
ms.assetid: c05b180f-c2b0-437b-8d1c-ce7f4da93287
author: David-Engel
ms.author: v-davidengel
---
# Retrieving the Values in Descriptor Fields
An application can call **SQLGetDescField** to obtain a single field of a descriptor record. **SQLGetDescField** gives the application access to all the descriptor fields defined in ODBC, and to driver-defined fields as well.  
  
 **SQLGetDescRec** can be called to retrieve the settings of multiple descriptor fields that affect the data type and storage of column or parameter data.
