---
title: SQL 2012 Admin Exam 08 Mirroring and Replication
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-12 09:39:37
---

So we're going to dip into some high availability options with mirroring and replication. High availability refers to making sure that your databases remain available when the primary fails. Rather handy!

### Video Exercises

* [SQL 2012 Mirroring Firewalls and Certificates](https://www.youtube.com/watch?v=xR7dzuI500Q&index=40&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Database Mirroring Preparation](https://www.youtube.com/watch?v=14vihg6GL6s&index=44&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&t=2s)
* [SQL 2012 Initiate Mirroring](https://www.youtube.com/watch?v=umcvUHq5WJY&index=51&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&t=8s)
* [SQL 2012 Replication Preparation](https://www.youtube.com/watch?v=zxp0OwtVsKk&index=58&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&t=3s)
* [SQL 2012 Configure Snapshot Replication](https://www.youtube.com/watch?v=qMJcUbAqWTA&index=64&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&t=2s)
* [SQL 2012 Transactional Replication and Monitor](https://www.youtube.com/watch?v=Ifv0jZx-HCI&index=67&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&t=2s)

### Key Terms

* **Mirroring** - Principal server has all transactions replicated through transaction logs to a single mirrored database and keeping them in sync.
* **Mirroring High-Safety Mode** - Failover can occur without data loss but can have higher transaction latency. (SAFETY OPTION = FULL)
* **Mirroring High-Performance Mode** - Data loss can occur but primary doesn't wait to confirm secondary has received transaction so lower latency. (SAFETY OPTION = OFF). Only enterprise supports this.
* **Mirroring Manual Failover** - Often used to perform updates on principal which require a restart.
* **Mirroring Automatic Failover** - Requires a witness and occurs when principal cannot be contacted.
* **Mirroring Forced Service** - Possible data loss where DBA forces mirror to become principal when principal cannot be contacted.
* **sys.database_mirroring** - Catalog view to see operating mode of mirror.
* **Database Mirroring Monitor** - See state of data being transferred between principal and secondary and connection status of witness. This can be launched under tasks of the principal database.

* **Replication** - Replicates objects and data between databases using a publisher, articles, publications, a distributor, a subscriber and an agent.
* **Snapshot Replication** - Complete refreshes of data. Good when infrequent changes, small changes over short period. Requires a snapshot folder that publisher can write to and subscriber can read from.  (max 32,767 articles, 1000 columns).
* **Snapshot Agent** - Uses bcp to create snapshot.
* **Transactional Replication** - Incremental updates keeping subscriber and publisher in sync as much as possible. Can actually publish from an Oracle database. Changes must be applied in same order as at publisher.  (max 32,767 articles, 1000 columns).
* **Peer-to-Peer Replication** - Peer nodes reads and writes to other nodes. Requires partitioning of data and conflicts are not resolved. You cannot filter, each instance requires own distribution database and must be enterprise.
* **Merge Replication** - Good for applications where conflicts might occur which need resolving like mobile applications. Uses triggers to track changes and publisher and subscriber exchange rows to resolve. Subscriber can be offline then push back. Non-SQL nodes may be involved. Does not have to be transactionally consistent. A conflict resolution priority logic gets configured. (max 256 articles, 246 columns).
* **Replication Monitor** - Ummm... Enables you to monitor replication! Can add publishers, configure alerts.
 * **NOT FOR REPLICATION** - Clause allows you to deal with constraints like foreign keys, triggers, identity columns for inserts, updates and deletes in replication.
* **Heterogenous Data** - Only snapshot and transactional replication for this. Oracle can publish to SQL Server 2012 and SQL Server 2012 to Oracle. Cannot subscribe to IBM DB2 publication butt can publish.

### Notes

* Mirroring to be deprecated.
* Automatic failover is possible with mirroring with a witness instance.
* Mirroring can't have FILESTREAM, distributed transactions, system databases, can't rename.
* Mirroring must have FULL recovery and must be started with a restore of database with NO RECOVERY.
* Mirroring requires traffic on port 7024.
* Mirroring requires an appropriate login on primary, secondary and witness. If it's a service account from primary then this will be needed on secondary etc.
* Mirroring steps - Restore with No Recovery > Necessary Logins > Right-click primary > Task and mirror > Security Wizard > Select and configure Witness > Setup listener port > use login that has CREATE ENDPOINT permissions for a secondary server > Same for witness > Confirm service accounts for each instance of Principal, secondary and witness  
* If not configured with  domain account must use certificate authentication for mirroring.
* Can only do certificate configured login mirroring using T-SQL not through GUI.  
* You can disable automatic failover by changing to high performance (asynchronous) or high safety without failover (asynchronous).
* Mirror upgrade - High safety no auto failover > full backup principal > DBCC CHECKDB > Upgrade mirror > Manual failover > upgrade witness > Resume mirror > switch back to high safety with failover > upgrade new mirror instance.
* Witness and principal service accounts require access to secondary so set these logins up there.

* Replication must be installed.
* Snapshot replication - Expand publisher replication node > choose local as distributor > configure SQL agent service > specify network share > choose database to publish > choose snapshot replication > choose articles (tables, procedures etc) > option to filter data > create snapshot immediately > choose snapshot agent account > can choose to script publication > name publication.
* Configure subscription - Right click Replication\\Local Subscriptions > Select Publisher > Run agent as subscriber > New database as subscriber > Select distributor > Run continuously for schedule > Initialise immediately > Create subscription
* Transactional replication - Expand publisher replication node > choose database to publish > choose transactional replication > choose articles (tables, procedures etc) > option to filter data > create snapshot immediately > choose snapshot agent account and log reader agent > can choose to script publication > create publication.
* Peer-to-Peer replication - Expand publisher replication node > choose local instance as distributor > sql agent start automatically > configure network share > choose database or objects > choose peer-to-peer replication > choose articles > specify log reader agent > name and create publication.
* Merge replication - Expand publisher replication node > choose local instance as distributor > configure network share > choose database > choose merge replication > choose SQL 2008 or later > choose articles > don't filter rows > specify snapshot agent > provide credentials for agent > name and create publication.
* Snapshot replication default every hour for creation schedule.

### Scripts
**Set Primary Database to Full Recovery for Mirroring**
```sql
USE [Master];
ALTER DATABASE [DatabaseName] SET RECOVERY FULL;
```
**Initiate Mirroring Backup and Restore Pattern**
```sql
--Backup Principal
BACKUP DATABASE [DatabaseName] TO DISK N'FilePath\\DatabaseName.bak' WITH FORMAT;
GO
--Restore to secondary
RESTORE DATABASE [DatabaseName]
  FROM DISK =  'FilePath\\DatabaseName.bak'
  WITH NORECOVERY
GO
--Back On Principal
BACKUP LOG [DatabaseName]
  TO DISK'FilePath\\DatabaseName.trn'
GO
--Restore to secondary
RESTORE LOG [DatabaseName]
  FROM DISK'FilePath\\DatabaseName.trn'
  WITH FILE=1, NORECOVERY
GO
```
**Configure Witness Endpoint from Principal**
```sql
USE [Master];
--On principal
ALTER DATABASE [DatabaseName]
  SET WITNESS = 'CTP//WITNESINSTANCENAME:7024';
```
**Prepare for Mirroring Pattern**
```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'P@$$w0rd';
GO

USE master;
GO
CREATE CERTIFICATE SQL_A_CERT WITH SUBJECT = 'SQL A Certificate';
GO

USE master;
GO
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7024,
	LISTENER_IP=ALL )  
    FOR DATABASE_MIRRORING (  
       AUTHENTICATION = CERTIFICATE SQL_A_CERT,
	   	ROLE = ALL
       );  
GO

BACKUP CERTIFICATE SQL_A_CERT
TO FILE = 'c:\\backup\\SQL_A_CERT.cer'
WITH PRIVATE KEY
(
    FILE='SQL_A_CERT',
    ENCRYPTION BY PASSWORD = 'P@$$w0rd'

);
GO
```
**Start Mirroring Pattern**
```sql
USE master;
CREATE LOGIN SQL_B_LOGIN WITH PASSWORD = 'P@$$w0rd';
GO

CREATE USER SQL_B_USER FOR LOGIN SQL_B_LOGIN;
GO

CREATE CERTIFICATE SQL_B_CERT
AUTHORIZATION SQL_B_USER
FROM FILE = 'c:\\backup\\SQL_B_CERT - Copy.cer';
GO

GRANT CONNECT ON ENDPOINT:: Endpoint_Mirroring TO [SQL_B_Login];

ALTER DATABASE AdventureMirror
	SET PARTNER = 'TCP://sqlb.contoso.com:7024';
```
**Mirror Manual Failover**
```sql
USE [Master];
ALTER DATABASE [DatabaseName] SET PARTNER FAILOVER;
```
**Mirror DMVs**
```sql
SELECT * FROM mirroring_safety_level_desc;
SELECT * FROM mirroring_witness_name;
SELECT * FROM mirroring_witness_state_desc;
```
**Monitor Mirroring Stored Procedures**
```sql
sp_dbmmonitoraddmonitoring
sp_dbmmonitordropmonitoring
sp_dbmmonitorchangemonitoring
sp_dbmmonitorresults
```
**Change mirroring to Synchrynous (high-safety)**
```sql
USE [master]
GO
ALTER DATABASE [AdventureMirror] SET SAFETY FULL
GO
```
**Change mirroring to Aynchrynous (high-performance)**
```sql
USE [master]
GO
ALTER DATABASE [AdventureMirror] SET SAFETY OFF
GO
```
