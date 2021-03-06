# Storage

## Graph mapping to key value pair

1. Vertex

Key: [type(1), partId(3), vertexId(4), tagId(4), version(8)] <br/>
Value: [property1, property2, ... propertyN]

2. Edge

Directed edge

Positive edge:

Key: [type(1), partId(3), srcId(4), edgeType(4), Ranking(4), dstId(4), version(8)] <br/>
Value: [property1, property2, ... propertyN] <br/>

Negative edge:

Key: [type(1), partId(3), dstId(4), edgeType(4), Ranking(4), srcId(4), version(8)] <br/>
Value: [property1, property2, ... propertyN] <br/>

This positive/negative edge for partition-friendly that the one directed edge will be stored in the
partition contains source vertex and partition destination vertex.

3. Key Space

[spaceId, partId, key]

In nebula, we can determine a key by the [spaceId, partId, key] triad.

The spaceId is used to separate the key.

The partId is used to identify the partition of raft group.

The key is used to identify the one record in the actual storage engine.

## Top

The storage architecture in code:

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

The storage architecture in logical:

```
                    Space                                      Space

      raft group     raft group   raft group  ...

      |P1|              |P2|          |P3|

    |P1|  |P1|      |P2|    |P2|   |P3|   |P3|
```

The storage architecture in physis:

```
                 Space                                  Space
|P1|              |P2|              |P3|
|P3| -consensus-  |P1| -consensus-  |P1|  ...
|P2|              |P3|              |P2|
host              host              host
```
