### 1. **What is an Idempotent API?**

- **Definition**: Idempotency ensures that the effect of a successfully performed request on a server resource is independent of the number of times it is executed.
    - Example: Adding zero to a number is an idempotent operation.
- **Importance**:
    - Prevents issues caused by duplicate requests (e.g., retries due to timeouts or network issues).
    - Enables fault-tolerant APIs and efficient caching strategies.

### 2. **Idempotency with HTTP Methods**

- An **idempotent HTTP method** produces the same outcome regardless of how many times it is called.
- **Idempotent Methods**: GET, PUT, DELETE, HEAD, OPTIONS, TRACE.
- **Non-Idempotent Methods**: POST, PATCH.

#### 2.1. **POST and PATCH**

- **POST**:
    - Typically used to create a new resource.
    - Repeated POST requests create new resources (not idempotent).
    - Side effects may include notifications or state changes.
- **PATCH**:
    - Used for partial updates to a resource.
    - Behavior depends on the initial state; repeated requests can lead to different outcomes.
    - Example: Incrementing a cart quantity with PATCH is non-idempotent.

#### 2.2. **GET, HEAD, OPTIONS, TRACE**

- Purely retrieve resource data or metadata.
- **No state changes on the server** – hence idempotent.

#### 2.3. **PUT**

- Typically used to update or replace a resource.
- Repeated PUT requests overwrite the resource state, leaving the outcome unchanged (idempotent).

#### 2.4. **DELETE**

- **With Resource Identifier**:
    - First request deletes the resource (200 OK or 204 No Content).
    - Subsequent requests return 404 Not Found, but no additional state changes occur (idempotent).
- **Without Resource Identifier**:
    - Example: `DELETE /item/last` deletes the last item.
    - Repeated requests delete multiple resources (non-idempotent).
    - Suggestion: Change such APIs to POST (closer to HTTP specs).

### 3. **Handling Non-Idempotent Operations**

- Strategies to make non-idempotent methods safer:
    1. **Unique Request Identifiers**: Prevent duplicate processing of the same request.
    2. **Transactional Mechanisms**: Rollback changes if duplicate requests are detected.
    3. **Idempotent Request Headers**: Add metadata to indicate duplicate requests and handle them appropriately.

### 4. **Summary**

- Idempotent APIs ensure predictable behavior, even with network failures or retries.
- Benefits:
    - Robust and consistent web services.
    - Simplified caching and optimization strategies.
    - Enhanced user experience through reliable API behavior.

### **Key Takeaways for Interviews**

- Understand the idempotent behavior of each HTTP method.
- Be prepared to explain how to handle non-idempotent operations safely.
- Highlight the importance of designing APIs that align with REST principles and ensure fault tolerance.