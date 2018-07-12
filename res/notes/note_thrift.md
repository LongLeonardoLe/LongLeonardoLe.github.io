# APACHE THRIFT
  - a software framework for cross-language services development
  - data normalization: pack parameters, data into Thrift type serialization, through a messaging platform (eg, message queue), unpack the serialization
  - when generate files from thrift, you are able to serialize the args/results of the declared services and send them over to other machines


# SERVERS IN THRIFT

### TSimpleServer
  - simple singlethreaded server, blocks incoming requests until it completes the current request
  - best for testing/debugging, a dedicated machine-machine connection

### TNonblockingServer
  - operates a set of IO threads (default: 1) and one worker thread
  - doesn't block request sent to server but only capable of processing one request at a time
  - fit for small task processing, so it won't take much resource from the OS

### THsHaServer
  - default 1 thread for IO and a thread pool of worker threads
  - doesn't block client requests and capable of processing multiple operations at a time
  - good for performing multiple Thrift server operation concurrently, client-server architecture

### TThreadedServer (threads-threads)
  - create one thread for each client connected
  - release a thread when the client is disconnected
  - good for dynamically scalable server up to a concurrent connection limit

### TThreadedPoolServer
  - manages clients using a thread pool, the number of threads in the pool is limited
  - accepts all connections
  - the idea is like TThreadedServer but a thread returns to the pool instead of being released
  - consumes a good amount of resources, can be ideal for peer-to-peer architecture to distribute the need of resources among users

### TThreadedSelectorServer
  - has two thread pools for IO and worker
  - doens't create IO threads for all requests -> use less resources
  - not so good for client-server architecture


# OPTIONAL vs. REQUIRED
  - quite obvious in the name
  - one thing to pay attention is that versions can change and if missing required fields in one version, especially the new version doesn't have one of requierd fields in the old one, troubles coming
