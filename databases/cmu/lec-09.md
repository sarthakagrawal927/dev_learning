# Concurrency Control
Making thread safe. Till now we have assumed the databases are single-threaded.

Concurrency control protocol is the method that DBMS use to ensure correct result of concurrent operations on a shared object (Traffic rules for navigating around the DB)

### Locks Vs Threads

| Locks (Lecture 15)                   | Latches (Our Focus) |
| ------------------------------------ | ------------------- |
| Uses transactions                    | Threads             |
| Protects Database contents           | inMemory DS         |
| During entire txns                   | Critical sections   |
| Shared, Exclusive, Update, Intention | Read, Write         |
| Deadlocks detected & resolved        | Avoided             |
| by waits, timeouts, aborts           | coding              |
| kept in lock manager                 | in protected DS     |

## Latch Modes

ReadMode :
- Multiple threads can read same object at same time
- A thread can acquire the read latch if another thread as it in read mode.

WriteMode:
- Only one thread can access the object
- A thread can't acquire write latch if another thread has it in any mode

## Latch Implementations

Blocking OS Mutex:
- Simple to use, non-scalable (about 25ns per lock/unlock invocation)
- Only 1 latch at a time
- Giving control to OS, not good
- std::mutex

Read-Writer Latches
- Allows for concurrent readers. Must manage read/write queues to avoid starvation
- Can be implemented on top of spin-locks.
- std::shared_mutex -> pthread_rwlock
- Increased metadata storage

## HashTable Latching
Linear Probe HashTable: easy to support concurrent access due to the limited ways thread access the DS -> all threads move in same direction & only access a single page/slot at a time.
Deadlocks aren't possible. To resize the table take a global write latch on entire table

### Page Latches
- Each page has its own reader-writer latch that protects its entire contents.
- threads acquire either a read or write latch before they access a page.
- Less metadata, less parallelism

### Slot Latches
- Each slot has its own reader-writer latch that protects its entire contents
- Can use single mode latch to reduce metadata & computational overhead
- more metadata, more parallelism

## B+ Tree latching
We want to allow multiple threads to read and write a B+ tree at the same time. We need to protect against 2 types of problems:
- Threads trying to modify the contents of a node at the same time
- Ont thread traversing the tree while other threads are modifying the tree (split/merge nodes)

### Latch Crabbing/Coupling
Protocol to allow multiple threads to access/modify B+ tree at the same time. Crabbing is the process of acquiring latches in a specific order to avoid deadlocks.
-> Get latch for parent
-> Get latch for child
-> Release latch for parent (if safe)

A safe node is one that will not split/merge when updated (not full (not full), more than half full (on deletion))

Observation: we are taking a write latch on root every time we insert a new key. This is a problem because we are serializing all inserts. We can avoid this by assuming most modifications will not require the node to split or merge. Instead of assuming that there will be a split/merge, optimistically traverse the tree using only read latches. If we guess wrong, repeat traversal with the pessimistic algorithm.

Threads so far can acquire latches in a top-down manner. A thread can only acquire a latch from a node that is below its current node. If unavailable the thread must wait until the latch is released.

But we have sequential pointers in child nodes. So thread can move across the child nodes during sequential scan. In case a node is held by write lock & another thread is trying to acquire a read lock on the same node, the read thread has no idea about the write thread. So it can't wait for the write thread to release the lock and kills itself to retry.

Latches do not support deadlock detection/avoidance. The only we can deal with this problem is with coding discipline. The lead node sibling latch acquisition protocol must support a 'no-wait' mode. The DBMS's DS must cope with failed latch acquisitions.