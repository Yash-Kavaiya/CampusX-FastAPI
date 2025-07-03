# FastAPI + Docker Tutorial for Beginners | How to Dockerize a FastAPI API Application | CampusX 

## Project Overview

### What is this project?
This is a Machine Learning API built with **FastAPI** that predicts insurance premium categories (High, Medium, Low) based on user details such as:
- Age
- Weight and Height (BMI calculation)
- Income
- Smoking status
- City
- Occupation

### Previous Development Steps:
1. **ML Model Creation**: Built an insurance premium prediction model
2. **FastAPI Development**: Created a basic prediction API with `/predict` endpoint
3. **API Improvement**: Enhanced the basic API with industry-grade best practices
4. **Dockerization**: Current step - containerizing the application


## Prerequisites

### Required Knowledge:
- **Docker Fundamentals** (Images, Containers, Dockerfile)
- **FastAPI Basics**
- **Command Line Interface**
- **Basic Python/ML concepts**

Docker for Machine Learning | Docker Crash Course | CampusX
https://youtu.be/GToyQTGDOS4?si=4B_GQuJ3hyYZQWxQ

### System Requirements:
- Docker Desktop installed
- Active internet connection
- Minimum 4GB RAM (Docker can be resource-intensive)

![image](https://github.com/user-attachments/assets/083fda7b-70ff-4fc3-8545-66961191c34d)

## Setup Requirements

### 1. Install Docker Desktop

**Steps:**
1. Go to [docker.com](https://www.docker.com/)
2. Download Docker Desktop for your operating system:
   - **Windows**: Docker Desktop for Windows
   - **macOS**: Docker Desktop for Mac
   - **Linux**: Docker Engine
3. Install using the standard installation process (Next → Next → Finish)
4. Start Docker Desktop to initialize Docker Engine

**Verification:**
```bash
docker --version
docker info
```

### 2. Create Docker Hub Account

**Steps:**
1. Visit [hub.docker.com](https://hub.docker.com/)
2. Sign up for a free account
3. Verify your email
4. Remember your username (you'll need it for image naming)

**Important:** Your Docker Hub username becomes part of your image name format: `username/repository-name`


## Step-by-Step Dockerization Process

### Step 1: Create a Dockerfile

**Location:** Place in your project root directory  
**Name:** `Dockerfile` (exactly, with capital D, no extension)

```dockerfile
# Use Python 3.11 base image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy rest of application code
COPY . .

# Expose the application port
EXPOSE 8000

# Command to start FastAPI application
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Step 2: Build the Docker Image

**Command Structure:**
```bash
docker build -t [dockerhub-username]/[repository-name] .
```

**Example:**
```bash
docker build -t tweakster24/insurance-premium-api .
```

**Process Explanation:**
- Docker reads the Dockerfile
- Creates layers step by step
- Each instruction creates a new layer
- First-time build takes longer (downloads base image and installs packages)
- Subsequent builds use cached layers for faster execution

**Expected Output:**
```
Step 1/6 : FROM python:3.11-slim
Step 2/6 : WORKDIR /app
Step 3/6 : COPY requirements.txt .
Step 4/6 : RUN pip install --no-cache-dir -r requirements.txt
Step 5/6 : COPY . .
Step 6/6 : CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
Successfully built [image-id]
Successfully tagged tweakster24/insurance-premium-api:latest
```

### Step 3: Login to Docker Hub

**Command:**
```bash
docker login
```

**Process:**
1. Enter your Docker Hub username
2. Enter your Docker Hub password
3. Successful authentication stores credentials locally

### Step 4: Push Image to Docker Hub

**Command:**
```bash
docker push [dockerhub-username]/[repository-name]
```

**Example:**
```bash
docker push tweakster24/insurance-premium-api
```

**Process:**
- Compresses image layers
- Uploads to Docker Hub
- Creates public repository (by default)
- Can take time depending on image size (~1GB typical for ML apps)

### Step 5: Pull and Test the Image

**Simulate Tester Perspective:**

1. **Remove local image** (to simulate fresh download):
```bash
docker rmi tweakster24/insurance-premium-api
```

2. **Pull from Docker Hub**:
```bash
docker pull tweakster24/insurance-premium-api
```

3. **Run the container**:
```bash
docker run -d -p 8000:8000 tweakster24/insurance-premium-api
```

### Step 6: Test the Application

**Access Points:**
- **Home API**: `http://localhost:8000/`
- **Health Check**: `http://localhost:8000/health`
- **API Documentation**: `http://localhost:8000/docs`
- **Prediction Endpoint**: `http://localhost:8000/predict`

**Sample Test Data:**
```json
{
  "age": 31,
  "weight": 91,
  "height": 1.72,
  "income": 10,
  "smoker": true,
  "city": "Gurgaon",
  "occupation": "Retired"
}
```

## Dockerfile Explained

### Layer-by-Layer Breakdown:

```dockerfile
# Base Image Selection
FROM python:3.11-slim
```
**Purpose:** 
- Provides Ubuntu-based OS with Python 3.11 pre-installed
- `slim` variant reduces image size
- Contains essential Python libraries and pip

```dockerfile
# Working Directory Setup
WORKDIR /app
```
**Purpose:**
- Sets `/app` as the default directory for subsequent commands
- All copied files and executed commands happen here
- Organizes container filesystem

```dockerfile
# Dependency Installation
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
```
**Purpose:**
- Copies only requirements.txt first (optimization for Docker layer caching)
- Installs Python packages
- `--no-cache-dir` reduces image size by not storing pip cache

```dockerfile
# Application Code Copy
COPY . .
```
**Purpose:**
- Copies all remaining application files
- Includes: app.py, model files, other Python scripts
- Done after dependency installation for better caching

```dockerfile
# Port Exposure
EXPOSE 8000
```
**Purpose:**
- Documents which port the application uses
- Doesn't actually publish the port (done during `docker run`)
- FastAPI default port for uvicorn

```dockerfile
# Application Startup
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```
**Purpose:**
- Defines the default command to run when container starts
- `--host 0.0.0.0` allows external connections (crucial for containers)
- Starts FastAPI application using uvicorn server

---

## Testing the Dockerized Application

### Using Docker Desktop GUI:
1. Open Docker Desktop
2. Navigate to **Images** tab
3. Find your image
4. Click **Run** button
5. Configure port mapping: `8000:8000`
6. Start container

### Using Command Line:
```bash
# Run container in background
docker run -d -p 8000:8000 tweakster24/insurance-premium-api

# Run container interactively (see logs)
docker run -p 8000:8000 tweakster24/insurance-premium-api

# Run with custom name
docker run -d -p 8000:8000 --name insurance-api tweakster24/insurance-premium-api
```

### API Testing:
1. **Browser Testing**: Visit `http://localhost:8000/docs`
2. **curl Testing**:
```bash
curl -X POST "http://localhost:8000/predict" \
     -H "Content-Type: application/json" \
     -d '{
       "age": 31,
       "weight": 91,
       "height": 1.72,
       "income": 10,
       "smoker": true,
       "city": "Gurgaon",
       "occupation": "Retired"
     }'
```

---

## Best Practices

### Dockerfile Optimization:
1. **Use specific base image versions**: `python:3.11-slim` instead of `python:latest`
2. **Layer caching**: Copy requirements.txt before application code
3. **Minimize image size**: Use slim images, multi-stage builds
4. **Security**: Don't run as root user in production

### Image Management:
1. **Tagging**: Use semantic versioning (`v1.0.0`, `v1.1.0`)
2. **Documentation**: Include README in repository
3. **Environment Variables**: Use for configuration instead of hardcoding

### Production Considerations:
1. **Health Checks**: Implement proper health check endpoints
2. **Logging**: Configure proper logging for containers
3. **Secrets Management**: Don't include sensitive data in images
4. **Resource Limits**: Set memory and CPU limits

---

## Troubleshooting

### Common Issues:

#### 1. Docker Build Fails
**Problem**: Package installation errors
**Solution**: 
- Check requirements.txt format
- Ensure base image has required system packages
- Use `--no-cache` flag to force fresh build

#### 2. Container Won't Start
**Problem**: Application crashes on startup
**Solution**:
- Check container logs: `docker logs [container-name]`
- Verify all files are copied correctly
- Test application locally first

#### 3. Can't Access Application
**Problem**: `localhost:8000` not responding
**Solution**:
- Verify port mapping: `-p 8000:8000`
- Check if container is running: `docker ps`
- Ensure `--host 0.0.0.0` in CMD instruction

#### 4. Push to Docker Hub Fails
**Problem**: Authentication or permission errors
**Solution**:
- Re-run `docker login`
- Check repository name format: `username/repo-name`
- Verify Docker Hub account status

### Useful Commands:

```bash
# View running containers
docker ps

# View all containers (including stopped)
docker ps -a

# View container logs
docker logs [container-name]

# Stop container
docker stop [container-name]

# Remove container
docker rm [container-name]

# Remove image
docker rmi [image-name]

# Execute command in running container
docker exec -it [container-name] bash

# View image details
docker inspect [image-name]
```

---

## Next Steps

After successful dockerization:

1. **AWS Deployment**: Deploy container to AWS ECS/EKS/EC2
2. **CI/CD Pipeline**: Automate build and deployment
3. **Monitoring**: Implement application monitoring
4. **Scaling**: Configure horizontal scaling
5. **Security**: Implement security scanning and secrets management

---

## Conclusion

Dockerization provides several key benefits:

- **Consistency**: Same environment across development, testing, and production
- **Portability**: Run anywhere Docker is available
- **Isolation**: Dependencies don't conflict with host system
- **Scalability**: Easy to scale horizontally
- **Deployment**: Simplified deployment process

The containerized FastAPI application is now ready for testing by team members and deployment to cloud platforms like AWS, making it production-ready and easily distributable.
