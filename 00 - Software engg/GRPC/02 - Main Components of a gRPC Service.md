1. **Service Definition**:
   - The foundation of a gRPC service is defined using Protocol Buffers (protobuf), which serves as the Interface Definition Language (IDL).
   - It specifies the methods available for remote invocation, along with their input parameters and return types.
   - Example structure in a `.proto` file:
     ```protobuf
     service ExampleService {
         rpc UnaryCall(ExampleRequest) returns (ExampleResponse);
         rpc StreamingFromServer(ExampleRequest) returns (stream ExampleResponse);
     }
     ```

2. **Protocol Buffers**:
   - A language-agnostic mechanism for serializing structured data, enabling efficient communication between clients and servers.
   - Converts defined data structures into a compact binary format, enhancing performance over text-based formats like JSON.

3. **gRPC Server**:
   - Implements the service methods defined in the `.proto` file.
   - Handles incoming requests from clients, processes them, and sends back responses.
   - The server runs a gRPC server instance to manage client connections and method invocations.

4. **Client Stub**:
   - A client-side representation of the gRPC service that allows clients to invoke remote methods as if they were local calls.
   - The stub handles the serialization of requests and deserialization of responses automatically.
   - It communicates with the server using HTTP/2 under the hood.

5. **Channels**:
   - A channel represents a connection to a gRPC server, specifying the host and port.
   - Channels manage multiple simultaneous calls, improving performance by allowing concurrent request handling.

6. **Service Methods**:
   - gRPC supports four types of service methods:
     - **Unary RPC**: Single request-response interaction.
     - **Server Streaming RPC**: Client sends one request and receives a stream of responses.
     - **Client Streaming RPC**: Client sends a stream of requests and receives a single response.
     - **Bi-directional Streaming RPC**: Both client and server can send messages independently.

7. **Metadata**:
   - Allows clients to attach additional information to calls, which can be used for validation or to describe services and methods.

8. **Interceptors**:
   - Middleware components that can be used to intercept calls for purposes such as logging, authentication, or monitoring.

Understanding these components is crucial for effectively designing and implementing gRPC services in your applications.

Citations:
[1] https://konghq.com/blog/learning-center/what-is-grpc
[2] https://www.oreilly.com/library/view/grpc-up-and/9781492058328/ch04.html
[3] https://daily.dev/blog/introduction-to-grpc
[4] https://learn.microsoft.com/en-us/aspnet/core/grpc/services?view=aspnetcore-9.0
[5] https://grpc.io/docs/what-is-grpc/core-concepts/
[6] https://www.wallarm.com/what/the-concept-of-grpc
[7] https://grpc.io/docs/languages/go/basics/