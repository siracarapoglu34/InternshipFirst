## What is Oracle Database
Oracle database is an RBDMS, which creates relations between table with primary and foreign key areas. I searched for three main structures of Oracle Database; Memory, Process and Storage structures.

## Why is Oracle Database
There so many reasons to use Oracle Database, like, **Scalability, Relianility&Security, Comprehensive Support, Compatibility&Integration** and so on. But main reason of using Oracle Database over other databases is **High Performance**.
- **Query Optimization:** Oracle's query optimization mechanisms provides efficient process for complex SQL queries. That helps developers to identify and resolve performance issues.
- **Indexing and Data Management:** Oracle employs indexing techniques, data partitioning, and efficient data loading mechanisms to manage large volumes of data effectively.
- **Concurrent Operations and High Availability:** Oracle can manage concurrent user operations effectively through parallel processing capabilities.
- **Data Storage Management:** Oracle provides various storage options to meet performance requirements, including strategies for hot and cold data storage to optimize data access times.
- **Technological Innovation and Developement:** Oracle continually innovates and invests in research and development to improve database management systems.

### Important Terms
- **RDBMS (Relational Database Management System):** 
- **OLAP (Online Analytical Processing):** Is optimized for conducting complex data analysis and designed for use by data scientists, business analysts and knowledge workers.
- **OLTP (Online Transaction Processing):** Is optimized for processing a massive number of transactions and designed for fast processing of large numbers of transactions per second.
- **DWH (Data Warehouse):** Is designed for easy access to massive amount of data.
- **SGA (System Global Area):** Provides a memory area to improve general database performance. It's shared and also easily accessible for all users.
- **PGA (Program Global Area):** Provides a memory area for each individual user. It manages temporary data and transactions.

## 1. Memory Structures
When an instance is started, Oracle Database allocates a memory area and starts **background process**. This area stores information such as program code, info about each connected session etc. Oracle Database has several memory areas. The basic structures are System Global Area(SGA), PRogram Global Area(PGA), User Global Area(UGA) and Software code areas.

### UGA (User Global Area)
The UGA is session memory, which is memory allocated for session variables, such as logon information and other information required by a database session. Essentially, the UGA stores the session state.

### SGA (System Global Area)
It's a group of shared memory structures, known as SGA components, that contain data and control information for one Oracle Database instance. All server and background processes share the SGA.

1. **Shared Pool:** The shared pool caches various types of program data. For example, the shared pool stores parsed SQL, PL/SQL code, system parameters, and data dictionary information. The shared pool is divided into several sub-components, such as;
- **Library Cache:** The library cache is a shared pool memory structure that stores executable SQL and PL/SQL code.
- **Shared SQL Areas:** The database represents each SQL statement that it runs in the shared SQL area and private SQL area.
- **Data Dictionary Cache:** The data dictionary is a collection of database tables and views containing reference information about the database, its structures, and its users. This cache holds information about database objects. The cache is also known as the row cache because it holds data as rows instead of buffers. 
- **Server Result Cache:** The server result cache is a memory pool within the shared pool. Unlike the buffer pools, the server result cache holds result sets and not data blocks. The server result cache contains the SQL query result cache and PL/SQL function result cache, which share the same infrastructure. The SQL query result cache is a subset of the server result cache that stores the results of queries and query fragments. The PL/SQL function result cache is a subset of the server result cache that stores function result sets. 
- **Reserved Pool:** The reserved pool is a memory area in the shared pool that Oracle Database can use to allocate large contiguous chunks of memory. 

2. **Buffer Cache:** The database buffer cache, also called the buffer cache, is the memory area that stores copies of data blocks read from data files. A buffer is a main memory address in which the buffer manager temporarily caches a currently or recently used data block. All users concurrently connected to a database instance share access to the buffer cache. There are three buffer states;
- **Unused:** The buffer is available for use because it has never been used or is currently unused. This type of buffer is the easiest for the database to use.
- **Clean:** This buffer was used earlier and now contains a read-consistent version of a block as of a point in time. The block contains data but is "clean" so it does not need to be checkpointed. The database can pin the block and reuse it.
- **Dirty:** The buffer contain modified data that has not yet been written to disk. The database must checkpoint the block before reusing it.

3. **Redo Log Buffer:** The redo log buffer is a circular buffer in the SGA that stores redo entries describing changes made to the database. A redo record is a data structure that contains the information necessary to reconstruct, or redo, changes made to the database by DML or DDL operations. Database recovery applies redo entries to data files to reconstruct lost changes. 

4. **Large Pool:** Optional memory area. Reduces load on shared pool. Provides area to store large size memory structures. Used by RMAN I/O Slaves(?).

5. **Java Pool:** Stores Java code and related data for sessions.

### PGA (Program Global Area)
A PGA is a non-shared memory region that contains data and control information exclusively for use by an Oracle process. Oracle Database creates the PGA when an Oracle process starts. One PGA exists for each server process and background process.

### Software Code Areas
A software code area is a portion of memory that stores code that is being run or can be run. Oracle Database code is stored in a software area that is typically more exclusive and protected than the location of user programs. They are usually static in size.

## 2. Process Structures
Oracle Database has three types of process structure. They are Client Process, Server Process and Background Process. We can group them by User Process and Oracle Database Processes. Oracle Databases Processes are Server Process and Background Process.

1. **Client Process**
When a user connects to the Oracle Database through an application or a tool, the Client Process is started for this user. Client Process cannot read and write to the SGA area. Client Process, unlike Oracle Database Processes, can run on a server other than the server where the database is running.

2. **Server Process**

