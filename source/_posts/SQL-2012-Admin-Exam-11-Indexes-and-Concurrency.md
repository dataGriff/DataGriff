---
title: SQL 2012 Admin Exam 11 Indexes and Concurrency
categories: SQL Administration
tags: ['Exam 70462', 'SQL', 'SQL Administration']
date: 2016-11-19 14:07:16
---

### Key Terms

* **B-Tree Structure** - The balanced tree structure is how SQL indexes are architected made up of a root node, intermediate nodes and leaf nodes (Data pages).
* **Heap** - Unorganised set of data with no indexes.
* **Clustered Index** - Leaf pages contain the actual data and physically ordered by the clustered index key (CIK).
* **Clustered Index Key (CIK)** - Row identifier in clustered index and should be unique, narrow and static.
* **Uniqueifier** - When clustered key has duplicate rows SQL server adds a 4-byte internal integer to these duplicates.
* **Non-Clustered Index** - No data, stores indexes columns and clustering key. Less pages to search through.
* **Covering Index** - Non-clustered index with all information required to cover a query by using included columns.
* **Filtered Index** - Where clause on the index to filter the rows so B-tree smaller. Can be used in conjunction with sparse columns.
* **Primary XML Index** - Builds a nodes table with each element in the document has a row in the nodes table.
* **Secondary XML Index** - Path, property and value index types. Must have primary first.
* **Spatial Index** - For geography and geometry data types.
* **Full-text Index** - For efficient searching of text data, one per table.
* **Columnstore Index** - Does not use B-tree, uses column-orientated storage and good for data warehouse.
* **Statistics** - Value distribution on SQL objects.
* **Cardinality** - Number of rows that exist for given value.
* **FILLFACTOR** - How much free space gets left on leaf nodes of index.
* **Fragmentation** - Logical ordering based on the key does not match the physical ordering within the data file.
* **REORGANISE (index)** - Fragmentation 10% to 30%, always online, requires 8KB space, single threaded only.
* **REBUILD (index)** - Fragmentation above 30%, long-term locks, can do with ONLINE clause, requires entire copy of index space-wise, can use parallel.

* **Transaction** - Batch of processes whereby either all or none of the processes are committed depending on success of failure of the whole transaction.  
* **ACID** - Atomic, consistent, isolation and durability. Transactions help ensure this.
* **Multi-Granular Locking** - SQL server locks the minimum resources to achieve its goal.
* **Lock Types** - Shared (S), Update (U), Exclusive (X), Intent, Schema, Bulk Update (BU), Key-Range.
* **Isolation Level Characteristics** - Dirty read, non-repeatable read, phantom read.  
* **ANSI Standard Isolation Levels** - SERIALIZABLE, REPEATABLE READ, READ COMMITTED (DEFAULT), READ UNCOMMITTED.
* **Microsoft SQL Server Isolation Level** - READ COMMITTED SNAPSHOT, which requires enabling in database, uses versioning in tempdb. This can limit reader/writer blocking.
* **Deadlock Types** - Conversion, writer-writer and reader-reader. Also can have cascading deadlocks.

### Notes
* Clustered index good for large % of rows from table, return single row based on clustered index key, or range based data.
* Non-Clustered index good for queries return few rows, can be covered by index. Covering index can be more performant than clustered index as less I/O as less pages as B-tree smaller.
* Filtered indexes good with sparse column to locate non-null rows or when queries require small subset of rows selected often.  
* PATH secondary XML index good for .exit() XQUERY.
* PROPERTY secondary XML index good for search nodes with particular values.
* VALUE secondary XML index good for search descendant nodes for a given PATH.
* AUTO_UPDATE_STATISTICS, AUTO_UPDATE_STATISTICS_ASYNC and AUTO_CREATE_STATISTICS.
* SAMPLED, LIMITED and DETAILED can be passed to sys.dm_db_index_physical_stats.
* sys.dm_db_index_physical_stats shows fragmentation also.
* Fragmentation above 30% rebuild, 10% to 30% reorganise.
* You can schedule statistics updates or do in the GUI.
* Remember DMVs only store information since last restart, for example you may want to disable indexes first rather than delete.
* In SQL 2012 only XML and spatial are excluded from online index rebuilds.

* SQL can lock   -RID, KEY, PAGE, EXTENT, HEAP or B-TREE, TABLE, FILE, APPLCIATION, METADATA, ALLOCATION UNIT, DATABASE.
* Heaps are bad for concurrency as table locks can occur.
* Lock durations change based on lock mode and isolation level.
* Can identify blocking with blocking_Session_id of sys.dm_os_waiting_tasks or sys.dm_exec_requests.
* Can use the read only version of always on replica using the ApplicationIntent=ReadOnly in the connection string.
* Deadlocks raise a 1205 error.
* Trace flag 1222 newer version of deadlock trace. Trace has been deprecated.
* Use extended events to capture deadlock information.
* Use activity monitor to see processes, waiting tasks, data file I/O and recent expensive queries.
* sys.dm_os_wait_stats DMV to determine waits.
* Remember SQL server has a load of built in reports.
* Blocked_process column of system_health extended events also shows where blocking is occuring.


### Scripts
**Create Columnstore Index**
```sql
CREATE NONCLUSTERED COLUMNSTORE INDEX IX_CS_TableName
  ON [SchemaName].[TableName]
  (Col1, Col2, Col3, Col4);
```
**Check to see if Stats are Auto Updated**
```sql
SELECT OBJECT_NAME(object_id), name, auto_created
FROM sys.Stats
WHERE auto_created = 1
```
**Create NonClustered Index**
```sql
CREATE NONCLUSTERED  INDEX IX_TableName
  ON [SchemaName].[TableName]
  (Col1, Col2, Col3, Col4);
```
**Create Clustered Index (With Compression)**
```sql
CREATE CLUSTERED  INDEX IX_PartTabCol1
  ON [SchemaName].[TableName]
  (Col1)
  WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)
, DATA_COMPRESSION = ROW ON PARTITIONS(2 TO 4));
```
**ALTER INDEX FILLFACTOR**
```sql
ALTER INDEX ALL ON [SchemaName].[TableName] REBUILD
WITH (FILLFACTOR = 80);
```
**Examples of Statistics Updates**
```sql
UPDATE STATISTICS schemaname.tablename WITH FULLSCAN;
UPDATE STATISTICS schemaname.tablename WITH FULLSCAN, INDEX;
UPDATE STATISTICS schemaname.tablename WITH FULLSCAN, COLUMNS;
UPDATE STATISTICS schemaname.tablename WITH SAMPLE 1000 ROWS;
UPDATE STATISTICS schemaname.tablename WITH 80 PERCENT;
```
**Index DMVs**
```sql
SELECT * FROM sys.dm_db_missing_index_columns;
SELECT * FROM sys.dm_db_missing_index_details;
SELECT * FROM sys.dm_db_missing_index_groups;
SELECT * FROM sys.dm_db_missing_index_group_stats;
```
**SQL Snapshot Isolation**
```sql
USE [DatabaseName]
GO
ALTER DATABASE [DatabaseName] SET ALLOW_SNAPSHOT_ISOLATION ON
GO
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;
GO
```
**Determine Transaction Isolation Level**
```sql
USE [DatabaseName]
GO
DBCC USEROPTIONS;
GO
```
**Clear Current Wait Stats**
```sql
USE [DatabaseName]
GO
DBCC SQLPERF('sys.dm_os_Wait_stats',CLEAR);
```
**Kill a process using SPID**
```sql
KILL 100;
```
**Kill a process with Time to End**
```sql
KILL 100 WITH STATUSONLY;
```
