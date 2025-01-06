- Redis or Memchached
- Caches are in-memory database with really high performance & low latency
- Helps reduce load off of DB for read intensive workloads
- helps make your application stateless
- AWs takes care of OS maintainance/patching, optimizations, setup, config, moitoring, failure recovery, backups

## Redis
- Multi AZ with failover
- Read Replicas to scale read & have High Availability (ASYNC) (ASYNC MultiAZ)
- Data Duarbilty using AOF persistence
- Backup & restore
- Sets & Sorted Sets
- The configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas

## Memcached
- Multi-node for partitioning data (sharding)
- No hgh vailability (replication)
- Non persistent
- No backup and restore
- Multi-threaded architecture

## CachingConsiderations
	- Data maybe out of date, eventually consistent
		- Patterns: data changing slowly, few keys are reqd
		- Anti patterns: data changing rapidly, all large key space freq needed
	- is data structured well for caching
		- k-v caching
		- caching aggregations result

## Strategies
	- LazyLoading/CacheAside/LazyPopulation

