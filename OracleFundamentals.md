## What is Oracle Database
Oracle database is an RBDMS, which creates relations between table with primary and foreign key areas. I searched for three main structures of Oracle Database; Memory, Process and Storage structures.

## Why is Oracle Database
There so many reasons to use Oracle Database, like, **Scalability, Relianility&Security, Comprehensive Support, Compatibility&Integration** and so on. But main reason of using Oracle Database over other databases is **High Performance**.
- **Query Optimization:** Oracle's query optimization mechanisms provides efficient process for complex SQL queries. That helps developers to identify and resolve performance issues.
- **Indexing and Data Management:** Oracle employs indexing techniques, data partitioning, and efficient data loading mechanisms to manage large volumes of data effectively.
- **Concurrent Operations and High Availability:** Oracle can manage concurrent user operations effectively through parallel processing capabilities.
- **Data Storage Management:** Oracle provides various storage options to meet performance requirements, including strategies for hot and cold data storage to optimize data access times.
- **Technological Innovation and Developement:** Oracle continually innovates and invests in research and development to improve database management systems.

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
Oracle Database has three types of processes. They are Client Process, Server Process and Background Process. We can group them by User Process and Oracle Database Processes. Oracle Databases Processes are Server Process and Background Process.

### **Client Process**
When a user runs an application such as a Pro*C program or SQL*Plus, the operating system creates a client process (sometimes called a user process) to run the user application. The client application has Oracle Database libraries linked into it that provide the APIs required to communicate with the database. Client processes differ in important ways from the Oracle processes interacting directly with the instance. The Oracle processes servicing the client process can read from and write to the SGA, whereas the client process cannot. A client process can run on a host other than the database host, whereas Oracle processes cannot.

### **Server Process**
Oracle Database creates server processes to handle the requests of client processes connected to the instance. A client process always communicates with a database through a separate server process.

1. **Dedicated Server Processes**
In dedicated server connections, the client connection is associated with one and only one server process. On Linux, 20 client processes connected to a database instance are serviced by 20 server processes. Each client process communicates directly with its server process. This server process is dedicated to its client process for the duration of the session. The server process stores process-specific information and the UGA in its PGA.

2. **Shared Server Processes**
In shared server connections, client applications connect over a network to a dispatcher process, not a server process. For example, 20 client processes can connect to a single dispatcher process. The dispatcher process receives requests from connected clients and puts them into a request queue in the large pool. The first available shared server process takes the request from the queue and processes it. Afterward, the shared server places the result into the dispatcher response queue. The dispatcher process monitors this queue and transmits the result to the client. Like a dedicated server process, a shared server process has its own PGA. However, the UGA for a session is in the SGA so that any shared server can access session data.

3. **Background Processes**
Background processes are additional processes used by a multiprocess Oracle database. The background processes perform maintenance tasks required to operate the database and to maximize performance for multiple users. Each background process has a separate task, but works with the other processes. For example, the LGWR process writes data from the redo log buffer to the online redo log. When a filled redo log file is ready to be archived, LGWR signals another process to archive the redo log file. Oracle Database creates background processes automatically when a database instance starts. An instance can have many background processes, not all of which always exist in every database configuration. There are three types of background processes. They are Mandatory, Optional and Slave Background Processes.

***Mandatory Background Processes***

- **Process Monitor Process (PMON) Group:** The PMON group includes PMON, Cleanup Main Process (CLMN), and Cleanup Helper Processes (CLnn). These processes are responsible for the monitoring and cleanup of other processes. The process monitor (PMON) detects the termination of other background processes. If a server or dispatcher process terminates abnormally, then the PMON group is responsible for performing process recovery. CLMN periodically performs cleanup of terminated processes, terminated sessions, transactions, network connections, idle sessions, detached transactions, and detached network connections that have exceeded their idle timeout. The CLnn processes assist in the cleanup of terminated processes and sessions. The number of helper processes is proportional to the amount of cleanup work to be done and the current efficiency of cleanup. 

- **Process Manager (PMAN):** Process Manager (PMAN) oversees several background processes including shared servers, pooled servers, and job queue processes. PMAN monitors, spawns, and stops the following types of processes: Dispatcher and shared server processes, connection broker and pooled server processes for database resident connection pools, job queue processes and restartable background processes.

- **Listener Registration Process (LREG):** The listener registration process (LREG) registers information about the database instance and dispatcher processes with the Oracle Net Listener. When an instance starts, LREG polls the listener to determine whether it is running. If the listener is running, then LREG passes it relevant parameters. If it is not running, then LREG periodically attempts to contact it.

- **System Monitor Process (SMON):** The system monitor process (SMON) is in charge of a variety of system-level cleanup duties. Duties assigned to SMON include: performing instance recovery, if necessary, at instance startup. In an Oracle RAC database, the SMON process of one database instance can perform instance recovery for a failed instance. Recovering terminated transactions that were skipped during instance recovery because of file-read or tablespace offline errors. SMON recovers the transactions when the tablespace or file is brought back online. Cleaning up unused temporary segments. For example, Oracle Database allocates extents when creating an index. If the operation fails, then SMON cleans up the temporary space. Coalescing contiguous free extents within dictionary-managed tablespaces.

- **Database Writer Process (DBW):** The database writer process (DBW) writes the contents of database buffers to data files. DBW processes write modified buffers in the database buffer cache to disk. 

- **Log Writer Process (LGWR):** The log writer process (LGWR) manages the online redo log buffer. LGWR writes one portion of the buffer to the online redo log. By separating the tasks of modifying database buffers, performing scattered writes of dirty buffers to disk, and performing fast sequential writes of redo to disk, the database improves performance.

