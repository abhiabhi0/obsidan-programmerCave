## Overview of URL Shortening Service
A URL shortening service takes a long URL and generates a shorter, unique alias for it. When users access the short URL, they are redirected to the original long URL. Key functionalities include:

- **Shortening URLs**: Generate a unique short URL for a given long URL.
- **Redirection**: Redirect users from the short URL back to the original long URL.
- **Analytics**: Track usage statistics such as click counts.

## Requirements

### Functional Requirements
1. **Shorten Long URLs**: Given a long URL, generate a shorter, unique alias.
2. **Redirect**: Users should be redirected to the original URL when accessing the short link.
3. **Link Expiration**: Shortened links may expire after a certain period.
4. **Custom Aliases**: Allow users to create custom short URLs.

### Non-Functional Requirements
1. **High Availability**: The service should be operational 99.99% of the time.
2. **Low Latency**: Redirection should occur with minimal delay.
3. **Scalability**: The system should handle increasing loads seamlessly.
4. **Durability**: Data must be reliably stored and retrievable.

## High-Level Architecture

### Components
1. **User Interface**: A web form or API for users to input long URLs and receive shortened links.
2. **Load Balancer**: Distributes incoming requests across multiple application servers to manage traffic efficiently.
3. **Application Servers**:
   - **URL Generation Service**: Creates short URLs using encoding techniques (e.g., Base62).
   - **Redirection Service**: Handles requests to redirect users from short URLs to long URLs.
4. **Database**:
   - Stores mappings between short URLs and long URLs (e.g., NoSQL databases like MongoDB or Cassandra).
5. **Cache Layer**: Utilizes in-memory storage (e.g., Redis) for frequently accessed data to reduce latency.
6. **Analytics Service (Optional)**: Collects data on usage patterns (e.g., number of clicks).

### Diagram
```plaintext
+------------------+
|   User Interface  |
+------------------+
         |
         v
+------------------+
|   Load Balancer   |
+------------------+
         |
         v
+------------------+     +------------------+
| Application Server|<--> |    Cache Layer    |
|  (URL Generation) |     +------------------+
+------------------+
         |
         v
+------------------+
|     Database      |
+------------------+
```

## Detailed Design Considerations

### URL Generation Techniques
1. **Base62 Encoding**:
   - Uses characters A-Z, a-z, and 0-9.
   - Generates a unique string based on the ID of the original URL in the database.

2. **Hashing (MD5)**:
   - Generates a fixed-length hash value for the original URL; however, this may lead to collisions.

3. **Counter-Based Approach**:
   - Maintains a counter that increments with each new shortened URL request, ensuring uniqueness.

### Data Model Design
- **Entities**:
  - `URLMapping`: Stores `shortURL`, `longURL`, `creationDate`, `expiryDate`.
  
```plaintext
| shortURL | longURL               | creationDate | expiryDate |
|----------|-----------------------|--------------|------------|
| abc123   | http://example.com/1  | 2025-01-14   | 2026-01-14 |
```

### API Design
- **POST /shorten**
  - Request body: `{ "longURL": "http://example.com" }`
  - Response: `{ "shortURL": "http://short.ly/abc123" }`

- **GET /{shortURL}**
  - Redirects to the corresponding long URL.

### Handling Scalability and Reliability
1. **Horizontal Scaling**:
   - Add more application servers behind the load balancer as traffic increases.

2. **Database Sharding**:
   - Split the database into smaller parts (shards) based on certain criteria (e.g., alphabetically by short URL).

3. **Caching Strategies**:
   - Store frequently accessed URLs in cache to minimize database load and speed up redirection.

4. **Monitoring and Alerts**:
   - Implement monitoring tools (e.g., Prometheus) to track system performance and set up alerts for failures.

## Conclusion
Designing a robust URL shortening service requires careful consideration of functional and non-functional requirements, as well as architectural choices that enable high availability, scalability, and reliability. By understanding these components and their interactions, you will be well-prepared for your interview on this topic.

Citations:
[1] https://www.geeksforgeeks.org/system-design-url-shortening-service/
[2] https://www.geeksforgeeks.org/how-to-crack-system-design-round-in-interviews/
[3] https://dev.to/karanpratapsingh/system-design-url-shortener-10i5
[4] https://in.indeed.com/career-advice/interviewing/system-design-interview-questions
[5] https://blog.algomaster.io/p/design-a-url-shortener
[6] https://www.tryexponent.com/blog/system-design-interview-guide
[7] https://systemdesign.one/url-shortening-system-design/
[8] https://www.geeksforgeeks.org/5-common-system-design-concepts-for-interview-preparation/