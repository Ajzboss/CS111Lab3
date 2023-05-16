# Hash Hash Hash
Aryan Janolkar, UID:805820938

This lab xxx

## Building
```shell
make clean & make
```

## Running
```shell
./hash-table-tester -t 8 -s 50000
```
-t: # of threads given to the command. 
-s: # of entries for  hash table
## First Implementation
In the `hash_table_v1_add_entry` function, I added a single static mutex lock call at the beginning of the hash_table_v1_add_entry function, and an unlock at all possible ends of the function. Becuase hash_table_v1_add_entry is the only method on the hash table that writes into shared memory, it is the only part of the program that can cause race conditions.Setting locks around it turned the entire method into a critical section, ensuring all shared writes are protected. 

### Performance
```shell
Generation: 105,579 usec
Hash table base: 1,557,926 usec
  - 0 missing
Hash table v1: 2,432,653 usec
  - 0 missing
```
This time version 1 is slower, becuase every entry addition is done sequentially instead of concurrently, which means processor utilization is greatly reduced and less work is accomplished in the same time. 

## Second Implementation
In the `hash_table_v2_add_entry` function, I used n mutexes, where n is the number of entries handled by the hash table. The  n mutexes are the 'create' mutexes for each hash table entry, which lock the ability to create a new entry or update an entry(the lock happens before the update clause) if two threads are attempting to add/update the same entry. However, if the entries are different, threads are free to update concurrently. 

### Performance
```shell
Generation: 105,579 usec
Hash table base: 1,557,926 usec
  - 0 missing
Hash table v1: 2,432,653 usec
  - 0 missing
Hash table v2: 400,445 usec
  - 0 missing
```

This time the speed up is apx 3x.

## Cleaning up
```shell
make clean
```
