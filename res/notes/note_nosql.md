# NOSQL
  - schema-less, allows grander flexibility
  - good use for big data, real-time web application
  - advantage:
    + simplicity for design
    + simpler scaling horizontally (expanding)
    + better control over availability
    + somehow faster than default SQL
  - disadvantage:
    + lack of standardized interfaces
    + the SQL has a huge established community and investment

## KEY-VALUE STORE
  - imagine dictionary, hash table
  - one (unique) key <-> one (many) objects/values


## REDIS
  - in-memory key-value db
  - atomic exectuion of command blocks and scripts
  - several supported data types: string, hash, set, sorted set, list, bit array
  - notable features:
    + real-time analysis
    + queue: has blocking queue
    + publish/subscribe
    + leaderboards thanks to sorted set
    + caching (in the same manner as memcache)
  - data type:
    + keys: binary safe, can use any sequence as a key, better to hash the key
    + strings: just string, useful for caching HTML fragments of pages support the INCR, INCRBY (increment by), DECR, DECRBY. Those are atomic which means mutilple clients issuing INCR against the same key will never enter into a race condition.
    + lists: collections of strings, to the order of insertion (linked lists) blocking ops: if the list is empty, only return to the caller when the new element is added, or timeout is reached. Good use cases:
      * remember the lastest updates of something
      * communication between processes in producer-consumer pattern
    + sets: collections of unique, unsorted elements - no 2 elements share the same content
    + sorted sets: every element is associated to a float value (score), add an element O(log(N)), use cases: leaderboard, when an element is updated/added, the set happens to be sorted at the time the modification/insertion is done
    + hashes: maps composed of fields associated to keys, both of them are strings, handy to represent objects which usually contains multiple fields
    + bit arrays (bitmaps): store bits (duh?!), has operations of groups of bits. Use cases:
        * extremely useful in storing space efficient
        * count the active users
    + hyperloglogs: probabilistic data structure to count unique things, use case: count how many unique viewers of a video, unique visitors of a website


## MEMCACHED
  - in-memory key-value store for small chunk of arbitrary data
  - distributed caching system, short-term memory
  - allow to make better use of memory, all of the servers look into the same virtual pool of memory
  - MySQL directly supports the Memcached API 


## KYOTOCABINET
  - successor of TokyoCabinet
  - on-disk B+ trees and hash tables for key-value -> better robustness
  - fast: store 1 mil records in 0.9 s (hash table) and 1.1 s (B+ tree)
  - thread safe: doesn't allow multiple processes access one DB file at the same time
