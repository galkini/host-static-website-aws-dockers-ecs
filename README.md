

# Hosting a Static Website on AWS Using Docker, ECS, and ECR

This project demonstrates how to host a static website on AWS using Docker containers, Amazon ECS (Elastic Container Service), and Amazon ECR (Elastic Container Registry). This readme provides a comprehensive guide leveraging various AWS services to ensure high availability, scalability, and security. The reference diagram and configuration files are available in the [GitHub repository](https://github.com/galkini/host-static-website-aws-dockers-ecs).

## Prerequisites

1. **Install Git and Visual Studio Code**:
   - Download and install Git from [git-scm.com](https://git-scm.com).
   - Download and install Visual Studio Code from [code.visualstudio.com](https://code.visualstudio.com).
   - Register for a GitHub account at [github.com](https://github.com).
   - Create an SSH Key Pair on your computer and add the public key to your GitHub account.

2. **Docker Hub Account**:
   - Sign up for a Docker Hub account at [hub.docker.com](https://hub.docker.com).

3. **Docker Installation**:
   - Download and install Docker from [docker.com](https://www.docker.com).

4. **AWS CLI**:
   - Install AWS CLI from [aws.amazon.com/cli](https://aws.amazon.com/cli).
   - Create an IAM User and a named profile for AWS CLI interactions.

## Steps to Deploy

### 1. GitHub and Docker Setup

1. **Create a GitHub Repository**:
   - Create a new repository on GitHub to store your Dockerfile.

2. **Clone the GitHub Repository**:
   - Clone the newly created GitHub repository to your local computer using:
   
     ```sh
     git clone git@github.com:your-username/your-repo-name.git
     ```

3. **Docker Hub Repository**:
   - Create a repository in your Docker Hub account to store your Docker image.

### 2. Docker Container Creation

1. **Create Dockerfile**:
   - Navigate to the cloned GitHub repository directory and create a Dockerfile.
     
2. **Build the Container Image**:
   
    ```sh
    docker build -t your-dockerhub-username/your-repo-name .
    ```

3. **Start the Container**:
   
    ```sh
    docker run -p 80:80 your-dockerhub-username/your-repo-name
    ```

4. **Push the Image to Docker Hub**:
   
    ```sh
    docker login
    docker push your-dockerhub-username/your-repo-name
    ```

### 3. AWS Setup

1. **Amazon ECR Repository**:
   - Create an Amazon ECR repository to store your Docker image.

2. **Push the Image to ECR**:
   - Authenticate Docker to the Amazon ECR registry and push the image.

     ```sh
     aws ecr get-login-password --region your-region | docker login --username AWS --password-stdin your-aws-account-id.dkr.ecr.your-region.amazonaws.com
     docker tag your-dockerhub-username/your-repo-name:latest your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-ecr-repository-name:latest
     docker push your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-ecr-repository-name:latest
     ```

3. **VPC Setup**:
   - Create a three-tier VPC with public and private subnets in two availability zones.

4. **Create NAT Gateways and Application Load Balancer**:
   - Use public subnets to create NAT Gateways and an Application Load Balancer.

5. **Create Security Groups**:
   - Define security groups for your load balancer and ECS cluster.

6. **Application Load Balancer**:
   - Configure the Application Load Balancer to distribute web traffic to the ECS services.

### 4. ECS Setup

1. **Create an ECS Cluster**:
   - Set up an ECS cluster to manage your containers.

2. **Task Definition**:
   - Create a task definition specifying the details of the Docker container to be used.

3. **ECS Service**:
   - Set up an ECS service to manage the deployment and scaling of your tasks.

### 5. Domain and Security

1. **Route 53**:
   - Use Route 53 to register your domain and create a DNS record pointing to the Application Load Balancer.

2. **AWS Certificate Manager**:
   - Secure web communication by obtaining and managing an SSL certificate using AWS Certificate Manager.

## Cleanup

After completing and testing the project, you can clean up the resources by deleting the created entities in the AWS Management Console or using the AWS CLI commands.

## Repository

Find all configuration files, diagrams, and scripts in the private GitHub repository [https://github.com/galkini/host-static-website-aws-dockers-ecs].

## Conclusion

By following the steps outlined above, this project successfully hosted a static website on AWS using Docker, ECS, and ECR. This setup leverages various AWS services to ensure high availability, security, and efficient resource management
