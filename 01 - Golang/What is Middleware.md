Middleware is a layer in software that intercepts and processes requests and responses in a system. It is commonly used in web development to implement reusable components that perform tasks such as logging, authentication, input validation, or error handling.

In the context of web applications, middleware sits between the client and the server, processing HTTP requests before they reach the final handler and responses before they are sent back to the client.

---

### Features of Middleware:

1. **Pre-processing and Post-processing**: Middleware can act on both incoming requests and outgoing responses.
2. **Reusable**: Common tasks like logging, authentication, or rate limiting can be modularized.
3. **Chaining**: Middleware functions can be chained together, where the output of one becomes the input of another.

---

### Middleware in Go

In Go, middleware is typically implemented as a function that takes an `http.Handler` as an argument and returns a new `http.Handler`. This allows chaining of middleware functions.

---

### Basic Middleware Implementation in Go

#### Example: Logging Middleware

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

// Middleware function
func loggingMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()
		log.Printf("Started %s %s", r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
		log.Printf("Completed in %v", time.Since(start))
	})
}

// Final handler
func helloHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Hello, World!")
}

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", helloHandler)

	// Wrap the mux with middleware
	loggedMux := loggingMiddleware(mux)

	// Start the server
	http.ListenAndServe(":8080", loggedMux)
}
```

---

### Explanation:

1. **Middleware Function (`loggingMiddleware`)**:
    - Takes an `http.Handler` (`next`) as input.
    - Wraps it with additional logic (logging in this case).
    - Calls `next.ServeHTTP(w, r)` to pass the request to the next handler in the chain.
2. **Chaining**:
    - Multiple middleware functions can be chained by wrapping handlers iteratively.

---

#### Example: Authentication Middleware

```go
func authMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		if r.Header.Get("Authorization") != "Bearer token123" {
			http.Error(w, "Forbidden", http.StatusForbidden)
			return
		}
		next.ServeHTTP(w, r)
	})
}

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", helloHandler)

	// Chain middleware
	securedMux := authMiddleware(loggingMiddleware(mux))

	http.ListenAndServe(":8080", securedMux)
}
```

---

### Chaining Middleware:

Middleware can be chained together for complex use cases. For example:

```go
func chainMiddleware(h http.Handler, middlewares ...func(http.Handler) http.Handler) http.Handler {
	for _, middleware := range middlewares {
		h = middleware(h)
	}
	return h
}

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", helloHandler)

	// Apply multiple middlewares
	combined := chainMiddleware(mux, loggingMiddleware, authMiddleware)

	http.ListenAndServe(":8080", combined)
}
```

---

### Summary:

Middleware in Go is implemented by wrapping `http.Handler` with additional logic to perform pre- and post-processing. This approach makes middleware reusable and composable, enabling clean separation of concerns in web applications.