# Meta

## Overview

The meta server maintain the ACL, replications and schema in database.

## Top

The architecture about meta server:

```
    (ACL, replications and schema API)         (ACL, replications and schema API)
               |                                             |
               V                                             V
             MetaServer                                  MetaServer
               |                                             |
 (kDefaultSpaceId, kDefaultPartId, key, value)   (kDefaultSpaceId, kDefaultPartId, key, value)
               V                                             V
            KVStore         --- consensus ---             KVStore
               |                                             |
               V                                             V
            RaftPart                                      RaftPart
```

The default implementation:

```
    (ACL, replications and schema API)         (ACL, replications and schema API)
               |                                             |
               V                                             V
             MetaServer                                  MetaServer
               |                                             |
 (kDefaultSpaceId, kDefaultPartId, key, value)   (kDefaultSpaceId, kDefaultPartId, key, value)
               V                                             V
           NebulaStore        --- RaftService ---       NebulaStore
               |                                             |
               V                                             V
             Part                                          Part
               |                                             |
               V                                             V
            RocksDB                                       RocksDB
```
