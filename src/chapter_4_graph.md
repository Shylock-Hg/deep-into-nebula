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

## Reference

[nGQL](https://docs.nebula-graph.io/manual-index/)
