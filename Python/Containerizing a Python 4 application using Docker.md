Containerizing a Python 4 application using Docker is a straightforward process. Here's a step-by-step guide to help you:

1. **Create a Dockerfile**: This file will contain instructions for Docker on how to build your container. In your project directory, create a file named `Dockerfile` (without any file extension).

2. **Specify Base Image**: In the Dockerfile, start by specifying the base image. Since you're working with a Python 4 application, you'll need a base image that supports Python 4. You can use an official Python image and specify the version you need. For example:

    ```Dockerfile
    FROM python:4
    ```

3. **Set Working Directory**: Set the working directory inside the container where your application code will reside:

    ```Dockerfile
    WORKDIR /app
    ```

4. **Copy Application Files**: Copy all your application files (Python scripts, requirements.txt, etc.) into the container:

    ```Dockerfile
    COPY . /app
    ```

5. **Install Dependencies**: If your application has any dependencies listed in a `requirements.txt` file, you can install them using pip:

    ```Dockerfile
    RUN pip install -r requirements.txt
    ```

6. **Expose Ports (if necessary)**: If your application listens on a specific port, you can expose it:

    ```Dockerfile
    EXPOSE 8000
    ```

    Replace `8000` with the port number your application uses.

7. **Command to Run Application**: Finally, specify the command to run your Python application:

    ```Dockerfile
    CMD ["python", "your_main_script.py"]
    ```

    Replace `"your_main_script.py"` with the entry point of your application.

8. **Build the Docker Image**: Once you have the Dockerfile ready, navigate to your project directory in the terminal and run the following command to build the Docker image:

    ```
    docker build -t my_python_app .
    ```

    Replace `my_python_app` with your desired image name.

9. **Run the Docker Container**: After the image is built successfully, you can run a container using the following command:

    ```
    docker run -d -p 8000:8000 my_python_app
    ```

    Replace `8000:8000` with the appropriate port mapping if needed.

That's it! Your Python 4 application should now be containerized using Docker.