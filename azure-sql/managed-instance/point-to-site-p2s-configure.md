---
title: Configure point-to-site connectivity using SSMS
titleSuffix: Azure SQL Managed Instance
description: Connect to Azure SQL Managed Instance with SQL Server Management Studio (SSMS) by using a point-to-site connection from an on-premises client computer.
author: zoran-rilak-msft
ms.author: zoranrilak
ms.reviewer: mathoma, bonova, jovanpop
ms.date: 09/27/2023
ms.service: azure-sql-managed-instance
ms.subservice: deployment-configuration
ms.topic: quickstart
ms.custom: mode-other
---
# Quickstart: Configure a point-to-site connection to Azure SQL Managed Instance from on-premises

[!INCLUDE [appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

This quickstart teaches you how to connect to Azure SQL Managed Instance by using [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS) from an on-premises client computer over a point-to-site connection. For information about point-to-site connections, see [About Point-to-Site VPN](/azure/vpn-gateway/point-to-site-about).

## Prerequisites

This quickstart:

- Uses the resources created in  [Create a managed instance](instance-create-quickstart.md) as its starting point.
- Requires PowerShell 5.1 and Azure PowerShell 1.4.0 or later on your on-premises client computer. If necessary, see the instructions to [install the Azure PowerShell module](/powershell/azure/install-az-ps#install-the-azure-powershell-module).
- Requires the newest version of [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) on your on-premises client computer.

## Attach a VPN gateway to a virtual network

1. Open PowerShell on your on-premises client computer.

1. Copy the following PowerShell script to attach  a VPN gateway to the SQL Managed Instance virtual network that you created in the [Create a managed instance](instance-create-quickstart.md) quickstart. This script uses the Azure PowerShell Az Module and does the following for either Windows or Linux-based hosts:

   - Creates and installs certificates on a client machine
   - Calculates the future VPN gateway subnet IP range
   - Creates the gateway subnet
   - Deploys the Azure Resource Manager template that attaches the VPN gateway to the VPN subnet

     ```powershell
     $scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/attach-vpn-gateway'

     $parameters = @{
       subscriptionId = '<subscriptionId>'
       resourceGroupName = '<resourceGroupName>'
       virtualNetworkName = '<virtualNetworkName>'
       certificateNamePrefix  = '<certificateNamePrefix>'
       }

     Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/attachVPNGateway.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters, $scriptUrlBase
     ```

1. Paste the script in your PowerShell window and provide the required parameters. The values for `<subscriptionId>`, `<resourceGroup>`, and `<virtualNetworkName>` should match the ones that you used for the [Create a managed instance](instance-create-quickstart.md) quickstart. The value for `<certificateNamePrefix>` can be a string of your choice.

    > [!NOTE]  
    > If you get an error about parsing the Internet Explorer engine, either launch Internet Explorer to complete the initial set up or upgrade to a newer version of PowerShell.

1. Execute the PowerShell script.

> [!IMPORTANT]  
> Do not continue until the PowerShell script completes.

## Create a VPN connection

1. Sign in to the [Azure portal](https://portal.azure.com/).
1. Open the resource group where you created the virtual network gateway, and then open the virtual network gateway resource.
1. Select **Point-to-site configuration** and then select **Download VPN client**.

    :::image type="content" source="./media/point-to-site-p2s-configure/download-vpn-client.png" alt-text="Screenshot of the Gateway Point to site configuration page for the virtual network of your SQL managed instance in the Azure portal." lightbox="./media/point-to-site-p2s-configure/download-vpn-client.png":::

1. On your on-premises client computer, extract the files from the zip file and then open the folder with the extracted files.
1. Open the **WindowsAmd64** folder and open the **VpnClientSetupAmd64.exe** file.
1. If you receive a **Windows protected your PC** message, select **More info** and then select **Run anyway**.

    :::image type="content" source="./media/point-to-site-p2s-configure/vpn-client-defender.png" alt-text="Screenshot of Windows Defender asking if you're sure you want to install the VPN client.":::

1. In the User Account Control dialog box, select **Yes** to continue.
1. In the dialog box referencing your virtual network, select **Yes** to install the VPN client for your virtual network.

## Connect to the VPN connection

1. Go to **VPN** in **Network & Internet** on your on-premises client computer and select your SQL managed instance virtual network to establish a connection to this VNet. In the following image, the VNet is named **MyNewVNet**:

    :::image type="content" source="./media/point-to-site-p2s-configure/vpn-connection.png" alt-text="Screenshot of the Windows VPN connection screen.":::

1. Select **Connect**.
1. In the dialog box, select **Connect**.

    :::image type="content" source="./media/point-to-site-p2s-configure/vpn-connection2.png" alt-text="Screenshot of the VPN that highlights the Connect button.":::

1. When you're prompted that Connection Manager needs elevated privileges to update your route table, choose **Continue**.
1. Select **Yes** in the User Account Control dialog box to continue.

   You've established a VPN connection to your SQL managed instance VNet.

    :::image type="content" source="./media/point-to-site-p2s-configure/vpn-connection-succeeded.png" alt-text="Screenshot of the Windows VPN connection screen that highlights the Connected message when you've established your connection.":::

## Connect with SSMS

1. On the on-premises client computer, open SQL Server Management Studio (SSMS).
1. In the **Connect to Server** dialog box, enter the fully qualified **host name** for your SQL managed instance in the **Server name** box.
1. Select **SQL Server Authentication**, provide your username and password, and then select **Connect**.

    :::image type="content" source="./media/point-to-site-p2s-configure/ssms-connect.png" alt-text="Screenshot of the Connect to Server dialog box in SSMS.":::

After you connect, you can view your system and user databases in the Databases node. You can also view various objects in the Security, Server Objects, Replication, Management, SQL Server Agent, and XEvent Profiler nodes.

## Connection could not be established 

If your connection works initially but after some time, you see the error `The connection could not be established` when you try to connect to the VPN, follow these steps: 

1. Open your Windows **VPN Settings**. 
1. Remove the VPN connection. 
1. Repeat the steps in [Create a VPN connection](#create-a-vpn-connection) to download the VPN client and install it again. 
1. Connect to the VPN. 

## Next steps

- For a quickstart showing how to connect from an Azure virtual machine, see [Configure a point-to-site connection](point-to-site-p2s-configure.md).
- For an overview of the connection options for applications, see [Connect your applications to SQL Managed Instance](connect-application-instance.md).
- To restore an existing SQL Server database from on-premises to a managed instance, you can use [Azure Database Migration Service for migration](/azure/dms/tutorial-sql-server-to-managed-instance) or the [T-SQL RESTORE command](restore-sample-database-quickstart.md) to restore from a database backup file.
