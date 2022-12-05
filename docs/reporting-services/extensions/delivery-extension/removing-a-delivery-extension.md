---
title: "Removing a Delivery Extension | Microsoft Docs"
description: "Learn how to remove a delivery extension from Reporting Services so the report server doesn't list it as available and deactivates subscriptions that use it."
ms.date: 03/06/2017
ms.prod: reporting-services
ms.technology: extensions


ms.topic: reference
helpviewer_keywords: 
  - "removing delivery extensions"
  - "deleting delivery extensions"
  - "delivery extensions [Reporting Services], removing"
ms.assetid: dcb7caf2-d19a-4bc5-afb3-2b61ad11cac5
author: maggiesMSFT
ms.author: maggies
---
# Removing a Delivery Extension
  To remove a [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] delivery extension, simply remove the **Extension** element for your delivery extension from the configuration file. After the configuration information is removed, the delivery extension is no longer available to the report server.  
  
 Once a delivery extension's corresponding **Extension** element is removed from the configuration file, it is no longer registered with the report server. The report server removes the entry from the list of delivery extensions and deactivates any subscriptions which use that delivery extension. When a delivery extension is removed, users are no longer able to select it as a method of notification.  
  
## See Also  
 [Implementing a Delivery Extension](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Reporting Services Extension Library](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
