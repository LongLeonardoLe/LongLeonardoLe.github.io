# Elasticsearch
- Search engine based on Lucene (an information retrieval software library)
- Its magic is to be able to search by HTTP requests using keywork `_search` and extra flexibility and power using `query`
- Provides a distributed, multitenant-capable full-text search engine with HTTP web interface and schema-free JSON docs
	- scalable search
	- near real-time search
	- supports multitenancy
	- indices can be divided into shards, each shard can have 0/more replicas
	- each node hosts 1/more shards, acts as a coordinator to delegate ops to the correct shards
	- rebalancing and routing are done automatically
- Supports facetiing and percolating - can be useful for notifying if new docs match for registered queries
- "gateway": handles long-term persistence of the index

## Cluster
- A collection of 1/more nodes (servers)
- Holds the entire data, provides federated indexing and seach capabilities across all nodes
- Each cluster has a unique name; one node can only be part of one cluster if the node is set up to join the cluster by its name; the default name is `elasticsearch`

## Node
- A node: a single server, part of a cluster, stores data, participates in the indexing and seach capabilities
- Is identified by a name; is assigned an UUID at startup
- Can be configured to join a cluster by the cluster name, by default join the cluster named `elasticsearch`

## Index
- An index: a collection of documents that have similar characteristics
- Is identified by a name: is used to refer to the index

## Document
- Is a basic unit of information that can be indexed
- Is expressed in JSON

## Shards & Replicas

### Shard
- An index can be subdivided into multiple shards
- Can define the number of shards when creating an index
- Each shard is fully-functional and independent "index" that can be hosted on any node in a cluster
- Allows to horizontally split/scale
- Allows to distribute and parallelize operations across shards -> increase performance/throughput

### Replicas
- Make 1/more copies of index's shards, called replicas
- Provides high availability in case a shard/node fails -> a replica shard is never allocated on the same node as its origin
- Allows to scale out the search volume/throughput

## Important Configuration

### Elasticsearch Configuration
1. `path.data` and `path.logs`: where data and log go to
2. `cluster.name` and `node.name`

As mentioned above, the cluster name is used for a node to join it; for node, it's good to have a name for the sake of administration
3. Setting heap size
	- The more heap available, the more memory for caching. However, too much can result long garbage collection pauses.
	- Ensure that the physical RAM has enough for kernel system caches
	- Pay attention to the cutoff that JVM uses for compressed object pointers and the threshold for zero-based compressed object pointers
4. JVM heap dump path

Can configure a scheduled task via OS to remove heap dumps that are older than a configured age
5. GC logging
6. JVM fatal error logs

### System Configuration
1. Disable swapping

Swapping in OS can result in parts of the JVM heap or its executable pages being swapped out to disk
2. File descriptors

Elasticsearch uses a lot of file descriptors or file handlers -> data loss if run out of file descriptors. Make sure to increase if necessary.
3. Virtual memory: make sure not result in out of memory exceptions
4. Number of threads
5. DNS cache settings
