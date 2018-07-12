SCRIBE:
  - A server for aggregating streaming log data
  - Scale a very large number of nodes, be robust to network/node failures
  - Doesn't provide transactional guarantees -> don't use it for database transactions

  - Topology: arranged in a directed graph, each server only knows the next server in the graph
  --> allows adding extra layers of fan-in, batching messages before sending, without needs to understand datacenter topology

  - Reliability:
    + middle-ground, enough to get all data almost of the time, but not enough to require heavyweight protocols and disk usage
    + spools data to disk to handle intermittent connectivity failures, but doesn't sync a log for every message -> possible for a small amount of data loss when crash or catastrophic failures
      (to spool data: to read and store it, to mediate the data between an app and something slow)
    + if scribe on a client (resender) is unable to send messages to the central scribe server -> saves them on the local disk -> sends them when the connection recovers
    + to avoid overloading the central server upon a restart, the resender waits for a random amount of time 
    + the central server has the same behavior: if the filesystem goes down -> the server writes to local disk until recovery 
    + when loss of data happen:
      ~ a client can't connect to either the local or central scribe server
      ~ if a scribe server crashes -> lose data that's in memory but not on disk
      ~ resender can't connect to any central server and its local disk fills up
      ~ timeout conditions

  - Data model:
    + a message has 2 strings: category and the actual message
    + category: description about the msg, same category -> same place
    + actual msg: data to be logged

  - Using Thrift for flexibility
