# Storage

## Graph mapping to key value pair

1. Vertex

Key: [partId, vertexId, tagId, version] <br/>
Value: [property1, property2, ... propertyN]

2. Edge

InKey: [partId, srcId, edgeType, Ranking, dstId, version] <br/>
OutKey: [partId, dstId, edgeType, Ranking, srcId, version] <br/>
Value: [property1, property2, ... propertyN] <br/>

3. Key Space

[spaceId, partId, key]

In nebula, we can determine a key by the [spaceId, partId, key] triad.

The spaceId is used to separate the key.

The partId is used to identify the partition of raft group.

The key is used to identify the one record in the actual storage engine.

## Top

The storage architecture:

```
 (spaceId, partId, vertex/edge)    (spaceId, partId, vertex/edge)
            |                                      |
            V                                      V
      StorageServer                          StorageServer
            |                                      |
 (spaceId, partId, key, value)       (spaceId, partId, key, value)
            |                                      |
            V                                      V
         KVStore              ---consensus---   KVStore
   |        |             |                    |        |            |
RaftPart  RaftPart ...  RaftPart            RaftPart  RaftPart ... RaftPart
```

Default Implementation:

```
 (spaceId, partId, vertex/edge)    (spaceId, partId, vertex/edge)
            |                                      |
            V                                      V
      StorageServer                          StorageServer
            |                                      |
 (spaceId, partId, key, value)       (spaceId, partId, key, value)
            |                                      |
            V                                      V
         NebulaStore       ---RaftService---  NebulaStore
   |       |        |                          |      |          |
  Part    Part ... Part                      Part   Part ...    Part
   |       |        |                          |      |          |
RocksDB RocksDB ... RocksDB                RocksDB RocksDB ...  RocksDB
```

The partitions with same [spaceId, partId] combined into one raft group.
