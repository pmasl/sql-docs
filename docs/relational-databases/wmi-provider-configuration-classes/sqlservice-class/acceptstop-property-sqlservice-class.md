---
description: "AcceptStop Property (SqlService Class)"
title: "AcceptStop Property (SqlService)"
ms.custom: seo-lt-2019
ms.date: "03/06/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: wmi
ms.topic: "reference"
apiname: 
  - "AcceptStop Property (SqlService Class)"
apilocation: 
  - "sqlmgmproviderxpsp2up.mof"
helpviewer_keywords: 
  - "AcceptStop property"
ms.assetid: bf8ffe79-4f4c-4a2d-82e5-2ae8f5d466c5
author: markingmyname
ms.author: maghan
---
# AcceptStop Property (SqlService Class)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Gets the Boolean property value that specifies whether the service can be stopped.  
  
## Syntax  
  
```  
  
object.AcceptStop [= value]  
```  
  
## Parts  
 *object*  
 A [SqlService Class](../../../relational-databases/wmi-provider-configuration-classes/sqlservice-class/sqlservice-class.md) object that represents the service  
  
## Property Value/Return Value  
 A Boolean value that specifies whether the service can be stopped: **true** if the service can be stopped, or **false** if the service cannot be stopped.  
  
## Remarks  
  
## See Also  
 [Starting and Stopping Services](https://technet.microsoft.com/library/ms174886\(v=sql.105\).aspx)  
  
  
