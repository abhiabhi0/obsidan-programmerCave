gRPC stubs are essential components in the gRPC framework that facilitate communication between clients and servers. They act as local proxies for remote procedure calls (RPCs), allowing clients to invoke methods on a server as if they were local function calls. Here’s a detailed explanation of gRPC stubs based on the information from the search results.

### What Are gRPC Stubs?

- **Definition**: A gRPC stub is an auto-generated client-side code that mirrors the methods defined in a gRPC service. It abstracts the complexities of network communication, allowing developers to invoke remote methods easily without dealing with the underlying details of serialization, deserialization, and network protocols.

- **Functionality**: When a client makes an RPC call using a stub, it sends the request to the gRPC server, which processes it and returns a response. The stub handles the serialization of request parameters into a binary format using Protocol Buffers (Protobuf) and deserializes the response back into a usable format for the client.

### Key Characteristics of gRPC Stubs

1. **Auto-Generated Code**: Stubs are generated from `.proto` files using the Protobuf compiler (`protoc`). This process creates client-side code that includes all RPC methods defined in the service, ensuring type safety and consistency.

2. **Types of Stubs**:
   - **Blocking Stub**: Provides synchronous method calls. The client waits for the server to respond before continuing execution.
   - **Non-blocking (Asynchronous) Stub**: Allows for asynchronous method calls, enabling clients to continue processing while waiting for responses.
   - **Future Stub**: Wraps asynchronous calls in a future object, allowing clients to handle responses when they become available without blocking.

3. **Channel Management**: Each stub wraps around a `Channel`, which represents a connection to the gRPC server. The stub uses this channel to send RPCs and receive responses.

4. **Method Signatures**: Each method in the stub corresponds to an RPC defined in the service. For example, if there is a `SayHello` method in the service, there will be a corresponding `SayHello` method in the stub that takes request parameters and returns response types.

### Example of gRPC Stubs in Action

Here’s how you might see gRPC stubs used in practice:

#### Step 1: Define Your Service in a `.proto` File

```protobuf
syntax = "proto3";

package hello;

service Greeter {
    rpc SayHello(HelloRequest) returns (HelloReply);
}

message HelloRequest {
    string name = 1;
}

message HelloReply {
    string message = 1;
}
```

#### Step 2: Generate Go Code from the Proto File

Run this command to generate client and server code:

```bash
protoc --go_out=. --go-grpc_out=. hello.proto
```

#### Step 3: Use the Generated Stub in Your Client Application

Here’s an example of how you might use the generated stub in a Go client:

```go
package main

import (
    "context"
    "log"
    "time"

    "google.golang.org/grpc"
    pb "path/to/your/proto/package" // Update with your actual package path
)

func main() {
    // Establish connection to the server
    conn, err := grpc.Dial("localhost:50051", grpc.WithInsecure(), grpc.WithBlock(), grpc.WithTimeout(5*time.Second))
    if err != nil {
        log.Fatalf("did not connect: %v", err)
    }
    defer conn.Close()

    // Create a new Greeter client using the generated stub
    client := pb.NewGreeterClient(conn)

    // Prepare request
    req := &pb.HelloRequest{Name: "World"}

    // Call SayHello method using the stub
    resp, err := client.SayHello(context.Background(), req)
    if err != nil {
        log.Fatalf("could not greet: %v", err)
    }

    log.Printf("Greeting: %s", resp.GetMessage())
}
```

### Conclusion

gRPC stubs are crucial for simplifying remote procedure calls by providing an easy-to-use interface for invoking methods on remote servers. They handle all necessary serialization and network communication tasks, allowing developers to focus on implementing business logic rather than dealing with low-level networking details. By leveraging auto-generated stubs from Protobuf definitions, developers can ensure consistency and type safety across their applications.

Citations:
[1] https://documentation.softwareag.com/webmethods/designer/sdf10-11/10-11_sdf-webhelp/sdf-webhelp/esb.grpc.html
[2] https://grpc.io/docs/languages/java/generated-code/
[3] https://en.wikipedia.org/wiki/Stub_(distributed_computing)
[4] https://grpc.github.io/grpc-java/javadoc/io/grpc/stub/package-summary.html
[5] https://www.wallarm.com/what/the-concept-of-grpc