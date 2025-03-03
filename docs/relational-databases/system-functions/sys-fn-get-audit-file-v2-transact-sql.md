---
title: "sys.fn_get_audit_file_v2 (Transact-SQL)"
description: "The sys.fn_get_audit_file_v2 system function replaces sys.fn_get_audit_file, and returns information from an audit file created by a server audit in SQL Server."
author: sravanisaluru
ms.author: srsaluru
ms.reviewer: randolphwest
ms.date: 06/12/2024
ms.service: sql
ms.subservice: system-objects
ms.topic: "reference"
f1_keywords:
  - "fn_get_audit_file_v2_TSQL"
  - "sys.fn_get_audit_file_v2_TSQL"
  - "fn_get_audit_file_v2"
  - "sys.fn_get_audit_file_v2"
helpviewer_keywords:
  - "sys.fn_get_audit_file_v2 function"
  - "fn_get_audit_file_v2 function"
dev_langs:
  - "TSQL"
monikerRange: "=azuresqldb-current || >=sql-server-2016 || >=sql-server-linux-2017 || =azuresqldb-mi-current || =azure-sqldw-latest"
---
# sys.fn_get_audit_file_v2 (Transact-SQL)

[!INCLUDE [asdb](../../includes/applies-to-version/asdb.md)]

The `sys.fn_get_audit_file_v2` system function in [!INCLUDE [ssazure-sqldb](../../includes/ssazure-sqldb.md)] is designed to retrieve audit log data with enhanced efficiency compared to its predecessor, `sys.fn_get_audit_file`. The function introduces time-based filtering at both the file and record levels, providing significant performance improvements, particularly for queries targeting specific time ranges.

> [!IMPORTANT]  
> `sys.fn_get_audit_file_v2` is currently supported on [!INCLUDE [ssazure-sqldb](../../includes/ssazure-sqldb.md)] only.

Returns information from an audit file created by a server audit in [!INCLUDE [ssazure-sqldb](../../includes/ssazure-sqldb.md)]. For more information, see [SQL Server Audit (Database Engine)](../security/auditing/sql-server-audit-database-engine.md).

