INFINISPAN
==========

Infinispan is a caching framework that implements JSR107 and provides services
from the ``javax.cache`` package.

Infinispan caches are organized in **cache containers**.

There are 4 default cache containers in EAP 7:
- **hibernate**: for entity caching
- **ejb**: for stateful session bean replication
- **web**: for session replication
- **server**: for singleton caching

Every cache container has a default cache.

When providing caches to cache containers we can choose among 4 types of caches:
- **Local**: Entries are local to the node and not ditributed
- **Invalidation**: Great for read-heavy systems with a Cache Store behing (ie: a DB),
  It caches only contents that have been updated and optimizes i/o.
- **Replicated**: all caches are replicated to all the nodes in the cluster
- **Distributed**: the content is replicated to a subset of nodes. It uses a 
  **consistent hash algorithm** to determine where in a cluster entries should be
  stored and the number of copies to be maintained. The number of copies is a tradeoff
  between performance and data integrity. The more copies are maintained, the lower 
  performance will be but also the risk of loosing data due to server outages.
  When using distributed caches an **L1** cache (aka **Near Cache**) can be used to
  speed up. There is a tradeff: every time the data in the L1 cache changes a broadcast
  must be done to all the nodes to ensure they invalidate the entry.

Infinispace provides a **CacheManager** as the API for exposing various operatinos related to
the cache containers.
The CacheManager exposes a **Cache Interface** that exposes simple methods for adding,
rethrieving and removing entries.
Applications use the Cache Interface API to access to the cache.

Persistance is managed by a **PersistentStore**. Possible backends: memory, db, flat files.
A **Persistence Interface** is exposed to allow communication with the store.

Infinspan uses **JGroups** as the framework for clustering.


