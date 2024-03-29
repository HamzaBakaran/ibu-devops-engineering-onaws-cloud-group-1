# ibu-devops-engineering-onaws-cloud-group-1

## Student Records Web Application Hosting Documentation
This documentation provides an overview and instructions for setting up the hosting infrastructure for the student records web application in the AWS Cloud.

To access the web application, click [HERE](http://hamzabakarandevopsprojectlb-827799244.us-east-1.elb.amazonaws.com/)

Team members:
* Mirza Novalic
* Hamza Bakaran
* Basil Bosnjak

## Table of Contents
Architecture Overview
* Infrastructure Components
* Setup Instructions
* Testing and Monitoring
* Security Considerations
* Cost Optimization

## Architecture Overview
* The architecture diagrams for each project phase can be found in the [Docs](https://github.com/HamzaBakaran/ibu-devops-engineering-onaws-cloud-group-1/tree/main/docs) folder.

The student records web application is hosted in the AWS Cloud using a scalable and highly available architecture. The architecture consists of the following components:

* Amazon Virtual Private Cloud (VPC): Provides isolated networking environment for the resources.
* Subnets: Includes two public and two private subnets across two Availability Zones (us-east-1a and us-east-1b).
* Route Tables: Control traffic routing between subnets and to the internet.
* Internet Gateway: Enables communication between the VPC and the internet.
* Security Groups: Manage inbound and outbound traffic rules for EC2 instances and RDS database.
* EC2 Instance: Hosts the web application using a launch template and auto scaling group.
* Load Balancer: Distributes incoming traffic across EC2 instances in the auto scaling group.
* Amazon RDS: Provides a managed MySQL database for storing student records.
* Subnet Group: Specifies the subnets where the RDS database is deployed.

## Infrastructure Components
### VPC and Subnets
The Virtual Private Cloud (VPC) is configured with a CIDR block of 10.0.0.0/16. It includes two public and two private subnets spread across two Availability Zones(us-east-1a and us-east-1b). The subnets are designed as follows:

Public Subnets: (Public subnet 1 10.0.0.0/24 us-east-1a) and (Public subnet 2 10.0.1.0/24 us-east-1b)  Located in the public network and associated with the internet gateway. These subnets host the EC2 instances and load balancer.  
Private Subnets: (Private subnet 1 10.0.2.0/23 us-east-1a) and (Private subnet 2 10.0.4.0/23 us-east-1b) Isolated from the internet and used for the RDS database. These subnets provide an extra layer of security.

### Route Tables
The route tables define the routing rules for the VPC's subnets. The public subnets have a route to the internet gateway (0.0.0.0/0), allowing outgoing traffic to the internet. The private subnets have routes to the NAT Gateway and local routes for internal communication.

### Security Groups
Security groups are used to control inbound and outbound traffic to the EC2 instances and RDS database. The following security groups are configured:

* Web Server Security Group: Allows inbound traffic on port 80 (HTTP)  from the load balancer and restricts outbound traffic to necessary services.
* Database Security Group: Permits inbound traffic only from the web server security group on the database port (e.g., 3306 for MySQL) and restricts outbound traffic to the necessary database services.

### EC2 Instance and Auto Scaling
The web application is hosted on an EC2 instance. A launch template is used to define the instance specifications, such as the Amazon Machine Image (AMI), instance type, and user data script for configuring the instance on launch.

An auto scaling group is set up to automatically adjust the number of EC2 instances based on demand. This ensures scalability and high availability of the web application during peak loads.

### Load Balancer
The load balancer distributes incoming traffic across multiple EC2 instances in the auto scaling group, improving availability and scalability. An Application Load Balancer (ALB) is used to load balance the web application traffic, and it is associated with a target group.

### Amazon RDS
The student records are stored in an Amazon RDS MySQL database. The database is deployed in the private subnets for improved security. The RDS database instance is automatically managed by AWS, ensuring high availability, backups, and scalability.

## Setup Instructions
Follow the steps below to set up the student records web application hosting infrastructure:

1. Create a new VPC with the desired CIDR block (10.0.0.0/16).
2. Configure public and private subnets across multiple Availability Zones.
3. Create route tables and associate them with the subnets.
4. Set up an internet gateway and associate it with the VPC.
5. Configure security groups for the EC2 instances and RDS database.
6. Launch an EC2 instance using the provided launch template, ensuring it's associated with the public subnet.
7. Create an auto scaling group and associate it with the EC2 instance.
8. Set up an Application Load Balancer (ALB) and create a target group for the EC2 instances.
9. Deploy the Amazon RDS MySQL database in the private subnet using the specified subnet group.

The scripts used for EC2, Database and Cloud 9 can be found [HERE](https://github.com/HamzaBakaran/ibu-devops-engineering-onaws-cloud-group-1/tree/main/helper-scripts)

## Testing and Monitoring

Test the web application by accessing the ALB's DNS name or the public IP address of the EC2 instance.
IPv4 Address is used and allocated automatically upon the EC2 instance creation.

## Security Considerations
The hosting infrastructure is designed with security in mind. Consider the following security best practices:

* Ensure that database credentials are not hardcoded in the web application code.
* Regularly patch and update the operating system, web server, and database software.
* Follow AWS security best practices and implement appropriate IAM roles and policies.
* For detailed security considerations, refer to the Security Guide.

## Cost Optimization
To optimize costs, consider the following practices:

* Use Auto Scaling to adjust the number of EC2 instances based on demand, ensuring optimal capacity while minimizing costs during periods of low traffic.
* Leverage AWS cost optimization tools, such as AWS Cost Explorer and AWS Trusted Advisor, to monitor and optimize costs.
* Periodically review resource utilization and right-size instances based on actual workload requirements.
