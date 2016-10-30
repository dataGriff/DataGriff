---
title: SQL 2012 Admin Exam 03 Configuring and Managing Instances
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-10-30 08:49:43
---

So on to the second chapter in 2 days, having to smash it on the weekend when I can!

### Video Exercises

* [SQL 2012 Configure Min Memory and Fill Factor](https://www.youtube.com/watch?v=cEaCRPlMUr8&index=14&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Configuring Resource Governor](https://www.youtube.com/watch?v=HV_0K9Gjwso&index=15&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Configure Database Mail](https://www.youtube.com/watch?v=TLzhw3Z-hSA&index=8&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Modeldb to Standardise Databases](https://www.youtube.com/watch?v=J_2Es-3Euq0&index=9&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Install Extra Instance](https://www.youtube.com/watch?v=0V0Y6qyakRo&index=10&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Connect Extra Instance](https://www.youtube.com/watch?v=CzIKDKzFV30&index=11&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Firewall Extra Instance](https://www.youtube.com/watch?v=4XXAMHL1Reg&index=12&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)
* [SQL 2012 Cycle Error Log](https://www.youtube.com/watch?v=WudFEC14Noc&index=13&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1)

### Key Terms

* **Fill Factor** - This is the percentage of disk space taken up in an indexes leaf node.
* **Process Affinity** - Assigns worker threads to specific server processors (CPUs).
* **I/O Affinity** - Binds instance disk I/O operations to specific set of CPUs.
* **Model Database** - The model database allows for standardisation of databases being created. For example if you set the model database recovery mode to SIMPLE and create a new database on that instance, the new database will have SIMPLE recover mode also.
* **Distributed Transaction Coordinator (MSDTC)** - Needed for when installing integration services and have transactions across servers (not needed across instances on same server).  
* **Resource Governor** - Allows only certain amount of resource to be used in a **resource pool** that you assign **workload groups** to. Found under management node and only available for database engine. Works on a per instance basis.
* **Windows System Resource Manager (WSRM)** - Allocate system resources to specific processes such as different database engine instances. Therfore this configures resource across instances unlike resource governor which is within instance. You can configure CPU equal per process, equal per user or equal per session.

### Notes

* Instance level settings include general, memory, processors, security, connections, database settings, advanced and permissions.
* Use memory settings to balance memory across instances.
* Database mail configured under management and you create a profile then accounts within the profile. You can have public profiles that can be accessed by any DBMail users and private where must be an msdb user.
* Remember to run Reconfigure after sp_configure.
* You can create classifier functions for resource governor that allow for dynamic allocation of resources that can be sourced from a configuration table. e.g. for Day and Night working.
* You can cycle the SQL error and SQL agent error log and also configure maximum number of error log files.
* You can deploy 50 instances of SQL server on a non-clustered server running windows server 2008 R2 enterprise.

### Scripts

**Configure a Server (Min and Max Memory)**
```sql
EXEC sys.sp_configure 'show advanced options', 1
GO
RECONFIGURE
EXEC sys.sp_configure 'min server memory', 1024
GO
EXEC sys.sp_configure 'max server memory', 8096
GO
RECONFIGURE
GO
```
**Configure a Server (Fill Factor)**
```sql
EXEC sys.sp_configure 'show advanced options', 1
GO
RECONFIGURE
EXEC sys.sp_configure 'fill factor', 90
GO
RECONFIGURE
GO
```
**Configure Affinity (sql workload across CPUs 2 and 3)**
```sql
ALTER SERVER CONFIGURATION SET PROCESS AFINITIY CPE = 2,3
```
**Configure Affinity (sql workload across all CPUs)**
```sql
ALTER SERVER CONFIGURATION SET PROCESS AFINITIY CPE = AUTO
```
**Set Model Database Recovery Mode to Simple**
```sql
USE [master]
GO
ALTER DATABASE [model] SET RECOVERY FULL WITH NO_WAIT
GO
```
**Enable Database Mail**
```sql
EXEC sys.sp_configure 'show advanced options', 1
GO
RECONFIGURE
EXEC sys.sp_configure 'Database Mail XPs', 1
GO
RECONFIGURE
GO
```
**Create Database Mail Account**
```sql
EXEC msdb.dbo.sysmail_add_account_sp
@account_name = 'Griff'
,@email_Address = 'griff@datagriff.com'
,@mailserver_name = 'smtp.datagriff.com'
```
**Configure Database Mail**
```sql
EXEC msdb.dbo.sysmail_configure_sp
'AccounttretryAttempts',15;
```
**Install Non-Default Instance on SQL Core**
```
Setup.exe /qs /Action=Install /Features=SQLEngine /INstanceName=Alternate /SQLSYSADMINACCOUNTS="Contoso\Kim_Akers" /IACCEPTSQLSERVERLICENSETERMS
```
**Update Patch on Core**
```
<Package_name>.exe /qs /IACCEPTSQLSERVERLICENSETERMS /Action=Patch /InstanceName=ALTERNATE
```
**Remove Patch on Core**
```
<Package_name>.exe /qs /IACCEPTSQLSERVERLICENSETERMS /Action=RemovePatch /InstanceName=ALTERNATE
```
**Enable Resource Governor**
```sql
ALTER RESOURCE GOVERNOR RECONFIGURE;
```
**Disable Resource Governor**
```sql
ALTER RESOURCE GOVERNOR DISABLE;
```
**Create Resource Pool**
```sql
CREATE RESOURCE POOL poolTest
WITH (MIN_CPU_PERCENT=25);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
**Create Workload Group**
```sql
CREATE WORKLOAD GROUP groupTest
USING poolTest;
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
**Use Resource Governor Classifier Function (user created)**
```sql
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION=dbo.DayNightClassifier)
ALTER RESOURCE GOVERNOR RECONFIGURE
GO
```
**Cycle SQL Error Log**
```sql
EXEC sp_cycle_errorlog;
GO
```
**Cycle SQL Agent Error Log**
```sql
USE msdb;
EXEC sp_cycle_agent_errorlog;
GO
```