:::image type="icon" source="../../includes/media/topic-link-icon.svg" border="false"::: [Transact-SQL syntax conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## Syntax

```syntaxsql
fn_get_audit_file_v2 ( file_pattern
    , { default | initial_file_name | NULL }
    , { default | audit_record_offset | NULL }
    , { default | start time | NULL }
    , { default | end time | NULL } )
```

## Arguments

#### *file_pattern*

Specifies the directory or path and file name for the audit file set to be read. *file_pattern* is **nvarchar(260)**.

Passing a path without a file name pattern generates an error.

This argument is used to specify a blob URL (including the storage endpoint and container). While it doesn't support an asterisk wildcard, you can use a partial file (blob) name prefix (instead of the full blob name) to collect multiple files (blobs) that begin with this prefix. For example:

- `<Storage_endpoint>/<Container>/<ServerName>/<DatabaseName>/` - collects all audit files (blobs) for the specific database.

- `<Storage_endpoint>/<Container>/<ServerName>/<DatabaseName>/<AuditName>/<CreationDate>/<FileName>.xel` - collects a specific audit file (blob).

#### *initial_file_name*

Specifies the path and name of a specific file in the audit file set to start reading audit records from. *initial_file_name* is **nvarchar(260)**.

The *initial_file_name* argument must contain valid entries or must contain either the `default` or `NULL` value.

#### *audit_record_offset*

Specifies a known location with the file specified for the *initial_file_name*. When this argument is used, the function starts reading at the first record of the buffer immediately following the specified offset.

The *audit_record_offset* argument must contain valid entries or must contain either the `default` or `NULL` value. *audit_record_offset* is **bigint**.

#### *start_time*

The start time for filtering the logs. Records before this time are excluded.

#### *end_time*

The end time for filtering the logs. Records after this time are excluded.

## Table returned

The following table describes the audit file content returned by this function.

| Column name | Type | Description |
| --- | --- | --- |
| `event_time` | **datetime2** | Date and time when the auditable action is fired. Not nullable. |
| `sequence_number` | **int** | Tracks the sequence of records within a single audit record that was too large to fit in the write buffer for audits. Not nullable. |
| `action_id` | **varchar(4)** | ID of the action. Not nullable. |
| `succeeded` | **bit** | Indicates whether the action that triggered the event succeeded. Not nullable. For all events other than login events, this only reports whether the permission check succeeded or failed, not the operation.<br /><br />`1` = success<br />`0` = fail |
| `permission_bitmask` | **varbinary(16)** | In some actions, this bitmask is the permissions that were grant, denied, or revoked. |
| `is_column_permission` | **bit** | Flag indicating if this is a column level permission. Not nullable. Returns `0` when the `permission_bitmask` = `0`.<br /><br />`1` = true<br />`0` = false |
| `session_id` | **smallint** | ID of the session on which the event occurred. Not nullable. |
| `server_principal_id` | **int** | ID of the login context that the action is performed in. Not nullable. |
| `database_principal_id` | **int** | ID of the database user context that the action is performed in. Not nullable. Returns `0` if this doesn't apply. For example, a server operation. |
| `target_server_principal_id` | **int** | Server principal that the `GRANT`/`DENY`/`REVOKE` operation is performed on. Not nullable. Returns `0` if not applicable. |
| `target_database_principal_id` | **int** | The database principal the `GRANT`/`DENY`/`REVOKE` operation is performed on. Not nullable. Returns `0` if not applicable. |
| `object_id` | **int** | The ID of the entity on which the audit occurred, which includes the following objects:<br /><br />- Server objects<br />- Databases<br />- Database objects<br />- Schema objects<br /><br />Not nullable. Returns `0` if the entity is the Server itself or if the audit isn't performed at an object level. For example, Authentication. |
| `class_type` | **varchar(2)** | The type of auditable entity that the audit occurs on. Not nullable. |
| `session_server_principal_name` | **sysname** | Server principal for session. Nullable. Returns the identity of the original login that was connected to the instance of the [!INCLUDE [ssde-md](../../includes/ssde-md.md)] in case there were explicit or implicit context switches. |
| `server_principal_name` | **sysname** | Current login. Nullable. |
| `server_principal_sid` | **varbinary** | Current login SID. Nullable. |
| `database_principal_name` | **sysname** | Current user. Nullable. Returns `NULL` if not available. |
| `target_server_principal_name` | **sysname** | Target login of action. Nullable. Returns `NULL` if not applicable. |
| `target_server_principal_sid` | **varbinary** | SID of target login. Nullable. Returns `NULL` if not applicable. |
| `target_database_principal_name` | **sysname** | Target user of action. Nullable. Returns `NULL` if not applicable. |
| `server_instance_name` | **sysname** | Name of the server instance where the audit occurred. The standard `server\instance` format is used. |
| `database_name` | **sysname** | The database context in which the action occurred. Nullable. Returns `NULL` for audits occurring at the server level. |
| `schema_name` | **sysname** | The schema context in which the action occurred. Nullable. Returns `NULL` for audits occurring outside a schema. |
| `object_name` | **sysname** | The name of the entity on which the audit occurred, which includes the following objects:<br /><br />- Server objects<br />- Databases<br />- Database objects<br />- Schema objects<br /><br />Nullable. Returns `NULL` if the entity is the Server itself or if the audit isn't performed at an object level. For example, Authentication. |
| `statement` | **nvarchar(4000)** | Transact-SQL statement if it exists. Nullable. Returns `NULL` if not applicable. |
| `additional_information` | **nvarchar(4000)** | Unique information that only applies to a single event is returned as XML. A few auditable actions contain this kind of information.<br /><br />One level of T-SQL stack is displayed in XML format for actions that have T-SQL stack associated with them. The XML format is: `<tsql_stack><frame nest_level = '%u' database_name = '%.*s' schema_name = '%.*s' object_name = '%.*s' /></tsql_stack>`<br /><br />`frame nest_level` indicates the current nesting level of the frame. The module name is represented in three part format (`database_name`, `schema_name`, and `object_name`). The module name is parsed to escape invalid XML characters like `<`, `>`, `/`, `_x`. They're escaped as `_xHHHH_`. The `HHHH` stands for the four-digit hexadecimal UCS-2 code for the character. Nullable. Returns `NULL` when there's no additional information reported by the event. |
| `file_name` | **varchar(260)** | The path and name of the audit log file that the record came from. Not nullable. |
| `audit_file_offset` | **bigint** | The buffer offset in the file that contains the audit record. Not nullable.<br /><br />**Applies to**: SQL Server only |
| `user_defined_event_id` | **smallint** | User defined event ID passed as an argument to `sp_audit_write`. `NULL` for system events (default) and non-zero for user-defined event. For more information, see [sp_audit_write (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md).<br /><br />**Applies to**: [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] and later, Azure SQL Database, and SQL Managed Instance |
| `user_defined_information` | **nvarchar(4000)** | Used to record any extra information the user wants to record in audit log by using the `sp_audit_write` stored procedure.<br /><br />**Applies to**: [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] and later versions, Azure SQL Database, and SQL Managed Instance |
| `audit_schema_version` | **int** | Always `1`. |
| `sequence_group_id` | **varbinary** | Unique identifier.<br /><br />**Applies to**: [!INCLUDE [sssql16-md](../../includes/sssql16-md.md)] and later versions |
| `transaction_id` | **bigint** | Unique identifier to identify multiple audit events in one transaction.<br /><br />**Applies to**: [!INCLUDE [sssql16-md](../../includes/sssql16-md.md)] and later versions |
| `client_ip` | **nvarchar(128)** | Source IP of the client application.<br /><br />**Applies to**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] and later versions, and Azure SQL Database |
| `application_name` | **nvarchar(128)** | Name of client application that executed the statement that caused the audit event.<br /><br />**Applies to**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] and later versions, and Azure SQL Database |
| `duration_milliseconds` | **bigint** | Query execution duration in milliseconds.<br /><br />**Applies to**: Azure SQL Database and SQL Managed Instance |
| `response_rows` | **bigint** | Number of rows returned in the result set.<br /><br />**Applies to**: Azure SQL Database and SQL Managed Instance |
| `affected_rows` | **bigint** | Number of rows affected by the executed statement.<br /><br />**Applies to**: Azure SQL Database only |
| `connection_id` | **uniqueidentifier** | ID of the connection in the server.<br /><br />**Applies to**: Azure SQL Database and SQL Managed Instance |
| `data_sensitivity_information` | **nvarchar(4000)** | Information types and sensitivity labels returned by the audited query, based on the classified columns in the database. Learn more about [Azure SQL Database data discover and classification](/azure/sql-database/sql-database-data-discovery-and-classification).<br /><br />**Applies to**: Azure SQL Database only |
| `host_name` | **nvarchar(128)** | Host Name of the client machine. |
| `session_context` | **nvarchar(4000)** | The key-value pairs that are a part of the current session context. |
| `client_tls_version` | **bigint** | Minimum TLS version supported by the client. |
| `client_tls_version_name` | **nvarchar(128)** | Minimum TLS version supported by the client. |
| `database_transaction_id` | **bigint** | Transaction ID of the current transaction in the current session. |
| `ledger_start_sequence_number` | **bigint** | The sequence number of an operation within a transaction that created a row version.<br /><br />**Applies to:** Azure SQL Database only |
| `external_policy_permissions_checked` | **nvarchar(4000)** | Information related to the external authorization permission check, when an audit event is generated, and Purview external authorization policies are evaluated.<br /><br />**Applies to:** Azure SQL Database only |
| `obo_middle_tier_app_id` | **varchar(120)** | The application ID of the middle tier application that connects to Azure SQL Database using on-behalf-of (OBO) access. Nullable. Returns `NULL` if the request isn't made using OBO access.<br /><br />**Applies to**: Azure SQL Database only |
| `is_local_secondary_replica` | **bit** | `True` if the audit record originates from a read-only local secondary replica, `False` otherwise.<br /><br />**Applies to:** Azure SQL Database only |

