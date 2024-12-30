### **1. Transport Layer Security (TLS)**

- **TLS** is the foundation for secure communication in gRPC.
- It encrypts data during transmission, ensuring confidentiality and integrity.
- **Mutual TLS (mTLS):**
    - Both the client and server present certificates to authenticate each other.
    - Adds a layer of authentication by validating both parties.

**Example in Go (mTLS):**

```go
import (
	"crypto/tls"
	"crypto/x509"
	"io/ioutil"
	"log"

	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials"
)

func main() {
	cert, err := tls.LoadX509KeyPair("server-cert.pem", "server-key.pem")
	if err != nil {
		log.Fatalf("Failed to load key pair: %v", err)
	}

	certPool := x509.NewCertPool()
	caCert, err := ioutil.ReadFile("ca-cert.pem")
	if err != nil {
		log.Fatalf("Failed to read CA certificate: %v", err)
	}
	certPool.AppendCertsFromPEM(caCert)

	creds := credentials.NewTLS(&tls.Config{
		Certificates: []tls.Certificate{cert},
		ClientCAs:    certPool,
		ClientAuth:   tls.RequireAndVerifyClientCert,
	})

	server := grpc.NewServer(grpc.Creds(creds))
	log.Println("Server started with mTLS...")
}
```

---
### **2. Token-based Authentication**

- Tokens, such as **JWT (JSON Web Token)** or API keys, are passed in gRPC metadata.
- The server validates these tokens to authenticate the client.

**Example in Go:**

```go
import (
	"context"
	"errors"
	"google.golang.org/grpc"
	"google.golang.org/grpc/metadata"
)

func authInterceptor(ctx context.Context) (context.Context, error) {
	md, ok := metadata.FromIncomingContext(ctx)
	if !ok {
		return nil, errors.New("missing metadata")
	}

	// Extract token
	token := md["authorization"]
	if len(token) == 0 || !validateToken(token[0]) {
		return nil, errors.New("invalid token")
	}

	return ctx, nil
}

func validateToken(token string) bool {
	// Implement token validation logic here
	return token == "valid-token"
}
```

In the example:

- Clients send tokens via metadata (`authorization` key).
- The server validates tokens before granting access.

---
### **3. OAuth2**

- OAuth2 is commonly used for delegated access using **access tokens**.
- Clients first obtain an access token from an OAuth provider and include it in the gRPC metadata.
- The server verifies the token with the OAuth provider.

**Client Example (Go):**

```go
import (
	"context"
	"google.golang.org/grpc"
	"google.golang.org/grpc/metadata"
)

func main() {
	conn, _ := grpc.Dial("server:50051", grpc.WithInsecure())
	defer conn.Close()

	client := NewMyServiceClient(conn)
	ctx := metadata.AppendToOutgoingContext(context.Background(), "authorization", "Bearer <access_token>")
	_, _ = client.MyMethod(ctx, &MyRequest{})
}
```

---
### **4. gRPC Interceptors for Authentication and Authorization**

- **Interceptors** act as middleware for pre-processing requests.
- You can implement authentication checks at a centralized point.

**Server-side Interceptor (Go):**

```go
import (
	"context"
	"log"

	"google.golang.org/grpc"
)

func authInterceptor(
	ctx context.Context,
	req interface{},
	info *grpc.UnaryServerInfo,
	handler grpc.UnaryHandler,
) (interface{}, error) {
	// Check metadata for token
	md, ok := metadata.FromIncomingContext(ctx)
	if !ok {
		return nil, grpc.Errorf(codes.Unauthenticated, "missing metadata")
	}

	// Validate token
	token := md["authorization"]
	if len(token) == 0 || !validateToken(token[0]) {
		return nil, grpc.Errorf(codes.Unauthenticated, "invalid token")
	}

	// Continue to the handler
	return handler(ctx, req)
}

func main() {
	server := grpc.NewServer(grpc.UnaryInterceptor(authInterceptor))
	log.Println("Server with auth interceptor running...")
}
```

---
### **5. Role-based Authorization**

- After authentication, implement role-based checks to determine what the client can access.

**Example Authorization Check:**

```go
func authAndRoleCheck(ctx context.Context, requiredRole string) error {
	md, ok := metadata.FromIncomingContext(ctx)
	if !ok {
		return errors.New("missing metadata")
	}

	token := md["authorization"]
	if !validateTokenAndRole(token[0], requiredRole) {
		return errors.New("access denied")
	}
	return nil
}

func validateTokenAndRole(token string, role string) bool {
	// Example: Decode JWT and check claims for the required role
	claims := decodeJWT(token)
	return claims.Role == role
}
```

---
### **6. Audit Logging**

- Use interceptors to log requests for auditing and debugging.

**Example Logging Interceptor:**

```go
func loggingInterceptor(
	ctx context.Context,
	req interface{},
	info *grpc.UnaryServerInfo,
	handler grpc.UnaryHandler,
) (interface{}, error) {
	log.Printf("Request: %v, Method: %s", req, info.FullMethod)
	return handler(ctx, req)
}
```

---

### **Best Practices**

1. **Combine Authentication & Authorization:** Authenticate clients first, then enforce access controls based on roles or permissions.
2. **Use Secure Protocols:** Always enable TLS or mTLS to secure data transmission.
3. **Token Expiry & Rotation:** Ensure tokens have a short lifespan and support rotation mechanisms.
4. **Centralize Logic:** Implement authentication/authorization using interceptors or middleware to reduce duplication.