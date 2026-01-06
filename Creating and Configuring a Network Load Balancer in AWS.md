# AWS Network Load Balancer Lab

## Overview
This lab demonstrates how to **create and configure a Network Load Balancer (NLB)** in AWS. You will:
- Prepare the AWS environment (subnets, network ACLs, EC2 instances)
- Deploy and configure an Internet-facing NLB
- Test load balancing and monitor traffic using CloudWatch

---

## Steps Completed

### 1. Create Public Subnet
- **Name:** `PublicB`
- **Availability Zone:** `us-east-1b`
- **CIDR Block:** `10.0.2.0/24`
- Added route to **Internet Gateway**
- Configured explicit subnet association

### 2. Modify Network ACL
Allowed inbound traffic for:
- **HTTP (80)**
- **HTTPS (443)**
- **SSH (22)**
- **Ephemeral Ports (1024–65535)**

### 3. Launch EC2 Instances
- **WebA** in `us-east-1a`
- **WebB** in `us-east-1b`
- **AMI:** Amazon Linux 2
- **Type:** `t3.micro`
- **Security Group:** Preconfigured EC2SecurityGroup
- **User Data Scripts:**
```bash
#!/bin/bash
yum update -y && yum -y install httpd
systemctl enable httpd && systemctl start httpd
usermod -a -G apache ec2-user
chown -R ec2-user:apache /var/www
chmod 2775 /var/www
find /var/www -type d -exec chmod 2775 {} \;
find /var/www -type f -exec chmod 0664 {} \;
echo "Request Handled by: WebA" >> /var/www/html/index.html
```
*(Similar script for WebB with text updated)*

### 4. Create Network Load Balancer
- **Name:** `NLB4LAB`
- **Scheme:** Internet-facing
- **Availability Zones:** `us-east-1a`, `us-east-1b`
- **Target Group:**
  - **Name:** `nlbTargets`
  - **Type:** Instances
  - **Protocol:** TCP
  - **Port:** 80
  - **Health Check:** TCP
- Registered **WebA** and **WebB** as targets

### 5. Test and Monitor
- Access NLB DNS name → Page served by WebA or WebB
- SSH into AdminInstance and run:
```bash
while true; do curl <LOAD_BALANCER_DNS_NAME>; done
```
- Observed CloudWatch metrics for traffic spikes

---

## Key Learnings
- Configuring subnets and routing for public access
- Setting up EC2 instances with user data scripts
- Deploying and testing an AWS Network Load Balancer
- Monitoring traffic using CloudWatch

✅ **Lab Completed Successfully!**
