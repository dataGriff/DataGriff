---
title: SQL 2012 Admin Exam 10 Troubleshooting SQL Server
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-19 11:27:23
---

Today I'm going to be looking at troubleshooting SQL server for the exam looking at performance monitor, profiler and data collector.

### Video Exercises

* [SQL 2012 Performance Monitor](https://www.youtube.com/watch?v=wKkQYYmC82Q&t=4s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=56)
* [SQL 2012 Profiler](https://www.youtube.com/watch?v=wKkQYYmC82Q&t=4s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=56)
* [SQL 2012 Profiler Query Trace File](https://www.youtube.com/watch?v=-Uq6S7H9vOc&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=58)
* [SQL 2012 DMVs and Activity Monitor](https://www.youtube.com/watch?v=iIFsO0vH6PM&t=3s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=59)
* [SQL 2012 Configure Master Data Warehouse](https://www.youtube.com/watch?v=GodD1evwGYQ&t=5s&list=PLA5YLvtN7pNP2t5SXqdyVfxvtla7oUwC1&index=60)


### Key Terms

* **Performance Monitor (PerfMon)** - Captures counters from hardware, operating system and applications. A counter object can be monitored using counters, where there can be multiple counters if multiple resources have the same type of counters, each one being a counter instance.
* **Data Collector Set** - Reusable sets of data collectors. These can contain performance counters, event trace data and system configuration information like registry key values.
* **Key SQL Server PerfMon Metrics** - CPU, Memory, File I/O, Locking, blocking, or deadlocking and Networking.
* **Key System PerfMon Metrics** - System processor queue length, network interface output queue length and physical disk Avg. Queue Length.
* **System Process Queue Length** - Number of system requests waiting for a processor.

* **SQL Server Profiler** - Provides a GUI for SQL Trace activity going on in SQL database engine, incurs overhead which can be significant.
* **SQL Profiler Category** - Related event classes.
* **SQL Profiler Event** - Action that occurs in SQL server defined by attributes.
* **SQL Profiler Commonly Used Event Groups** - Locks, performance, security audit, Stored Procedures and TSQL.
* **SQL Trace** - Uses system stored procedures to creates traces in SQL database engine. More lightweight than profiler.
* **Default Trace** - This runs on SQL server start and has the trace ID of 1. Captures key events and can be disabled using sp_configure. Helps DBAs with key issues.

* **Dynamic Management Objects (DMO)** - Dynamic management views (DMVs) and dynamic management functions (DMFs) to monitor SQL server.
* **Activity Monitor** - Available through SSMS and can identify what is currently happening on the server.
* **Server Process ID (SPID)** - Unique id for tasks occurring in database engine.
* **sys.dm_exec Objects** - Connections, sessions, requests and executions.
* **sys.dm_os Objects** - Operating system info.
* **sys.dm_tran Objects** - Transaction management.
* **sys.dm_io Objects** - I/O processes.
* **sys.dm_db Objects** - Database scoped information.

* **Data Collector** - One central point to collect information about SQL server. Can collect from DMVs, DMFs, or metrics of entire server.
* **Management Data Warehouse (MDW)** - Storage area for data collector.
* **MDW Roles** - mdw_admin, mdw_writer, mdw_reader.
* **Data Collector Roles** -  dc_admin, dc_operator, dc_proxy.
* **Server Activity Report** - Waits, locks, latches and memory from DMVs. Collected Every 15 minutes, kept for 14 days.
* **Disk Usage Report** -  Disk usage. Every 6 hours, kept for 2 years. Good for capacity planning.
* **Query Statistics Report** -   Expensive queries. Collected Every 15 minutes via SSIS as so big, kept for 14 days.

* **PhysicalDisk: % Disk Time** - Time disk is busy with reading and writing.
* **PhysicalDisk: Current Disk Queue Length** - Requests waiting for system access (1.5 to 2 spindles on disk).
* **PhysicalDisk: Avg. Disk Queue Length** - Average requests waiting for system access.
* **Memory: Page Faults/sec** - Make sure disk issues not caused by paging.
* **SQL Server: Buffer Manager: Page read/sec** - Isolate I/O created by SQL Server. Writing pages to disk.
* **SQL Server: Buffer Manager: Page writes/sec** -  Isolate I/O created by SQL Server. Reading pages from disk.
* **Memory: Available Bytes** - Bytes available for processes.
* **Memory: Pages/sec** -
* **Process: Working Set** - Isolate SQL memory usage. Amount of memory process uses.
* **SQL Server: Buffer Manager: Buffer Cache Hit Ratio** - Isolate SQL memory usage. Application specific. 90% or more best. Add RAM until there.
* **SQL Server: Buffer Manager: Total Pages** - Isolate SQL memory usage.
* **SQL Server: Buffer Manager: Total Server Memory (KB)** - Isolate SQL memory usage. If this consistently higher than server memory, add more RAM.
* **Processor: % Privileged Time** - CPU time on execution such as processing SQL server I/O requests. If this high may need better disk subsystem.
* **Processor: % User Time** - Time spent on user processors such as SQL server.
* **Processor: % Processor Queue Length** - Threads waiting for processor time. May need faster or more processors if this consistently high.

### Notes

* Minimise the number of counters with PerfMon.
* Run > PerfMon. To start performance monitor. Then > Add counters.
* Data collection sets can be scheduled.

* SQL profiler being deprecated, use extended events instead.
* With SQL profiler you can filter and create templates.
* You can save SQL profiler trace to a file (.trc). You can save trace quick and examine so not to take up resources with profiler.
* It is recommended that traces are saved to another server, not the one being monitored.
* Common trace events - SQL:BatchComplete, Deadlock Graph, Audit Login.
* You can import traces into a SQL table to view or view through profiler.

* Database DMOs require VIEW DATABASE STATE permissions and server DMOs require VIEW SERVER STATE permissions.
* DMOs get cleared if system is restarted.
* Activity monitor has recent expensive queries section.
* dm_exec_requests view - looks fairly analagous to activity monitor main view and contains spid.

* You can report on information collected by data collector, right click data collector, management data warehouse and reports.
* Low volume data sent straight to MDW and high volume is cached first and sent via SSIS.
* Configure central management data warehouse > Configure each server to send required data.
* Data collector under management node.
* 300MB per day in typical configuration.
* Data collector config and logs written to msdb.
* Logging can be set by sp_syscollector_update_collection_set system procedure.

* I/O - operating system coordinates SQL requests.
* If disk time counters are high consider faster disk drives, move files to another server, add disks to RAID.
* Database engine tuning advisor when reads and writes causing issues, better indexes and coverage may be needed.
* More RAM if memory issues.
* Try and optimise application before spend on new hardware.

### Scripts
**Import SQL Trace File from Profiler Output**
```sql
IF OBJECT_ID('tempdb.. ##BaseLine') IS NOT NULL  
DROP TABLE  ##BaseLine;

SELECT * INTO ##BaseLine
FROM fn_trace_gettable('C:\\temp\\70462.trc',default)

SELECT * FROM ##BaseLine
ORDER BY Duration DESC;
```
**Wait Stats DMV**
```sql
SELECT wait_type, wait_time_ms
FROM sys.dm_os_wait_stats;
```
**"Activity Monitor" DMV**
```sql
SELECT * FROM sys.dm_exec_requests;
```
**DMV Combined to show tasks waiting over certain time**
```sql
SELECT s.original_login_name, s.program_name, t.wait_type, t.wait_duration_ms
  FROM sys.dm_os_waiting_tasks t
  JOIN sys.dm_exec_sessions s
  ON t.session_id = s.session_id
    WHERE s.is_user_process = 1
    AND t.wait_Duration_ms > 1000;
```
**DMV Missing Indexes of Recent Executions**
```sql
 SELECT * FROM
 (SELECT
 user_seeks * avg_total_user_cost * (avg_user_impact * 0.01) AS index_advantage, migs.*
 FROM sys.dm_db_missing_index_group_stats migs) AS migs_adv
 JOIN sys.dm_db_missing_index_groups as mig ON
  mig.index_group_handle = migs_adv.group_handle
 JOIN sys.dm_db_missing_index_Details AS mid ON
 mig.index_handle = mid.index_handle
 ORDER BY migs_adv.index_advantage;
```
**Data Collector msdb Objects**
```sql
USE msdb;
SELECT * FROM fn_syscollector_get_execution_details();
SELECT * FROM fn_syscollector_get_execution_stats();
SELECT * FROM fn_syscollector_execution_log;
SELECT * FROM fn_syscollector_execution_log_full;
SELECT * FROM fn_syscollector_executionstats;
```
