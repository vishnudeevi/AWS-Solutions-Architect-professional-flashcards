# AWS Storage Services
Performance at Scale with Amazon ElastiCache white paper - hhttps://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

### Questions goes here
```

 1. What is ElastiCache Architecture?
```
<details><summary>show</summary>
<p>

- Amazon ElastiCache allows you to seamlessly set up, run, and scale popular open-Source compatible in-memory data stores in the cloud.

- Build data-intensive apps or boost the performance of your existing databases by retrieving data from high throughput and low latency in-memory data stores.

- Amazon ElastiCache is a popular choice for real-time use cases like Caching, Session Stores, Gaming, Geospatial Services, Real-Time Analytics, and Queuing.


</p>
</details>


```

 2. What is in-memory caching and how does it help my applications?
```
<details><summary>show</summary>
<p>

- The in-memory caching provided by Amazon ElastiCache can be used to significantly improve latency and throughput for many read-heavy application workloads (such as social networking, gaming, media sharing and Q&A portals) or compute-intensive workloads (such as a recommendation engine).

- In-memory caching improves application performance by storing critical pieces of data in memory for low-latency access. Cached information may include the results of I/O-intensive database queries or the results of computationally-intensive calculations.

</p>
</details>


```

 3. What are the two different in-memory key-value engines currently supported by Amazon ElastiCache ?
```
<details><summary>show</summary>
<p>

- Memcached

- Redis
</p>
</details>

```

 4. Redis 
```
<details><summary>show</summary>
<p>

- an increasingly popular open-source key-value store that supports more advanced data structures such as sorted sets, hashes, and lists.

- Unlike Memcached, Redis has disk persistence built in, meaning that you can use it for long-lived data.

- supports replication, which can be used to achieve Multi-AZ redundancy, similar to Amazon RDS.

- Because of the replication and persistence features of Redis, ElastiCache manages Redis more as a relational database.
</p>
</details>

```

 5. Memcached 
```
<details><summary>show</summary>
<p>
- a widely adopted in-memory key store, and historically the gold standard of web caching.

- multithreaded, meaning it makes good use of larger Amazon EC2 instance sizes with multiple cores.

- because Memcached is designed as a pure caching solution with no persistence, ElastiCache manages Memcached nodes as a pool that can grow and shrink, similar to an Amazon EC2 Auto Scaling group.

- Individual nodes are expendable, and ElastiCache provides additional capabilities here such as automatic node replacement and Auto Discovery.
</p>
</details>

```

  6. When to choose Memcached ?
```
<details><summary>show</summary>
<p>

- you want the ability to scale your cache horizontally as you grow

- object caching is your primary goal

- you are planning on running large cache nodes, and require multithreaded performance with utilization of multiple cores
</p>
</details>

```

  7. When to choose Redis ? 
```
<details><summary>show</summary>
<p>

- you are looking for more advanced data types, such as lists, hashes, bit arrays, HyperLogLogs, and sets

- when sorting and ranking datasets in memory is needed. example : leaderboards

- publish and subscribe (pub/sub) capabilities are needed.

- persistence of your key store important

- you want to run in multiple AWS Availability Zones (Multi-AZ) with failover

-encryption and compliance to standards, such as PCI DSS, HIPAA, and FedRAMP, required for your business
</p>
</details>


```

  8. Caching Design Patterns 
```
<details><summary>show</summary>
<p>

1. Lazy Caching

2. Write-Through

3. Time-to-live

4. Evictions

5. The Thundering Herd (also called dog piling)

</p>
</details>

```

  9. Lazy Caching 
```
<details><summary>show</summary>
<p>

Laziness should serve as the foundation of any good caching strategy. 

 Advantages :

- The cache only contains objects that the application actually requests, which helps keep the cache size manageable. 

- As new cache nodes come online, for example as your application scales up, the lazy population method will automatically add objects to the new cache nodes when the application first requests them.

- Cache expiration is easily handled by simply deleting the cached object. A new object will be fetched from the database the next time it is requested.

- Lazy caching is widely understood, and many web and app frameworks include support out of the box.

</p>
</details>


```

  10. Time-to-live 
```
<details><summary>show</summary>
<p>

- Always apply a time to live (TTL) to all of your cache keys, except those you are updating by write-through caching. 

- You can use a long time, say hours or even days. This approach catches application bugs, where you forget to update or delete a given cache key when updating the underlying record. Eventually, the cache key will auto-expire and get refreshed.

- For rapidly changing data such as comments, leaderboards, or activity streams, rather than adding write-through caching or complex expiration logic, just set a short TTL of a few seconds. If you have a database query that is getting hammered in production, it's just a few lines of code to add a cache key with a 5 second TTL around the query. This code can be a wonderful Band-Aid to keep your application up and running while you evaluate more elegant solutions.

- A newer pattern, Russian doll caching, has come out of work done by the Ruby on Rails team. In this pattern, nested records are managed with their own cache keys, and then the top-level resource is a collection of those cache keys. Say you have a news webpage that contains users, stories, and comments. In this approach, each of those is its own cache key, and the page queries each of those keys respectively.

- When in doubt, just delete a cache key if you're not sure whether it's affected by a given database update or not. Your lazy caching foundation will refresh the key when needed. In the meantime, your database will be no worse off than it was without caching.

</p>
</details>

```

  11. Evictions
```
<details><summary>show</summary>
<p>

