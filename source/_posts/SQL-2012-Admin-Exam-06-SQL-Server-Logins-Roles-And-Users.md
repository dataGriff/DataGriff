---
title: SQL 2012 Admin Exam 06 SQL Server Logins Roles And Users
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-05 17:32:14
---

So on to some exciting security stuff...

### Video Exercises

* [SQL 2012 Create Logins](https://youtu.be/_uJ6ZaBQvsM)
* [SQL 2012 Fixed Server Roles](https://youtu.be/Bpi1O2AUAeA)
* [SQL 2012 User Defined Server Roles](https://youtu.be/qbPP7oSMu2U)
* [SQL 2012 Create Database Users](https://youtu.be/a0lVUsNBb2A)
* [SQL 2012 Administer Database Roles](https://youtu.be/ayhosQqyr24)
* [SQL 2012 Partially Contained Databases](https://youtu.be/e6jQ42f4N90)

### Key Terms

* **Windows Authenticated Login** - Inherited from local or domain active directory.
* **SQL Server Authenticated Login** - Specific to SQL and requires password.
* **Certificate** - Can create certificate if master key is present and then a login from certificate.
* **Assymetric Key** - Can create login from asymmetric key and have a public and private key.
* **Fixed Server Roles** - sysadmin, serveradmin, securityadmin, processadmin, setupadmin, bulkadmin, diskadmin, dbcreator, public.
* **User Defined Server Roles** - Can create user defined server roles which allow bespoke server level permissions.
* **Credential** - Stores authentication which can be used for example as a proxy in a SQL agent job.
* **Database User** - User in a database (mapped to a single login).
* **Orphaned User** - A database user without a login.
* **Fixed Database-Level Roles** - db_owner, db_Securityadmin, db_accessadmin, db_backupoperator, db_ddladmin, db_datawriter, db_datareader, db_denydatawriter, db_denydatareader.
* **Flexible Database-Level Roles** - create roles with custom database permissions.
* **msdb Roles** - The msdb database has a lot of specific roles that are worth noting.
  * db_ssisadmin, db_ssisoperator, db_ssisltduser (SSIS)
  * dc_admin, dc_operator, dc_proxy (Data Collector)
  * ServerGroupAdministratorRole, ServerGroupReaderRole
  * dbm_monitor (mirroring)
  * PolicyAdministerRole
  * SQLAgentOperatorRole, SQLAgentReaderRole, SQLAgentUser
* **Partially Contained Database** - Allows for connections without external dependencies on SQL logins.
* **Contained User** - Does not have login and applied directly to database.
* **Principle of Least Privilege** - Grant only access that is required.
* **Application Role** -  Allows application to access SQL database and not based on user credentials.  

### Notes

* ALTER ANY LOGIN permission required to create logins.
* sys.server_principals catalog of logins.
* sys.database_principals, sys.database_role_members, sys.database_permissions
* ALTER ROLE and sp_addrolemember allow you to add members to roles.
* Enabling of contained databases configured at the instance level. 

### Scripts
**Create SQL Login**
```sql
CREATE LOGIN [sql_user_c] WITH PASSWORD=N'P@ssw0rd';
```
**Create Windows Login**
```sql
CREATE LOGIN [sqla\local_group_b] FROM WINDOWS;
```
**Create Certificate**
```sql
CREATE CERTIFICATE Richard_Griffiths
WITH SUBJECT = 'Richard Griffiths certificate in master database',
EXPIRY_DATE = '01/01/2020';
```
**Create Login from Certificate**
```sql
CREATE LOGIN [sqla\local_group_b] FROM CERTIFICATE Richard_Griffiths;
```
**Create Asymmetric Key**
```sql
CREATE ASYMMETRIC KEY sql_user_a WITH ALGORITHM = RSA_2048;
```
**Create Login from Asymmetric Key**
```sql
CREATE LOGIN [sqla\local_group_b] FROM ASYMMETRIC KEY sql_user_a;
```
**Alter a Login**
```sql
ALTER LOGIN sql_user_a DISABLE;
```
**Catalog of Logins**
```sql
SELECT * FROM sys.server_principals;
```
**Create Credential**
```sql
CREATE CREDENTIAL SSISStage WITH IDENTITY = 'SSISStage', SECRET = 'P@$w0rd';
```
**Add Login to Server Role**
```sql
ALTER SERVER ROLE bulkadmin ADD MEMBER [contoso\domain_group_b];
```
**Identify Fixed Server Role Members**
```sql
sp_helpsrvrolemember;
```
**Create User Defined Server Role**
```sql
CREATE SERVER ROLE Login_Manager;
```
**Grant Permissions to User Defined Server Role**
```sql
GRANT ALTER ANY LOGIN TO Login_Manager;
```
**Add Member to User Defined Server Role**
```sql
ALTER SERVER ROLE Login_Manager ADD MEMBER [contoso\domain_group_b];
```
**Check all Server Role Membership**
```sql
SELECT SP1.NAME AS 'RoleName'
,SP2.NAME AS 'LoginName'
FROM SYS.SERVER_ROLE_MEMBERS M

JOIN sys.server_principals SP1 ON
sp1.principal_id = m.role_principal_id

JOIN sys.server_principals SP2 ON
sp2.principal_id = m.member_principal_id

ORDER BY sp1.name, sp2.name
```
**Create Database User**
```sql
CREATE USER [SQLA\local_account_b] FOR LOGIN [SQLA\local_account_b];
```
**Detect Orphaned Database User**
```sql
USE DBName;
GO
sp_change_users_login @Action='Report';
GO
```
**Create Database Role**
```sql
CREATE ROLE TableAdmin;
```
**Grant Permission to Database Role**
```sql
GRANT CREATE SCHEMA TO TableAdmin;
```
**Add Member to Database Role**
```sql
EXEC sp_addrolemember 'TableAdmin','sql_user_c';
```
**Create Partially Contained Database**
```sql
CREATE DATABASE partialdb
CONTAINMENT = PARTIAL;
```
**Create Partially Contained Windows User**
```sql
USE partialdb;
CREATE USER [contoso\contained_user_b];
```
**Create Partially Contained SQL User**
```sql
USE partialdb;
CREATE USER [contained_user_c] WITH PASSWORD='Pa$$w0rd';
```
**Create Application Role**
```sql
USE DBNAME;
CREATE APPLICATION TOLE app_role_test WITH PASSWORD 'P@$$w0rd';
```
