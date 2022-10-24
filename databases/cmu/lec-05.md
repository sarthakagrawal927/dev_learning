# Problem 2 - How DBMS manages its memory & move data back-and-forth from disk.

By Von Neumen architecture, we can NOT directly access data from disk, we have to first add it into memory.

NOTE: All intermediate data from queries are also stored in buffer pools (where? - depends on implementation)

Spatial Control
- Where to write pages on disk
- Goal is to keep pages that are used together often as physically close as possible on disk.

Temporal Control
- When to read pages into memory & when to write into disk
- Goal is to minimize the number of stalls caused by disk I/O.

## Buffer Pool Manager (Buffer Cache)
Memory managed by database.
Buffer pool organization - memory region organized as an array of fixed size pages. An array entry is called a frame. When the DBMS requests a page, an exact copy is placed into one of these frames.

Page table - maps page ID to frame ID. Page table is stored in memory. Also maintains additional meta-data per page: dirty bit, pin/reference counter.

### Locks vs Latches

Locks:
- Protects database's logical contents from other transactions
- Help for transaction duration. To be able to rollback.

Latches: (Mutex/Semaphores of OS)
- Protects critical sections of DBMS's internal data structure from other threads.
- Help for critical operation duration. Do not need to be able to rollback changes.
- Unlike locks, they hold only on the operation on the page.
- Used to guarantee physical consistency of data.

### Page directory vs Page Table
Page directory: Mapping from pageIDs to pageLocations in database files. All changes must be recorded to allow DBMS to find on restart.
Page Table: Mapping from pageIDs to frameIDs (copy of page in buffer pool frames). In memory DS, that does not need to be stored on disk.

### Allocation Policies
Global : Make decisions for all active txns, (eg: LRU)
Local : Allocate frames to a specific txn without considering the behavior of concurrent txns.

## Buffer Pool Optimizations

### Multiple Buffer Pools
DBMS can have:
- Multiple Buffer pool instances
- Per database buffer pool
- per page-type buffer pool

Helps reduce latch contention & improve locality. Can have different allocation policies for different buffer pools.

Approach 1: ObjectId
Embedded object identifier in record Ids and then maintain a mapping from objects to specific buffer pools.
To get data : we will then need ObjectId to get buffer pool, pageId to get page & then slotNum to get record.

Approach 2: Hashing
Hash the pageId to select which buffer pool to access.

### Prefetching
DBMS can also prefetch pages based on query plan. Eg: If database knows that we will access entire table & knows multiple sequential/indexed pages will be needed, it can prefetch them. OS can also do this, but DBMS does it better because it understands the query plan & what it will need.

### Scan Sharing
Queries can reuse data retrieved from storage or operator commutations. NOT like result caching.
Allow multiple queries to attach to a single cursor that scans a table. Queries can share intermediate data.

If query starts a scan & if there is one already doing this, then the DBMS will attach to the second query's cursor. The DBMS keeps track of where the second query joined with the first so that it can finish the scan when it reaches the end of the DS. Hard to implement.

The smartest systems can even share the tuples in memory if the queries need same tuples.

-- More on this in advance class.

can even share scans across transactions. Eg: If a transaction is doing a scan & another transaction needs to do a scan on the same table, then the DBMS can attach the second transaction to the first transaction's scan.

### Buffer Pool Bypass

The sequential scan operator will not store fetched pages in the buffer pool to avoid overhead.
Used when I that I won't be needing the data in near future - no need to pollute the cache, nor check the page table or buffer pool. Just directly from disk.

Small amount of memory is allocated to the thread running the query. This memory is used to store the pages that are fetched from disk. This memory is called the bypass buffer. Once its done everything is dropped.

Works well if operator needs to read a large sequence if pages that can be contiguous on disk.
Can also be used for temporary tables.

### OS Page Cache
Most disk operations go through OS API. OS can also cache pages in memory. Then in buffer pool from OS cache. Most DBMS wants to avoid that to use direct IO bypassing the OS's cache. Unless you tell it not to, OS maintains its own filesystem cache.

Postgres uses OS Cache, they still has buffer pool but very less.

```sql
EXPLAIN (ANALYZE, BUFFERS) SELECT SUM(A+B) FROM testReals.
--- This will show how many pages were read from disk & how many were read from buffer pool.

SHOW shared_buffers -- shows buffer sizes (configurable)

SELECT pg_prewarm('testReals'); -- prewarm the buffer pool with data from table. Will only load what it can fit in buffer pool.
```


### Buffer replacement Policies
When DBMS needs to free up a frame to make room for a new page, it must decide which page to evict.

Most BASIC: LRU (Least Recently Used) - Maintain timestamp of when each page was last used. When DBMS want to evict, choose the page that was least recently used. Keep pages in sorted order (priorityQ)

Enterprise & expensive systems use much more advanced policies.

#### Clock Algorithm
Approximation of LRU without needing a separate timestamp per page. Each page has a reference bit. When a page is accessed set to 1.

When DBMS wants to evict, start from the head of the list & evict the first page with reference bit 0 (ie not been accessed since last check). If it was 1, then set it to 0 & move to next page. Remember the marker position of evicted. Need to check the pinned status of the page.

If you reach the page you started at, then evict the first page you find.

Problems : Sequential Flooding
- A query performs a sequential scan that reads every page. This pollutes the buffer pool with pages that are read once & then never again. The most recently used page is actually the most unneeded page. Buffer Pool Bypass or different Buffer Pools can help with this.

#### Better Policies (LRU-K)

Track history of last K references to each page as timestamps & compute the interval between subsequent accesses. The DBMS then uses this history to estimate the next time that page is going to be used. ML can also be heavily used in predicting which page will be used next.

#### Localization

Basically use of Buffer Bypass Buffer Pool
The DBMS chooses which pages to evict based on a per txn/query basis. This minimizes the pollution of the buffer pool from which query.

Ex: Postgres maintains a small ring buffer that is private to the query.

#### Priority Hints
The DBMS what the context of each page during execution. Based on query, it provides hints to buffer pool whether the page is important or not.


### Dirty Pages
Fast: If a page is NOT dirty, then just DROP it.
Slow: If a page is dirty, then write it to disk & then drop it.

There are tradeoffs between fast evictions vs dirty writing pages that will be not read again in the future.

Background Writing: DBMS can periodically walk through the page table and write dirty pages to disk. Then it can evict the page or unset the flag. Logs should also be maintained for disk writes.

Github Copilot's recommendation: Adaptive Replacement Cache
ARC improves on LRU by keeping track of both the most recently used pages & the most frequently used pages. It has 2 ghosts lists (B1 & B2) keeping track (just keys) of evicted pages from LRU & LFU buffers. It adjusts the size of the LRU & LFU buffers based on the hit rate of the pages in the ghosts lists. Hits in B1 will result in eviction of last element of B2. Actual implementation is more complex.