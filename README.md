# Working Dockerized Nginx with HTML Page - Complete Guide

## Overview
This project demonstrates how we can Dockerize a simple HTML page using **Nginx** as the web server and deploy the Docker image to **Amazon Elastic Container Registry (ECR)**.

### Key Features
- A simple HTML page (`index.html`) configured with Nginx reverse proxy server.
- Custom Nginx configuration (`nginx.conf`).
- Dockerfile to build the Docker image.
- Instructions to push the Docker image to Amazon ECR.

---

## Project Structure

```
Docker_assignment/
│
├── index.html         # HTML file with customizations
├── nginx.conf         # Custom Nginx configuration file.
├── Dockerfile         # Dockerfile to define the image.
└── README.md          # Documentation file (this file).
```

---

## Files Description

- **index.html**: A basic HTML file that will be served by the Nginx web server. It contains a simple "Hello, Docker!" message.
  
- **nginx.conf**: Custom Nginx configuration that serves the `index.html` file on port 80.
  
- **Dockerfile**: Defines the steps to create a Docker image that includes Nginx as the web server and serves the HTML page.

---

## Prerequisites

- **Docker** installed on your local machine. [Get Docker](https://www.docker.com/get-started)
- **AWS CLI** installed and configured with your AWS account. [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- **Amazon ECR repository** created in your AWS account.
- **Basic knowledge of Docker and AWS**.

---

## Step-by-Step Instructions

### 1. Clone the Repository
```bash
git clone <your-github-repo-url>
cd Docker_assignment
```

### 2. Create HTML and Configuration Files

1. **index.html**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
      <title>Hello Docker</title>
   </head>
   <body>
      <h1>Hello, Docker!</h1>
      <h2>This is Shubham! I am doing my assignment</h2>
   </body>
   </html>

   ```

2. **nginx.conf**
   ```nginx
   events {
       worker_connections 1024;
   }

   http {
       server {
           listen 80;
           server_name localhost;

           location / {
               root /usr/share/nginx/html;
               index index.html;
           }
       }
   }
   ```

### 3. Create Dockerfile

```Dockerfile
# Use the official Nginx image from Docker Hub
FROM nginx:alpine

# Copy the custom Nginx configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the HTML file to the default Nginx directory
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start the Nginx server
CMD ["nginx", "-g", "daemon off;"]
```

---

### 4. Build the Docker Image

Run the following command to build the Docker image:

```bash
docker build -t shubham-nginx-image:v2 .
```

### 5. Run the Docker Container Locally

You can test the Docker container locally to verify it serves the HTML page:

```bash
docker run -d -p 80:80 shubham-nginx-image:v2
```

- Open `http://localhost` in your web browser, and you should see "Hello, Docker!".

---

### 6. Push Docker Image to Amazon ECR

#### 6.1. Authenticate Docker to Your ECR

Run the following command to log Docker into your ECR registry:

```bash
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com
```

Replace:
- `<your-region>` with AWS region shown in the AWS console(e.g., `us-west-2`).
- `<aws-account-id>` with 12-digit AWS account ID.

#### 6.2. Tag Your Docker Image

Taggin my Docker image for pushing it to my ECR repository:

```bash
docker tag shubham-nginx-image:v1 <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/my-nginx-repo:v1
```

Replacing `my-nginx-repo` with the name of my ECR repository.

#### 6.3. Push the Docker Image to ECR

Finally, push the Docker image to ECR:

```bash
docker push <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/my-nginx-repo:v1
```
Replace:
- `<your-region>` with working AWS region shown in the AWS consold (I have used `us-west-2`).
- `<aws-account-id>` with the 12-digit AWS account ID.
---

### 7. View the Docker Image in ECR

Check in the **Amazon ECR** section in the **AWS Management Console** to see the Docker image under your repository.

---

## Clean-Up (Optional)

To stop and remove the Docker container running locally:

#### 1. List all running Docker containers:
   ```bash
   docker ps
   ```

#### 2. Stop the container:
   ```bash
   docker stop <container-id>
   ```

#### 3. Remove the container:
   ```bash
   docker rm <container-id>
   ```

---

## Conclusion

In this project, we learned how to:
- Create a Docker image using Nginx and a simple HTML page.
- Build and run the Docker image locally.
- Push the Docker image to Amazon Elastic Container Registry (ECR).

Please feel free to customize the project further or add more features (e.g., serving additional static files, enabling HTTPS, etc.).

---

## Troubleshooting

- **Docker login failed**: Ensure AWS credentials are properly configured, and you are using the correct AWS region and account ID.
- **Port already in use**: If port 80 is in use, stop any services running on port 80 or use another port (e.g., `-p 8080:80`).
- **Permission issues with AWS CLI**: Ensure IAM user or role has the appropriate permissions to access and push to ECR.

---

## References
- [Docker Documentation](https://docs.docker.com/)
- [AWS ECR Documentation](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

---
