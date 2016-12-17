---
title: SQL 2012 Admin Exam 12 SQL Agent Backup and Restore
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-25 19:13:09
---

### Video Exercises

* [SQL 2012 Configure SQL Agent](https://www.youtube.com/watch?v=etyataFYx08&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=61)
* [SQL 2012 Create Agent Job](https://www.youtube.com/watch?v=HjnnRYN3uI4&t=36s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=62)
* [SQL 2012 Create Operator](https://www.youtube.com/watch?v=QL6_GvZkokY&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=63)
* [SQL 2012 Create Alert](https://www.youtube.com/watch?v=z2MtAQ4gx3k&t=2s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=64)
* [SQL 2012 Create Backup Device](https://www.youtube.com/watch?v=EWyvEjr1wQo&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=65)
* [SQL 2012 Perform Backups](https://www.youtube.com/watch?v=Urtcxw_tqG4&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=66)
* [SQL 2012 Restore a Database](https://www.youtube.com/watch?v=G9RgSGSCr4s&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=67)

### Key Terms

* **SQL Server Agent** - Automates execution of jobs.
* **SQL Server Agent Service account** - Requires logon as a service right, membership of windows 2000 compatible access security group at domain level, should not be member of local Administrators, must be member of SQL sysadmin server role.
* **SQLAgentUserRole** - Least privileges, local jobs and scheduled jobs they own. No to multi-server.
* **SQLAgentReaderRole** - Includes above and view properties and histories of all jobs and schedules. Yes to multi-server.
* **SQLAgentOperatorRole** - As above and can start, stop, delete history of any local job. Also enable and disable schedules.
* **SQL Agent Error Log** - Warnings and errors about sql agent. Found under sql agent.
* **Alerts** - When certain condition met (performance, WMI events) can trigger an alert.
* **Job Activity Monitor** - To monitor job activity!  
* **Operators** - Who receive notifications. Email, pager notifications, net send.

* **Full Database Backup** - Everything backed up and all transactions during full backup.
* **Differential Backups** - Backups what changed since last FULL, even if differentials in between. Always since last full.
* **Transaction Log Backups** - Backs up changes since last transaction log backup and truncates transaction log.
* **File and FileGroup Backups** - Backs up individual files and filegroups, must back up transaction log.
* **Copy-Only Backup** - Functionally same as full or transaction log backups but do not mess up backup sequence.
* **Backup Compression** - Saves disk space but increases CPU.
* **Recovery Point Objective (RPO)** - Allowable data loss in event of failure.
* **Simple Recovery Mode** - No transaction, no point in time, just last full backup.
* **Full Recovery Mode** - Transaction logs and allows for point in time.
* **Bulk-Logged Recovery Mode** - Special for bulk operations, minimises transaction log, no point in time recovery.
* **Database Checkpoint** - writes modifies pages held in memory to disk and records in transaction log.
* **Media Set** - single file, multiple files, tape, multiple tapes, up to 32 backup devices.

* **RESTORE WITH RECOVERY** - Restores database back to normal state.
* **RESTORE WITH NORECOVERY** - Restores database but not accessible.
* **RESTORE WITH STANDBY** - Restores database in read-only mode for admins.
* **File Restore** - Restore a database file.
* **Page Restore** - restore small number of pages damaged where rest of database is not. Full or bulk-logged, offline or online, read/write filegroups only, only database pages (not transaction log).
* **Automatic Page Repair** - AlwaysOn Availability Groups and database mirrors can repair own pages from health secondaries.

### Notes
* SQL Agent setting configured through SQL Server configuration manager.
* For multi server job processing Agent service account needs to be in the TargetServerRole in msdb.
* Restart agent to apply config changes.
* SQL Agent Mail - Right click agent > click alerts > enable mail profile tick > select DB mail > restart agent service.
* SQL Agent Alert - Right-click alerts under agent > enter details in general page e.g. alert name, counter, value, condition for alert > Response tab is kicking off job etc > Options - can e-mail, pager or net send message.
* You can specify event for specific error number, severity and error contains message text.
* You can configure jobs on a schedule, or when sql starts, or when CPU falls below idle threshold.  
* Operator - right-click operators under sql agent node > new operator > name and address of operator.
* Multi Server jobs - right click agent > choose multi server administration > make this a master > master server operator information > configure target servers > master server login credentials create a new login.
* Target servers report to master servers.


* You cannot backup offline data.
* Altering a database to add a file during a backup will not complete until after backup completes.
* Resource governor could be used to limit CPU during compressed backup operation.
* System databases SIMPLE recovery by default.
* Backup master and msdb regularly. Model only when change it. Tempdb always regenerated so no need to backup.
* Replication - publisher - backup master, msdb and publication databases.
* Replication - Distributor - backup master, msdb and distribution databases.
* Replication - Subscriber - backup master, msdb and subscription databases.
* Can only backup primary with mirroring and cannot use BACKUP LOG WITH NORECOVERY.
* AlwaysOn backup options - Prefer Secondary (default, primary still possible), secondary only, primary, any replica (has preference settings).
* Can change target recovery (Checkpoint) time at instance in server properties (Database settings) or using sp_configure.
* Backups can go to local volumes, network shares, compatible tapes.
* Add media set SSMS - Server Objects Node > New Backup Device > Provide device name (or path)
* Perform backup - SSMS > right-click Database > Tasks > Back Up > Name backup set > existing backup device or create > backup to new or existing media set > option to checksum > option to compress.
* You can see backup events in log file history.

* Restore - SSMS - perform tail log backup (checkbox in backup options) > Right-click db to restore > if backups were done in SSMS or maintenance plan will auto populate to get to most recent > You can click timeline button to get to a point in time > RECOVERY, NORECOVERY or STANDBY >  
* Remember take tail-log backup before a restore of a restore from full backup.
* File Restore - tail-log backup if possible otherwise complete restore > restore most recent backup that includes file > restore differential backups > restore transaction logs in sequence
* You can perform online secondary filegroup restores as long as primary is online.  
* Page Restores - Right-click db > tasks > restore > check database pages to find corrupt > Ok to restore those that are corrupt.
* Can only restore database with TDE when have certificate and private key. Make sure include both of these in backup strategy for TDE databases. Once restored if certificate not present need to restore this too.
* When restore databases involved in replication you must restore master and msdb on instance too.

### Scripts
**SQL Agent Scripts**
```sql
sp_add_job --creates job
sp_add_jobstep --creates job step
sp_add_schedule --creates schedule
sp_attach_schedule --attaches schedule to existing job
sp_add_jobserver --Adds job to server
sp_msx_enlist --execute on target server to enlist target server in master server
```

**Enable Instance Backup Compression**
```sql
EXEC sys.sp_configure [backup compression default],1;
GO
RECONFIGURE WITH OVERRIDE;
GO
```
**Alter Availability Group Backup Settings**
```sql
ALTER AVAILABILITY GROUP [AG_NAME] SET ( AUTOMATED_BACUP_PREFERENCE = SEONDARY_ONLY);
```
**Alter Checkpoint Time at Database Level**
```sql
ALTER DATABASE [DBName] SET TARGET_RECOVERY_TIME = 1; --in minutes
```
**Add Backup Device**
```sql
EXEC sp_addumpdevice 'disk', 'BackupName','c:\\backup\\backupname.bak';
```
**Full Backup**
```sql
BACKUP DATABASE [DBName] to [BackupName]; --device already exists
```
**Differential Backup**
```sql
BACKUP DATABASE [DBName] to [BackupName] WITH DIFFERENTIAL; --device already exists
```
**Log Backup**
```sql
BACKUP LOG [DBName] to [BackupName] WITH DIFFERENTIAL; --device already exists
```
**DMV Backup History**
```sql
SELECT * FROM msdb.dbo.backupset;
```

**Restore Database Pages**
```sql
RESTORE DATABASE [DBNAME] PAGE='2:42, 2:81' FROM FileBackupName WITH NORECOVERY; --restores pages
RESTORE LOG DBName FROM log_backup1 WITH NORECOVERY; --restores most recent log
BACKUP LOG DBName TO log_backup_new --takes another log backup
RESTORE LOG FROM log_backup_new WITH RECOVERY; --restores log backup just taken and back online
```
**Automatic Page Repair DMVs**
```sql
SELECT * FROM sys.dm_hadr_auto_page_repair; --for AlwaysOn Availability Groups
SELECT * FROM sys.dm_db_mirroring_auto_page_repair; --for mirroring
```
**Automatic Page Repair DMVs**
```sql
SELECT * FROM sys.dm_hadr_auto_page_repair; --for AlwaysOn Availability Groups
SELECT * FROM sys.dm_db_mirroring_auto_page_repair; --for mirroring
```
**Restore Master Database**
```sql
RESTORE DATABASE master FROM BackupDevice WITH REPLACE;
```
**Rebuild System Databases**
```
setup /Q /ACTION=REBUILDDATABASE /INSTANCENAME=MSSQLSERVER
```
**Check Database Properties**
```sql
SELECT databasepropertyex('databasename','Status'); --Whether ONLINE, OFFLINE, RESTORING, RECOVERING, SUSPECT, EMERGENCY
SELECT databasepropertyex('databasename','UserAccess'); --SINGLE_USER, RESTRICTED_USER, MULTI_USER
SELECT databasepropertyex('databasename','Updateability'); --READ_WRITE, READ_ONLY
```