## Improvements over sys.fn_get_audit_file

The `sys.fn_get_audit_file_v2` function offers a substantial improvement over the older [sys.fn_get_audit_file](sys-fn-get-audit-file-transact-sql.md) by introducing efficient time-based filtering at both the file and record levels. This optimization is particularly beneficial for queries targeting smaller time ranges and can help maintain performance in multi-database environments.

### Dual-level filtering

**File-level filtering**: The function first filters the files based on the specified time range, reducing the number of files that need to be scanned.

**Record-level filtering**: It then applies filtering within the selected files to extract only the relevant records.

### Performance enhancements

The performance improvements are primarily dependent on the rollover time of the blob files and the queried time range. Assuming a uniform distribution of audit records:

- **Reduced load**: By minimizing the number of files and records to scan, it reduces the load on the system and improves query response times.

- **Scalability**: Helps maintain performance even as the number of databases increases, though the net improvement might be less pronounced in environments with a high number of databases.

For information on setting up Azure SQL Database auditing, see [Get Started with SQL Database auditing](/azure/sql-database/sql-database-auditing).

## Remarks

- If the *file_pattern* argument passed to `fn_get_audit_file_v2` references a path or file that doesn't exist, or if the file isn't an audit file, the `MSG_INVALID_AUDIT_FILE` error message is returned.

