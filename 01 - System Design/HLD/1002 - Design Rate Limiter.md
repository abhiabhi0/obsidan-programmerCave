#### **Definition**

A rate limiter controls the rate of traffic sent from a client to a server, helping to prevent abuse such as DDoS attacks. It limits the number of requests a client can send within a given time frame.

#### **Throttling**

Throttling refers to controlling API usage by clients during a specified time period.

---

### **Types of Rate Limiter**

1. **Client-Side Rate Limiter**: Implemented on the client to limit the frequency of outgoing requests.
2. **Server-Side Rate Limiter**: Implemented on the server to limit incoming requests from clients.

---

### **Rate Limiting Example**

- IP Address: `10.20.30.40`
- Limit: Maximum of 5 requests per 60 seconds.
- If the 6th request is made within the same 60-second window:
    - The request is rejected.
    - HTTP error code `429` (Too Many Requests) is returned.

---

### **Algorithms for Rate Limiting**

1. **Token Bucket Algorithm**
2. **Leaking Bucket Algorithm**
3. **Fixed Window Counter**
4. **Sliding Window Counter**

---

### **Data Structures for Rate Limiting**

1. **HashMap**:
    
    - Key: IP address
    - Value: Metadata (e.g., request timestamps or counters)
2. **Deque Solution**:
    
    - Store request timestamps for each IP in a deque.
        
    - Example with a threshold of 5 seconds:  
        At time `t = 15`, the deque: `[10, 11, 12, 12, 13, 14, 15]`.
        
        - At `t = 16`, remove timestamps older than `16 - 5 + 1 = 12`.  
            Updated deque: `[12, 12, 13, 14, 15, 16]`.
    - If deque size < threshold, allow the request.
        
    - **Challenges**:
        
        - High memory usage for large numbers of requests (e.g., 10k requests).
        - Maintaining and updating deques for each IP is computationally expensive.

---

### **Rate Limiting Solutions**

#### **1. Fixed Window Counter**

- **Mechanism**:  
    Divide time into fixed buckets (e.g., 50 seconds) and count the number of requests per bucket.
- **Challenge**:  
    If 100 requests are made in the last second of one bucket and another 100 in the first second of the next, the combined 200 requests in 50 seconds exceed the threshold.
    **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBfWcE_KYyStx2dNkWc3090cJo2q_O_RXMy7Tb92ZqHu-yJbFdeA9CJsw9ZTSX_bFykkgEqucMqX05ae6Qhd799Oy20oqs_H6FhffBiR_KkgWVm-6DibgxbLxlVx8rTVcge6motYtpEmH1K_EGdBu9pMOC?key=1mUvxiYLB-AZevkkTAPX6g)**

---

#### **2. Sliding Window Counter** (Best Solution)

- **Mechanism**:  
    Approximate rate limiting by overlapping consecutive buckets and using percentages to calculate request rates.
    
- **Example**:
    
    - Timeline divided into buckets of size 50 (e.g., `1-50`, `51-100`, `101-150`).
        
    - Requests:
        
        - 60 in `51-100`.
        - 40 in the first half of `101-150` (`101-120`).
    - Percentage of the interval `(101-120)` covered:  
        `(20/50) * 100% = 40%`.
        
    - Approximate count for the remaining 60% of the interval using the previous bucket:  
        `60% of 60 = 36`.
        
    - Total approximate count for `101-150`:  
        `36 (from prev) + 40 (current) = 76`.
        
- **Advantages**:
    
    - Significantly reduces metadata storage.
        
    - Instead of tracking all requests (as in the deque solution), only maintain:
        
        - `prev_bucket` count
        - `cur_bucket` count.
    - Data structure: `map<string, pair<int, int>>`.
        
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfApB7TGxsOuh5V7BKrqZ7kVXY16ryUE4fnVTo8DN9rWjXJvwRHj6tAVPSZrqo2MQMr7EQ9GTysk7uZ5Y4jYkH9LVb3-7xbBh5FucZwvygmIcxPBnr6mu2JLQw7t-znw0Hha4EiDX2MR3MI20DT-0n0uRBX?key=1mUvxiYLB-AZevkkTAPX6g)**

---
### **Summary of Key Points**

1. **Deque Solution**:
    
    - Memory-intensive.
    - Suitable for small-scale systems or low traffic.
2. **Fixed Window Counter**:
    
    - Easy to implement.
    - Suffers from edge-case inaccuracies (e.g., boundary requests).
3. **Sliding Window Counter**:
    
    - Best solution for large-scale systems.
    - Combines accuracy with minimal resource usage.
https://docs.google.com/document/d/1s-XVwY2NfyvIH-kEpQOixml72SqBRJr9nUMNycTqwgc/edit?tab=t.0#heading=h.3znysh7
---

### **Implementation Pseudocode**

#### Sliding Window Counter (Python Example):

```python
import time
from collections import defaultdict

class SlidingWindowRateLimiter:
    def __init__(self, limit, window_size):
        self.limit = limit
        self.window_size = window_size
        self.data = defaultdict(lambda: {"prev": 0, "cur": 0, "last_update": 0})

    def allow_request(self, ip):
        now = time.time()
        bucket = int(now // self.window_size)

        # Update buckets
        if self.data[ip]["last_update"] < bucket:
            self.data[ip]["prev"] = self.data[ip]["cur"]
            self.data[ip]["cur"] = 0
            self.data[ip]["last_update"] = bucket

        # Calculate sliding window requests
        elapsed = now % self.window_size
        prev_percentage = (self.window_size - elapsed) / self.window_size
        total_requests = self.data[ip]["cur"] + self.data[ip]["prev"] * prev_percentage

        if total_requests < self.limit:
            self.data[ip]["cur"] += 1
            return True  # Allow request
        else:
            return False  # Deny request
```