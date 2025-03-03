---
title: "RsReportServer.config configuration file"
description: Learn about the configuration file that stores settings that are used by the Report Server web service and background processing.
author: maggiesMSFT
ms.author: maggies
ms.date: 09/25/2024
ms.service: reporting-services
ms.subservice: report-server
ms.topic: conceptual
ms.custom:
  - updatefrequency5
---
# RsReportServer.config configuration file
The [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] `RsReportServer.config` file stores settings that are used by the Report Server web service and background processing. All [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] applications run within a single process that reads configuration settings stored in the `RSReportServer.config` file. Both native mode and SharePoint mode report servers use the `RSReportServer.config`, however the two modes don't use all of the same settings in the configuration file. The SharePoint mode version of the file is smaller as many of the settings for SharePoint mode are stored in SharePoint configuration databases rather than the file. This article describes the default configuration file that is installed for native mode and SharePoint mode. The article also describes some of the important settings and behaviors that the configuration file controls.  

In SharePoint mode, the configuration file contains those settings that apply to all service application instances running on that computer. The SharePoint configuration database contains configuration settings that apply to specific service applications. The settings that are stored in the Configuration database and managed through the SharePoint management pages can be different for each [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] service application.  
  
 The settings are presented in following content in the order in which they appear in the configuration file that is installed by default. For instructions on how to edit this file, see [Modify a Reporting Services configuration file &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
 
##  <a name="bkmk_file_location"></a> File location  

The `RSReportServer.config` is located in the following folders, depending on the report server mode:  


  
### Native mode report server 

 
[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

```  
C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
```

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)]

```  
C:\Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer
```  

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

```  
C:\Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer
```  
  
### SharePoint mode report server 

> [!NOTE]
> Reporting Services integration with SharePoint is no longer available after SQL Server 2016.
  
```  
C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting  
```  
 