- **Checkpoint Process (CKPT):** The checkpoint process (CKPT) updates the control file and data file headers with checkpoint information and signals DBW to write blocks to disk. Checkpoint information includes the checkpoint position, SCN, and location in online redo log to begin recovery. 

- **Manageability Monitor Processes (MMON and MMNL):** The manageability monitor process (MMON) performs many tasks related to the Automatic Workload Repository (AWR). For example, MMON writes when a metric violates its threshold value, taking snapshots, and capturing statistics value for recently modified SQL objects. The manageability monitor lite process (MMNL) writes statistics from the Active Session History (ASH) buffer in the SGA to disk. MMNL writes to disk when the ASH buffer is full.

- **Recoverer Process (RECO):** In a distributed database, the recoverer process (RECO) automatically resolves failures in distributed transactions. The RECO process of a node automatically connects to other databases involved in an in-doubt distributed transaction. When RECO reestablishes a connection between the databases, it automatically resolves all in-doubt transactions, removing from each database's pending transaction table any rows that correspond to the resolved transactions.

***Optional Background Processes***

- **Archiver Processes (ARCn):** An archiver process (ARCn) copies online redo log files to offline storage after a redo log switch occurs. These processes can also collect transaction redo data and transmit it to standby database destinations. ARCn processes exist only when the database is in ARCHIVELOG mode and automatic archiving is enabled. 

- **Job Queue Processes (CJQ0 and Jnnn):** A queue process runs user jobs, often in batch mode. A job is a user-defined task scheduled to run one or more times.

- **Flashback Data Archive Process (FBDA):** The flashback data archive process (FBDA) archives historical rows of tracked tables into Flashback Data Archives. When a transaction containing DML on a tracked table commits, this process stores the pre-image of the changed rows into the Flashback Data Archive. It also keeps metadata on the current rows. FBDA automatically manages the Flashback Data Archive for space, organization, and retention. Additionally, the process keeps track of how long the archiving of tracked transactions has occurred.

- **Space Management Coordinator Process (SMCO):** The SMCO process coordinates the execution of various space management related tasks. Typical tasks include proactive space allocation and space reclamation. SMCO dynamically spawns slave processes (Wnnn) to implement the task. 

***Slave Processes***

- **I/O Slave Processes:** I/O slave processes (Innn) simulate asynchronous I/O for systems and devices that do not support it.

- **Parallel Execution (PX) Server Processes:** In parallel execution, multiple processes work together simultaneously to run a single SQL statement. By dividing the work among multiple processes, Oracle Database can run the statement more quickly. For example, four processes handle four different quarters in a year instead of one process handling all four quarters by itself.

## 3. Storage Structures

1. **Physical Storage Structures**
One characteristic of an RDBMS is the independence of logical data structures such as tables, views, and indexes from physical storage structures. Because physical and logical structures are separate, you can manage physical storage of data without affecting access to logical structures. For example, renaming a database file does not rename the tables stored in it.

- **Data Files:** At the operating system level, Oracle Database stores database data in structures called data files. Every Oracle database must have at least one data file. Each nonpartitioned schema object and each partition of an object is stored in its own segment, which belongs to only one tablespace. For example, the data for a nonpartitioned table is stored in a single segment, which in turn is stored in one tablespace. 

- **Control Files:** The database control file is a small binary file associated with only one database. Each database has one unique control file, although multiple identical copies are permitted. Oracle Database uses the control file to locate database files and to manage the state of the database generally. A control file contains information like the database name and database unique identifier, the time stamp of database creation, tablespace information and RMAN backups.

- **Online Redo Log:** The most crucial structure for recovery is the online redo log, which consists of two or more preallocated files that store changes to the database as they occur. The online redo log records changes to the data files. The database maintains online redo log files to protect against data loss. Specifically, after an instance failure, the online redo log files enable Oracle Database to recover committed data that it has not yet written to the data files. Server processes write every transaction synchronously to the redo log buffer, which the LGWR process then writes to the online redo log. Contents of the online redo log include uncommitted transactions, and schema and object management statements. 

2. **Logical Storage Structures**
Oracle Database allocates logical space for all data in the database. The logical units of database space allocation are data blocks, extents, segments, and tablespaces. At a physical level, the data is stored in data files on disk. The data in the data files is stored in operating system blocks.

- **Data Blocks:** Oracle Database manages the logical storage space in the data files of a database in a unit called a data block, also called an Oracle block or page. A data block is the minimum unit of database I/O. At the physical level, database data is stored in disk files made up of operating system blocks. An operating system block is the minimum unit of data that the operating system can read or write. In contrast, an Oracle block is a logical storage structure whose size and structure are not known to the operating system.

- **Extents:** An extent is a unit of database storage made up of logically contiguous data blocks. Data blocks can be physically spread out on disk because of RAID striping and file system implementations. By default, the database allocates an initial extent for a data segment when the segment is created. An extent is always contained in one data file. Although no data has been added to the segment, data blocks in the initial extent are reserved for this segment exclusively. The first data block of every segment contains a directory of the extents in the segment.

- **Segments:** A segment is a set of extents that contains all the data for a logical storage structure within a tablespace. For example, Oracle Database allocates one or more extents to form the data segment for a table. The database also allocates one or more extents to form the index segment for an index on a table. Oracle Database manages segment space automatically or manually. This section assumes the use of ASSM.

- **Tablespaces:** A tablespace is a logical storage container for segments. Segments are database objects, such as tables and indexes, that consume storage space. At the physical level, a tablespace stores data in one or more data files or temp files. A database must have the SYSTEM and SYSAUX tablespaces. There are plenty of tablespaces. I'll look into them separately.
