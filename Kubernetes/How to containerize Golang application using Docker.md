Containerizing a Go application using Docker is a straightforward process. Here's a basic guide to get you started:

1. **Install Docker**: Ensure Docker is installed on your system. You can download and install Docker from the official website: https://www.docker.com/get-started

2. **Write Your Go Application**: Develop your Go application. Let's assume you have a simple Go application with a `main.go` file.

3. **Create a Dockerfile**: Create a file named `Dockerfile` in the root directory of your Go project. This file will contain instructions for Docker on how to build your container image.

   ```Dockerfile
   # Start from the official golang base image
   FROM golang:latest
   
   # Set the Current Working Directory inside the container
   WORKDIR /app
   
   # Copy go mod and sum files
   COPY go.mod go.sum ./
   
   # Download all dependencies. Dependencies will be cached if the go.mod and go.sum files are not changed
   RUN go mod download
   
   # Copy the source code into the container
   COPY . .
   
   # Build the Go app
   RUN go build -o main .
   
   # Expose port 8080 to the outside world
   EXPOSE 8080
   
   # Command to run the executable
   CMD ["./main"]
   ```

   Adjust the `EXPOSE` directive if your Go application listens on a different port.

4. **Build the Docker Image**: Open a terminal, navigate to the directory containing your `Dockerfile`, and run the following command to build your Docker image:

   ```
   docker build -t your_image_name .
   ```

   Replace `your_image_name` with a name for your Docker image.

5. **Run the Docker Container**: Once the Docker image is built, you can run it using the following command:

   ```
   docker run -p 8080:8080 your_image_name
   ```

   This command will run your Go application inside a Docker container, exposing port 8080 on your local machine.

That's it! Your Go application is now containerized using Docker. You can distribute this Docker image and run your application on any system that supports Docker.