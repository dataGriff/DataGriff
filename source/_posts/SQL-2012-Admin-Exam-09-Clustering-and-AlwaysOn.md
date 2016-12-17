---
title: SQL 2012 Admin Exam 09 Clustering and AlwaysOn
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-12 14:28:01
---

Two more high availability options with clustering and the new super duper AlwaysOn!

Right I'm not going to lie to you.. I failed to get the cluster to work and so my AlwaysOn was scuppered as well as it required windows clustering. Unfortunately my exam deadline was tight so I just had to move on. I caught up with AlwaysOn by watching some cbtNuggets online and also looked at how it was setup in work. The only exercises I have failed on gutted! Still here are some notes on clustering and AlwaysOn, not my strongest subjects!

### Video Exercises

* [SQL 2012 Add Cluster User and Logon as a Service Right](https://www.youtube.com/watch?v=HXBve36imJg&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=46)
* [SQL 2012 Advanced Cluster Preparation](https://www.youtube.com/watch?v=LOxo6-m7S3k&t=25s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=47)
* [SQL 2012 Create DNS Record for Cluster](https://www.youtube.com/watch?v=LOxo6-m7S3k&t=25s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=47)
* [SQL 2012 Create ISCSI Targets and Disks](https://www.youtube.com/watch?v=1TUpu5Az7BM&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=49)
* [SQL 2012 Install ISCSI Target](https://www.youtube.com/watch?v=pLZAuKdcIfc&t=4s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=50)
* [SQL 2012 Initalise Volumes and Create Simple Volumes](https://www.youtube.com/watch?v=vga9MiV-1fA&t=6s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=51)
* [SQL 2012 ISCSI Initiator to Target](https://www.youtube.com/watch?v=79Oq_bGUDe8&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=52)
* [SQL 2012 Organisational Unit and Firewall Rules](https://www.youtube.com/watch?v=MG9uJVpeWp8&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=53)
* [SQL 2012 AlwaysOn Firewall Rules](https://www.youtube.com/watch?v=DgZKQ9kRsto&t=5s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=55)

### Key Terms

* **Clustering** - High availability which stores database files on shared  (e.g. SAN) where if one instance fails another takes over.
* **iSCSI Software** - Required when setting up a virtual machine SAN. Configure in administrative tools.
* **iSCSI Target** - Computer that will act as the shared storage for virtual machines.
* **Logical Unit Number (LUN)** - Need a LUN for quorum information and database files.   
* **Stretch Cluster** - Geographically dispersed cluster.   
* **Multi-Subnet Failover Clustering** - Each node on different TCP/IP subnet and does not use shared storage.    
* **Quorum Failure** - Caused by communications failure in cluster or issues with the configuration.  

* **AlwaysOn Availability Group** - Collection of user databases called availability databases. Can have up to four read-write secondary replicas and one or more read-only secondary replicas.
* **AlwaysOn Availability Databases** - User databases that can fail over together.  
* **AlwaysOn Asynchrynous-commit Mode** - Primary does not wait for secondary to commit log. Has minimum transaction latency and good when replicas in geographically dispersed locations.
* **AlwaysOn Synchrynous-commit Mode** - Primary waits for secondary to commit log. Has higher transaction latency but minimal data loss.
* **Automatic Failover** - The current primary and one secondary must be configured with failover mode set to AUTOMATIC, the secondary must be SYNCHRYNOUS, and there is not data loss.
* **Planned Manual Failover** - Initiated by a DBA for example when maintenance needs to occur. One secondary needs to be in SYNCHRONIZED state and primary and replica in Synchrynous-commit mode. Has no data loss.
* **Forced Manual Failover** - Possible data. Use when no secondary in SYNCHRONIZED state.
* **Failover Availability Group Wizard** - Right-click availability group on secondary server in SSMS and click failover to start this manual failover.
* **Readable Secondary Replicas** - Service read-only access. Can be configured Yes, No and read-intent. Yes allows all connections but only for read access but read-intent only allows read connections.
* **Availability Group Listener** - Network connectivity endpoint for an availability group. Client connects to listener which routes to the primary replica. One listener per availability group.  

### Notes

* Clustering on enterprise supports 16 nodes, BI and standard edition supports 2 nodes.
* Windows server failover cluster must work before sql server cluster can.
* Configure failover cluster - Connect each node to shared storage > Fail Over Manager Console > Create a Cluster > Add nodes > Name and IP of cluster > Confirm to create.
* Install SQL Failover Cluster - .Net Framework 3.5.1 Installed > SQL Setup.exe on first node > Advanced Cluster Preparation > Select features for cluster > Instance Properties > Disk Space > Domain Account for Service Account > Install.. Do this on all nodes.
* Complete SQL Cluster - SQL Setup.exe > Advanced Cluster Completion > Instance and SQL Network Name (must be different from any existing instance) > Name cluster resource group > select disk for default drive of databases > IP address for cluster resource > collation > Auth Mode and SA > Install
* You only need to complete the SQL cluster on the node that has control of the hardware.
* Manual Failover - Administrative Tools > Failover Cluster Manager > SQLRG Node > Move to another node > SQLCRG to SQL-X > Verify SQLCRG on SQL-X Node.
* Can use powershell to move from one node to another.
* If failed node is irreparable evict it through failover cluster manager. Fix hardware and then re-add to cluster.
* When creating a new cluster you must be logged in with a domain account! Won't work otherwise.

### Notes when creating windows cluster...
* I had a lot of difficulty with this.
* Ensure get all these [firewall rules](https://technet.microsoft.com/en-us/library/dd573342(v=ws.10).aspx) in place. I did them on the DC machine and in the group policy object that goes to all the computers in the network.
* Do some gpupdate /force in command prompt. Do some reboots.
* Remove and re-add the cluster in DNS if need to.
* Don't forget to reboot machines after Advanced Cluster Preparation!
* Sacrifice a lamb.  

* AlwaysOn only in SQL 2012 enterprise.
* AlwaysOn must be on windows server failover cluster and cannot be domain controller.
* Deploying AlwaysOn Availability Groups - Create mirroring endpoint > enable AlwaysOn > Create Availability Group > Create Listener > Create Secondary Replica.
* Can only be one mirroring endpoint per instance. (check sys.database_mirroring_endpoints)
* Can use mirroring endpoints when using windows authentication but must use certificates if using local account.
* Enable AlwaysOn - Configuration Manager > SQL Services node > Go to Instance > Enable AlwaysOn > Restart
* Create Availability Group - SSMS of Primary > Expand AlwaysOn Availability Node > Right-Click Availability Groups > Name AG > Select User Databases > Add Replicas > Specify Network Share > all done except configure listener > complete wizard.
* Add Listener - SSMS > AlwaysOn Node > Expand AG > Right-Click Create Listener > Specify DNS name and TCP Port > Select DHCP or Static IP.
* Max number of replicas is 4, with one primary. So 5 in total.
* Failover occurs per replica.  
* Can only add replicas if primary online, you can connect to new replica, replica can connect to mirroring endpoint of primary, AlwaysOn enabled on replica.
* Only one failover cluster partner can hold replica, failover cluster supports only manual failover
* If using AlwaysOn with failover cluster must use backup and restore to prepare secondary database rather than join to availability group.
* Listener must have unique DNS name.


### Scripts
**Configure iSCSI Software Target**
```
netsh advfirewall firewall add rule name = Microsoft iSCSI Software Target Service-TCP-3260" dir=in action=allow protocol=TCP localport=3260

netsh advfirewall firewall add rule name = Microsoft iSCSI Software Target Service-TCP-135" dir=in action=allow protocol=TCP localport=135

netsh advfirewall firewall add rule name = Microsoft iSCSI Software Target Service-UDP-138" dir=in action=allow protocol=UDP localport=138

netsh advfirewall firewall add rule name = Microsoft iSCSI Software Target Service-TCP-3260" dir=in action=allow program=""%SYtemRoot%\System32\WinTarget.exe" TCP ""enable=yes

netsh advfirewall firewall add rule name = Microsoft iSCSI Software Target Service-TCP-3260" dir=in action=allow program=""%SYtemRoot%\System32\WTStatusProxy.exe" TCP ""enable=yes
```
**Create Windows Failover Cluster**
```
Add-WindowsFeature Failover-Clustering
```
**Move Cluster Nodes**
```
Move-ClusterGroup SQLCRG SQL-D
```
**Set Primary Database to Full Recovery for Mirroring**
```sql
USE [Master];
ALTER DATABASE [DatabaseName] SET RECOVERY FULL;
```
**Alter Availability Group**
```sql
ALTER AVAILABILITY GROUP AvailabiltyGroupName MODEIFY REPLICA ON 'SQLInstanceName\\AlwaysOn'
  WITH (AVAILABILITY_MODE = SYNCHRYNOUS_COMMIT);
```
**Availability Group Manual Failover**
```sql
ALTER AVAILABILITY GROUP AvailabiltyGroupName FAILOVER;
```
**Availability Group Manual Failover Powershell**
```
Switch-SqlAvailabilityGroup - Path SQLSERVER:\SQL\SQLInstanceToFailoverToName\AlwaysOn\AvailabilityGroups\AvailabiltyGroupName
```
**Availability Group Manual Failover Forced**
```sql
ALTER AVAILABILITY GROUP AvailabiltyGroupName FORCE_FAILOVER_ALLOW_DATA_LOSS;
```
**Availability Group Manual Failover Forced Powershell**
```
Switch-SqlAvailabilityGroup - Path SQLSERVER:\SQL\SQLInstanceToFailoverToName\AlwaysOn\AvailabilityGroups\AvailabiltyGroupName - AllowDataLoss
```
**Availability Group Read Only**
```sql

```
**Create AlwaysOnEndpoint Powershell**
```
$endpoint = New-SqlHadrEndpoint AlwaysOnEndpoint -Port 7028 -Path SQLSERVER:\SQL\SQLMACHINENAME\SQLINSTANCENAME
```
**Start AlwaysOnEndpoint Powershell**
```
Set-SqlHadrEndpoint -InputObject $endpoint -State "Started"
```
**Enable AlwaysOn Powershell**
```
Enable-SqlAlwaysOn - Path SQLSERVER:\SQL\SQLMACHINENAME\SQLINSTANCENAME
```
**Create Availability Group**
```sql
CREATE AVAILABILITY GROUP AvailabiltyGroupName  
  FOR
      DATABASE DatabaseName
      REPLICA ON
      'SQLMachineName1\\Instancename' WITH
      (
          ENDPOINT_URL = 'TCP://SQLMachineName.DomainName.com:7030',
          AVAILABILITY_MODE = ASYNSCHRYNOUS_COMMIT,
          FAILOVER_MODE = MANUAL
      ),
        'SQLMachineName2\\Instancename' WITH
      (
          ENDPOINT_URL = 'TCP://SQLMachineName2.DomainName.com:7030',
          AVAILABILITY_MODE = ASYNSCHRYNOUS_COMMIT,
          FAILOVER_MODE = MANUAL
      )    
GO
```
**Availability Group Add Listener**
```sql
ALTER AVAILABILITY GROUP [AGName]
  ADD LISTENER 'ListenerName' (WITH IP (('10.0.0.222','255.0.0.0')), PORT=7028);
GO
```
**Availability Group Add Listener Powershell**
```
New-SqlAvailabillityGroupListener -Name ListenerName -StaticIP '10.0.0.222'/'255.0.0.0' -Port 7030 -Path SQLSERER:\SQL\SQLMachineName\ALWAYSON\AvailabilityGroups\AGName
```
**Add Secondary Replica**
```sql
ALTER AVAILABILITY GROUP [AGName] JOIN;
GO
```
**Add Secondary Replica Powershell**
```
Join-SqlAvailabilityGroup = Path SERVER:\SQL\SQLMachineName\AlwaysOn -Name 'AGName'
```
