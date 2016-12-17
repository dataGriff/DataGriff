---
title: SQL 2012 Admin Exam 07 Securing SQL Server
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-07 19:01:57
---

So on to some... err more... exciting security stuff...

### Video Exercises

Rmemeber to to do this video!
* [SQL 2012 Create Schema and Role](https://youtu.be/DpW1cN_p_FE)
* [SQL 2012 Modifying Privileges](https://youtu.be/uHgxKNKKtwk)
* [SQL 2012 Security DMVs](https://youtu.be/8ZZtrFJEP7Y)
* [SQL 2012 Service Account Permissions](https://youtu.be/HbMpTcKIhbc)
* [SQL 2012 Setup Service Account](https://youtu.be/n51BJqCVuxw)
* [SQL 2012 Create Audit](https://youtu.be/n51BJqCVuxw)
* [SQL 2012 Create Audit Specification](https://youtu.be/MQdiqkMobKc)
* [SQL 2012 Group Policy Management](https://youtu.be/4leRkv3qYSQ)

### Key Terms

* **Securable** - Something in SQL server you can assign permissions for.
* **Scope** - Securables can be within a hierarchy of securables, where the permissions get assigned in the hierarchy is known as scope.
* **Permissions** - ALTER, BACKUP, CONTROL, CREATE, DELETE, EXECUTE, IMPERSONATE, INSERT, RECEIVE, REFERENCE, RESTORE, SELECT, TAKE OWNERSHIP, UPDATE, VIEW DEFINITION
* **Fixed Database Roles** - You cannot modify the permissions of fixed database roles and they are db_owner, db_Securityadmin, db_accessadmin, db_backupoperator, db_ddladmin, db_datawriter, db_datareader, db_denydatawriter and db_denydatareader.
* **Flexible Database Roles** - Roles with permissions you can define yourself.
* **Schemas*** - Act as containers for groups of objects.
* **SQL Server Audit** - Audit at instance and database level outputting to flat file, windows security or application log on host machine.
* **Server Audit** - Comprises of Audit Name, Queue Delay, On Audit Log Failure and Audit Destination.
* **Server Audit Specification** - Requires an audit be created first and the specification contains action groups and actions e.g. Database changes, DBCC executions, failed authentications, permission changes for instance level and for database level a lot of the same as instance and also SELECT, UPDATE, DELETE, EXECUTE, INSERT commands.
* **cr Audit Mode** - records access to objects and statements in default instance data directory which is recycled after 200MB. This feature is deprecated.
* **Common Criteria Compliance** - Supersedes c2 Audit and enables security options like auditing logins. Can see information in sys.dm_exec_sessions DMV.
* **Policy Based Management** - Manage multiple instances like blocking certain actions like auto shrink. It is configured by a Facet, a Condition, Policy, Category and Target. Found under management in SSMS.


### Notes

* You can drop columns in read-only Filegroups! Crazy!
* You can grant permissions at the schema level.
* HAS_PERMS_BY_NAME is a function where you can determine if you have permissions on objects.
* DENY take precedence over GRANT.
* REVOKE is removing a grant - it is not a DENY.
* ALTER on schema permissions required to create tables.
* When SQL server authentication is enabled group policy controls Account Lockout duration, Account Lockout Threshold, Reset Account Lockout Counter After.
* **sys.server_principals** dmv to see if account is disabled.
* You can use ALTER LOGIN to unlock a locked out account.
* You may also need to ensure SQL browser service is running for users to connect.
* When troubleshooting you may want to look certificates, assymetric keys, endpoints, server and database permissions. All of which have DMVs which you can see in code examples below.
* Ensure that keys and certificates are regularly backed up as they may expire!
* You can use sys.database_principals to determine what authentication a database user is using.
* When setup SQL Audit the SQL Service account needs to be added to the Generate Security Audits policy and have read write permissions on file if file output.
* Audits can be at database level so don't do every database.
* Database audit also requires a server level audit is in place.
* Audits can be created under security folders in SSMS.
* There are a selection of DMVs you can see below for looking at audit stuff.
* **fn_get_audit_file** - allows you to query audit file.
* An audit failure can cause a server shutdown.
* You can configure Login Auditing under server properties. e.g. failed only, successful, both, none.
* Must be member of PolicyAdminstratorRole to use Policy Based Management.
* Can transfer policies using import and export functionality.
* Can also manage using registered servers on a single server and can then evaluate policies.
* Only enterprise supports database audit.
* After a server shutdown caused by failed audit you can either restart database engine using -f option or restart in single user mode.
* Remember need a server audit before can make server audit specification.
* SQL server service account must be added to the Generate Security Audits policy and must enable success and failure audits on audit object access to use local security log.
* Remember explcit permissions a schema like SELECT, INSERT or on a database BACKUP and RESTORE, not just built in roles.
* sys.certificates view tells you most recent backup of certificate key and its expiration date.
* sys.assymetric_keys would give you length of the key.
* ALTER ANY DATABASE AUDIT, ALTER (on the database) or CONTROL permissions are required to create a database audit.
* You can configure login auditing at instance level.
* sys.database_audit_specifications_details tells you all you need to know about audit specifications...


### Scripts
**Grant Permissions**
```sql
USE [DBName];
GO
GRANT SELECT ON [SchemaName].[TableName] TO [TestRole];
GO
```
**Revoke Permissions**
```sql
USE [DBName];
GO
REVOKE SELECT ON [SchemaName].[TableName] TO [TestRole];
GO
```
**Add member to role using procedure**
```sql
EXEC sp_addrolemember [db_datawriter],[TestRole];
```
**Add member to role using ALTER role**
```sql
ALTER ROLE [db_datawriter] ADD MEMBER [TestRole2];
```
**Create role**
```sql
CREATE ROLE [TestRole];
```
**Deny**
```sql
DENY SELECT ON [SchemaName].[TableName] TO [TestRole];
```
**Alter Permissions on a Schema**
```sql
GRANT VIEW DEFINITION ON SCHEMA::[SchemaName]  TO [RoleName];
```
**Move Objects Across Schemas**
```sql
ALTER SCHEMA [SchemaName] TRANSFER [SchemaName2].[TableName];
```
**[HAS_PERMS_BY_NAME](https://msdn.microsoft.com/en-us/library/ms189802.aspx) Example to Check if have Select Permissions in Database**
```sql
SELECT HAS_PERMS_BY_NAME  
(QUOTENAME(SCHEMA_NAME(schema_id)) + '.' + QUOTENAME(name),   
    'OBJECT', 'SELECT') AS have_select, * FROM sys.tables  
```
**Troubleshoot asymmetric keys and certificates**
```sql
SELECT * FROM sys.asymmetric_keys;
SELECT * FROM sys.certificates;
SELECT * FROM sys.key_encryptions;
SELECT * FROM sys.symmetric_keys ;
```
**Troubleshoot Endpoints**
```sql
SELECT * FROM sys.database_mirroring_endpoints;
SELECT * FROM sys.endpoints;
SELECT * FROM sys.http_endpoints;
SELECT * FROM sys.service_broker_endpoints;
SELECT * FROM sys.tcp_endpoints;
```
**Troubleshoot Server Permissions**
```sql
SELECT * FROM sys.server_permissions;
SELECT * FROM sys.server_principals;
SELECT * FROM sys.server_role_members;
SELECT * FROM sys.sql_logins;
SELECT * FROM sys.system_components_surface_area_configuration;
```
**Troubleshoot Database Permissions**
```sql
SELECT * FROM sys.database_principals;
SELECT * FROM sys.database_permissions;
SELECT * FROM sys.database_role_members;
SELECT * FROM sys.master_key_passwords; 				
```
**Create a Server Audit**
```sql
CREATE SERVER AUDIT [AuditName]
TO APPLICATION_LOG
WITH
(
  QUEUE_DELAY = 1000,
  ON_FAILURE = SHUTDOWN
)		
```
**Create a Server Audit Specification**
```sql
CREATE SERVER AUDIT SPECIFICATION [AuditSpecificationName]
  FOR SERVER AUDIT [AuditName]
  ADD (DATABASE_CHANGE_GROUP)
```
**Alter a Server Audit Specification**
```sql
ALTER SERVER AUDIT SPECIFICATION [AuditSpecificationName]
  ADD (DATABASE_LOGOUT_GROUP)
```
**Create a Database Audit Specification**
```sql
CREATE DATABASE AUDIT SPECIFICATION [AuditSpecificationName]
  FOR SERVER AUDIT [AuditName]
  ADD (INSERT ON OBJECT::[dbo].[TableName] BY [Exemplar])
```
**Create a Database Audit Specification**
```sql
SELECT * FROM sys.dm_audit_actions;
SELECT * FROM sys.server_audits;
SELECT * FROM sys.dm_Server_audit_status;
SELECT * FROM sys.server_audit_specifications; 	
SELECT * FROM sys.server_audit_specification_details; 	
SELECT * FROM sys.database_audit_specifications; 	
SELECT * FROM sys.database_audit_specifications_details; 	
SELECT * FROM fn_get_audit_file; 	
```
**Enable c2 Audit Mode**
```sql
sp_configure 'show advanced options',1;
GO
RECONFIGURE
GO
sp_configure 'c2 audit mode',1;
GO
RECONFIGURE
GO
```
**Enable Common Criteria Compliance**
```sql
sp_configure 'show advanced options',1;
GO
RECONFIGURE
GO
sp_configure 'common criteria compliance enabled',1;
GO
RECONFIGURE
GO
```
**Policy Based Management DMVs**
```sql
SELECT * FROM syspolicy_conditions;
SELECT * FROM syspolicy_policies;
SELECT * FROM syspolicy_policy_execution_history;
SELECT * FROM syspolicy_policy_execution_history_Details;
SELECT * FROM syspolicy_policy_group_subscriptions;
SELECT * FROM syspolicy_policy_categories;
SELECT * FROM syspolicy_system_health_state;
```
