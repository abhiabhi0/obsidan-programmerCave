#### Structure

1. Defining the MVP
2. Estimation of Scale
3. Design Goals
4. API and Design

---

### 1. Minimum Viable Product (MVP)

- **Send a message to the recipient**
- **Realtime chat**
- **Message history**
- **Most recent conversations**

---

### 2. Estimation of Scale

- Starting scale: **20 billion messages/day**.
- Each message size: **200 bytes or less**.
- Daily storage: **4TB/day**.
- Yearly storage: **>1PB/year** (for 1 year). For 5 years, **multiple PB**.
- Conclusion: **Sharding is necessary**.
- System type: **Both read-heavy and write-heavy**.

---

### 3. Design Goals

1. **High Consistency**: Communication must remain consistent.
2. **Low Latency**: For real-time responsiveness.

---

### 4. API + Design

#### **Idempotency**

- Essential due to intermittent mobile network connections.
- Writes must not create duplicate messages due to retries (application or network-level).

**How to achieve idempotency:**

- Use a unique **messageId** for each message.
    - Generated using:
        - Timestamp (date and time)
        - SenderID
        - DeviceID
        - RecipientID (for broadcast differentiation)
- Deduplication: Backend checks if a message with the same messageId already exists before storing.

---

#### **APIs**

1. **sendMessage(sender, recipient, text, messageId)**
    - Sends a message from sender to recipient.
    
1. **getMessages(userId, conversationId, offset, limit)**
    - Fetches paginated messages for a specific conversation.
    - Offset: Starting point for messages.
    - Limit: Number of messages to retrieve.
    
1. **getConversations(userId, offset, limit)**
    - Fetches recent conversations (name, snippet, last message timestamp).
    
1. **createUser(...)**
    - Creates a new user.

---

### System Design

#### Problem #1: Sharding Strategy

1. **UserID-Based Sharding**
    - Each user is assigned to a specific machine ("mailbox").
    
    **Operations:**
    - **getConversations**: Query the user’s shard for recent conversations.
    - **getMessages**: Query the user’s shard for messages in a conversation.
    - **sendMessage**: Requires **two writes**: one in the sender’s shard and one in the recipient’s shard (ensuring consistency is complex).
    
1. **ConversationID-Based Sharding**
    - Each conversation is assigned to a specific machine.
    
    **Operations:**
    - **getMessages**: Efficient as all messages for a conversation are on the same machine.
    - **sendMessage**: Write to the machine hosting the conversation.
    - **getConversations**: Inefficient as it requires searching across multiple machines.
    
    **Solution for getConversations:**
    - Use a **secondary database** to store user-to-conversations mapping (sorted by recency).

**Comparison:**
- **UserID Sharding:** Better for 1:1 chats (2 writes).
- **ConversationID Sharding:** Better for group chats (avoids excessive writes in large groups).

---

#### Problem #2: sendMessage Consistency

Consistency definition: If a user sends a message and doesn’t receive an error, the recipient must get the message.

**Approaches:**

1. **Write to Sender First**
    - If write to sender shard fails, return an error.
    - If write to recipient shard fails after sender succeeds:
        - Rollback the sender’s write (complex).
        
1. **Write to Recipient First** (Preferred)
    - If write to recipient shard fails, return an error.
    - If write to sender shard fails after recipient succeeds:
        - Add to a **retry queue** to ensure eventual consistency.

---

#### Problem #3: Choosing the Right DB / Cache

1. **Storage Type**
    - Needs to handle **both read-heavy and write-heavy** workloads.
    - Prioritize write consistency; reduce read load through caching.
    
1. **Caching Strategy**
    - Use **write-through cache** for consistency.
    - Shard cache to handle large datasets.
    - Address write concurrency using **appservers as cache**:
        - Assign users to appservers using consistent hashing.
        - Appservers act as a cache and lock writes to prevent race conditions.
        - Implement LRU caching for users.
    
    **Pros:**
    - Horizontal scalability.
    - Graceful handling of consistency.
    
    **Cons:**
    - Unavailability during appserver reassignment.
    - Cold cache start delays.
    
1. **Database**
    - Choose a NoSQL database optimized for **write-heavy workloads** and **high consistency**.
    - Example: **HBase**
        - Supports column-family structure (ideal for mailbox/message storage).
        - Optimized for high write volumes.
https://docs.google.com/document/d/1FE10Pu4nd6sz89RHq82rkcO2fdNoOZA0jQgO8pLgsOc/edit?tab=t.0