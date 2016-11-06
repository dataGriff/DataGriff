---
title: SQL 2012 Admin Exam 05 Migrating Importing and Exporting
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-05 17:31:42
---


The migrating wasn't super exciting but the bulk inserts were :)

### Video Exercises

* [SQL 2012 Migrate Using Backup and Restore] (https://youtu.be/FwKvXygmp48)
* [SQL 2012 Migrate Using Database Copy Wizard] (https://youtu.be/DS9wzR9xEpk)
* [SQL 2012 Open Firewall for Database Copy Wizard] (https://youtu.be/bZgsAVhIzDE)
* [SQL 2012 Detach Attach] (https://youtu.be/hcB_qxE3CV4)
* [SQL 2012 Migrate Logins Using Scripts] (https://youtu.be/_Ma-ZCnS_Hc)
* [SQL 2012 Create New Domain User] (https://youtu.be/IY1-fwU6cJA)
* [SQL 2012 Import Export Wizard] (https://youtu.be/FMvzlJAJXQw)
* [SQL 2012 BCP and BULK INSERT] (https://youtu.be/DTXowRhj4kg)

### Key Terms

* **Upgrade Advisor** - You can run this against SQL 2005, 2008 and 2008 R2 to check if okay to upgrade to 2012.
* **Distributed Replay Utility** - Can simulate mission-critical work environments before upgrade.
* **Detach and Attach Database** - Allows you to migrate database across instances. Detached database must have no snapshots, no replication, not mirrored and not be a system database.
* **Copy Database Wizard** - Copy database across instances, choosing with Move means gets delete from source. Can copy server level objects too like jobs, logins, messages and can also schedule the move. You also create an SSIS package when creating the copy task.
* **Backup and Restore** - You can copy databases across instances using backup and restore. When copying from a previous version remember to use WITH MOVE.
* **Generate and Publish Scripts Wizard** - You can publish to a web hosting provider (web service) or produce a script to transfer a database.
* **Import/Export Wizard** - Useful tool in SSMS to easily transfer data. You can save the transfer as an SSIS package as long as user has appropriate msdb privileges.
* **BCP Utility** - bcp utility can copy data from database to file or file to database using command line.
* **BULK INSERT** - Run from database engine and can load data into a table from a file.
* **OPENROWSET(BULK)** - Connect to OLEDB linked server to bulk import data or insert from file via OLEDB provider.   

## Tips on Bulk Insert Operations
* Metadata same in source and destination objects
* Use format file if inserting from fixed length fields
* Can use a csv as data file
* Requires select and insert permissions
* Disable constraints
* ADMINISTER BULK OPERATIONS permissions
* Read permissions on file server
* Cannot bulk insert into a partitioned view
* Can bulk insert in parallel using multiple tools
* Disable triggers
* Order by from source on destination clustered index column.

### Notes

* You cannot upgrade from 32-bit to 64-bit and vice versa.
* SQL 2012 requires windows server 2008 or later.
* Pre-upgrade steps - DBCC commands to ensure consistent state, configured with AutoGrow enabled, disable startup procedures and replication, ensure enough disk space present.
* When using copy database wizard attach detach method is faster but database goes offline, object method is slower but database stays online.
* You can migrate SQL logins by scripting and source and executing on destination.
* Cannot create partitioned table using SELECT INTO.


### Scripts

**Detach a Database**
```sql
USE master;
GO
EXEC sp_detach_db @dbname = [DBName];
GO
```
**Attach a Database**
```sql
USE master;
GO
CREATE DATABASE DBName ON (FileName='C:\FilePath\FileName.mdf'),
(FileName='C:\FilePath\FileName_log.ldf') FOR ATTACH;
GO
```
**BCP From SQL into File**
```
bcp "SELECT FirstName, LastName FROM AdventureWorks2012.Person.Person ORDER BY LastName, FirstName;" queryout file.txt -c -T
```
**BCP From File into SQL**
```
bcp DBName.SchemaName.TableName in C:\Data\file.txt -T -c
```
**BULK INSERT into SQL**
```sql
BULK INSERT DBName.SchemaName.TableName FROM 'C:\Data\file.txt';
```
**OPENROWSET Bulk from File**
```sql
INSERT INTO T(XmlCol)  
SELECT * FROM OPENROWSET(  
   BULK 'c:\Folder\FileName.txt',  
   SINGLE_BLOB) AS x;  
 ```
 **OPENROWSET from OLEDB**
 ```sql
 SELECT a.*  
 FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',  
      'SELECT GroupName, Name, DepartmentID  
       FROM AdventureWorks2012.HumanResources.Department  
       ORDER BY GroupName, Name') AS a;  
  ```
  **OPENDATASOURCE from Excel INTO**
  ```sql
  SELECT * INTO TableExcel FROM OPENDATASOURCE('Microsoft.Jet.OLEDB.4.0',  
  'Data Source=C:\DataFolder\Documents\TestExcel.xls;Extended Properties=EXCEL 5.0')...[Sheet1$] ;  
   ```
   **OPENQUERY**
   ```sql
    OPENQUERY ( linked_server ,'query' )  
    ```