For more information on editing the file, see [Modify a Reporting Services configuration file &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
##  <a name="bkmk_generalconfiguration"></a> General configuration settings (rsreportserver.config)  
 The following table provides information about general configuration settings that appear in the first part of the file. Settings are presented in the order in which they appear in the configuration file. The last column of the table indicates if the setting applies to a native mode report server **(N)** or a SharePoint mode report server **(S)** or both.  
  
> [!NOTE]  
>  In this article, "maximum integer" refers to `INT_MAX` value of 2147483647.  For more information, see [Integer limits](https://msdn.microsoft.com/library/296az74e\(v=vs.110\).aspx) (https://msdn.microsoft.com/library/296az74e(v=vs.110).aspx).  
  
|Setting|Description|Mode|  
|-------------|-----------------|----------|  
|**Dsn**|Specifies the connection string to the database server that hosts the report server database. This value is encrypted and is added to the configuration file when you create the report server database. For SharePoint, the database connection information is taken from the SharePoint configuration database.|N,S|  
|**ConnectionType**|Specifies the type of credentials that the report server uses to connect to the report server database. Valid values are **Default** and **Impersonate**. **Default** is specified if the report server is configured to use a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sign in or the service account to connect to the report server database. **Impersonate** is specified if the report server uses a Windows account to connect to the report server database.|N|  
|**LogonUser, LogonDomain, LogonCred**|Stores the domain, user name, and password of a domain account that is used by a report server to connect to a report server database. Values for **LogonUser**, **LogonDomain**, and **LogonCred** are created when the report server connection is configured to use a domain account. For more information about a report server database connection, see [Configure a report server database connection  &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).|N|  
|**InstanceID**|An identifier for the report server instance. Report server instance names are based on [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance names. This value specifies a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance name. By default, this value is `MSRS12_\<instancename>_`. Don't modify this setting. The following example shows the complete value: `<InstanceId>MSRS13.MSSQLSERVER</InstanceId>`<br /><br /> The following example shows a SharePoint mode value:<br /><br /> `<InstanceId>MSRS12.@Sharepoint</InstanceId>`|N,S|  
|**InstallationID**|An identifier for the report server installation that Setup creates. This value is set to a GUID. Don't modify this setting.|N|  
|**SecureConnectionLevel**|Specifies the degree to which web service calls must use Transport Layer Security (TLS), previously known as Secure Sockets Layer (SSL). This setting is used for both the Report Server web service and the web portal. This value is set when you configure a URL to use HTTP or HTTPS in the [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration tool. In SQL Server 2008 R2, **SecureConnectionLevel** is made an on/off switch. For earlier versions than SQL Server 2008 R2, the valid values range from 0 through 3, where 0 is least secure. For more information, see [ConfigurationSetting method - SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md), [Use secure web service methods](../../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md) and [Configure TLS connections on a native mode report server](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).|N,S|
|**DisableSecureFormsAuthenticationCookie**|Default value is **False**.<br /><br /> Specifies whether to disable the forcing of the cookie used for form and custom authentication to be marked secure. As early as SQL Server 2012, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] automatically marks forms authentication cookies used with custom authentication extensions, as a secure cookie when sent to the client. When report server administrators and custom security extension authors change this property, they can revert to the previous behavior. The previous behavior allowed the custom security extension author to determine whether to mark the cookie as a secure cookie. You should use secure cookies for forms authentication to help prevent network sniffing and replay attacks.|N|  
|**CleanupCycleMinutes**|Specifies the number of minutes after which old sessions and expired snapshots are removed from the report server databases. Valid values range from 1 to maximum integer. The default is 10. |N,S|  
|**MaxActiveReqForOneUser**|Specifies the maximum number of reports that one user can process at the same time. Once the limit is reached, further report processing requests are denied. Valid values are 1 to a maximum integer. The default is 20.<br /><br /> Most requests process very quickly so it's unlikely that a single user has more than 20 open connections at any given time. If users are opening more than 15 process-intensive reports at the same time, you might need to increase this value.<br /><br /> This setting is ignored for report servers that run in SharePoint integrated mode.|N,S|  
|**MaxActiveReqForAnonymous**|Specifies the maximum number of anonymous requests that can be in process at the same time. Once the limit is reached, further processing requests are denied. Valid values are 1 to a maximum integer. The default is 200.
|**DatabaseQueryTimeout**|Specifies the number of seconds after which a connection to the report server database times out. This value is passed to the `System.Data.SQLClient.SQLCommand.CommandTimeout` property. Valid values range from 0 to 2147483647. The default is 120. A value of 0 specifies an unlimited wait time and therefore isn't recommended.|N|  
|**AlertingCleanupCycleMinutes**|The default is 20.<br /><br /> Determines how often the cleanup of temporary data stored in the Alerting database occurs.|S|  
|**AlertingDataCleanupMinutes**|The default is 360.<br /><br /> Determines how long session data used for creating or editing an alert definition is retained within the Alerting database. Default is 6 hours.|S|  
|**AlertingExecutionLogCleanupMinutes**|The default is 10080.<br /><br /> Determines how long to keep Alerting execution log values. Default is seven days.|S|  
|**AlertingMaxDataRetentionDays**|The default is 180.<br /><br /> Determines how long to keep alert data required to prevent duplicate alert messages when the data for the alert isn't changed.|S|  
|**RunningRequestsScavengerCycle**|Specifies how often orphaned and expired requests are canceled. This value is specified in seconds. Valid values range from 0 to maximum integer. The default is 60.|N,S|  
|**RunningRequestsDbCycle**|Specifies how often the report server evaluates running jobs to check whether they exceeded report execution time outs, and when to present running job information in the **Manage Jobs** page of the web portal. This value is specified in seconds. Valid values range from 0 to 2147483647. The default is 60.|N,S|  
|**RunningRequestsAge**|Specifies an interval in seconds after which the status of a running job changes from new to running. Valid values range from 0 to 2147483647. The default is 30.|N,S|  
|**MaxScheduleWait**|Specifies the number of seconds the Report Server Windows service waits for a schedule to be updated by [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent service when **Next Run Time** is requested. Valid values range from 1 to 60.<br /><br /> In the default configuration file, **MaxScheduleWait** is set to **5**.<br /><br /> If the report server can't find or read the configuration file, the server defaults **MaxScheduleWait** to 1.|N,S|  
|**DisplayErrorLink**|Indicates whether a link to the [!INCLUDE[msCoName](../../includes/msconame-md.md)] Help and Support site is displayed when errors occur. This link appears in error messages. Users can select the link to open updated error message content on the site. Valid values include **True** (default) and **False**.|N,S|  
|**WebServiceuseFileShareStorage**|Specifies whether to store cached reports and temporary snapshots (created by the Report Server Web service during a user session) on the file system. Valid values are **True** and **False** (default). If the value is set to false, temporary data is stored in the reportservertempdb database.|N,S|  
|**ProcessTimeout**|Specifies the number of seconds the report server Process Monitor waits for any service activity operation to complete before stopping the service. Valid values range from 0 to maximum integer. The default is 150. This setting is disabled by default and can be enabled by removing the comment syntax (`<!-- and -->`).|N|  
|**ProcessTimeoutGcExtension**|Specifies the number of seconds the report server Process Monitor waits for a service activity operation to complete before stopping the service. This setting applies only if .NET Garbage Collection is in progress and the **ProcessTimeout** value was reached. Valid values range from 0 to maximum integer. The default is 30. This setting is disabled by default and can be enabled by removing the comment syntax (`<!-- and -->`).|N|  
|**WatsonFlags**|Specifies how much information is logged for error conditions that are reported to [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> `0x0430 = a full dump`<br /><br /> `0x0428 =a minidump`<br /><br /> `0x0002 = no dump`|N,S|  
|**WatsonDumpOnExceptions**|Specifies a list of exceptions that you want to report in an error log. This list is useful when you have a recurring issue and want to create a dump with information to send to [!INCLUDE[msCoName](../../includes/msconame-md.md)] for analysis. Creating dumps affects performance, so change this setting only when you're diagnosing a problem.|N,S|  
|**WatsonDumpExcludeIfContainsExceptions**|Specifies a list of exceptions that you don't want to report in an error log. This list is useful when you're diagnosing a problem and don't want the server to create dumps for a specific exception.|N,S|  
  
##  <a name="bkmk_URLReservations"></a> URLReservations (RSReportServer.config file)  
 **URLReservations** defines HTTP access to the Report Server web service and the web portal for the current instance. URLs are reserved and stored in `HTTP.SYS` when you configure the report server.  
  
> [!WARNING]  
>  For SharePoint mode, URL reservations are configured in SharePoint Central Administration. For more information, see [Configure alternate access mapping](https://technet.microsoft.com/library/cc263208\(office.12\).aspx).  
  
 Don't modify URL reservations in the configuration file directly. Always use the [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration Manager or the Report Server WMI provider to create or modify URL reservations for a Native mode report server. If you modify the values in the configuration file, you might corrupt the reservation, which causes server errors at run time or leave orphan reservations in `HTTP.SYS` that aren't removed if you uninstall the software. For more information, see [Configure report server URLs  &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) and [URLs in configuration files  &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md).  
  
 **URLReservations** is an optional element. If it isn't present in the `RSReportServer.config` file, the server might not be configured. If the element is specified, all child elements except for `AccountName` are required.  
  
 The last column of the table indicates if the setting applies to a Native mode report server (N) or a SharePoint mode report server (S) or both.  
  
|Setting|Description|Mode|  
|-------------|-----------------|----------|  
|**Application**|Contains settings for [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] applications.|N|  
|**Name**|Specifies the [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] applications. Valid values are ReportServerWebService or ReportManager.|N|  
|**VirtualDirectory**|Specifies the virtual directory name of the application.|N|  
|**URLs, URL**|Contains one or more URL reservations for the application.|N|  
|**UrlString**|Specifies URL syntax that is valid for HTTP.SYS. For more information about the syntax, see [URL Reservation Syntax  &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md).|N|  
|**AccountSid**|Specifies the security identifier (SID) of the account for which the URL reservation was created. This account should be the account under which the Report Server service runs. If the SID doesn't match the service account, the report server might not be able to listen for requests on that URL.|N|  
|**AccountName**|Specifies a readable account name that corresponds to the **AccountSid**. The name isn't used, but it appears in the file so that you can easily determine the service account for the account that is used for URL reservation.|N|  
  
##  <a name="bkmk_Authentication"></a> Authentication (RSReportServer.config file)  
 **Authentication** specifies one or more authentication types accepted by the report server. The default settings and values are a subset of the settings and values that are possible for this section. Only the default settings are added automatically. To add other settings, you must use a text editor to add the element structure to the `RSReportServer.config` file and set the values.  
  
 Default values include **RSWindowsNegotiate** and **RSWindowsNTLM** with **EnableAuthPersistance** set to **True**:  
  
```  
   <Authentication>  
      <AuthenticationTypes>  
         <RSWindowsNegotiate/>  
         <RSWindowsNTLM/>  
      </AuthenticationTypes>  
      <EnableAuthPersistence>true</EnableAuthPersistence>  
   </Authentication>  
```  
  
 All other values must be added manually. For more information and examples, see [Authentication with the Report Server](../../reporting-services/security/authentication-with-the-report-server.md).  
  
 The last column of the following table indicates if the setting applies to a Native mode report server (N) or a SharePoint mode report server (S) or both.  
  
|Setting|Description|Mode|  
|-------------|-----------------|----------|  
|**AuthenticationTypes**|Specifies one or more authentication types. Valid values are: **RSWindowsNegotiate**, **RSWindowsKerberos**, **RSWindowsNTLM**, **RSWindowsBasic**, and **Custom**.<br /><br /> **RSWindows** types and **Custom** are mutually exclusive.<br /><br /> **RSWindowsNegotiate**, **RSWindowsKerberos**, **RSWindowsNTLM**, and **RSWindowsBasic** are cumulative and can be used together, as illustrated in the default value example earlier in this section.<br /><br /> Specifying multiple authentication types is necessary if you expect requests from various client applications or browsers that use different types of authentication.<br /><br /> Don't remove **RSWindowsNTLM**, otherwise you limit browser support to a portion of the supported browser types. For more information, see [Browser support for Reporting Services](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).|N|  
|**RSWindowsNegotiate**|The report server accepts either Kerberos or NTLM security tokens. This setting is the default setting when the report server is running in native mode and the service account is Network Service. This setting is omitted when the report server is running in native mode and the service account is configured as a domain user account.<br /><br /> This setting might prevent users from logging on to the server. This result happens If you configure a domain account for the Report Server Service account and don't configure a Service Principal Name (SPN) for the report server.|N|  
|**RSWindowsNTLM**|The server accepts NTLM security tokens.<br /><br /> If you remove this setting, browser support is limited for some of the supported browser types. For more information, see [Browser support for Reporting Services](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).|N, S|  
|**RSWindowsKerberos**|The server accepts Kerberos security tokens.<br /><br /> Use this setting or RSWindowsNegotiate when you use Kerberos authentication in a constrained delegation authentication scheme.|N|  
|**RSWindowsBasic**|The server accepts Basic credentials and issues a challenge/response when a connection is made without credentials.<br /><br /> Basic authentication passes credentials in the HTTP requests in clear text. If you use Basic authentication, use TLS to encrypt network traffic to and from the report server. To view example configuration syntax for Basic authentication in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], see [Authentication with the Report Server](../../reporting-services/security/authentication-with-the-report-server.md).|N|  
|**Custom**|Specify this value if you deployed a custom security extension on the report server computer. For more information, see [Implementing a security extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).|N|  
|**LogonMethod**|This value specifies the sign in type for **RSWindowsBasic**. If you specify **RSWindowsBasic**, this value is required. Valid values are 2 or 3, where each value represents:<br /><br /> **2** = Network logon high-performance servers to authenticate plaintext passwords<br /><br /> **3** = Cleartext logon, which preserves logon credentials in the authentication package that is sent with each HTTP request, allowing the server to impersonate the user when connecting to other servers in the network.<br /><br /> <br /><br /> Note: Values 0 (for interactive logon) and 1 (for batch logon) aren't supported in [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)].|N|  
|**Realm**|This value is used for **RSWindowsBasic**. It specifies a resource partition that includes authorization and authentication features used to control access to protected resources in your organization.|N|  
|**DefaultDomain**|This value is used for **RSWindowsBasic**. The value is used to determine the domain used by the server to authenticate the user. This value is optional, but if you omit it, the report server uses the computer name as the domain. If you installed the report server on a domain controller, the domain used is the one controlled by the computer.|N|  
|**RSWindowsExtendedProtectionLevel**|The Default value is **off**. For more information, see [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)|N|  
|**RSWindowsExtendedProtectionScenario**|The default value is **Proxy**|N|  
|**EnableAuthPersistence**|Determines whether authentication is performed on the connection or for every request.<br /><br /> Valid values are **True** (default) or **False**. If set to **True**, subsequent requests from the same connection assume the impersonation context of the first request.<br /><br /> This value must be set to **False** if you're using proxy server software (such as ISA Server) to access the report server. Using a proxy server allows a single connection from the proxy server to be used by multiple users. For this scenario, you should disable authentication persistence so that each user request can be authenticated separately. If you don't set **EnableAuthPersistence** to **False**, all users connect using the impersonation context of the first request.|N,S|  
  
##  <a name="bkmk_service"></a> Service (RSReportServer.config file)  
 **Service** specifies the application settings that apply to the service as a whole.  
  
 The last column of the following table indicates if the setting applies to a Native mode report server (N), a SharePoint mode report server (S), or Power BI Report Server (P).  

  
|Setting|Description|Mode|  
|-------------|-----------------|----------|  
|**IsSchedulingService**|Specifies whether the report server maintains a set of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent jobs that correspond to schedules and subscriptions created by [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] users. Valid values include **True** (default) and **False**.<br /><br /> This setting is affected when you enable or disable [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] features using the Surface Area Configuration for Reporting Services facet of Policy-Based Management. For more information, see [Start and stop the Report Server service](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsNotificationService**|Specifies whether the report server processing notifications and deliveries. Valid values include **True** (default) and **False**. When the value is **False**, subscriptions aren't delivered.<br /><br /> This setting is affected when you enable or disable [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] features using the Surface Area Configuration for Reporting Services facet of Policy-Based Management. For more information, see [Start and stop the Report Server service](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsEventService**|Specifies whether service processes events in the event queue. Valid values include **True** (default) and **False**. When the value is **False**, the report server doesn't perform operations for schedules or subscriptions.<br /><br /> This setting is affected when you enable or disable [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] features using the Surface Area Configuration for Reporting Services facet of Policy-Based Management. For more information, see [Start and stop the Report Server Service](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsAlertingService**|The default value is **True**|S|  
|**PollingInterval**|Specifies the interval, in seconds, between polls of the event table by the report server. Valid values range from 0 to maximum integer. The default is 10.|N,S,P|  
|**IsDataModelRefreshService**|Specifies whether service processes scheduled data model refresh events for Power BI reports. Valid values include **True** (default) and **False**. When the value is **False**, the report server doesn't perform operations for scheduled data model refresh.|N|  
|**WindowsServiceUseFileShareStorage**|Specifies whether to store cached reports and temporary snapshots (created by the Report Server service during a user session) on the file system. Valid values are **True** and **False** (default).|N,S,P|  
|**MemorySafetyMargin**|Specifies a percentage of **WorkingSetMaximum** that defines the boundary between medium and low-pressure scenarios. The default value is 80. For more information about **WorkingSetMaximum** and configuring available memory, see [Configure available memory for report server applications](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**MemoryThreshold**|Specifies a percentage of **WorkingSetMaximum** that defines the boundary between high and medium pressure scenarios. The default value is **90**. This value should be greater than the value set for **MemorySafetyMargin**. For more information, see [Configure available memory for report server applications](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**WorkingSetMaximum**|Specifies a memory threshold after which no new memory allocation requests are granted to report server applications. By default, the report server sets WorkingSetMaximum to the amount of available memory on the computer. This value is detected when the service starts. This setting doesn't appear in the `RSReportServer.config` file unless you add it manually. Valid values range from 0 to maximum integer. This value is expressed in kilobytes. For more information, see [Configure available memory for report server applications](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N|  
|**WorkingSetMinimum**|Specifies a lower limit for memory consumption. The report server doesn't release memory if overall memory use is below this limit. By default, the value is calculated at service startup and the initial memory allocation request is for 60 percent of **WorkingSetMaximum**. This setting doesn't appear in the `RSReportServer.config` file unless you add it manually. If you want to customize this value, you must add the **WorkingSetMaximum** element to the `RSReportServer.config` file. Valid values range from 0 to maximum integer. This value is expressed in kilobytes. For more information, see [Configure available memory for report server applications](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N| 
|**RecycleTime**|Specifies a recycle time for the application domain, measured in minutes. Valid values range from 0 to maximum integer. The default is 720.|N,S|  
|**MaxAppDomainUnloadTime**|Specifies an interval during which the application domain is allowed to unload during a recycle operation. If recycling doesn't complete during this time period, all processing in the application domain is stopped. For more information, see [Application domains for report server applications](../../reporting-services/report-server/application-domains-for-report-server-applications.md).<br /><br /> This value is specified in minutes. Valid values range from 0 to maximum integer. The default is **30**.|N,S,P|  
|**MaxQueueThreads**|Specifies the number of threads used by the Report Server Windows service for concurrent processing of subscriptions and notifications. Valid values range from 0 to maximum integer. The default is 0. If you choose 0, the report server determines the maximum number of threads. If you specify an integer, the value you specify sets the upper limit on threads that can be created at one time. For more information about how the Report Server Windows service manages memory for running processes, see [Configure available memory for report server applications](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**UrlRoot**|Used by the report server delivery extensions to compose URLs that are used by reports delivered in email and file share subscriptions. Also used by report processing when resolving expressions using `Globals!ReportServerUrl`. The value must be a valid URL address to the report server from which the published report is accessed. Used by the report server to generate URLs for offline or unattended access. These URLs are used in exported reports, and by delivery extensions to compose a URL that is included in delivery messages such as links in emails. The report server determines URLs in reports based on the following behavior:<br /><br /> When **UrlRoot** is blank, which is the default value, and there are URL reservations, the report server automatically determines URLs the same way that URLs are generated for the `ListReportServerUrls` method. The first URL returned by the `ListReportServerUrls` method is used. Or, if `SecureConnectionLevel` is greater than zero (0), the first TLS URL is used.<br /><br /> When **UrlRoot** is set to a specific value, the explicit value is used.<br /><br /> When **UrlRoot** is blank and there are no URL reservations configured, the URLs in rendered reports and in email links are incorrect.|N,S,P|  
|**UnattendedExecutionAccount**|Specifies a user name, password, and domain used by the report server to run a report. These values are encrypted. Use the [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration tool or the **rsconfig** utility to set these values. For more information, see [Configure the Unattended Execution Account &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).<br /><br /> For SharePoint mode, you set the execution account for a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] service application using SharePoint Central Administration. For more information, see [Manage a Reporting Services SharePoint Service Application](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)|N,P|  
|**PolicyLevel**|Specifies the security policy configuration file. The valid value is Rssrvrpolicy.config. For more information, see [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|N,S,P|  
|**IsWebServiceEnabled**|Specifies whether the Report Server Web service responds to SOAP and URL access requests. This value is set when you enable or disable service using the Surface Area Configuration for Reporting Services facet of Policy-Based Management.|N,S|  
|**IsReportManagerEnabled**|This setting is deprecated as of SQL Server 2016 Reporting Services Cumulative Update 2. The web portal is always enabled.|N|  
|**FileShareStorageLocation**|Specifies a single folder on the file system for storing temporary snapshots. Although you can specify the folder path as a UNC path, doing so isn't recommended. The default value is empty.<br /><br /> `<FileShareStorageLocation>`<br /><br /> `<Path>`<br /><br /> `</Path>`<br /><br /> `</FileShareStorageLocation>`|N,S,P|  
|**IsRdceEnabled**|Specifies whether Report Definition Customization Extension (RDCE) is enabled. Valid values are **True** and **False**.|N,S,P|
|**IsDataModelRefreshService**|Specifies whether the server should process Power BI Reports refresh. Valid values are **True** and **False**.|P|
|**MaxCatalogConnectionPoolSizePerProcess**|Specifies the maximum size of the connection pool when connecting to the server catalog. The default value is 0. If you choose 0, the report server determines the maximum number of connections for the `reportingservices.exe` process, for the other processes it's the [Sql client default](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.maxpoolsize).|P|  

  
##  <a name="bkmk_UI"></a> UI (RSReportServer.config file)  
 **UI** specifies configuration settings that apply to the web portal application.  
  
 The last column of the following table indicates if the setting applies to a Native mode report server (N) or a SharePoint mode report server (S) or both.  
  
|Setting|Description|Mode|  
|-------------|-----------------|----------|  
|**ReportServerUrl**|Specifies the URL of the report server that the web portal connects to. Only modify this value if you're configuring the web portal to connect to a report server in another instance or on a remote computer.|N,S|  
|**ReportBuilderTrustLevel**|Don't modify this value; it isn't configurable. In [!INCLUDE[sql2008-md](../../includes/sql2008-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] and later versions, Report Builder runs only in **FullTrust**. For more information about discontinuing partial trust mode, see [Discontinued functionality to SQL Server Reporting Services in SQL Server 2016](../../reporting-services/discontinued-functionality-to-sql-server-reporting-services-in-sql-server.md).|N,S|  
|**PageCountMode**|This setting is for the web portal only. This setting specifies whether the report server calculates a page count value before it renders the report, or as the report is viewed. Valid values are **Estimate** (default) and **Actual**. Use **Estimate** to calculate page count information as the user views the report. Initially, the page count is set to 2 (for the current page plus one more page), but adjusts upwards as the user pages through the report. Use **Actual** if you want to calculate page count in advance before the report is displayed. **Actual** is provided for backward compatibility. If you set **PageCountMode** to **Actual**, the entire report must be processed to get a valid page count, increasing wait time before the report is displayed.|N,S|  
  
##  <a name="bkmk_extensions"></a> Extensions (RSReportServer.config file) native mode  
 The `Extensions` section appears in the `rsreportserver.config` file **only for native mode** report servers. Extension information for SharePoint mode report servers is stored in the SharePoint configuration database and is configured per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] service application.  
  
 **Extensions** specifies configuration settings for the following extensible modules of a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installation:  
  
-   Delivery extensions  
  
-   DeliveryUI extensions  
  
-   Rendering extensions  
  
-   Data processing extensions  
  
-   Semantic query extensions (internal only)  
  
-   Model generation extensions (internal only)  
  
-   Security extensions  
  
-   Authentication extensions  
  
-   Event processing extensions (internal only)  
  
-   Report definition customization extensions  
  
 Some of these extensions are strictly for internal use by the report server. Configuration settings for internal-use-only extensions aren't documented. The following sections describe configuration settings for the default extensions. If you're using report server that has custom extensions, your configuration files might contain settings that aren't described here. This section lists the extensions in the order in which they appear. Settings that occur repeatedly for multiple instances of the same kind of extension are described once.  
  
###  <a name="bkmk_extensionsgeneral"></a> Delivery extension(s) general configuration  
 Specifies default (and possibly custom) delivery extensions used to deliver reports through subscriptions. The RSReportServer.config file includes application settings for four delivery extensions:  
  
1.  Report server email.  
  
1.  File share delivery.  
  
1.  Report server document library used for a report server that runs in SharePoint integrated mode.  
  
1.  Null delivery provider used to preload the report cache.  
  
 For more information about delivery extensions, see [Subscriptions and delivery &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)  
  
 All delivery extensions have **Extension Name**, **MaxRetries**, **SecondsBeforeRetry**, and **Configuration**. These shared settings are documented first. Descriptions of extension-specific settings follow in a second table.  
  
|Setting|Description|  
|-------------|-----------------|  
|**Extension Name**|Specifies a friendly name and assembly of the delivery extension. Don't modify this value.|  
|**MaxRetries**|Specifies the number of times a report server retries a delivery if the first attempt doesn't succeed. The default value is 3.|  
|**SecondsBeforeRetry**|Specifies the interval of time (in seconds) between each retry attempt. The default value is 900.|  
|**Configuration**|Contains the configuration settings that are specific to each delivery extension.|  
  
####  <a name="bkmk_fileshare_extension"></a> File Share Delivery Extension Configuration Settings  
 File Share delivery sends an exported report to an application file format to a shared folder on the network. For more information, see [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
|Setting|Description|  
|-------------|-----------------|  
|**ExcludedRenderFormats**, **RenderingExtension**|These settings are used to intentionally exclude export formats that don't work well with file share delivery. These formats are typically used for interactive reporting, preview, or to preload the report cache. They don't produce application files that can be easily viewed from a desktop application.<br /><br /> `HTMLOWC`<br /><br /> `RGDI`<br /><br /> `Null`|  
  
####  <a name="bkmk_email_extension"></a> Report Server Email extension configuration settings  
 Report Server Email uses an SMTP network device to send reports to email addresses. This delivery extension must be configured before it can be used. For more information, see [Email delivery in Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md).  
  
|Setting|Description|  
|-------------|-----------------|  
|**SMTPServer**|Specifies a string value indicating the address of a remote SMTP server or forwarder. This value is required for remote SMTP service. It can be an IP address, a UNC name of a computer on your corporate intranet, or a fully qualified domain name.|  
|**SMTPServerPort**|Specifies an integer value indicating the port on which the SMTP service uses to send outgoing mail. Port 25 is typically used to send email.|  
|**SMTPAccountName**|Contains a string value that assigns [!INCLUDE[msCoName](../../includes/msconame-md.md)] Outlook Express account name. You can set this value if your SMTP server is configured to use it in some way; otherwise you can leave it blank. Use **From** to specify an email account used to send reports.|  
|**SMTPConnectionTimeout**|Specifies an integer value indicating the number of seconds to wait for a valid socket connection with the SMTP service before timing out. The default is 30 seconds, but this value is ignored if **SendUsing** is set to 2.|  
|**SMTPServerPickupDirectory**|Specifies a string value indicating the pickup directory for the local SMTP service. This value must be a fully qualified local folder path (for example, `d:\rs-emails`).|  
|**SMTPUseSSL**|Specifies a Boolean value that can be set to use Transport Layer Security (TLS) when sending an SMTP message over the network. The default value is 0 (or false). This setting can be used when the **SendUsing** element is set to 2.|  
|**SendUsing**|Specifies which method to use for sending messages. Valid values are:<br /><br /> `1=Sends a message from the local SMTP service pickup directory.`<br /><br /> `2=Sends the message from the network SMTP service.`|  
|**SMTPAuthenticate**|Specifies an integer value that indicates the kind of authentication to use when sending messages to an SMTP service over a TCP/IP connection. Valid values are:<br /><br /> 0=No authentication.<br /><br /> `1= (not supported).`<br /><br /> `2= NTLM (NT LanMan) authentication.` The security context of the Report Server Windows service is used to connect to the network SMTP server.|  
|**From**|Specifies an email address from which reports are sent in the format `abc@host.xyz`. The address appears on the **From** line of an outgoing email message. This value is required if you're using a remote SMTP server. It should be a valid email account that has permission to send mail.|  
|**EmbeddedRenderFormats, RenderingExtension**|Specifies the rendering format used to encapsulate a report within the body of an email message. Images within the report are later embedded within the report. Valid values are MHTML and HTML4.0.|  
|**PrivilegedUserRenderFormats**|Specifies rendering formats that a user can select from for a report subscription when subscribing is enabled through the "Manage all subscriptions" task. If this value isn't set, all render formats that aren't intentionally excluded are available to use.|  
|**ExcludedRenderFormats, RenderingExtension**|Purposely excludes formats that don't work well with a given delivery extension. You can't exclude multiple instances of the same rendering extension. Excluding multiple instances result in an error when the report server reads the configuration file. By default, the following extensions are excluded for email delivery:<br /><br /> `HTMLOWC`<br /><br /> `Null`<br /><br /> `RGDI`|  
|**SendEmailToUserAlias**|This value works with **DefaultHostName**.<br /><br /> When **SendEmailToUserAlias** is set to **True**, users who define individual subscriptions are automatically specified as recipients of the report. The **To** field is hidden. If this value is **False**, the **To** field is visible. Set this value to **True** if you want maximum control over report distribution. Valid values are:<br /><br /> **True**: True is the default value and means the email address of the user creating the subscription is used.<br /><br /> **False**: Any email address can be specified.|  
|**DefaultHostName**|This value works with **SendEmailToUserAlias**.<br /><br /> Specifies a string value indicating the host name to append to the user alias when the **SendEmailToUserAlias** is set to `true`. This value can be a Domain Name System (DNS) name or IP address.|  
|**PermittedHosts**|Limits report distribution by explicitly specifying which hosts can receive email delivery. Within **PermittedHosts**, each host is specified as a `HostName` element, where the value is either an IP address or a DNS name.<br /><br /> Only email accounts defined for the host are valid recipients. If you specified **DefaultHostName**, be sure to include that host as `HostName` element of **PermittedHosts**. This value must be one or more DNS names or IP addresses. By default, this value isn't set. If the value isn't set, there are no restrictions on who can receive emailed reports.|  
  
####  <a name="bkmk_documentlibrary_extension"></a> Report Server SharePoint Document Library extension configuration  
 Report Server Document Library sends an exported report to an application file format to a document library. A report server can use this delivery extension when the server is configured to run in SharePoint integrated mode. For more information, see [SharePoint library delivery in Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md).  
  
|Setting|Description|  
|-------------|-----------------|  
|**ExcludedRenderFormats, RenderingExtension**|These settings are used to intentionally exclude export formats that don't work well with document library delivery. `HTMLOWC`, `RGDI`, and `Null` delivery extensions are excluded. These formats are typically used for interactive reporting, preview, or to preload the report cache. They don't produce application files that can be easily viewed from a desktop application.|  
  
####  <a name="bkmk_null_extension"></a> NULL delivery extension configuration  
 The NULL delivery provider is used to preload the cache with pregenerated reports for individual users. There are no configuration settings for this delivery extension. For more information, see [Cache reports &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md).  
  
###  <a name="bkmk_ui"></a> Delivery UI extension(s) general configuration  
 Specifies delivery extensions. These extensions contain a user interface component that appears in the subscription definition pages used when defining individual subscriptions in the web portal. If you create and deploy a custom delivery extension with user-defined options, you must register the delivery extension. This registration is necessary if you want to use the web portal. By default, there are configuration settings for Report Server Email and Report Server File Share. Delivery extensions used only in data-driven subscriptions or in SharePoint application pages don't have settings in this section.  
  
|Setting|Description|  
|-------------|-----------------|  
|**DefaultDeliveryExtension**|This setting determines which delivery extension appears first in the list of delivery types in the subscription definition page. Only one delivery extension can contain this setting. Valid values include **True** or **False**. When this value is set to **True**, that extension is default selection.|  
|**Configuration**|Specifies configuration options for a delivery extension. You can set a default rendering format for each delivery extension. Valid values are the rendering extension names noted in the render section of the `rsreportserver.config` file.|  
|**DefaultRenderingExtension**|Specifies whether a delivery extension is the default. Report Server Email is the default delivery extension. Valid values include **True** or **False**. If more than one extension contains a value of **True**, the first extension is considered the default extension.|  
  
###  <a name="bkmk_rendering"></a> Rendering extensions general configuration  
 Specifies default (and possibly custom) rendering extensions used in report presentation.  
  
 Don't modify this section unless you're deploying a custom rendering extension. For more information, see [Implementing a rendering extension](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md).  
  
 Default rendering extensions include the following values:  
  
-   XML  
  
-   Null  
  
-   CSV  
  
-   PDF  
  
-   RGDI  
  
-   HTML4.0  
  
-   MHTML  
  
-   EXCEL  
  
-   RPL  
  
-   IMAGE  
  
 With the [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] release, the MHTML and HTML 4.0 renders contain by default the following device information setting to control the behavior of sizing data visualizations.  
  
```  
<DeviceInfo><DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing></DeviceInfo>  
```  
  
 For more information on DeviceInfo settings, see the following articles:  
  
-   [MHTML device information settings](../../reporting-services/mhtml-device-information-settings.md)  
  
-   [HTML device information settings](../../reporting-services/html-device-information-settings.md)  
  
-   [Device information settings for Rendering extensions &#40;Reporting Services&#41;](../../reporting-services/device-information-settings-for-rendering-extensions-reporting-services.md)  
  
 For information about the attributes for the child `<Extension>` element under `<Render>`, see the following articles:  
  
-   [Customize Rendering extension parameters in RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)  
  
-   [Deploying a Rendering extension](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
 Don't modify this section unless you're deploying a custom rendering extension. For more information, see [Implementing a Rendering extension](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md).  
  
###  <a name="bkmk_data"></a> Data extension(s) general configuration  
 Specifies default (and possibly custom) data processing extensions used to process queries. Default data processing extensions include the following values:  
  
-   SQL  
  
-   SQLAZURE  
  
-   SQLPDW  
  
-   OLEDB  
  
-   OLEDB-MD  
  
-   ORACLE  
  
-   ODBC  
  
-   XML  
  
-   SHAREPOINTLIST  
  
-   SAPBW  
  
-   ESSBASE  
  
-   TERADATA  
  
 Don't modify this section unless you're adding custom data processing extensions. For more information, see [Implement a data processing extension](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
###  <a name="bkmk_semantic"></a> Semantic query extensions general configuration  
 Specifies semantic query processing extension used to process report models. The semantic query processing extensions included with [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] provide support for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relational data, Oracle, and [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] multidimensional data. Don't modify this section. Query processing isn't extensible.  
  
###  <a name="bkmk_model"></a> Model generation configuration  
 Specifies a model generation extension used to create report models from a shared data source that is already published on a report server. You can generate models for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relational data, Oracle, and [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] multidimensional data sources. Don't modify this section. Model generation isn't extensible.  
  
###  <a name="bkmk_security"></a> Security extension configuration  
 Specifies the authorization component used by [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. This component is used by the authentication extension registered in the `Authentication` element of the `RSReportServer.config` file. Don't modify this section unless you're implementing a custom authentication extension. For more information about adding custom security features, see [Implement a security extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md). For more information about authorization, see [Authorization in Reporting Services](../../reporting-services/extensions/security-extension/authorization-in-reporting-services.md).  
  
###  <a name="bkmk_authentication"></a> Authentication extension configuration  
 Specifies the default and custom authentication extensions used by the report server. The default extension is based on Windows Authentication. Don't modify this section unless you're implementing a custom authentication extension. For more information about authentication in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], see [Authentication in Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md) and [Authentication with the report server](../../reporting-services/security/authentication-with-the-report-server.md). For more information about adding custom security features, see [Implement a security extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).  
  
###  <a name="bkmk_eventprocessing"></a> Event processing  
 Specifies default event handlers. Don't modify this section. This section isn't extensible.  
  
###  <a name="bkmk_reportdefinition"></a> Report definition customization  
 Specifies the name and type of a custom extension that modifies a report definition.  
  
###  <a name="bkmk_rdlsandboxing"></a> RDLSandboxing  
 Specifies a Report Definition Language (RDL) mode. In this mode, you can help detect and restrict the use of specific types of report resources. This setting is relevant in scenarios where multiple tenants share a single Web farm of report servers. For more information, see [Enable and disable RDL sandboxing](../../reporting-services/report-server-sharepoint/enable-and-disable-rdl-sandboxing.md).  
  
##  <a name="bkmk_MapTileServer"></a> MapTileServerConfiguration (RSReportServer.config file)  
 **MapTileServerConfiguration** defines configuration settings for [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing Maps Web Services that provides a tile background for a map report item in a report that is published on a report server. All child elements are required.  
  
|Setting|Description|  
|-------------|-----------------|  
|**MaxConnections**|Specifies the maximum number of connections to Bing Maps Web Services.|  
|**Timeout**|Specifies the timeout in seconds to wait for a response from Bing Maps Web Services.|  
|**AppID**|Specifies the application identifier (AppID) to use for Bing Maps Web Services. **(Default)** specifies the [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] default AppID.<br /><br /> For more information about the use of Bing map tiles, see [Terms of use](https://go.microsoft.com/fwlink/?LinkId=151371).<br /><br /> Don't change this value unless you must specify a custom AppID for your own Bing Maps license agreement. When you change the AppID, you don't have to restart [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] for the change to take effect.|  
|**CacheLevel**|Specifies a value from the HttpRequestCacheLevel Enumeration of `System.Net.Cache`. The default value is **Default**. For more information, see [HttpRequestCacheLevel enumeration](/dotnet/api/system.net.cache.httprequestcachelevel).|  
  
##  <a name="bkmk_nativedefaultfile"></a> Default configuration file for a native mode report server  
 The `rsreportserver.config` file is installed to the following location by default:  
  
 `C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer`  
  
```  
<Configuration>
	<Dsn>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAR58DMGebHUeMvyR6HR04kQQAAAAiAAAAUgBlAHAAbwBy
AHQAaQBuAGcAIABTAGUAcgB2AGUAcgAAAANmAADAAAAAEAAAADczfLRgZ4GF44iBHkLrKY4AAAAA
BIAAAKAAAAAQAAAAJ9wQOmDNauH+LS30rboJ2OAAAAAp0kiFFBrc3r3ypKaldZJtjCORX9LTZRzt
0/JCSVIZc4GXx0peGKqd+f85UyrY/KOyUSHogOC/XoBp9Ppxv6ITbdunsS/LXEcMUBVqEdQD4ylh
x6K1NTC/u8hl9v0MgK+xMQKaiV7BuNYbgGgkaViABcNH0xVzcc5rMTHUkrABbGDFGKyAFniGQ1qu
/rqHibNNyvYbP/2uiqvgC0tQl6u8VkVbXpWrkvO+bFCqxlaJlCoDc2f3rIO321SZEvoFbsYNgPLd
+mIAkSCnH3Z3gm/bI8bqVkFaHblKyQuSfFsi6RQAAACb87b26dV0GjHmMJnE0Tk8CzNmhg==</Dsn>
	<ConnectionType>Default</ConnectionType>
	<LogonUser></LogonUser>
	<LogonDomain></LogonDomain>
	<LogonCred></LogonCred>
	<InstanceId>MSRS13.MSSQLSERVER</InstanceId>
	<InstallationID>{cd920604-a5c7-4554-b2a0-aadc04312fe5}</InstallationID>
	<Add Key="SecureConnectionLevel" Value="0"/>
	<Add Key="DisableSecureFormsAuthenticationCookie" Value="false"/>
	<Add Key="CleanupCycleMinutes" Value="10"/>
	<Add Key="MaxActiveReqForOneUser" Value="20"/>
	<Add Key="DatabaseQueryTimeout" Value="120"/>
	<Add Key="RunningRequestsScavengerCycle" Value="60"/>
	<Add Key="RunningRequestsDbCycle" Value="60"/>
	<Add Key="RunningRequestsAge" Value="30"/>
	<Add Key="MaxScheduleWait" Value="5"/>
	<Add Key="DisplayErrorLink" Value="true"/>
	<Add Key="WebServiceUseFileShareStorage" Value="false"/>
	<!--  <Add Key="ProcessTimeout" Value="150" /> -->
	<!--  <Add Key="ProcessTimeoutGcExtension" Value="30" /> -->
	<!--  <Add Key="WatsonFlags" Value="0x0430" /> full dump-->
	<!--  <Add Key="WatsonFlags" Value="0x0428" /> minidump -->
	<!--  <Add Key="WatsonFlags" Value="0x0002" /> no dump-->
	<Add Key="WatsonFlags" Value="0x0428"/>
	<Add Key="WatsonDumpOnExceptions" Value="Microsoft.ReportingServices.Diagnostics.Utilities.InternalCatalogException,Microsoft.ReportingServices.Modeling.InternalModelingException,Microsoft.ReportingServices.ReportProcessing.UnhandledReportRenderingException"/>
	<Add Key="WatsonDumpExcludeIfContainsExceptions" Value="System.Threading.ThreadAbortException,System.Web.UI.ViewStateException,System.OutOfMemoryException,System.Web.HttpException,System.IO.IOException,System.IO.FileLoadException,Microsoft.SharePoint.SPException,Microsoft.ReportingServices.WmiProvider.WMIProviderException,System.AppDomainUnloadedException"/>
	<URLReservations>
		<Application>
			<Name>ReportServerWebService</Name>
			<VirtualDirectory>ReportServer</VirtualDirectory>
			<URLs>
				<URL>
					<UrlString>https://+:80</UrlString>
					<AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>
					<AccountName>NT SERVICE\ReportServer</AccountName>
				</URL>
			</URLs>
		</Application>
		<Application>
			<Name>ReportServerWebApp</Name>
			<VirtualDirectory>Reports</VirtualDirectory>
			<URLs>
				<URL>
					<UrlString>https://+:80</UrlString>
					<AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>
					<AccountName>NT SERVICE\ReportServer</AccountName>
				</URL>
			</URLs>
		</Application>
	</URLReservations>
	<Authentication>
		<AuthenticationTypes>
			<RSWindowsNTLM/>
		</AuthenticationTypes>
		<RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
		<RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>
		<EnableAuthPersistence>true</EnableAuthPersistence>
	</Authentication>
	<Service>
		<IsSchedulingService>True</IsSchedulingService>
		<IsNotificationService>True</IsNotificationService>
		<IsEventService>True</IsEventService>
		<PollingInterval>10</PollingInterval>
		<WindowsServiceUseFileShareStorage>False</WindowsServiceUseFileShareStorage>
		<MemorySafetyMargin>80</MemorySafetyMargin>
		<MemoryThreshold>90</MemoryThreshold>
		<RecycleTime>720</RecycleTime>
		<MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>
		<MaxQueueThreads>0</MaxQueueThreads>
		<UrlRoot>
		</UrlRoot>
		<UnattendedExecutionAccount>
			<UserName></UserName>
			<Password></Password>
			<Domain></Domain>
		</UnattendedExecutionAccount>
		<PolicyLevel>rssrvpolicy.config</PolicyLevel>
		<IsWebServiceEnabled>True</IsWebServiceEnabled>
		<IsReportManagerEnabled>True</IsReportManagerEnabled>
		<FileShareStorageLocation>
			<Path>
			</Path>
		</FileShareStorageLocation>
		<DefaultFileShareAccount>
			<Domain></Domain>
			<UserName></UserName>
			<Password></Password>
		</DefaultFileShareAccount>
	</Service>
	<UI>
		<ReportServerUrl>
		</ReportServerUrl>
		<PageCountMode>Estimate</PageCountMode>
	</UI>
	<Extensions>
		<Delivery>
			<Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareProvider,ReportingServicesFileShareDeliveryProvider">
				<MaxRetries>3</MaxRetries>
				<SecondsBeforeRetry>900</SecondsBeforeRetry>
				<Configuration>
					<FileShareConfiguration>
						<ExcludedRenderFormats>
							<RenderingExtension>HTMLOWC</RenderingExtension>
							<RenderingExtension>NULL</RenderingExtension>
							<RenderingExtension>RGDI</RenderingExtension>
						</ExcludedRenderFormats>
					</FileShareConfiguration>
				</Configuration>
			</Extension>
			<Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailProvider,ReportingServicesEmailDeliveryProvider">
				<MaxRetries>3</MaxRetries>
				<SecondsBeforeRetry>900</SecondsBeforeRetry>
				<Configuration>
					<RSEmailDPConfiguration>
						<SMTPServer></SMTPServer>
						<SMTPServerPort>
						</SMTPServerPort>
						<SMTPAccountName>
						</SMTPAccountName>
						<SMTPConnectionTimeout>
						</SMTPConnectionTimeout>
						<SMTPServerPickupDirectory>
						</SMTPServerPickupDirectory>
						<SMTPUseSSL>False</SMTPUseSSL>
						<SendUsing>2</SendUsing>
						<SMTPAuthenticate>0</SMTPAuthenticate>
						<SendUserName></SendUserName>
						<SendPassword></SendPassword>
						<From></From>
						<EmbeddedRenderFormats>
							<RenderingExtension>MHTML</RenderingExtension>
						</EmbeddedRenderFormats>
						<PrivilegedUserRenderFormats>
						</PrivilegedUserRenderFormats>
						<ExcludedRenderFormats>
							<RenderingExtension>HTMLOWC</RenderingExtension>
							<RenderingExtension>NULL</RenderingExtension>
							<RenderingExtension>RGDI</RenderingExtension>
						</ExcludedRenderFormats>
						<SendEmailToUserAlias>True</SendEmailToUserAlias>
						<DefaultHostName>
						</DefaultHostName>
						<PermittedHosts>
						</PermittedHosts>
					</RSEmailDPConfiguration>
				</Configuration>
			</Extension>
			<Extension Name="Report Server DocumentLibrary" Type="Microsoft.ReportingServices.SharePoint.SharePointDeliveryExtension.DocumentLibraryProvider,ReportingServicesSharePointDeliveryExtension">
				<MaxRetries>3</MaxRetries>
				<SecondsBeforeRetry>900</SecondsBeforeRetry>
				<Configuration>
					<DocumentLibraryConfiguration>
						<ExcludedRenderFormats>
							<RenderingExtension>HTMLOWC</RenderingExtension>
							<RenderingExtension>NULL</RenderingExtension>
							<RenderingExtension>RGDI</RenderingExtension>
						</ExcludedRenderFormats>
					</DocumentLibraryConfiguration>
				</Configuration>
			</Extension>
			<Extension Name="NULL" Type="Microsoft.ReportingServices.NullDeliveryProvider.NullProvider,ReportingServicesNullDeliveryProvider"/>
			<Extension Name="Report Server PowerBI" Type="Microsoft.ReportingServices.PowerBIDeliveryProvider.PowerBIDeliveryProvider,ReportingServicesPowerBIDeliveryProvider">
				<MaxRetries>3</MaxRetries>
				<SecondsBeforeRetry>900</SecondsBeforeRetry>
				<Configuration>
					<PowerBIDeliveryConfiguration>
					</PowerBIDeliveryConfiguration>
				</Configuration>
			</Extension>
		</Delivery>
		<DeliveryUI>
			<Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailDeliveryProviderControl,ReportingServicesEmailDeliveryProvider">
				<DefaultDeliveryExtension>True</DefaultDeliveryExtension>
				<Configuration>
					<RSEmailDPConfiguration>
						<DefaultRenderingExtension>MHTML</DefaultRenderingExtension>
					</RSEmailDPConfiguration>
				</Configuration>
			</Extension>
			<Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareUIControl,ReportingServicesFileShareDeliveryProvider"/>
			<Extension Name="Report Server PowerBI" Type="Microsoft.ReportingServices.PowerBIDeliveryProvider.PowerBIDeliveryUIControl,ReportingServicesPowerBIDeliveryProvider"/>
		</DeliveryUI>
		<Render>
			<Extension Name="WORDOPENXML" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordOpenXmlRenderer.WordOpenXmlDocumentRenderer,Microsoft.ReportingServices.WordRendering"/>
			<Extension Name="WORD" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordDocumentRenderer,Microsoft.ReportingServices.WordRendering" Visible="false"/>
			<Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>
			<Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>
			<Extension Name="PPTX" Type="Microsoft.ReportingServices.Rendering.PowerPointRendering.PptxRenderingExtension,Microsoft.ReportingServices.PowerPointRendering"/>
			<Extension Name="PDF" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.PDFRenderer,Microsoft.ReportingServices.ImageRendering"/>
			<Extension Name="IMAGE" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.ImageRenderer,Microsoft.ReportingServices.ImageRendering"/>
			<Extension Name="MHTML" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.MHtmlRenderingExtension,Microsoft.ReportingServices.HtmlRendering">
				<Configuration>
					<DeviceInfo>
						<DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
					</DeviceInfo>
				</Configuration>
			</Extension>
			<Extension Name="CSV" Type="Microsoft.ReportingServices.Rendering.DataRenderer.CsvReport,Microsoft.ReportingServices.DataRendering"/>
			<Extension Name="XML" Type="Microsoft.ReportingServices.Rendering.DataRenderer.XmlDataReport,Microsoft.ReportingServices.DataRendering"/>
			<Extension Name="ATOM" Type="Microsoft.ReportingServices.Rendering.DataRenderer.AtomDataReport,Microsoft.ReportingServices.DataRendering"/>
			<Extension Name="NULL" Type="Microsoft.ReportingServices.Rendering.NullRenderer.NullReport,Microsoft.ReportingServices.NullRendering" Visible="false"/>
			<Extension Name="RGDI" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.RGDIRenderer,Microsoft.ReportingServices.ImageRendering" Visible="false"/>
			<Extension Name="HTML4.0" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.Html40RenderingExtension,Microsoft.ReportingServices.HtmlRendering" Visible="false">
				<Configuration>
					<DeviceInfo>
						<DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
					</DeviceInfo>
				</Configuration>
			</Extension>
			<Extension Name="HTML5" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.Html5RenderingExtension,Microsoft.ReportingServices.HtmlRendering" Visible="false">
				<Configuration>
					<DeviceInfo>
						<DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
					</DeviceInfo>
				</Configuration>
			</Extension>
			<Extension Name="RPL" Type="Microsoft.ReportingServices.Rendering.RPLRendering.RPLRenderer,Microsoft.ReportingServices.RPLRendering" Visible="false" LogAllExecutionRequests="false"/>
		</Render>
		<!--
        For the SQLPDW extension to work, install the SQL Server PDW Client Tools on the report server.
        NOTE: The SQLPDW extension is deprecated. It supports old versions of SQL Server Parallel Data Warehouse (PDW).        
        To connect to Analytics Platform System, use the SQL (SQL Server) extension.        
        For the ORACLE extension to work, install the Oracle Data Provider for NET (ODP.NET) on the report server.
        For TERADATA extension to work, install the .NET Provider for Teradata on the report server.
      -->
		<Data>
			<Extension Name="SQL" Type="Microsoft.ReportingServices.DataExtensions.SqlConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.DataExtensions.SqlAzureConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="SQLPDW" Type="Microsoft.ReportingServices.DataExtensions.SqlDwConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="OLEDB-MD" Type="Microsoft.ReportingServices.DataExtensions.AdoMdConnection,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="SHAREPOINTLIST" Type="Microsoft.ReportingServices.DataExtensions.SharePointList.SPListConnection,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="ORACLE" Type="Microsoft.ReportingServices.DataExtensions.OracleClientConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="ESSBASE" Type="Microsoft.ReportingServices.DataExtensions.Essbase.EssbaseConnection,Microsoft.ReportingServices.DataExtensions.Essbase"/>
			<Extension Name="SAPBW" Type="Microsoft.ReportingServices.DataExtensions.SapBw.SapBwConnection,Microsoft.ReportingServices.DataExtensions.SapBw"/>
			<Extension Name="TERADATA" Type="Microsoft.ReportingServices.DataExtensions.TeradataConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="OLEDB" Type="Microsoft.ReportingServices.DataExtensions.OleDbConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="ODBC" Type="Microsoft.ReportingServices.DataExtensions.OdbcConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
			<Extension Name="XML" Type="Microsoft.ReportingServices.DataExtensions.XmlDPConnection,Microsoft.ReportingServices.DataExtensions"/>
		</Data>
		<SemanticQuery>
			<Extension Name="SQL" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MSSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
				<Configuration>
					<EnableMathOpCasting>False</EnableMathOpCasting>
				</Configuration>
			</Extension>
			<Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MSSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
				<Configuration>
					<EnableMathOpCasting>False</EnableMathOpCasting>
				</Configuration>
			</Extension>
			<Extension Name="SQLPDW" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQLADW.MSSqlAdwSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
				<Configuration>
					<EnableMathOpCasting>False</EnableMathOpCasting>
				</Configuration>
			</Extension>
			<Extension Name="ORACLE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Oracle.OraSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
				<Configuration>
					<EnableMathOpCasting>True</EnableMathOpCasting>
					<DisableNO_MERGEInLeftOuters>False</DisableNO_MERGEInLeftOuters>
					<EnableUnistr>False</EnableUnistr>
					<DisableTSTruncation>False</DisableTSTruncation>
				</Configuration>
			</Extension>
			<Extension Name="TERADATA" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Teradata.TdSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
				<Configuration>
					<EnableMathOpCasting>True</EnableMathOpCasting>
					<ReplaceFunctionName>oREPLACE</ReplaceFunctionName>
				</Configuration>
			</Extension>
			<Extension Name="OLEDB-MD" Type="Microsoft.AnalysisServices.Modeling.QueryExecution.ASSemanticQueryCommand,Microsoft.AnalysisServices.Modeling"/>
		</SemanticQuery>
		<ModelGeneration>
			<Extension Name="SQL" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MsSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
			<Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MsSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
			<Extension Name="ORACLE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Oracle.OraSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
			<Extension Name="TERADATA" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Teradata.TdSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
			<Extension Name="OLEDB-MD" Type="Microsoft.AnalysisServices.Modeling.Generation.ModelGeneratorExtension,Microsoft.AnalysisServices.Modeling"/>
		</ModelGeneration>
		<Security>
			<Extension Name="Windows" Type="Microsoft.ReportingServices.Authorization.WindowsAuthorization, Microsoft.ReportingServices.Authorization"/>
		</Security>
		<Authentication>
			<Extension Name="Windows" Type="Microsoft.ReportingServices.Authentication.WindowsAuthentication, Microsoft.ReportingServices.Authorization"/>
		</Authentication>
		<EventProcessing>
			<Extension Name="SnapShot Extension" Type="Microsoft.ReportingServices.Library.HistorySnapShotCreatedHandler,ReportingServicesLibrary">
				<Event>
					<Type>ReportHistorySnapshotCreated</Type>
				</Event>
			</Extension>
			<Extension Name="Timed Subscription Extension" Type="Microsoft.ReportingServices.Library.TimedSubscriptionHandler,ReportingServicesLibrary">
				<Event>
					<Type>TimedSubscription</Type>
				</Event>
			</Extension>
			<Extension Name="Cache Refresh Plan Extension" Type="Microsoft.ReportingServices.Library.CacheRefreshPlanHandler,ReportingServicesLibrary">
				<Event>
					<Type>RefreshCache</Type>
				</Event>
			</Extension>
			<Extension Name="Shared Dataset Cache Update Extension" Type="Microsoft.ReportingServices.Library.SharedDatasetCacheUpdatePlanHandler,ReportingServicesLibrary">
				<Event>
					<Type>SharedDatasetCacheUpdate</Type>
				</Event>
			</Extension>
			<Extension Name="Cache Update Extension" Type="Microsoft.ReportingServices.Library.ReportExecutionSnapshotUpdateEventHandler,ReportingServicesLibrary">
				<Event>
					<Type>SnapshotUpdated</Type>
				</Event>
			</Extension>
		</EventProcessing>
	</Extensions>
	<MapTileServerConfiguration>
		<MaxConnections>2</MaxConnections>
		<Timeout>10</Timeout>
		<AppID>(Default)</AppID>
		<CacheLevel>Default</CacheLevel>
	</MapTileServerConfiguration>
</Configuration> 
```  
  
##  <a name="bkmk_sharepointdefaultfile"></a> Default Configuration File for a SharePoint Mode Report Server  
 The rsreportserver.config file is installed to the following location by default:  
  
 **C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting**  
  
```  
<Configuration>  
  <Dsn />  
  <ConnectionType>Default</ConnectionType>  
  <LogonUser>  
  </LogonUser>  
  <LogonDomain>  
  </LogonDomain>  
  <LogonCred>  
  </LogonCred>  
  <InstanceId>MSRS12.@Sharepoint</InstanceId>  
  <Add Key="SecureConnectionLevel" Value="0" />  
  <Add Key="CleanupCycleMinutes" Value="10" />  
  <Add Key="MaxActiveReqForOneUser" Value="20" />  
  <Add Key="AlertingCleanupCycleMinutes" Value="20" />  
  <Add Key="AlertingDataCleanupMinutes" Value="360" />  
  <Add Key="AlertingExecutionLogCleanupMinutes" Value="10080" />  
  <Add Key="AlertingMaxDataRetentionDays" Value="180" />  
  <Add Key="RunningRequestsScavengerCycle" Value="60" />  
  <Add Key="RunningRequestsDbCycle" Value="60" />  
  <Add Key="RunningRequestsAge" Value="30" />  
  <Add Key="MaxScheduleWait" Value="5" />  
  <Add Key="DisplayErrorLink" Value="true" />  
  <Add Key="WebServiceUseFileShareStorage" Value="false" />  
  <!--  <Add Key="ProcessTimeout" Value="150" /> -->  
  <!--  <Add Key="ProcessTimeoutGcExtension" Value="30" /> -->  
  <!--  <Add Key="WatsonFlags" Value="0x0430" /> full dump-->  
  <!--  <Add Key="WatsonFlags" Value="0x0428" /> minidump -->  
  <!--  <Add Key="WatsonFlags" Value="0x0002" /> no dump-->  
  <Add Key="WatsonFlags" Value="0x0428" />  
  <Add Key="WatsonDumpOnExceptions" Value="Microsoft.ReportingServices.Diagnostics.Utilities.InternalCatalogException,Microsoft.ReportingServices.Modeling.InternalModelingException,Microsoft.ReportingServices.ReportProcessing.UnhandledReportRenderingException" />  
  <Add Key="WatsonDumpExcludeIfContainsExceptions" Value="System.Threading.ThreadAbortException,System.Web.UI.ViewStateException,System.OutOfMemoryException,System.Web.HttpException,System.IO.IOException,System.IO.FileLoadException,Microsoft.SharePoint.SPException,Microsoft.ReportingServices.WmiProvider.WMIProviderException" />  
  <RStrace>  
    <add name="FileName" value="ReportServerService" />  
    <add name="FileSizeLimitMb" value="32" />  
    <add name="KeepFilesForDays" value="14" />  
    <add name="Prefix" value="tid, time" />  
    <add name="TraceListeners" value="file" />  
    <add name="TraceFileMode" value="unique" />  
    <add name="Components" value="all:3" />  
  </RStrace>  
  <URLReservations>  
    <Application>  
      <Name>ReportServerWebService</Name>  
      <VirtualDirectory>ReportServer</VirtualDirectory>  
      <URLs>  
        <URL>  
          <UrlString>https://+:80</UrlString>  
          <AccountSid>  
          </AccountSid>  
          <AccountName>  
          </AccountName>  
        </URL>  
      </URLs>  
    </Application>  
    <Application>  
      <Name>ReportManager</Name>  
      <VirtualDirectory>Reports</VirtualDirectory>  
      <URLs>  
        <URL>  
          <UrlString>https://+:80</UrlString>  
          <AccountSid>  
          </AccountSid>  
          <AccountName>  
          </AccountName>  
        </URL>  
      </URLs>  
    </Application>  
  </URLReservations>  
  <Authentication>  
    <AuthenticationTypes>  
      <RSWindowsNTLM />  
    </AuthenticationTypes>  
    <EnableAuthPersistence>true</EnableAuthPersistence>  
  </Authentication>  
  <Service>  
    <IsSchedulingService>True</IsSchedulingService>  
    <IsNotificationService>True</IsNotificationService>  
    <IsEventService>True</IsEventService>  
    <IsAlertingService>True</IsAlertingService>  
    <PollingInterval>10</PollingInterval>  
    <WindowsServiceUseFileShareStorage>False</WindowsServiceUseFileShareStorage>  
    <MemorySafetyMargin>80</MemorySafetyMargin>  
    <MemoryThreshold>90</MemoryThreshold>  
    <RecycleTime>720</RecycleTime>  
    <MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>  
    <MaxQueueThreads>0</MaxQueueThreads>  
    <UrlRoot>  
    </UrlRoot>  
    <PolicyLevel>rssrvpolicy.config</PolicyLevel>  
    <IsWebServiceEnabled>True</IsWebServiceEnabled>    
    <FileShareStorageLocation>  
      <Path>  
      </Path>  
    </FileShareStorageLocation>  
  </Service>  
  <UI>  
    <ReportServerUrl>  
    </ReportServerUrl>  
    <PageCountMode>Estimate</PageCountMode>  
  </UI>  
  <MapTileServerConfiguration>  
    <MaxConnections>2</MaxConnections>  
    <Timeout>10</Timeout>  
    <AppID>(Default)</AppID>  
    <CacheLevel>Default</CacheLevel>  
  </MapTileServerConfiguration>  
</Configuration>  
```  
  
## Related content

- [Modify a Reporting Services configuration file &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)
- [Configure available memory for report server applications](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)
- [Reporting Services configuration files](../../reporting-services/report-server/reporting-services-configuration-files.md)
- [Initialize a report server &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md)
- [Store encrypted report server Data &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)
- [Report Server Configuration Manager &#40;native mode&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)
- [Try the Reporting Services forum](https://go.microsoft.com/fwlink/?LinkId=620231)
