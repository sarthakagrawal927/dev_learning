# Database Storage 1

## Storage Heirarchy

Volatile -> Fast Random Access -> Byte Addressable
- CPU Registers (Advanced)
- CPU Cache (0.5ns - L1, 7ns - L2) (Advanced)
- DRAM (100ns)

Non-Volatile -> Fast Sequential Access -> Block Addressable (need 64bytes ? -> take whole 4kb page & pick out 64bytes)
- SSD (150,000ns)
- HDD (10,000,000ns)
- Network Storage

## Problem 1 - How DBMS represents the database in files on disk

The DBMS stores a database as file(s) on disk & OS does not know anything about contents of the files.

The storage manager is responsible for maintaining the database files on disk. It organizes files as a collection of pages. Each page is a fixed size block of data & has an unique identifier. Containers tuples, meta-data, indexes, log record etc.

3 different pages :
Hardware Page - lowest level, 4kb
OS Page,
Database Page (512B - 16KB)

Different DBMS manage pages in files on disk in different ways.

Heap File Organization - dbms maintains special pages that track the location of data pages in the database fiiles, also records number of free slots/page. They should be in sync.

LinkedList Structure - Head pointer points has pointer 2 lists (full page list & empty(or partial) pages)

### Page Organization

Has header:
- Page Size
- Checksum
- DBMS version
- Transaction Visibility
- Compression Info

Data: 2 ways : tuple oriented or log oriented

Tuple Oriented - each tuple is stored in a fixed size slot. Each slot has a header that contains the length of the tuple. If the tuple is deleted, the slot is marked as free. If the tuple is updated, the old tuple is marked as deleted & a new tuple is inserted in the same slot.

Each tuple is assigned a unique record identifier (Most commonly - page_id + offset/slot)
In case of delete the empty space is concatenated back based on the implementation

### Tuple Layout

Essentially a sequence of bytes. DBMS interprets the bytes into attributes & values.
Each tuple has a header:
- Visibility Info ( transactionID / concurrency control)
- Bit Map for NULL Values

Attributes are stored in order specified in schema.
Most system don't store different tuples in different pages.

Denormalized tuple data: (relation b/w tables - foreign keys n all) (not recommended)
In these cases, different tuples can be stored in same page as they are related and queries together often. But for end user they are different tables.

TODO: Understand more on why foreign keys are frowned upon by most architects.