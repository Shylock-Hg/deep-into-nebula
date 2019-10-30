# Graph

## Overview

The graph server provide the Graph semantics API, in fact the *nGQL* language.

## Top

```
      (nGQL)
         |
         V
    GraphService
     |         |
(Vertex, Edge) (ACL, replications, schema)
     V                  V
StorageClient       MetaClient
```

## nGQL Flow

```
query -Parser-> SequentialSentences --> Executors --> Prepare one by one
--> Set onFinish/onError one by one --> execute Executor one by one
```

The front executor call next executor from onFinish, the last fill response from
onFinish.

## Sentences

### Fetch Edges

Synopsis

```sql
FETCH PROP ON <dege_type> <vid>-><vid> [, <vid>-><vid>...] [YIELD [DISTINCT] <edge_type.p>...]
```

Execute flow

1. edge schema
2. yield clause
3. edge key (src->dst)
4. yield column schema
5. construct each edge keys from src to dst
6. fetch properties by edge keys from storage (async)
7. write to tables, edge->row, properties->columns

### Yield

Synopsis

```sql
YIELD [DISTINCT] <col_name> [AS <alias>] [, <col_name> [AS <alias>]...] [WHERE <conditions>]
```

Execute flow

1. where clause
2. yield schema
3. filter and collect to RowSetWriter from inputs/constants

## Reference

[nGQL](https://docs.nebula-graph.io/manual-index/)
