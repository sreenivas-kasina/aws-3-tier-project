# Tier Architecture explanation
Web Server: This is the front part of a web application that displays the website to users. It handles how the site looks and interacts with users, like showing product pages or allowing people to sign up.

Application Server: This middle part manages the business logic and processes user actions. For example, it checks the inventory to see if a product is in stock or updates a user's account details.

Database Server: This is the back-end part that stores all the data for the application. It uses databases like MySQL or PostgreSQL to keep track of information.

# AWS Three Tier Web Architecture Workshop

## Description: 
This workshop is a hands-on walk through of a three-tier web architecture in AWS. We will be manually creating the necessary network, security, app, and database components and configurations in order to run this architecture in an available and scalable manner.

## Architecture Overview
![AWS Architecture - DrawIO](https://github.com/pandacloud1/AWS_Project1/assets/134182273/3e46931f-0802-48a7-b044-22cd2afde467)

In this architecture, a public-facing Application Load Balancer forwards client traffic to our web tier EC2 instances. The web tier is running Nginx webservers that are configured to serve a React.js website and redirects our API calls to the application tier’s internal facing load balancer. The internal facing load balancer then forwards that traffic to the application tier, which is written in Node.js. The application tier manipulates data in an Aurora MySQL multi-AZ database and returns it to our web tier. Load balancing, health checks and autoscaling groups are created at each layer to maintain the availability of this architecture.

## Algorithm
### AWS PROJECT
---

# Creating 3 Tier Architecture & Integrating Other AWS Resources

## Step 1: Download Code from GitHub in Your Local System

## Step 2: Create Two S3 Buckets
- Create one S3 bucket for storing web-server & app-server code.
- Upload the code to your S3 from your local system.
- Create another S3 bucket for VPC flow logs.

## Step 3: Create IAM Role with Policies
- S3 read only.
- SSM managed instance core.

## Step 4: Create VPC, Subnets, IGW, NAT-GW, RT
- Enable auto-assign public IP for web-tier public subnets.
- Create flow logs for VPC & use the S3 bucket created above.

## Step 5: Create Security Groups
1. **External-Load-Balancer-SG** --> HTTP (80): 0.0.0.0/0.
2. **Web-Tier-SG** --> HTTP --> Ext-LB-SG.
3. **Internal-Load-Balancer-SG** --> HTTP --> Web-Tier-SG.
4. **App-Tier-SG** --> Port 4000 --> Internal-LB-SG.
5. **DB-Tier-SG** --> MySQL (3306) --> App-Tier-SG.

## Step 6: Create DB Subnet Group & RDS
- Create DB subnet group.
- Create RDS - Multi-AZ.
- Place them in DB subnet group created above.

## Step 7: Create Test App Server, Install Packages, Test Connections
- [Test App-Server Commands](https://github.com/pandacloud1/AWS_Project1/blob/main/app-server-commands)
- Create AMI.
- Create launch template using AMI.
- Create target group.
- Create internal load balancer.
- Create autoscaling group.
- Edit `nginx.conf` file in local system by adding Internal-LB-DNS & upload the file in S3.

## Step 8: Create Test Web Server, Install Packages (Nginx, Node.js (React)), Test Connections
- [Test Web-Server Commands](https://github.com/pandacloud1/AWS_Project1/blob/main/web-server-commands)
- Create AMI.
- Create launch template using AMI.
- Create target group.
- Create external load balancer.
- Create autoscaling group.

## Step 9: Add External-ALB-DNS Record in Route 53

## Step 10: Create CloudWatch Alarms Along with SNS

## Step 11: Create CloudTrail

## step 12: AWS Certificate Manager (ACM)
- Generate SSL Certificate and apply it to external load balancer and map domain name to external load balancer 
## Summary
This architecture ensures high availability, scalability, and reliability by distributing the load, monitoring instance health, and scaling resources dynamically. The web tier serves the front-end and routes API calls, the application tier handles business logic and interacts with the database, and the database tier provides robust data storage and retrieval.

## Step 13: Deleting the Entire Infrastructure
- Delete CloudFront.
- Delete CloudWatch alarms.
- Delete records from Route 53.
- Delete load balancers, target groups, ASG, launch templates.
- Delete security group.
- Delete NAT gateway (it will take 5 mins).
- Release elastic IP.
- Delete VPC.
- Delete RDS subnet group, RDS.

---


