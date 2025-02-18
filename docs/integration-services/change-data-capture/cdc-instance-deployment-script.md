---
title: "CDC Instance Deployment Script"
description: "CDC Instance Deployment Script"
author: chugugrace
ms.author: chugu
ms.date: "03/14/2017"
ms.service: sql
ms.subservice: integration-services
ms.topic: conceptual
---
# CDC Instance Deployment Script

[!INCLUDE [oracle-cdc-retirement](../includes/attunity-oracle-cdc-retirement.md)]

  The CDC Instance Deployment Script dialog box that displays the CDC instance deployment script. This script can be used to re-create the CDC database with all of its artifacts on a different [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.  
  
 At the completion of the deployment script, you should make sure of the following:  
  
-   The deployment script assumes that the target [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance was prepared for Oracle CDC, by using the Oracle CDC Service Configuration Console or by using **prepare script** that program creates.  
  
-   The part of the script that is used to enable the database for CDC needs to be run by a `sysadmin`.  
  
-   The script does not preserve the Oracle log-mining password. This needs to be set manually after the script is run and the Oracle CDC Service is started.  
  
 Select the following options in the **CDC Instance Deployment Script** dialog box.  
  
 **Save As**  
 Saves the script in a text file that you can save in any location you want. You can copy the file with the script to any other server to run it there.  
  
 **Copy**  
 Copies the script to the clipboard. You can then paste the script into the [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] or any text editor to run the scripts later.  
  
## See Also  
 [Prepare SQL Server for CDC](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md)  
  
  
