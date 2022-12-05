---
description: "MultiIpConfigurationSupport Property (ServerNetworkProtocol Class)"
title: "MultiIpConfigurationSupport Property (ServerNetworkProtocol)"
ms.custom: seo-lt-2019
ms.date: "03/14/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: wmi
ms.topic: "reference"
apiname: 
  - "MultiIpConfigurationSupport Property (ServerNetworkProtocol"
apilocation: 
  - "sqlmgmproviderxpsp2up.mof"
apitype: "MOFDef"
helpviewer_keywords: 
  - "MultiIpConfigurationSupport property"
ms.assetid: 442c6133-4038-42db-a67d-2631285ac76b
author: markingmyname
ms.author: maghan
---
# MultiIpConfigurationSupport Property (ServerNetworkProtocol Class)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Gets the Boolean property that specifies whether multiple IP addresses are supported by a server network protocol.  
  
## Syntax  
  
```  
  
object.MultiIpConfigurationSupport [= value]  
```  
  
## Parts  
 *object*  
 A [ProtocolName Property (ServerNetworkProtocol Class)](../../../relational-databases/wmi-provider-configuration-classes/servernetworkprotocol-class/protocolname-property-servernetworkprotocol-class.md) object that represents the network protocol used by the instance of [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## Property Value/Return Value  
 A Boolean value that specifies whether multiple IP addresses are supported by the server network protocol: **true** if multiple IP addresses are supported by the server network protocol, or **false** if multiple IP addresses are not supported by the server network protocol.  
  
## Remarks  
  
## See Also  
 [Configuring Server Network Protocols and Net-Libraries](https://msdn.microsoft.com/library/ms177485\(v=sql.100\).aspx)  
  
  
