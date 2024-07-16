## Detailed Overview of Lift & Shift Web App on AWS Cloud

### Resources created in this project:
##### 1. Virtual Private Cloud (VPC):
- Definition: VPC allows you to launch AWS resources into a virtual network that you've defined.
- Usage: The web application was deployed within a VPC to ensure network isolation and security.
- Components: Subnets (public and private), Internet Gateway, NAT Gateway, Route Tables, and Network ACLs were configured to control traffic flow and enhance security.

##### 2. Identity and Access Management (IAM):
- Definition: IAM provides secure control of access to AWS services and resources.
- Usage: IAM roles and policies were created to define permissions for different users and services. Roles were assigned to AWS services like EC2, RDS, and Beanstalk to manage resources securely.
- Components: IAM Users, Groups, Roles, and Policies were used to manage permissions and ensure least privilege access.

##### 3. Elastic Cloud Compute (EC2):
- Definition: Amazon EC2 provides scalable computing capacity in the AWS cloud, allowing developers to run virtual servers (instances) to host applications. EC2 enables you to increase or decrease capacity within minutes, thus providing a flexible and cost-effective way to manage computing resources.

##### 4. Application Load Balancer (ALB):
- Definition: The Application Load Balancer (ALB) is a managed load balancing service that operates at the application layer (Layer 7). It distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones.

##### 5. Amazon Relational Database Service (RDS):
- Definition: Amazon RDS is a managed relational database service that supports several database engines including MySQL, PostgreSQL, Oracle, SQL Server, and MariaDB. It simplifies database management tasks such as backups, patching, and scaling.
- Components: Multi-AZ deployment for high availability, automated backups, and read replicas for improved performance.

##### 6. Rabbit MQ:
- Definition: RabbitMQ is an open-source message broker that facilitates communication between applications by sending messages between them in an asynchronous manner. It supports various messaging protocols.

##### 7. MemCache:
- Definition: Memcached is an in-memory key-value store for small chunks of arbitrary data (strings, objects) from results of database calls, API calls, or page rendering. It is used to speed up dynamic web applications by alleviating database load.

##### 8. Route 53 & DNS Zones:
8.1 Route53
- Definition: Amazon Route 53 is a scalable and highly available Domain Name System (DNS) web service designed to route end-user requests to internet applications.

8.2 DNS Zones:
- Public DNS Zone: Manages DNS records for a domain accessible on the internet.
- Private DNS Zone: Manages DNS records within a VPC, accessible only to VPC resources.


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
- Define listeners for HTTP and HTTPS.
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
- Definition: Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers.
- Usage: Amazon MQ was used to handle messaging between different components of the web application, ensuring reliable and asynchronous communication.
- Components: Brokers and Queues were configured to manage message flow and ensure durability.

##### ElasticCache
- Definition: ElastiCache is a web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud.
- Usage: ElastiCache was used to improve application performance by caching frequently accessed data, reducing latency, and offloading read traffic from the database.
- Components: Redis or Memcached clusters were set up, with replication and automatic failover for high availability.