
Containerizing a Go application using Docker involves several steps, including writing a Dockerfile, building the Docker image, and running the container. Here's a step-by-step guide to containerizing a Go application:

### Step 1: Write Your Go Application
Ensure you have a simple Go application. For example, let's create a simple web server:

```go
// main.go
package main

import (
    "fmt"
    "net/http"
)

func hello(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/", hello)
    http.ListenAndServe(":8080", nil)
}
```

### Step 2: Create a Dockerfile
A Dockerfile is a script that contains a series of instructions on how to build a Docker image for your application. Here's a sample Dockerfile for the Go web server:

```Dockerfile
# Use the official Golang image as a build stage
FROM golang:1.19 AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the application source code
COPY . .

# Build the Go application
RUN go build -o main .

# Use a minimal base image
FROM alpine:latest

# Install ca-certificates (required for HTTPS)
RUN apk --no-cache add ca-certificates

# Set the working directory inside the container
WORKDIR /root/

# Copy the binary from the builder stage
COPY --from=builder /app/main .

# Expose the port on which the application will run
EXPOSE 8080

# Command to run the binary
CMD ["./main"]
```

### Step 3: Build the Docker Image
In the directory containing your `Dockerfile` and Go application, run the following command to build the Docker image:

```sh
docker build -t my-go-app .
```

This command builds the Docker image and tags it as `my-go-app`.

### Step 4: Run the Docker Container
After building the Docker image, you can run it in a container using the following command:

```sh
docker run -p 8080:8080 my-go-app
```

This command maps port 8080 on your host to port 8080 in the container, allowing you to access your Go application via `http://localhost:8080`.

### Step 5: Verify the Application
Open your web browser or use `curl` to verify that the application is running:

```sh
curl http://localhost:8080
```

You should see the output `Hello, World!`.

### Full Example
For completeness, here’s the full directory structure and files:

```
my-go-app/
│
├── Dockerfile
├── go.mod
├── go.sum
└── main.go
```

### Example go.mod (if you use modules)
To manage dependencies, you might need a `go.mod` file. If you don't already have one, create it by running:

```sh
go mod init my-go-app
```

Then, ensure your `go.mod` and `go.sum` files are included in the Docker build context by copying them in the Dockerfile as shown above.

### Conclusion
You have now successfully containerized a Go application using Docker. This process involves creating a Dockerfile with appropriate instructions, building the Docker image, and running the container. This method ensures that your Go application runs consistently across different environments.