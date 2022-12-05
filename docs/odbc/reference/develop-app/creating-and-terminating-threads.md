---
description: "Creating and Terminating Threads"
title: "Creating and Terminating Threads | Microsoft Docs"
ms.custom: ""
ms.date: "01/19/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: connectivity
ms.topic: conceptual
helpviewer_keywords: 
  - "terminating threads [ODBC]"
  - "threads [ODBC]"
  - "multithreaded applications [ODBC]"
ms.assetid: a2cf98ef-1c71-4742-8ee2-b53fd8e04333
author: David-Engel
ms.author: v-davidengel
---
# Creating and Terminating Threads
Multithread applications that use ODBC should call the Microsoft® Visual C++® Run-Time Library functions **_beginthread** and **_endthread** (or **_beginthreadex** and **_endthreadex**) to create and terminate threads that call the ODBC Driver Manager. If applications call the Microsoft Windows NT® functions **CreateThread** and **EndThread** instead, memory leaks will occur because the Driver Manager and some ODBC drivers call C run-time functions that will not work on a thread created by calling **CreateThread**. For more information, see the Microsoft Windows® documentation.
