Access Methods: 2 types of DS : Hash-tables & Trees

DS used for Internal Meta-data, Core Data Storage, Temporary DS, Table Indexes

# Hash Tables
Design Decisions: Data Organization & Concurrency Control

Implements an unordered associative array that maps keys to values. Uses hash function to compute an offset into the array for given a given key, from which the desired value can be found.

Space Efficiency: O(n) space for n key-value pairs
Operation Efficiency: - Average : O(1) for search, insert, delete
                     - Worst: O(n) for search, insert, delete

## Static Hash Table
- Fixed size
- No Collisions (Perfect hash function : key1 != key2 => hash(key1) != hash(key2))

To find an entry, mod the key by the size of the table, to find the offset in the array.

## Hash Function
How to map a large key space into a smaller domain.
Trade-off between - being fast vs collision date.

For any input key, return an integer representation of that key.
Cryptographic hash functions are not used, as they are slow & we don't care about encrypting.

Best right now : XXHash3 by Facebook.

## Hashing Scheme
Handling collisions after hashing
Trade-off between -  allocating a large hash table vs additional instructions to handle collisions.

### Static Hashing Schemes
We know the size in advance, we need more ? - we double the size (recreate)

#### 1. Linear Probe Hashing
Single giant table of slots. (Dictionary in Python)

- Resolves collisions by linearly searching for the next free slot in the table.
- Have to store keys in table, as we need to know when moving to next slot whether the key is same or not

Deleting:
- Tombstone - Mark the slot as deleted, but don't reuse it. Space is wasted, will have to clear up eventually. (Most common)
- Movement - Move the key-value pair to avoid empty spaces. As we have a circular buffer, it can be complicated to move the pairs while making sure we don't move the wrong value. For every element we plan to move we will have to check whether it was original chosen position by hash.

#### Non-Unique Keys

Separate Linked List
- Store values in separate storage area for each key.

Redundant keys (common)
- Store duplicate keys entries in the same hash table

#### 2. Robin Hood Hashing

Variant of linear probe hashing that steals slots from rich keys and give them to poor keys.
- Each key tracks the number of positions they are from their ideal position.
- On insert, a second key takes the slot of another key(first) if they first key is farther away from its optimal position than the second key.

Purpose is to minimize the distance of all keys from their optimal position.

Makes the writes more expensive as we have to move the keys around. But makes the reads faster as we have to search for fewer slots.

#### 3. Cuckoo Hashing

Use multiple hash tables (usually 2) with different hash function seeds. On insert, check every table and pick anyone with a free slot.
If all tables are full, pick an element (random) from any of the tables and rehash it to find its new location.

If multiple tables are empty: pick randomly (mostly) or write some complex logic to pick the table based on collision rate or fill percentage of hash table.

Lookups & Deletions will always be O(1) as we have to check only one location. Inserts would be expensive.

We also need to handle cycle cases, we need maintain the start & if we come back to it - resize the hash table.


### Dynamic Hash tables
Hash table grows as needed.

#### Chained Hashing
Maintain a LinkedList of buckets for each slot in hash table. Resolve collisions by placing all elements with same hash in the same bucket (pages). Can end up as a sequential scan.

#### Extendible Hashing

Chained Hashing approach where we split buckets instead of letting the linked list grow forever. Multiple slot locations can point to the same bucket.

Reshuffling bucket entries on split & increase the number of bits to examine. Data movement is localized to the bucket that is split.

Explanation (Disclaimer : (Wont understand by reading))

Completely based on bits. We have a global depth & local depth (for each bucket). Global depth is the number of bits we use to compute the hash.

When a bucket is split, we increase the global depth by 1. We also increase the local depth of the bucket that is split. We also create a new bucket with the same local depth as the old bucket. We then move half the entries from the old bucket to the new bucket.

#### Linear Hashing
Hash table maintains a pointer that tracks the next bucket to split. When any bucket overflows, split the bucket at the pointer location. Move the pointer to the next bucket. Eventually the split counter will get to the overflowed buckets. When pointer reaches the last slot deletes the first hash function & move back to the beginning. Keep creating new hash functions for each split. ??

Use multiple hashes to find right bucket.

Deleting is just reversing the process. Remove the bucket from the hash table & move all the entries to the previous bucket & delete the new hash function.

TODO: Understand Linear Hashing & Extendible Hashing better.

### Concluding
Not good for range queries. Good for equality queries.