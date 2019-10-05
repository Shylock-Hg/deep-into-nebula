# Overview

The [nebula](https://nebula-graph.io/), a distributed graph database.

Which is combined by there component `Meta`, `Graph`, `Storage` as below:

|Component|Note|
|:--|:--|
|Meta|Manage Host, Schema and ACL etc.|
|Graph|Graph compute|
|Storage|Storage vertex/edge|

## Top

```
Query Engine      Query Engine   ...    Query Engine

-------------------------------------------------------  Meta

Partition         Partition      ...    Partition
            ----- Consensus/Raft ----
Storage Engine    Storage Engine        Storage Engine
```

## Query Execute Flow

```
client --query--> parser --AST--> Execution Planner
                    |                    |
              plan from cache           AST
                    |                    |
                    V                    V
              Execution Engine<--plan--Optimizer
                |           |
            edge/vertex  schema/acl
                |           |
                V           V
          Storage Client  Meta Client
```

## Reference

[Raft](https://raft.github.io/)
