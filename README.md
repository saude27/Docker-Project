
# Dockerized Static Web App with AWS Deployment

This project demonstrates how to containerize a static web app, push the container image to Docker Hub and Amazon Elastic Container Registry (ECR), and deploy it using a three-tier architecture on AWS. Below are the steps followed and their explanations.

---

## Project Steps

### **1. Building and Saving the Docker Container**
1. Built the container image in Visual Studio Code.
2. Saved the image in the local Docker repository.
3. Pushed the container image to Docker Hub repository.

### **2. Configuring AWS CLI and IAM**
1. Installed the AWS Command Line Interface (CLI).
2. Created an IAM user with programmatic access.
3. Generated an access key and secret access key for the IAM user.
4. Configured the access keys on the local machine using `aws configure`.

### **3. Creating and Pushing to Amazon ECR**
1. Created an Amazon Elastic Container Registry (ECR) repository.
2. Tagged and pushed the Docker image to the ECR repository.

### **4. Building a Three-Tier AWS Network VPC**
1. Created a custom Virtual Private Cloud (VPC).
2. Set up an Internet Gateway to enable internet access.
3. Created public and private subnets.
4. Configured a route table and associated it with the subnets.
5. Added a NAT Gateway to allow internet access for instances in private subnets.
6. Configured security groups for the various components.

### **5. Setting Up Load Balancer and ECS Cluster**
1. Created a target group for routing traffic.
2. Deployed an Application Load Balancer (ALB).
3. Set up an ECS cluster.
4. Created a task definition for the container.
5. Defined a service to run the containerized application.

### **6. Configuring Domain and SSL**
1. Created a DNS record set to point the domain to the load balancer.
2. Generated and attached an SSL certificate to secure the application.

---

## Deployment Script

The following Dockerfile was used to build the container image:

```dockerfile
FROM amazonlinux:latest

# Install dependencies
RUN yum update -y && \
    yum install -y httpd && \
    yum search wget && \
    yum install wget -y && \
    yum install unzip -y

# Change directory
RUN cd /var/www/html

# Download web files
RUN wget https://github.com/azeezsalu/jupiter/archive/refs/heads/main.zip

# Unzip folder
RUN unzip main.zip

# Copy files into html directory
RUN cp -r jupiter-main/* /var/www/html/

# Remove unwanted folder
RUN rm -rf jupiter-main main.zip

# Expose port 80 on the container
EXPOSE 80

# Set the default application that will start when the container starts
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

---

## Notes
- Ensure the AWS resources are properly cleaned up after testing to avoid unnecessary costs.
- The SSL certificate was created using AWS Certificate Manager (ACM) and attached to the Application Load Balancer.
- The `wget` command downloads the static web app files from a GitHub repository. Update the URL if the repository changes.

---

## Tools and Technologies Used
- **Docker**: For containerization.
- **AWS CLI**: For managing AWS resources from the command line.
- **Amazon VPC**: For setting up the network infrastructure.
- **Amazon ECS**: For running the containerized application.
- **Amazon ECR**: For storing the container image.
- **Amazon ACM**: For SSL certificate management.

---

## How to Run
1. Clone the repository.
2. Build the Docker image using the provided Dockerfile.
   ```bash
   docker build -t jupiter .
   ```
3. Push the Docker image to Docker Hub or Amazon ECR.
4. Deploy the application using the AWS resources created.
5. Access the application via the Application Load Balancer’s DNS name or configured domain.

---

## Conclusion
This project demonstrates a complete workflow from containerization to deployment of a static web app using Docker and AWS. It utilizes AWS’s robust services to ensure scalability, reliability, and security of the application.

