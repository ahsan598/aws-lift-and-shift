## Detailed Overview of Lift & Shift Web App on AWS Cloud
Purpose of Moving to AWS:
The goal of a "Lift and Shift" migration is to move an on-premises application and its data to AWS with minimal or no changes to the application architecture. Organizations pursue this migration strategy for several reasons:

1. **Scalability**
AWS provides on-demand scalability, allowing the web app to handle traffic spikes without significant upfront investment in hardware.

2. **Cost Optimization**
Organizations can eliminate the need for costly data centers and physical hardware. AWS’s pay-as-you-go pricing model enables cost savings by paying only for resources used.

3. **Global Reach**
AWS has a wide range of data centers (regions and availability zones) worldwide, enabling reduced latency and a better user experience for global audiences.

4. **Reliability and Availability**
AWS offers high-availability services with built-in failover and disaster recovery mechanisms to ensure uptime and business continuity.

5. **Security and Compliance**
AWS adheres to industry-standard security and compliance requirements, providing secure hosting with managed access controls, encryption, and monitoring.

6. **Faster Time to Market**
AWS’s wide array of managed services (e.g., RDS for databases, S3 for storage, etc.) reduces the time required to set up the necessary infrastructure for the application.


### Resources created in this project:
##### 1. Virtual Private Cloud (VPC):
**Definition:** A VPC is a virtual network that allows you to launch AWS resources in an isolated environment.
**Purpose:** The web application was deployed within a VPC to ensure secure and isolated networking.
**Key Components:**
- Public and private subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Network ACLs

These components collectively controlled traffic flow and enhanced network security.

##### 2. Identity and Access Management (IAM):
**Definition:** IAM provides secure and granular control over access to AWS services and resources.
**Purpose:** IAM roles and policies were created to define permissions for users and services, ensuring secure and least-privilege access.
**Key Components:**
- IAM Users and Groups: For managing access to the AWS account.
- Roles: Assigned to AWS services like EC2 and RDS for secure resource management.
- Policies: Defined permissions to enforce access control.

##### 3. Elastic Cloud Compute (EC2):
**Definition:** Amazon EC2 provides scalable computing capacity in the cloud, enabling flexible hosting for applications.
**Purpose:** Virtual servers (instances) hosted the web application, with the ability to scale capacity up or down as needed, ensuring cost efficiency and flexibility.

##### 4. Amazon S3 (Simple Storage Service):
**Definition:** A highly scalable and secure object storage service for storing and retrieving data from anywhere.
**Purpose:** Used for storing static assets and backups, with built-in encryption and fine-grained access control. S3 ensures high durability and availability through multi-device storage.

##### 5. Auto Scaling:
**Definition:** Auto Scaling ensures high availability by automatically adjusting EC2 capacity to match application demand.
**Purpose:** It dynamically scales resources based on traffic patterns, performs health checks to replace unhealthy instances, and optimizes costs by provisioning resources only when required.

##### 6. Security Groups:
**Definition:** Virtual firewalls for Amazon EC2 instances, controlling inbound and outbound traffic with stateful filtering.
**Purpose:** Enhanced network security by specifying rules based on IP ranges, protocols, and ports. Security groups operate within the VPC to provide detailed access control.

##### 7. Application Load Balancer (ALB):
**Definition:** A managed Layer 7 load balancer that distributes incoming traffic across multiple targets (e.g., EC2 instances) in various Availability Zones.
**Purpose:** Ensures high availability, balances application traffic, and supports features like path-based routing.

##### 8. Amazon Relational Database Service (RDS):
**Definition:** A managed database service that supports engines such as MySQL, PostgreSQL, Oracle, and more.
**Purpose:** Simplifies database management with features like automated backups, Multi-AZ deployments for high availability, and read replicas for improved performance.

##### 9. Rabbit MQ:
**Definition:** An open-source message broker enabling asynchronous communication between applications via message queues.
**Purpose:** Facilitates reliable communication between application components by supporting multiple messaging protocols.

##### 10. MemCache:
**Definition:** An in-memory key-value store used to speed up dynamic web applications by reducing database load.
**Purpose:** Caches frequently requested data, such as API responses or rendered pages, to improve performance.

##### 11. Route 53 & DNS Zones:
**11.1 Route 53**
**Definition:** A scalable and highly available DNS web service for routing end-user requests to internet applications.
**Purpose:** Ensures efficient and reliable domain name resolution for web applications.

