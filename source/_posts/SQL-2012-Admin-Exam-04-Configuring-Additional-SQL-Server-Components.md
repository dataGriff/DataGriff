---
title: SQL 2012 Admin Exam 04 Configuring Additional SQL Server Components
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-10-30 11:20:43
---

So on to the third chapter in 2 days! My brain may be slightly frazzled...

### Video Exercises

* [SQL 2012 Configure FILESTREAM](https://youtu.be/tUWm6qNte20)
* [SQL 2012 Create a FileTable](https://youtu.be/h2615ZtaZXk)
* [SQL 2012 Create Full Text Index](https://youtu.be/B0XF02sHKeM)
* [SQL 2012 Create Partitioned Table](https://youtu.be/36D9HColDeY)
* [SQL 2012 Transparent Data Encryption](https://youtu.be/ugrzwf8ZPzw)
* [SQL 2012 Data Compression](https://youtu.be/PLfL7FZbnWs)
* [SQL 2012 Managing Transaction Logs](https://youtu.be/DJ_LFyEylIU)

### Key Terms

* **Full-Text Indexing** - Stores significant words and their location. There can be only one per table or indexed view and can contain up to 1024 columns. This can then be used in querying used commands like CONTAINSTABLE and FREETEXTTABLE.
* **FileStream** - Allows SQL Server to store unstructured data like files and documents (BLOBs, Binary Large Objects).
* **FileTables** - Allows you to store files and documents in SQL server and can be accessed via applications as if they were stored in file system.
* **Contained Database** - All settings and properties within database and can connect without authentication at server level.
* **Row-Level Compression** - Less CPU than page but not as efficient and useful for fixed length data types.
* **Page-Level Compression** - More CPU that row and compresses repeating data.
* **Unicode Compression**- - Good for NVARCHAR and NCHAR types.
* **Transparent Data Encryption (TDE)** - Encrypts database that requires no extra effort in application layer.
* **Partition Function** - Defines how rows of index map to partitions based on values of Partitioning column.
* **Partition Scheme** - Maps partition function to filegroups.
* **Partitioning Column** -Column or index that partition function uses.
* **Aligned Index** - Index that uses same partition scheme as the table to which it belongs.
* **NonAligned Index** - Index independently partition from table to which it belongs.
* **Partition Elimination** - Optimiser can access only partitions it needs for query.


**There are a shedload of [DBCC Commands..]()**
* **Maintenance** - DBCC CLEANTABLE, DBCC DBREINDEX, DBCC DROPCLEANBUFFERS, DBCC FREEPROCCACHE, DBCC INDEXDEFRAG, DBCC SHRINKDATABSE, DBCC SHRINKFILE, DBCC UPDATEUSAGE
* **Informational** -  DBCC INPUTBUFFER, DBCC OPENTRAN, DBCC OUTPUTBUFFER, DBCC PROCACHE, DBCC SHOW_STATISICS, DBCC SHOWCONTIG, DBCC SQLPERF, DBCC TRACESTATUS, DBCC USEROPTIONS
* **Validation** -  DBCC CHECKALLOC, DBCC CHECKCATALOG, DBCC CHECKCONSTRAINTS, DBCC CHECKDB, DBCC CHECKFILEGROUP, DBCC CHECKIDENT, DBCC CHECKTABLE
* **Miscellaenous** -  DBCC dllname (FREE), DBCC FREESESSIONCACHE, DBCC HELP, DBCC TRACEOFF

### Notes

* DComconfg.exe is a tool to give non-admin users access to IS.
* Reporting Services Configuration manager is how you would changed the RS execution account.
* To enable FILESTREAM you must edit properties of SQL server Service in SQL Server Configuration manager.
* FILESTREAM steps: SQL Server Config Manager > Properties > FILESTREAM enable > sp_configure > Restart > FILESTREAM Filegroup > FILESTREAM Filegroup
* FileTable steps: Enable FILESTREAM > NON_TRANSACTED_ACCESS = FULL on Database > Create table as FileTable
* TDE Steps: Create master encryption key > Certificate > Database Encryption Key > Encrypt database

### Scripts

**Install AS from Command-Line in MultiDimensional Mode**
```
Setup.exe /q /IAcceptSQLServerLicenseTerms /Action=install /Features=AS /ASSERVERMODE=MULTIDIMENSIONAL /INSTANCENAME=ASMulti /ASSVCACCOUNT=contoso\kim_akers /ASSYSADMINACCOUNTS=contoso\kim_akers
```
**Install AS from Command-Line in Tabular Mode**
```
Setup.exe /q /IAcceptSQLServerLicenseTerms /Action=install /Features=AS /ASSERVERMODE=TABULAR /INSTANCENAME=ASTabular /ASSVCACCOUNT=contoso\kim_akers /ASSYSADMINACCOUNTS=contoso\kim_akers
```
**Install AS from Command-Line in PowerPivot Mode**
```
Setup.exe /q /IAcceptSQLServerLicenseTerms /Action=install /Features=AS /ASSERVERMODE=POWERPIVOT /INSTANCENAME=ASPower /ASSVCACCOUNT=contoso\kim_akers /ASSYSADMINACCOUNTS=contoso\kim_akers
```
**Powershell RS Prerequisite 01**
```
Import-module ServerManager
```
**Powershell RS Prerequisite 02**
```
Add-WindowsFeature Web-Server -IncludeAllSubfeature
```
**Install RS from Command-Line**
```
setup.exe /q /IAcceptSQLServerLicenseTerms /Action=instal/ Features=SQL,RS,TOOLS /INSTANCENAME=RPTSVR /SQLSYSADMINACCOUNTS="BUILTIN\ADMINISTRATORS" /RSSVCACCOUNT=NetworkService /SQLSVCACCOUNT=NetworkService /AGTSVCACCOUNT=NetworkService /RSSVCSTARTUPTYPE="Manual" /RSINSTALLMODE="DefaultNativeMode"
```
**Create Full Text Catalog**
```sql
USE DbName;
GO
CREATE FULLTEXT CATALOG CatalogName;
GO
CREATE FULLTEXT INDEX ON SchemaName.TableName
(
Column1, Column2, Column3
)
KEY INDEX PK_SchemaName_TabName_Column1
ON CatalogName;
GO
```
**Drop Full Text Catalog**
```sql
DROP FULLTEXT INDEX ON SchemaName.TableName;
```
**Configure FILESTREAM to allow T-SQL and Win32 Streaming**
```sql
EXEC sp_configure filestream_access_level, 2
RECONFIGURE
```
**Create a FILESTREAM FileGroup**
```sql
ALTER DATABASE DBName ADD
FILEGROUP FileStreamFG CONTAINS FILESTREAM;
```
**Create a FILESTREAM File**
```sql
USE master;
ALTER DATABASE DBName ADD FILE
( NAME = FileStrmFile
, FILENAME = N'C:\FSTRM')
TO FILEGROUP FileStreamFG;
```
**Enable FILESTREAM NON_TRANSACTED_ACCESS**
```sql
USE master;
ALTER DATABASE DBName
SET FILESTREAM (NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory')
```
**Create FileTable**
```sql
CREATE TABLE DocStore AS FileTable;
```
**Row-Level Compression**
```sql
ALTER TABLE TabName REBUILD WITH (DATA_COMPRESSION=ROW);
ALTER INDEX idx_Name ON TabName REBUILD WITH (DATA_COMPRESSION=ROW);
```
**Page-Level Compression**
```sql
ALTER TABLE TabName REBUILD WITH (DATA_COMPRESSION=PAGE);
ALTER INDEX idx_Name ON TabName REBUILD WITH (DATA_COMPRESSION=PAGE);
```
**Create Partition Function**
```sql
CREATE PARTITION FUNCTION PartFun ( INT )
AS RANGE LEFT FOR VALUES (100,200);
```
**Create Partition Scheme**
```sql
CREATE PARTITION SCHEME PartScheme
AS
	PARTITION PartFun
	TO (FG1, FG2, FG3)
GO
```
**Create Partitioned Table**
```sql
CREATE TABLE Test
(
id int,
value char(30)
) on PartScheme(id)
```
**Transparent Data Encryption (TDE) Part 1 - Create Master Key**
```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Password';
GO
```
**TDE Part 2 - Create Certificate**
```sql
USE master;
GO
CREATE CERTIFICATE ServerCert WITH SUBJECT = 'Server Cert';
GO
```
**TDE Part 3 - Create Database Encryption Key (DEK)**
```sql
USE DBName;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_128
ENCRYPTION BY SERVER CERTIFICATE ServerCert;
GO
```
**TDE Part 4 - Enable Encryption**
```sql
USE master;
GO
ALTER DATABASE DBName
SET ENCRYPTION ON;
GO
```
**TDE Backup Certificate**
```sql
BACKUP CERTIFICATE ServerCert
TO FILE = 'ServerCertExport'
WITH PRIVATE KEY
(
    FILE='PrivateKeyFile',
    ENCRYPTION BY PASSWORD = 'Password'
);
GO
```
**Add a Log File**
```sql
USE [master]
GO
ALTER DATABASE [DBName] ADD LOG FILE
( NAME = N'LogFile2'
, FILENAME = N'FilePath'
, SIZE = 1024KB , FILEGROWTH = 10%)
GO
```
**Trigger a Log CheckPoint**
```sql
CHECKPOINT;
```
**Check to see Log Space Used**
```sql
DBCC SQLPERF(LOGSPACE);
```
