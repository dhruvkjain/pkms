[[Database Internals/index|Database Internals/index]]

Databases are modular systems and generally consist of:
- **a transport layer**: accepting requests, 
- **a query processor**: determines the most efficient way to run queries, 
- **an execution engine**: carrying out the operations,
-  **a storage / database engine**: storing, retrieving, and managing data in memory and on disk

Yahoo! Cloud Serving Benchmark (YCSB)

TPC-C is an online transaction processing (OLTP) benchmark

ACID properties
- Atomicity
- Consistency
- Isolation
- Durability

CAP Theorem (Any two at a time):
- Consistiency
- Availability
- Partition

# Three Common Categories

- Online Transactions Processing (OLTP)
	- row-based
	- query needs a lot of short data, mostly relational data (everything about a person)
	- Examples: MySQL, Postgres, MongoDB, SQLite, Google Spanner
- Online Analytical Processing (OLAP)
	- columnar based,
	- query need full particular columns for analytics
	- Examples: Clickhouse, DuckDB, e6data, Google BigQuery, Amazon RedShift
- Hybrid Transactions Analytical Processing (HTAP) 
	- SingleStore

Categories based on how data is stored: 
- key-value stores, 
- relational databases, 
- document-oriented stores, 
- graph databases

# Components of a DBMS
Database management systems (DBMS) use a client/server model, where database system instances (nodes) take the role of servers, and application instances take the role of clients.

![[Pasted image 20260817014856.png]]

The optimizer handles both 
- relational operations required for query resolution, usually presented as a dependency tree, and 
- optimizations, such as index ordering, cardinality estimation, and choosing access methods.

Buffer Manager -> works on a buffer pool -> holds frequently accessed pages

> Disk -> RAM(frames) -> OS virtual pages -> L1-3 caches -> processes PCB
> 
> Write Ahead Log (WAL) -> logs the changes
> query -> DBMS process -> L1-3 cache memory -> OS virtual pages -> RAM(frames) -> WAL on disk -> DBMS sends executed to user
> DBMS asynchronously -> WAL on disk -> actual tables  
> 
> WAL helps in creating snapshots / checkpoints in db to find inconsistency or tackle other problems

> Locality
> in a for loop 
> iterating variable (like int i=0) -> temporal locality (same memory referenced consecutively in a time frame)
> arr[i] -> spatial locality (as arr[i] is accessed it is highly likely that arr[i+1] will be accessed to so preload memory chunk in cache)

columnar memory -> SIMD, better compression (same data type)

Wide Column Stores -> BigTable -> data is represented as a multidimensional map, columns are grouped into column families, and inside each column family, data is stored row-wise.

DBMS's store engine represent data records in two files -> data files and index files
- data / primary files -> data records -> implemented as index-organized tables (IOT), heap-organized tables (heap files), or hash-organized tables (hashed files)
- index files -> records metadata and location

The storage structures for data records can be mutable or immutable (copy on write -> write the updated data to a new location rather than doing an in-place update)

# B-Tree Basics




