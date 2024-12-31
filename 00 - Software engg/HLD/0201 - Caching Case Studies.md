### **3. Local Cache Case Study**

#### **Problem Statement**:

- **Scenario**: In a DSA problem submission system (e.g., Scaler), app servers fetch large input/output files (~2 seconds) from file storage during code execution. This leads to delays.
- **Goal**: Minimize file fetch time and ensure updated files are used.

#### **Assumptions**:

- Reading from HDD: **40ms**
- Reading metadata from MySQL: **50ms**
- Fetching from file storage: **~2 seconds**

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf0BPpnfAqsN7o1xEIpcsZ0huHjkR7g9Zwy0-V9M0wDfInJqyb7wXDId4h0y_KT_p6eiYYPKxl_NzmkSO2NuznP85z3PznsAPdjCZLd_r_1srHxLof4WCuXKyHtvyCr6HQEme7WNUXJHjmZza7l9clJVn19G46SN8fu-xTuGlxru7nahq0LxF4?key=yf4iT5t-bYpY_UJHGSwSdA)**

#### **Solutions**:

1. **Time-To-Live (TTL)**:
    - **Low TTL**: Frequent cache invalidations lead to high cache misses.
    - **High TTL**: Updates in files might not reflect promptly.
    - **Drawback**: TTL alone is insufficient due to trade-offs in cache miss rates and update latency.
    
1. **Metadata-Based Caching**:
    - Use metadata (e.g., `problem_id`, `updated_at`) stored in MySQL to construct unique filenames (`problem_id_updated_at_input.txt`).
    - Steps:
        1. Query MySQL for the file’s last updated time.
        2. Check if the constructed filename exists in the local cache.
        3. If not, fetch the file from storage, store it locally, and serve it.
    - **Advantages**:
        - Accurate cache invalidation.
        - Efficient revalidation using metadata.
        
1. **File Updates**:
    - Upload the new file to storage.
    - Update MySQL with the new path and timestamp.
    - On subsequent requests, detect changes via metadata mismatch and refresh the cache.
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd8F-LYAUQXFNYUp9C8H_qdlyg5Tt9H9DXtzBIsOvu6VNkTjtz16LSd7ZBmYnrDYKLKARrSk2v8P-fdyjwIkFuNXq7CVWtsdd4MkpNaJM4kXy2UMuB9RjZZtRxGdDvMp0DZjTioiwzigWFrrAzyP2ZebWDqIyuwvrohSFpctSjjxjg0JtNkkzU?key=yf4iT5t-bYpY_UJHGSwSdA)**

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcLOvvn9mzxo8wTpU_yKvFKtl4j0p6eCrM1GhCs-GDW9iQU-LGH7VQal-pY9rGI2Jan6UriRlUVxeNBYE7V6BuZHrDbqMdBpxy-WjtDkOkKFxSlJoV23A2raDWRdrOLH9LRQpiAAgC3NhvQsubjB0Mah9n9ztEBGPXZdiPrwuippA4_DvRJWmk?key=yf4iT5t-bYpY_UJHGSwSdA)**

#### **Key Takeaways**:

- Metadata-driven cache invalidation is robust and avoids TTL trade-offs.
- Local caches reduce fetch times significantly but require careful management.

---

### **4. Global Cache Overview**

#### **Why Global Cache?**

1. **Issues with Local Caching**:
    - Limited to individual servers.
    - High cache miss rates due to lack of sharing.
2. **Advantages of Global Cache**:
    - Shared data across all app servers.
    - Reduces redundancy and improves cache hit rates.

#### **Case Study: Contest Rank List**

- **Problem**: Rank lists require frequent sorting and querying during contests, causing high DB load.
- **Solution**:
    - Compute rank lists periodically (e.g., every 1 minute).
    - Cache the result in a global cache (e.g., Redis).
    - Serve cached results to reduce database load.
- **Why Global Cache?**
    - Eliminates redundant local caching.
    - Only one cache miss per update period, benefiting all servers.

#### **Redis for Global Caching**:

- **Key Features**:
    - Single-threaded, in-memory key-value store.
    - Supports strings, integers, lists, sets, and sorted sets.
- **Use Cases**:
    1. Caching frequently queried data.
    2. Storing derived information that is expensive to compute.
- **Hands-On Resources**:
    - Redis tutorials: [try.redis.io](https://try.redis.io/)
    - Data types: [Redis Sets](https://redis.io/docs/data-types/sets/), [Redis Sorted Sets](https://redis.io/docs/data-types/sorted-sets/)

---

### **5. Facebook Newsfeed Case Study**

#### **Problem Statement**:

- **Goal**: Fetch recent posts made by a user’s friends efficiently.
- **Challenges**:
    - Posts and user data are sharded across multiple machines.
    - Fetching posts from all friends’ shards is slow.

#### **Proposed Solution**:

1. **Estimate Storage Needs**:
    
    - **Daily Post Volume**:
        - Active Users (DAU): **500M**
        - Posting Users (~1% of DAU): **5M**
        - Average posts per user: **4**
        - Total daily posts: **20M**
    - **Storage per Post**:
        - Metadata (poster_id, timestamp, etc.): **300 bytes**
        - Daily storage: **20M posts × 300 bytes = ~6GB**
        - Recent 30 days: **6GB × 30 = ~180GB**
2. **Optimize Newsfeed Fetch**:
    
    - Store recent posts (30 days) in a separate database for fast access.
    - Use HDD for cost-efficient storage.
    - Periodically delete posts older than 30 days.
3. **Query Design**:
    
    - Fetch friend IDs of the user.
    - Query recent posts:
        
        ```sql
        SELECT * FROM all_posts WHERE user_id IN (friend_ids) LIMIT x OFFSET y;
        ```
        
    - Paginate results to handle large datasets.

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXekydaACi9hetf1_5vlvO6OzPZWJPzYDLKA7xAmJ5k21nlWBM4s3W_FsvBxgf77VFt6FrD1Z4yAVJtv1y5N61Z9vUI6wYDS-Ahy1UMSkmTBeKrEqVS6uEEfbMEyI6XE6eRvHfGiJBGTvswqL7qgBcd7vXRrN5RpKwhYC6UOjYVL6uN1_eZQCWc?key=yf4iT5t-bYpY_UJHGSwSdA)**

#### **Advantages**:

- Reduces data latency and improves scalability.
- Separates frequently accessed data from long-term storage.

---

### **6. Key Interview Takeaways**

1. **General Caching Concepts**:
    
    - Understand the pros and cons of local vs. global caching.
    - Familiarize yourself with TTL and metadata-based invalidation strategies.
2. **Case Studies**:
    
    - Practice explaining solutions for contest rank lists and large-scale systems like Facebook newsfeeds.
    - Emphasize trade-offs and scalability in your answers.
3. **Hands-On Practice**:
    
    - Explore Redis for practical experience.
    - Work on designing efficient SQL queries for paginated data.
4. **Performance Metrics**:
    
    - Cache hit/miss rates.
    - Data latency and eviction policies.
    - System throughput and scalability.

By integrating these concepts and examples into your preparation, you'll be well-equipped to tackle caching-related interview questions effectively.