### **1. Unary RPC (One Request, One Response)**

- **What it is:**  
    A single request is sent from the client, and the server sends back a single response.
    - Think of it like a regular API call: "Ask once, get one answer."
- **Example Use Case:**  
    Fetching a user's profile by user ID.
- **Proto Example:**
    
    ```proto
    service MyService {
        rpc UnaryMethod(MyRequest) returns (MyResponse);
    }
    ```
    

---

### **2. Server Streaming (One Request, Multiple Responses)**

- **What it is:**  
    The client sends one request, and the server sends back multiple responses as a stream.
    - Think of it as: "Ask once, get answers piece by piece over time."
- **Example Use Case:**  
    Streaming logs or a live feed of updates to the client.
- **Proto Example:**
    
    ```proto
    service MyService {
        rpc ServerStreamingMethod(MyRequest) returns (stream MyResponse);
    }
    ```
    
    - `stream` means the server sends multiple responses.

---

### **3. Client Streaming (Multiple Requests, One Response)**

- **What it is:**  
    The client sends a stream of multiple requests, and the server responds once after processing all of them.
    - Think of it as: "Send all your data, get one summary back."
- **Example Use Case:**  
    Uploading chunks of a large file, and the server confirms once the entire file is received.
- **Proto Example:**
    
    ```proto
    service MyService {
        rpc ClientStreamingMethod(stream MyRequest) returns (MyResponse);
    }
    ```
    
    - `stream` here means the client sends multiple requests.

---

### **4. Bidirectional Streaming (Multiple Requests and Responses)**

- **What it is:**  
    Both the client and server send streams of messages to each other. Communication happens in **real-time** and can be interleaved.
    - Think of it as: "Talk back and forth continuously."
- **Example Use Case:**  
    Real-time chat applications, where both sides send and receive messages.
- **Proto Example:**
    
    ```proto
    service MyService {
        rpc BidirectionalStreamingMethod(stream MyRequest) returns (stream MyResponse);
    }
    ```
    
    - Both the request and response are streams.

---

### **Key Differences at a Glance**

|Pattern|Client Sends|Server Sends|Example Use Case|
|---|---|---|---|
|**Unary**|1 request|1 response|Fetch user profile|
|**Server Streaming**|1 request|Stream of responses|Streaming logs or real-time updates|
|**Client Streaming**|Stream of requests|1 response|Uploading a large file|
|**Bidirectional Streaming**|Stream of requests|Stream of responses|Real-time chat or video streaming|

---

### **Why Use Different Patterns?**

Each pattern fits a specific need:

- **Unary:** Simple, one-shot operations.
- **Server Streaming:** When the client needs to process data over time as it arrives.
- **Client Streaming:** When the client collects data in chunks and sends it all at once.
- **Bidirectional Streaming:** Real-time, continuous interactions.
