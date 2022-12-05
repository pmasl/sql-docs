---
description: "STNumCurves (geometry Data Type)"
title: "STNumCurves (geometry Data Type) | Microsoft Docs"
ms.custom: ""
ms.date: "08/03/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: t-sql
ms.topic: reference
dev_langs: 
  - "TSQL"
helpviewer_keywords: 
  - "STNumCurves method (geometry)"
ms.assetid: 20c2fa0b-656b-4519-b34c-cc8f094290d4
author: MladjoA
ms.author: mlandzic 
---
# STNumCurves (geometry Data Type)
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

This method returns the number of curves in a **geometry** instance when the instance is a one-dimensional spatial data type. One-dimensional spatial data types include **LineString**, **CircularString**, and **CompoundCurve**. `STNumCurves`() works only on simple types; it does not work with **geometry** collections like **MultiLineString**.
  
## Syntax  
  
```  
  
.STNumCurves()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## Return Types
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] return type: **geometry**  
  
 CLR return type: **SqlGeometry**  
  
## Remarks  
 An empty one-dimensional **geometry** instance returns 0. **NULL** is returned when the **geometry** instance is not a one-dimensional instance or is an uninitialized instance.  
  
## Examples  
  
### A. Using STNumCurves() on a CircularString instance  
 The following example shows how to get the number of curves in a `CircularString` instance:  
  
```sql
 DECLARE @g geometry;  
 SET @g = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)');  
 SELECT @g.STNumCurves();
 ```  
  
### B. Using STNumCurves() on a CompoundCurve instance  
 The following example uses `STNumCurves()` to return the number of curves in a `CompoundCurve` instance.  
  
```sql
 DECLARE @g geometry;  
 SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))');  
 SELECT @g.STNumCurves();
 ```  
  
## See Also  
 [Spatial Data Types Overview](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [OGC Methods on Geometry Instances](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