**11.2 DNS Zones**
**Public DNS Zone:** Manages DNS records for domains accessible on the internet.
**Private DNS Zone:** Manages DNS records within a VPC, accessible only to resources within that VPC.


### Project Architecture:
##### Deploying a new virtual private cloud (VPC) with default parameters builds the following environment on the AWS Cloud.

![Project Diagram](https://github.com/ahsan598/aws-lift-and-shift-webapp/blob/main/aws-lift-and-shift-webapp.png)


This detailed overview provides a step-by-step guide to implementing a Lift & Shift web app on AWS Cloud. By following these steps, we can migrate our application to AWS, ensuring high availability, scalability, and security using AWS services such as VPC, IAM, EC2, ALB, RDS, RabbitMQ, Memcached, and Route 53.

### Implementation:

##### 1. Virtual Private Cloud (VPC) Setup

1.1. Create a VPC
- Define a CIDR block, e.g., `10.0.0.0/16`.

1.2. Create Subnets
- Create public and private subnets in multiple availability zones for high availability.

1.3. Configure Route Tables
- Public subnet route table: Add a route to the Internet Gateway.
- Private subnet route table: Add a route to the NAT Gateway for outbound traffic.

1.4. Internet Gateway and NAT Gateway
- Attach an Internet Gateway to the VPC.
- Create and attach a NAT Gateway to the public subnet.

1.5. Security Groups
- Define security groups to control inbound and outbound traffic for your instances.


##### 2. Identity and Access Management (IAM) Setup

2.1. Create IAM Roles
- Define roles for EC2 instances to access AWS services like S3, RDS, etc.

2.2. Attach Policies
- Attach appropriate policies to IAM roles (e.g., AmazonS3ReadOnlyAccess, AmazonRDSFullAccess).


##### 3. EC2 Instances with Tomcat Server

3.1. Launch EC2 Instances
- Launch instances in the public subnet for the web application and private subnet for backend services.

3.2. Install and Configure Tomcat
- Install Tomcat on EC2 instances:

```sh
sudo yum install tomcat -y
sudo systemctl start tomcat
sudo systemctl enable tomcat
```
- Deploy your web application to the Tomcat server


##### 4. Application Load Balancer (ALB)

4.1. Create an ALB
- Define listeners for HTTP(80) and HTTPS(443).
- Configure target groups to include the EC2 instances running the Tomcat server.

4.2. Configure Health Checks
- Set up health checks for the target groups to ensure only healthy instances receive traffic.


##### 5. Amazon RDS for Database

5.1. Launch Amazon RDS Instance
- Choose the appropriate database engine (e.g., MySQL, PostgreSQL).
- Configure the instance details and connect it to the private subnet.

5.2. Security Groups
- Ensure the RDS security group allows traffic from the EC2 instances’ security group.


##### 6. RabbitMQ Setup

6.1. Deploy RabbitMQ on EC2
- Launch an EC2 instance in the private subnet and Install RabbitMQ:

```sh
sudo yum install rabbitmq-server -y
sudo systemctl start rabbitmq-server
sudo systemctl enable rabbitmq-server
```

6.2. Configure Security Groups
- Allow traffic on the default RabbitMQ port (5672) from the relevant security groups.


##### 7. Memcached Setup

7.1. Deploy Memcached on EC2
- Launch an EC2 instance in the private subnet and Install Memcached:

```sh
sudo yum install memcached -y
sudo systemctl start memcached
sudo systemctl enable memcached
```

7.2. Configure Security Groups
- Allow traffic on the default Memcached port (11211) from the relevant security groups.


##### 8. DNS Zone & Route 53 DNS Private Zone

8.1. Create Route 53 Hosted Zone
- Create a private hosted zone associated with your VPC.

8.2. Configure DNS Records
- Add A and CNAME records to route traffic to the ALB and internal services.




#### Note: We can use Amazon MQ in place of Rabbit MQ and ElasticCache instaed of Memcache

##### Amazon MQ
**Definition:** Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers.
**Usage:** Amazon MQ was used to handle messaging between different components of the web application, ensuring reliable and asynchronous communication.
**Components:** Brokers and Queues were configured to manage message flow and ensure durability.

##### ElasticCache
**Definition:** ElastiCache is a web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud.
**Usage:** ElastiCache was used to improve application performance by caching frequently accessed data, reducing latency, and offloading read traffic from the database.
**Components:** Redis or Memcached clusters were set up, with replication and automatic failover for high availability.