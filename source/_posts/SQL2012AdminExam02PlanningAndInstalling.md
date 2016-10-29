---
title: SQL 2012 Admin Exam 02 Planning And Installing
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-10-29 17:05:49
---

So I'm on to the actual first chapter of the Administration book! Still a long way to go but here's what I learned...

### Key Terms

* **Scaling up** - Increasing system resources on current server. e.g. adding more RAM.
* **Scaling out** - Increase capacity across servers. e.g. AlwaysOn with readable secondaries.  
*  **[SQLIO](https://www.microsoft.com/en-gb/download/details.aspx?id=20163)** - Checks I/O capacity before SQL server deployed. Tests IOPS (I/Os per second), throughput (MB/s) and latency.    
* **[SQLIOSIM](https://support.microsoft.com/en-gb/kb/231619)** - Simulate read, write, checkpoint, backup, sort and read0ahead activities for SQL server.     
* **[Chkdsk](https://en.wikipedia.org/wiki/CHKDSK)** - Tool that checks for disk errors.
* **Port 1433** - Database engine default instance.
* **Port 1434** - Dedicated admin connection.
* **ConfigurationFile.ini** - Stores installation settings and can reuse to ensure that same installation carried out for multiple servers.

### Notes

* Consider operating system, processor, RAM, hard disk, software and virtualization requirements.
* In general think newer OS for more high spec SQL. 
* 64-bit version of most SQL editions require 1.4Ghz minimum or recommended 2.0Ghz processor.
* Minimum RAM is 1GB with recommend 4GB.
* Express can do 512MB minimum ram with 1GB recommended.
* You can't install reporting services on Windows Server 2008 R2 SP1 Enterprise Edition in Server Core Configuration.
* Can't install 64-bit Enterprise SQL 2012 on Windows 7 or Windows Server 2003.
* SSIS can only run on servers.
* Be careful when implementing the firewall allow program rule through, should use the % path and not direct drive letter e.g.
**%ProgramFiles%\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe**
* You can see what features are installed on SQL Server instance using a [HTML summary report](https://blogs.msdn.microsoft.com/samlester/2013/06/13/sql-server-2012-discovery-report-determining-what-sql-server-components-are-installed/) which is dont via installation centre.
* You can run SSIS in 32-bit mode on 64-bit machine if have SSDT installed.
* Use Add/Remove programs to remove a feature.
* Developer edition is good for developers as license is okay and same features as Enterprise.
* Make sure 4GB of space on operating system before installing SQL 2012.

### Scripts

**Grow a database**
```sql
USE master;
    ALTER DATABASE [DBName] MODIFY FILE
      (Name=N'DBName_Data'), SIZE=256000KB;
GO
```
**Shrink a database**
```sql
USE DBName;
GO
DBCC SHRINKDATABASE(N'DBName')
GO
```
**Add a File**
```sql
ALTER DATABASE DBName
ADD FILE
(
    NAME = DBName1,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\DBName1.ndf',
    SIZE = 100MB,
    MAXSIZE = 1000MB,
    FILEGROWTH = 5MB
)
TO FILEGROUP PRIMARY;
```
**Add a FileGroup**
```sql
ALTER DATABASE DBName
  ADD FILEGROUP DBName_2;
```
**Example Core Command-Line Installing SQL**
```
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,Replication /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /TCPENABLED=1 /IACCEPTSQLSERVERLICENSETERMS  
```
**Command-Line Connect to Another Server**
```
Sqlcmd.exe -S ServerName
```

### Videos

* [SQL 2012 Configuration Checker](https://www.youtube.com/watch?v=I22ATtPU6QI&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=1)
* [SQL 2012 Configure Firewall Rule](https://www.youtube.com/watch?v=C--Spuldyps&t=1s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=2)
* [SQL 2012 Configure Firewall Rule Properly](https://www.youtube.com/watch?v=8PQ7hfeqdiw&t=1s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=3)
* [SQL 2012 Install](https://www.youtube.com/watch?v=7IbOerftR9o&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=4)
* [SQL 2012 Add Features to an Installation](https://www.youtube.com/watch?v=QKEiweNexD4&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=5)
* [SQL 2012 Core Install](https://www.youtube.com/watch?v=1v_ukk--kBw&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=6)
* [SQL 2012 Attach a Database](https://www.youtube.com/watch?v=UUnGAg6bPAM&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=7)
