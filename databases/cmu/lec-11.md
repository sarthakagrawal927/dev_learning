# Join Algorithms

Why do we need join?
We normalize our data to avoid data redundancy. But, we need to join data to reconstruct the original tuple without any information loss.

We will focus on performing binary joins (2 tables) using inner equijoin algorithms.
These algorithms can be tweaked to support other joins. Multi-way joins exists primarily in research literature.

In general we want smaller table to always be the left table (outer) in the query plan. The optimizer will figure this out when generating physical plan.

## Join Operators
- Decision #1: Output depends on
  - Processing model
  - Storage model
  - Data requirements in query

Early materialization: copy values for the attribute into a new output tuple (Bad for space) -> Subsequent operators in query never need to access the base tables to get more data. (Used most of the time)

Late materialization: only copy the join keys along with record IDs of the matching tuples. Ideal for column stores as DBMS doesn't copy data that is not needed for query

- Decision #2: Cost Analysis Criteria
Number of IOs to compute join. Since no writes are made, only this is the bottleneck.

### Join Vs Cross Product
Cross Join is never used as the number of tuples in the output is the product of the # of tuples in the input tables. This is not feasible for large tables.

## Nested Loops Join

M pages, m tuples per page/block, N pages in the right table
Cost: M + (m ∙ N)

```
foreach tuple r ∈ R:
  foreach tuple s ∈ S:
    emit, if r and s match
```

Stupid, simple as for every tuple, it scan the entire other table.

## Index Nested Loops Join
We can avoid sequential scan using index to find inner table matches. Assume cost of each index probe is C per tuple.
Cost: M + (m.C)

```
foreach tuple r ∈ R:
  foreach tuple s ∈ Index(ri = sj):
    emit, if r and s match
```

## Block Nested Loops Join

Assumes 3 blocks at a time in memory.
Fewer disk access
Cost: M + (M ∙ N)

```
foreach block BR ∈ R:
  foreach block BS ∈ S:
    foreach tuple r ∈ BR:
      foreach tuple s ∈ Bs:
        emit, if r and s match
```

If we have more buffers (B).
- Use B-2 buffers for scanning the outer table.
- Use one buffer for the inner table, one buffer for storing output.

```
foreach B - 2 pages pR ∈ R:
  foreach page pS ∈ S:
    foreach tuple r ∈ B - 2 pages:
      foreach tuple s ∈ ps:
        emit, if r and s match
```

Cost: M + (M/B-2) ∙ N

What if outer relation completely fits in memory (B > M+2)? Cost: M + N, which is very fast.

Key Takeaways:
→ Pick the smaller table as the outer table.
→ Buffer as much of the outer table in memory as possible.
→ Loop over the inner table (or use an index).

## Sort-Merge Join
Phase #1: Sort
→ Sort both tables on the join key(s).
→ We can use the external merge sort algorithm that we talked about last class.

Phase #2: Merge
→ Step through the two sorted tables with cursors and emit matching tuples.
→ May need to backtrack depending on the join type.

The worst case for the merging phase is when the join attribute of all tuples in both relations contains the same value.

When useful -> One or both tables are sorted on the join attribute. (e.g. sorted index)

## Hash Join
Phase #1: Build
Scan outer relation & populate hash table using hash function h1 on the join attributes.
We can use any hash table that we discussed before but in practice linear probing works the best.

Phase #2: Probe
Scan inner relation and use h1 on each tuple to jump to a location in hash table and find matching tuples.

Optimization -> Create a Bloom Filter during build phase when the key is likely to not exist in hash table. Threads check the filter before probing the hash table. This will be faster since filter will fit in CPU Caches.

### Bloom Filter
Probabilistic data structure that tells us if an element is in a set or not. False positives are possible but false negatives are not.

What is we don't have enough memory for entire hash table? We don't want to let the buffer pool manager swap out the hash table pages at random.

### Partitioned Hash Join
Hash both tables on the join attribute into partitions. Compare tuples in corresponding partitions for each table. Also called GRACE Hash Join.

Perform regular hash join on each pair of matching buckets in the same level between R and S.

If the buckets do not fit in memory, then use recursive partitioning to split tables into chunks that will fit.
- Build another hash table for bucketR,i using hash function h2 (with h2≠h1).
- Then probe it for each tuple of the other table's bucket at that level.

Optimization: If the keys are skewed, then the DBMS keeps the hot partition in-memory and immediately perform the comparison instead of spilling it to disk.
→ Difficult to get to work correctly. Rarely done in practice

| Algorithm               | IO                  | Cost Example |
| ----------------------- | ------------------- | ------------ |
| Simple Nested Loop Join | M + (m ∙ N)         | 1.3 hours    |
| Block Nested Loop Join  | M + (M ∙ N)         | 50 seconds   |
| Index Nested Loop Join  | M + (m ∙ C)         | Variable     |
| Sort-Merge Join         | M + N + (sort cost) | 0.75 seconds |
| Hash Join               | 3 ∙ (M + N)         | 0.45 seconds |

Hashing is almost always better than sorting for operator execution.
Caveats:
→ Sorting is better on non-uniform data.
→ Sorting is better when result needs to be sorted.
Good DBMSs use either (or both).