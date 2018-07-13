# ELASTICSEARCH
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

### Cluster
- A collection of 1/more nodes (servers)
- Holds the entire data, provides federated indexing and seach capabilities across all nodes
- Each cluster has a unique name; one node can only be part of one cluster if the node is set up to join the cluster by its name; the default name is `elasticsearch`

### Node
- A node: a single server, part of a cluster, stores data, participates in the indexing and seach capabilities
- Is identified by a name; is assigned an UUID at startup
- Can be configured to join a cluster by the cluster name, by default join the cluster named `elasticsearch`

### Index
- An index: a collection of documents that have similar characteristics
- Is identified by a name: is used to refer to the index

### Document
- Is a basic unit of information that can be indexed
- Is expressed in JSON

### Shards & Replicas
#### Shard
- An index can be subdivided into multiple shards
- Can define the number of shards when creating an index
- Each shard is fully-functional and independent "index" that can be hosted on any node in a cluster
- Allows to horizontally split/scale
- Allows to distribute and parallelize operations across shards -> increase performance/throughput
#### Replicas
- Make 1/more copies of index's shards, called replicas
- Provides high availability in case a shard/node fails -> a replica shard is never allocated on the same node as its origin
- Allows to scale out the search volume/throughput

