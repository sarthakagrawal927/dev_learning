# Algorithms

## Query Plan
The operators are arranged in a tree. Data flows from the root to the leaves. The leaves are the data sources. The root is the final result.

Disk-oriented DBMS
Just like we cannot assume that a table fits entirely in memory, a disk oriented DBMS cannot assume that the query plan fits entirely in memory. We will use buffer pool to implement algorithms that need to spill the disk. Prefer algorithms that maximize the amount of sequential I/O.'

## Sorting
Relational Model is unsorted. Queries can ask to sort.
Else: Sorting is:
- Trivial to support duplicate elimination
- Bulk loading sorted tuples into B+ tree index is faster
- Aggregations (GROUP BY)

If data fits in memory, we can just use standard sorting algorithms. Else we need to use a technique that is aware of cost of reading & writing disk pages.

### Top-N Heap Sort
If query contains ORDER BY with a LIMIT, then DBMS only needs to scan data once to find top N elements. If top N elements can fit in memory, then we can use heap sort (maintain sorted PQ in memory).

### External Merge Sort
Sorts chunks of data that fit in memory and then write back the sorted chunks to a file on disk. Then merge the sorted chunks into a single sorted file.
Ran on a list of key/value pairs.

Key: the value of the attribute on which we are sorting
Value: 2 choices:
-> Early Materialization -> Entire Tuple -> Less keys in memory -> but instant access to entire tuple
-> Late Materialization -> Record ID -> more keys in memory -> but need to read tuple from disk -> Common in column store.

#### 2-Way External Merge Sort
2 -> number of runs we are going to merge into a new run for each pass. Data is broken into N pages.

Pass #0: Read all N pages into memory. Sort them. Write them back to disk. We now have N runs of size 1.
Pass #1,2,3..: Recursively merge pairs of runs into runs twice as long. Uses 3 buffer pages (2 for input, 1 for output).

Memory storage: (3 pages at max)
Page1 -> Page1_sorted
Page2 -> Page2_sorted
Page1_sorted, Page2_sorted -> Page12_merged
Page1_sorted, Page2_sorted -> Page12_2_merged

In each pass, we read and write every page in the file.
Number of passes = 1 + log2(N)
Total number of I/Os = 2N + 2N.log2(N) = 2N.(# of passes) [1read, 1write for all pages for each run]

#### Double Buffering Optimization
Prefetch the next run in the background ad store it in second buffer while the system is processing current run. Thus reducing the wait time for I/O requests at each step by continuously utilizing the disk.

General External MergeSort

Pass #0 - Use B buffer pages, produce N/B runs of size B.
Pass #1,2,3... - Merge B-1 runes (K-way merge)

Number of passes = 1 + log<sub>b-1</sub><sup>N/B</sup>

Optimizations:
Approach #1: Code Specialization
-> Instead of providing a comparison function as a pointer to sorting algorithm, create hardcoded version of sort that is specific to a key type

Approach #2: Suffix Truncation
-> First compare a binary prefix of long VARCHAR keys instead of slower string comparison. Fallback to slower version if prefixes are equal.

#### Using B+ Tree Indexes
If table that must be sorted already has B+ tree index on the sorting attribute, then we can use the index to accelerate the sort. Retrieve tuples in desired sorted order by simply traversing the leaf node pages of the tree.

#### Clustered B+ Tree Indexes
Applies sorting on the table itself by using the clustered index (just 1 per table)
Traverse to the left-most leaf page, & then retrieve tuples from all leaf pages.

#### Non-clustered B+ Tree Indexes
RecordId is stored in a sorted way, data is stored at a separate place, thus multiple non-clustered indexes can be created on the same table.
Chase each pointer to the page that contains the data. This is almost always a bad idea.

Adaptive query processing -> first start with Top-N Heap Sort, if it doesn't fit in memory, then use External Merge Sort. Hard to implement.

## Aggregations
Collapse values for a single attribute from multiple tuples into a single value. The DBMS needs a way to quickly find tuples with distinct attributes for grouping.

### Sorting Aggregation
Useful for distinct, in (multiple values), order by.

If we don't need sorting: GROUP BY, DISTINCT (remove duplicates)
Hashing is better

### Hashing Aggregation
Populate an ephemeral hash table as DBMS scans the table. For each record check whether there is already an entry in hash table. If everything is memory its easy. If not, then spill to disk.

### External Hashing Aggregate
#### Phase 1: Partition
- Divide tuples into buckets based on hash key
- Write them out to disk when they get full

Use hash function h1 to split tuples into partitions (one or more pages that contain the set of keys with the same hash value) on disk.
Partitions are spilled to disk via output buffers. Assuming we have B buffers, we use B-1 buffers for partitions and 1 buffer for input data.

Phase 2: Rehash
Build in-memory hash table for each partition & compute the aggregation

For each partition, we read it into memory and build a hash table based on second hash function. Then go through each bucket of this hash table to bring together matching tuples. [Assumes each partition fits in memory]

Choice of sorting vs hashing is subtle & depends on optimizations done in each case. Similar optimizations can be done in both cases:
- Chunk I/O into large blocks to amortize costs
- Double buffering to overlap CPU & I/O

Most databases are build from ground up assuming that the data from query won't fit in memory at once. So they will do all this stuff including buffer pool management regardless of how much memory you give it.