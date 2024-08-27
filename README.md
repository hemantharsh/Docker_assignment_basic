Here's a step-by-step guide for documenting your Docker and Docker Compose assignment:

---

### **1. Install Docker and Docker Compose**

**Docker Installation:**

1. **Update Your Package Index**:  
   ```
   sudo apt-get update
   ```

2. **Install Necessary Packages**:  
   ```
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Add Docker’s Official GPG Key**:  
   ```
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. **Set up the Stable Repository**:  
   ```
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. **Install Docker CE (Community Edition)**:  
   ```
   sudo apt-get update
   sudo apt-get install docker-ce
   ```

6. **Verify Docker Installation**:  
   ```
   docker --version
   ```

**Docker Compose Installation:**

1. **Download the Latest Version of Docker Compose**:  
   ```
   sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -Po '"tag_name": "\K[^\"]*')" /docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Apply Executable Permissions**:  
   ```
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Verify Docker Compose Installation**:  
   ```
   docker-compose --version
   ```

### **2. Verify the Installation by Running a Test Container**

1. **Run a Test Container**:  
   ```
   sudo docker run hello-world
   ```
   - This command pulls the "hello-world" image from Docker Hub and runs it in a container. If Docker is installed correctly, you'll see a message confirming the successful execution of the container.

### **3. Create a Simple Web Application**

Choose a simple web application for demonstration. Here is a Python Flask example:

1. **Create a Directory for the Application**:  
   ```
   mkdir my-web-app
   cd my-web-app
   ```

2. **Create a Simple Flask Application**:  
   - **`app.py`**:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello, Docker!'

   if __name__ == '__main__':
       app.run(host='0.0.0.0')
   ```

3. **Create a `requirements.txt` File**:  
   ```
   Flask==2.1.2
   ```

### **4. Write a Dockerfile to Containerize the Application**

1. **Create a `Dockerfile`**:  
   ```Dockerfile
   # Use an official Python runtime as a parent image
   FROM python:3.8-slim

   # Set the working directory in the container
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY . /app

   # Install any needed packages specified in requirements.txt
   RUN pip install --no-cache-dir -r requirements.txt

   # Make port 5001 available to the world outside this container
   EXPOSE 5001

   # Define environment variable
   ENV NAME World

   # Run app.py when the container launches
   CMD ["python", "app.py"]
   ```

### **5. Build the Docker Image and Run a Container from It**

1. **Build the Docker Image**:  
   ```
   docker build -t my-flask-app .
   ```

2. **Run the Docker Container**:  
   ```
   docker run -p 5000:5000 my-flask-app
   ```
   - This command maps port 5000 on the host to port 5000 in the container.

### **6. Use Docker Commands to Manage Containers**

1. **List Running Containers**:  
   ```
   docker ps
   ```

2. **Start a Container**:  
   ```
   docker start <container_id>
   ```

3. **Stop a Container**:  
   ```
   docker stop <container_id>
   ```

4. **Remove a Container**:  
   ```
   docker rm <container_id>
   ```

### **7. Inspect Running Containers and View Logs**

1. **Inspect a Container**:  
   ```
   docker inspect <container_id>
   ```

2. **View Container Logs**:  
   ```
   docker logs <container_id>
   ```

### **8. Write a `docker-compose.yml` File for a Multi-Container Application**

1. **Create a `docker-compose.yml` File**:  
   ```yaml
   version: '3'
   services:
     web:
       build: .
       ports:
         - "5000:5000"
     redis:
       image: "redis:alpine"
   ```
   - This example defines a web service that builds from the current directory and a Redis service using the Redis image.

### **9. Use Docker Compose to Manage the Application**

1. **Bring Up the Application**:  
   ```
   docker compose up
   ```
   - This command builds, (re)creates, starts, and attaches to containers for a service.

2. **Ensure All Services Are Running**:  
   ```
   docker-compose ps
   ```

### **10. Create a Private Docker Registry or Use Docker Hub**

1. **Sign in to Docker Hub**:  
   ```
   docker login
   ```
   - Enter your Docker Hub credentials when prompted.

### **11. Push Your Docker Images to the Registry**

1. **Tag the Image**:  
   ```
   docker tag my-flask-app:latest your_dockerhub_username/my-flask-app:latest
   ```

2. **Push the Image**:  
   ```
   docker push your_dockerhub_username/my-flask-app:latest
   ```

### **12. Pull the Images from the Registry and Run Them Locally**

1. **Pull the Image**:  
   ```
   docker pull your_dockerhub_username/my-flask-app:latest
   ```

2. **Run the Pulled Image**:  
   ```
   docker run -p 5000:5001 your_dockerhub_username/my-flask-app:latest
   ```
