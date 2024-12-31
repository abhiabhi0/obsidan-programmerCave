#### 1. Motivation

##### Local Cache

Adding a local cache to application servers reduces database queries by storing frequently accessed data. While this speeds up responses, it introduces:

- **Cache Misses**: Each server’s cache contains only a subset of the data.
- **Stale Data**: Lack of synchronization across server caches.
- **Memory Overhead**: High memory consumption.

##### Global Cache

A global cache decouples caching from application servers, enabling independent scaling. Challenges include:

- **Single Point of Failure**: If the cache server crashes, data retrieval slows.
- **Scalability Limitations**: Scaling a single server has practical and financial limits.

##### Distributed Cache

To address these challenges, a distributed cache clusters multiple servers for:

- **Fault Tolerance**: Multiple nodes reduce single-point failures.
- **Scalability**: Horizontal scaling increases capacity. Redis is a preferred solution for distributed caching.

---

#### 2. Redis

Redis is an in-memory key-value data store offering:

- **Fast Access**: In-memory storage reduces latency.
- **Optional Durability**: Data persistence to disk is configurable.
- **Versatility**: Functions as a cache, database, or message broker.

##### Usage

- **Key-Value Store**: Efficiently store and retrieve data.
    
    ```redis
    SET user:1 "John Doe"
    GET user:1
    ```
    

##### Setup

1. Install Redis:
    
    ```
    brew install redis
    ```
    
    (Other installation options are available [here](https://redis.io/docs/getting-started/installation/).)
    
2. Start Redis server:
    
    ```
    redis-server
    ```
    
3. Connect via CLI:
    
    ```
    redis-cli
    ```
    

##### Basic Commands

- **SET**: Add a key-value pair.
    
    ```
    SET key value
    ```
    
- **GET**: Retrieve a value.
    
    ```
    GET key
    ```
    
- **DEL**: Remove a key.
    
    ```
    DEL key
    ```
    
- **INCR**: Increment a numeric value.
    
    ```
    INCR counter
    ```
    
- **EXISTS**: Check if a key exists.
    
    ```
    EXISTS key
    ```
    
- **CONFIG**: Configure parameters.
    
    ```
    CONFIG SET maxmemory 1mb
    ```
    

---

#### 3. Eviction Policies

##### Background

Redis stores data in memory, a limited resource. When the memory limit is reached, eviction policies dictate how to remove data to free up space.

##### No Eviction (Default)

Redis does not evict data, returning an error if memory is full. This can cause application crashes unless mitigated.

##### Least Recently Used (LRU)

LRU evicts the least recently accessed keys first:

1. Configure LRU:
    
    ```
    CONFIG SET maxmemory-policy allkeys-lru
    CONFIG SET maxmemory 1mb
    ```
    
2. Test with Python (using `redis-py`):
    
    ```python
    from redis import Redis
    
    instance = Redis()
    instance.flushall()
    instance.set('key', 'value')
    print(instance.get('key'))  # Output: b'value'
    
    instance.set('key-1', 'value-1')
    instance.get('key-1')
    print(f"Used memory: {instance.info()['used_memory_human']}")
    ```
    
3. Simulate LRU eviction by filling memory and verifying key retention.

---

#### 4. Cluster Mode

##### Background

Cluster mode enables horizontal scaling and fault tolerance. Redis clusters use:

- **Sharding**: Partition data across nodes using slots (fixed at 16,384).
- **Replication**: Ensure availability by replicating data across nodes.

##### Configuration

1. Enable clustering in `redis.conf`:
    
    ```
    cluster-enabled yes
    cluster-config-file nodes.conf
    cluster-node-timeout 5000
    ```
    
2. Minimum three master nodes are required for a functional cluster.

##### Setup

1. Create a Docker network:
    
    ```
    docker network create redis-cluster
    ```
    
2. Start Redis nodes on the network:
    
    ```
    docker run -d --name redis1 --net redis-cluster redis:latest
    docker run -d --name redis2 --net redis-cluster redis:latest
    docker run -d --name redis3 --net redis-cluster redis:latest
    ```
    
3. Connect nodes to form a cluster:
    
    ```
    redis-cli --cluster create \
      127.0.0.1:6379 127.0.0.1:6380 127.0.0.1:6381 \
      --cluster-replicas 1
    ```
    

##### Testing the Cluster

Perform operations like `SET` and `GET` to verify:

```redis
SET key value
GET key
```

---

Redis’s eviction policies and cluster mode provide scalable solutions for caching challenges, ensuring reliability and performance for high-demand applications.