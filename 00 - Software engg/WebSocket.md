### What are WebSockets?
- Communication protocol providing full-duplex communication channels over a single TCP connection.
- Enables real-time, event-driven connection between client and server.
- Allows bi-directional communication without continuous polling.
- Client and server can send data to each other anytime once the connection is established.

### Traditional HTTP vs. WebSockets
- **HTTP:**
  - Follows a request-response model.
  - Client sends requests; server responds with requested data.
  - Requires continuous polling for updates, increasing latency and reducing efficiency.
- **WebSockets:**
  - Establish a persistent connection.
  - Allow instant data transmission between client and server.
  - Supports real-time communication with minimal latency.

### HTTP Connections vs. WebSockets
- **HTTP:**
  - Application layer protocol for web-based communication.
  - Operates on a request-response cycle.
  - Has versions HTTP/1.1, HTTP/2, and secure HTTPS.
  - Suitable for non-time-sensitive information.
- **WebSockets:**
  - More efficient for real-time applications.
  - Avoids the inefficiency of opening new HTTP connections for each request.
  - Supports bi-directional communication, unlike HTTP’s client-initiated communication model.

### Short Polling vs. WebSockets
- **Short Polling:**
  - Client repeatedly sends requests to the server until it updates.
  - Supported by modern web browsers using XMLHttpRequest.
  - Inefficient due to resending non-payload data for each request.

### Long Polling vs. WebSockets
- **Long Polling:**
  - Client polls the server and keeps the connection open until new data is available.
  - Connection remains open for up to 280 seconds before sending another request.
  - Emulates an HTTP server push.
  - Used for fast communication but requires continuous server resources.
- **WebSockets:**
  - True push-based method for real-time communication.
  - More efficient than long polling for real-time data updates.

### How do WebSockets Work (and Their Connections)
- **Establishing Connection:**
  - WebSockets use the TCP (Transport Control Protocol) layer to establish a connection.
  - WebSockets act as a transport layer over the TCP connection.

- **Switching from HTTP to WebSockets:**
  - Connection starts with an HTTP request/response pair.
  - Clients use an HTTP/1.1 upgrade header to switch the connection from HTTP to WebSockets.
  - WebSocket connections are fully asynchronous, unlike HTTP/1.1.

- **WebSocket Handshake:**
  - Connection is established through a WebSocket handshake over TCP.
  - During the handshake, the client and server agree on which subprotocol to use for interactions.
  - After the handshake, the connection operates on the WebSocket protocol.

- **URI Scheme:**
  - WebSockets require a uniform resource identifier (URI) with a “ws:” or “wss:” scheme.
  - This is similar to how HTTP URLs use “http:” or “https:” schemes.

### Drawbacks of WebSockets
- **Browser Support:**
  - Limited support in older browsers.
  - Requires fallback mechanisms for unsupported browsers.

- **Proxy and Firewall Limitations:**
  - Some proxy servers and firewalls may block or interfere with WebSocket connections.
  - Can cause connectivity issues, especially in secured corporate or restricted network environments.

- **Scalability:**
  - Persistent connections can strain server resources with many concurrent connections.
  - Requires proper load balancing and resource management for scalability.
  - Open-source resources like Socket.io are not ideal for large-scale operations.

- **Stateful Nature:**
  - Servers need to maintain the connection state for each client.
  - Leads to increased memory usage and potential scalability challenges.

- **Security Considerations:**
  - Persistent connections require proper security measures to prevent vulnerabilities (e.g., XSS, CSRF).
  - Secure WebSocket connections (wss://) using SSL/TLS encryption are necessary for data privacy and integrity.

- **Connection Loss:**
  - No built-in load balancing or reconnecting mechanisms.
  - Requires fallback options like HTTP streaming or long polling in environments where WebSockets may not be supported.

- **Presence Detection:**
  - Features like Presence do not work well because disconnections are hard to detect.