- `fn_get_audit_file_v2` can't be used when the audit is created with the `APPLICATION_LOG`, `SECURITY_LOG`, or `EXTERNAL_MONITOR` options.

## Permissions

Requires the `CONTROL DATABASE` permission.

- Server admins can access audit logs of all databases on the server.

- Non server admins can only access audit logs from the current database.

- Blobs that don't meet the above criteria are skipped (a list of skipped blobs is displayed in the query output message). The function returns logs only from blobs for which access is allowed.

## Examples

This example retrieves audit logs from a specific Azure Blob Storage location, filtering records between `2023-11-17T08:40:40Z` and `2023-11-17T09:10:40Z`.

```sql
SELECT *
FROM sys. fn_get_audit_file_v2(
    'https://yourstorageaccount.blob.core.windows.net/sqldbauditlogs/server_name/database_name/SqlDbAuditing_ServerAudit/',
    DEFAULT,
    DEFAULT,
    '2023-11-17T08:40:40Z',
    '2023-11-17T09:10:40Z')
```

## More information

System catalog views:

- [sys.server_audit_specifications (Transact-SQL)](../system-catalog-views/sys-server-audit-specifications-transact-sql.md)
- [sys.server_audit_specification_details (Transact-SQL)](../system-catalog-views/sys-server-audit-specification-details-transact-sql.md)
- [sys.database_audit_specifications (Transact-SQL)](../system-catalog-views/sys-database-audit-specifications-transact-sql.md)
- [sys.database_audit_specification_details (Transact-SQL)](../system-catalog-views/sys-database-audit-specification-details-transact-sql.md)

Transact-SQL:

- [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)
- [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)
- [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)
- [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)
- [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)
- [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)
- [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)
- [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)
- [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)
- [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)

## Related content

- [Create a Server Audit and Server Audit Specification](../security/auditing/create-a-server-audit-and-server-audit-specification.md)
- [sys.dm_server_audit_status (Transact-SQL)](../system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)
- [sys.dm_audit_actions (Transact-SQL)](../system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)
- [sys.dm_audit_class_type_map (Transact-SQL)](../system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)
- [sys.server_audits (Transact-SQL)](../system-catalog-views/sys-server-audits-transact-sql.md)
- [sys.server_file_audits (Transact-SQL)](../system-catalog-views/sys-server-file-audits-transact-sql.md)
