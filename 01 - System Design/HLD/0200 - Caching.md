## **Key Learnings**

### **1. Application Server Design**

- **Traditional Model Issues**:
    - Coupled code and database increase unavailability during deployments.
    - Limited resources as the database consumes part of the machine's capacity.
    - **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdZtvBmTl8JPHbn1XDw-pOCa7bZZi1OyqKZi8e7RLNyxgdgvavc997l3sysNEjcUU71b7XH63dha114Yreep01D6QcNzcGVYu-wb32lHNRw22yaDXh6vMXolS7mEAWoJJ7VzeVmJeg0BJ-KUWM6POu_gsid?key=opvrDHxFl-yzIfVOKFuiHA)**

- **Solution**: Decoupling code and storage to:
    - Eliminate resource contention.
    - Achieve scalability using stateless application server machines.
    - **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBCXOufncYNc3ehqHLWVQyGXOXT7Cj6Y105N3aopstJURuqmoYfZ4Z64VTLS4qtxBIC_rfXnz8Kjq9rAL9A1OLxPiRJmtI2vIVsBECRVGvQdXO5O_CHnhgeYM0OhoyHWPlnljymdhyRPhnFVlBTSW9OnrR?key=opvrDHxFl-yzIfVOKFuiHA)**

### **2. Caching**

Caching is the process of storing frequently accessed data closer to the system or in a faster storage medium to improve performance.

#### **2.1 Levels of Caching**

1. **Browser Caching**:
    - Caches DNS entries and static assets like images and JavaScript to reduce latency.
    - Enables faster page loads after the first visit.
    
1. **Content Delivery Network (CDN)**:
    - Distributes multimedia and static content to geographically closer servers.
    - Optimizes user experience by providing faster access to resources.
    - Examples: Akamai, Cloudflare, Amazon CloudFront, Fastly.
    - Uses:
        - Tight ISP integrations for regional proximity.
        - Anycast to direct traffic to the nearest server.
        
1. **Local Caching**:
    - Performed at the application server to minimize database calls.
    
1. **Global Caching**:
    - Centralized in-memory stores (e.g., Redis, Memcached).
    - Holds actual or derived data for quicker access.

#### **2.2 Cache Challenges**

1. **Data Staleness**: Cache may not reflect updates in the database.
2. **Limited Size**: Cache overflow necessitates an eviction strategy.
3. **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf40bxgtON0A-nRD1rnIF5lHf0AcNnF1C03_JqprBmlcn-cvssbX-R6mBWqp9Qyufo8aQxK1ub8RT1YYPbtjJIikqDnX6XtL6nA-lygtwrYBVkxRcDpaRy2mDwgJqju9VU-33CZFU_iOrtC6sCm9gBnDo46?key=opvrDHxFl-yzIfVOKFuiHA)**

#### **2.3 Solutions to Cache Challenges**

1. **Cache Invalidation**:
    - **TTL (Time to Live)**:
        - Entries are valid for a specific duration.
        - Periodic refresh ensures stale data is not served.
    - **Write Strategies**:
        - **Write-Through**: Updates cache and DB synchronously. Ensures consistency, preferred for read-heavy systems.
        - **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcbBXRsxYJY_qp9PGdiJUV94VsZm49jtjVs8z-Hbx9i8rVWvJtQ8OzIvdcbxb-Vf-dEo8qvE9o5Aq7eGPuj6DnIzrg9tAFqTTSbGe9pNxPLJISXqJQX1w45dWZ0i3zBqKkQQX10qM2PATFyz_VcQrCfL5vx?key=opvrDHxFl-yzIfVOKFuiHA)**
        
        - **Write-Back**: Updates cache first and syncs with DB asynchronously. Suitable for high-throughput, low-latency scenarios where data loss is tolerable.
        - **Write-Around**: Updates DB directly, with the cache eventually syncing via TTL.
2. **Eviction Strategies**:
    - **FIFO**: Remove the oldest entries.
    - **LRU**: Remove the least recently accessed entries.
    - **MRU**: Remove the most recently accessed entries.
    - **LIFO**: Remove the newest entries.
    - [[0202 - Scaling with Redis - Eviction Policies and Cluster Mode]]

#### **2.4 Caching Case Studies**

[[0201 - Caching Case Studies]]

1. **Facebook Newsfeed**:
    - Stores recent posts in HDD (e.g., last 30 days) for fast retrieval.
    - Fetch recent posts using queries like `SELECT * FROM all_posts WHERE user_id IN friend_ids LIMIT x OFFSET y`.
    - Reduces latency with distributed reads.
2. **DSA Problem Submissions**:
    - Local caching of test data in app servers minimizes repeated DB calls.
    - Use file metadata (e.g., `problem_id_updated_at_input.txt`) for cache validation.
    - TTL or event-driven updates ensure data consistency.

---

### **3. Assignments**

1. **Local Cache Invalidation**:
    - How to invalidate local cache on test data updates.
2. **Optimizing Newsfeed**:
    - How to calculate Facebook’s newsfeed efficiently.

---

Let me know if you'd like to expand on any specific topic or include more details!