- Evictions occur when memory is over filled or greater than maxmemory setting in the cache, resulting into the engine to select keys to evict in order to manage its memory. The keys that are chosen are based on the eviction policy that is selected.
- Eviction policies can be summarized as the following:
  - allkeys-lru: The cache evicts the least recently used (LRU) regardless of TTL
  - setvolatile-lru: The cache evicts the least recently used (LRU) from those that have a TTL
  - setvolatile-ttl: The cache evicts the keys with shortest TTL
  - setvolatile-random: The cache randomly evicts keys with a TTL
  - setallkeys-random: The cache randomly evicts keys regardless of TTL
  - setno-eviction: The cache doesn't evict keys at all. This blocks future writes until memory frees up.

</p>
</details>


```

  12. Write-Through
```
<details><summary>show</summary>
<p>

In a write-through cache, the cache is updated in real time when the database is updated.
So, if a user updates his or her profile, the updated profile is also pushed into the cache.

- A good example is any type of aggregate, such as a top 100 game leaderboard, or the top 10 most popular news stories, or even recommendations.

- Because this data is typically updated by a specific piece of application or background job code, it's straightforward to update the cache as well.

</p>
</details>

```

   13. The Thundering Herd 
```
<details><summary>show</summary>
<p>

- Also known as dog piling, the thundering herd effect is what happens when many different application processes simultaneously request a cache key, get a cache miss, and then each hits the same database query in parallel.

- The more expensive this query is, the bigger impact it has on the database. If the query involved is a top 10 query that requires ranking a large dataset, the impact can be a significant hit.

solutions is to prewarm the cache by following these steps:
- Write a script that performs the same requests that your application will. If it's a web app, this script can be a shell script that hits a set of URLs.

- If your app is set up for lazy caching, cache misses will result in cache keys being populated, and the new cache node will fill up.

- When you add new cache nodes, run your script before you attach the new node to your application. Because your application needs to be reconfigured to add a new node to the consistent hashing ring, insert this script as a step before triggering the app reconfiguration.
- If you anticipate adding and removing cache nodes on a regular basis, prewarming can be automated by triggering the script to run whenever your app receives a cluster reconfiguration event through Amazon Simple Notification Service (Amazon SNS).
</p>
</details>


```

   14. Architecture with ElastiCache for Redis- Part 1
```
<details><summary>show</summary>
<p>

- Unlike Memcached, ElastiCache clusters for Redis only contain a single primary node.

- After you create the primary node, you can configure one or more replica nodes and attach them to the primary Redis node.

- An ElastiCache for Redis replication group consists of a primary and up to five read replicas. Redis asynchronously replicates the data from the primary to the read replicas.

- Because Redis supports persistence, it is technically possible to use Redis as your only data store. In practice, customers find that a managed database such as Amazon DynamoDB or Amazon RDS is a better fit for most use cases of long-term data storage.
</p>
</details>

```

  15. Architecture with ElastiCache for Redis- Part 2 (Distributing Reads and Writes) 
```
<details><summary>show</summary>
<p>

Distributing Reads and Writes :
- Using read replicas with Redis, you can separate your read and write workloads. 

- This separation lets you scale reads by adding additional replicas as your application grows. In this pattern, you configure your application to send writes to the primary endpoint.
</p>
</details>

```

  16. Architecture with ElastiCache for Redis- Part 3  (Multi-AZ with Auto-Failover)  
```
<details><summary>show</summary>
<p>

Multi-AZ with Auto-Failover:

- Amazon ElastiCache can be configured to automatically detect the failure of the primary node, select a read replica, and promote it to become the new primary.

- ElastiCache auto-failover will then update the DNS primary endpoint with the IP address of the promoted read replica. If your application is writing to the primary node endpoint as recommended earlier, no application change will be needed.

- Unless you have a specific need otherwise, all production deployments should use Multi-AZ with auto-failover. Keep in mind that Redis replication is asynchronous, meaning if a failover occurs, the read replica that is selected might be slightly behind the master
</p>
</details>

```

  17. Architecture with ElastiCache for Redis- Part 4  (Sharding with Redis)  
```
<details><summary>show</summary>
<p>

Sharding with Redis :

- Redis has two categories of data structures: simple keys and counters, and multidimensional sets, lists, and hashes. The bad news is the second category cannot be sharded horizontally. But the good news is that simple keys and counters can.

- In the simplest case, you can treat a single Redis node just like a single Memcached node. Just like you might spin up multiple Memcached nodes, you can spin up multiple Redis clusters, and each Redis cluster is responsible for part of the sharded dataset.
</details>

```

  18. Monitoring and Tuning   
```
<details><summary>show</summary>
<p>
Here is some additional guidance for monitoring cache memory utilization. Each of these metrics is available in CloudWatch for your ElastiCache cluster:

Evictions—both Memcached and Redis manage cache memory internally, and when memory starts to fill up they evict (delete) unused cache keys to free space. A small number of evictions shouldn't alarm you, but a large number means that your cache is running out of space. •

CacheMisses—the number of times a key was requested but not found in the cache. This number can be fairly large if you're using lazy population as your main strategy. If this number is remaining steady, it's likely nothing to worry about. However, a large number of cache misses combined with a large eviction number can indicate that your cache is thrashing due to lack of memory.

BytesUsedForCacheItems—this value is the actual amount of cache memory that Memcached or Redis is using.
Both Memcached and Redis attempt to allocate as much system memory as possible, even if it's not used by actual cache keys. Thus, monitoring the system memory usage on a cache node doesn't tell you how full your cache actually is. •

SwapUsage—in normal usage, neither Memcached nor Redis should be performing swaps.

Currconnections—this is a cache engine metric representing the number of clients connected to the engine. We recommend that you determine your own alarm threshold for this metric based on your application needs.

An increasing number of CurrConnections might indicate a problem with your application— you'll need to investigate the application's behavior to address this issue.
</details>

