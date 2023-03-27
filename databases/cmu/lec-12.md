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
  Start with root and pull data from its children. Tuples are always passed with function calls. Used by most.

- Bottom-up
  Start with leaves and push data up to the parents. Allow for tighter control of cache/registers in pipelines