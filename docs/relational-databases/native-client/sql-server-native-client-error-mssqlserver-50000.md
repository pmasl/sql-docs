---
description: "SQL Server Native Client Error MSSQLSERVER_50000"
title: "MSSQLSERVER_50000 | Microsoft Docs"
ms.custom: ""
ms.date: "03/14/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: native-client
ms.topic: "reference"
helpviewer_keywords: 
  - "50000 [SQL Server Native Client setup error]"
ms.assetid: 5426d87a-d5d9-4984-b211-b07d69e834a2
author: markingmyname
ms.author: maghan
monikerRange: ">=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"
---
# SQL Server Native Client Error MSSQLSERVER_50000
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!IMPORTANT]
> [!INCLUDE[snac-removed-oledb-and-odbc](../../includes/snac-removed-oledb-and-odbc.md)]
    
## Details  
  
| Attribute | Value |
| --------- | ----- |
|Product Name|SQL Server|  
|Product Version|11.0|  
|Event ID|50000|  
|Event Source|SETUP|  
|Component|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client|  
|Symbolic Name||  
|Message Text|A network error occurred while attempting to read from the file '%.*ls'.|  
  
## Explanation  
 An attempt was made to install (or update) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client on a computer where [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client is already installed, and where the existing installation was from an MSI file that was renamed from sqlncli.msi.  
  
## User Action  
 To resolve this error, uninstall the existing version of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. To prevent this error, do not install [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client from an MSI file that is not named sqlncli.msi. 