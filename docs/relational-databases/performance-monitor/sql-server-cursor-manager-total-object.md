---
title: "SQL Server, Cursor Manager Total object"
description: "Learn about the SQLServer:Cursor Manager Total object, which provides counters to monitor cursors in SQL Server."
author: MikeRayMSFT
ms.author: mikeray
ms.date: 12/04/2023
ms.service: sql
ms.subservice: performance
ms.topic: reference
helpviewer_keywords:
  - "SQLServer:Cursor Manager Total"
  - "Cursor Manager Total object"
---
# SQL Server, Cursor Manager Total object
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  The **SQLServer:Cursor Manager Total** object provides counters to monitor cursors.  
  
 This table describes the SQL Server **Cursor Manager Total** counters.  
  
|Cursor Manager Total counters|Description|  
|-----------------------------------|-----------------|  
|**Async population count**|Number of cursors being populated asynchronously.|  
|**Cursor conversion rate**|Number of cursor conversions per second.|  
|**Cursor flushes**|Total number of run-time statement recreations by cursors.|  

## Example

You begin to explore the query performance counters in this object using this T-SQL query on the [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) dynamic management view:

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name LIKE '%Cursor Manager Total%';
```  
  
## Related content

- [Monitor Resource Usage (Performance Monitor)](monitor-resource-usage-system-monitor.md)
