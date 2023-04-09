# Query Plan

## Processing Models
A DBMS processing model defines how the system executes a query plan.

### Iterator Model (Volcano | Pipeline)
Each query plan iterator implements a Next() function.
- On each invocation the operation returns tuple or null marker if there are no more tuples.
- The operator implements a loop that calls Next() on its children to retrieve their tuples & returns them.

This is used in almost every DBMS (that were started 20years ago). Allows for tuple pipelining. Some operators must block until their children emit all their tuples. (e.g. Join, Subqueries, order by)

Cons:
Number of function calls is high
We are sending just one tuple at a time (can batch or aromatize)

### Materialized model
Each operator processes its input all at once and then emits it output all at once.
- The operator "materializes" its output in single result.
- DBMS can push down hints(eg: LIMIT) to avoid scanning too many tuples.
- Can send either a materialized row or a single column.

The output can be either whole tuples (NSM) or subset of columns (DSM).

Better of OLTP workloads because queries access a small number of tuples at a time. For OLAP there would be large intermediate results, might cause memory issues.
- Lower execution/coordination overhead.
- Fewer function calls.

### Vectorization model
Iterator model gave just 1 tuple at a time. Materialized model gave all tuples. Vectorization model gives a batch of tuples at a time.
Can speed it through the likes of SIMD instructions.

Like iterator model where each operator implements a Next() function. But instead of returning a single tuple, it returns a batch of tuples.
- The operator's internal loop processes multiple at a time
- The size of batch can vary based on hardware/query ops.

All OLAP systems use this. Greatly reduces the number of invocation per operator. Allows for operators to more easily vectorized (SIMD) instructions to process batches of tuples. Snowflake uses this.


## Plan processing direction

- Top-down
  Start with root and pull data from its children. Tuples are always passed with function calls. Used by most. (Most)

- Bottom-up
  Start with leaves and push data up to the parents. Allow for tighter control of cache/registers in pipelines.

## Access Methods
Way DBMS access the data stored in table. 3 approaches: Sequential, Index (many variants), Multi-Index.

### Sequential
For each page in table:
- retrieve it from buffer pool
- Iterate over each tuple in page and check whether to include it.
DBMS maintains an internal cursor that tracks the last page/slot.

Optimizations:
Always worst case. Can be improved by: Prefetching (Lec-6), Buffer Pool Bypass (Lec-6), Parallelization (Lec-13), Heap Clustering (Lec-8), Late Materialization (Lec-11), Data Skipping.

#### Data Skipping
Approach 1: Approximate queries (Lossy)
- Execute queries on a sampled subset of entire gtable to produce approximate results.

Approach 2: Zone Maps (Lossless) -> very common, specially in cloud systems.
- Pre-compute columnar aggregations per page that allow the DBMS to check whether queries need to access it.
- Trade-off between page size vs. filter efficacy.

### Index Scans
DBMS picks an index ahead of time to find the tuples that query needs.
Which index to use depends on: (Lec-14)
- What attributes the index contains
- What attributes the query references
- The attribute's value domains
- Predicate composition
- Whether the index has unique or non-unique keys.

### Multi Index Scan
Called Bitmap Scan on Postgres, Index-Merge in MySQL
- Compute sets of Record IDs using each matching index.
- Combine these sets based on query predicates.
- Retrieve the records and apply any remaining predicates.
Set Intersections can be done by bitmaps, hash tables or Bloom filters. Straight Forward.

## Modification Queries
Insert, Update, Upsert, Delete -> both target table and indexes.
- Constraint checks can either happen immediately inside of operator or deferred until later in txn.

Update/Delete
- Child operators pass Record IDs for target tuples.
- Must keep track of previously seen tuples

Insert
- Option #1: Materialize tuples inside of the operator.
- Option #2: Operator inserts any tuple passed in from child operators (Used)

### Halloween Problem
(Update usually deletes and reinserts in index)
Anomaly where an update operation changes the physical location of a tuple, which causes a scan operator to visit the tuple multiple times.
-> Can occur on clustered tables or index scans.
So track which tuples have been modified.

## Expression Evaluation
DBMS represents a WHERE clause as an expression tree. (which is flexible  but slow)
Evaluation using JIT compilation. (Potentially faster) (Supported by Postgres) (In advanced class)