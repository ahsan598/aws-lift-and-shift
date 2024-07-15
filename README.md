## Detailed Overview of Lift & Shift Web App on AWS Cloud

### Resources created in this project:
1. Virtual Private Cloud (VPC)
Definition: VPC allows you to launch AWS resources into a virtual network that you've defined.
Usage: The web application was deployed within a VPC to ensure network isolation and security.
Components: Subnets (public and private), Internet Gateway, NAT Gateway, Route Tables, and Network ACLs were configured to control traffic flow and enhance security.
2. Identity and Access Management (IAM)
Definition: IAM provides secure control of access to AWS services and resources.
Usage: IAM roles and policies were created to define permissions for different users and services. Roles were assigned to AWS services like EC2, RDS, and Beanstalk to manage resources securely.
Components: IAM Users, Groups, Roles, and Policies were used to manage permissions and ensure least privilege access.
3. Elastic Beanstalk
Definition: Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services.
Usage: Elastic Beanstalk was used to deploy the web application, handling the deployment details such as capacity provisioning, load balancing, and auto-scaling.
Components: Environments, Applications, and Application Versions were managed in Beanstalk, with configuration for health monitoring and updates.
4. Elastic Load Balancer (ELB)
Definition: ELB automatically distributes incoming application traffic across multiple targets, such as EC2 instances.
Usage: An ELB was used to ensure high availability and fault tolerance by distributing incoming traffic to multiple EC2 instances within the VPC.
Components: Application Load Balancer (ALB) was configured with target groups and listeners to manage HTTP/HTTPS traffic effectively.
5. Relational Database Service (RDS)
Definition: RDS makes it easy to set up, operate, and scale a relational database in the cloud.
Usage: RDS was used to host the web application's database, ensuring high availability, automated backups, and seamless scaling.
Components: Multi-AZ deployment for high availability, automated backups, and read replicas for improved performance.
6. Amazon MQ
Definition: Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers.
Usage: Amazon MQ was used to handle messaging between different components of the web application, ensuring reliable and asynchronous communication.
Components: Brokers and Queues were configured to manage message flow and ensure durability.
7. ElastiCache
Definition: ElastiCache is a web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud.
Usage: ElastiCache was used to improve application performance by caching frequently accessed data, reducing latency, and offloading read traffic from the database.
Components: Redis or Memcached clusters were set up, with replication and automatic failover for high availability.


### Project Architecture:
##### Deploying this Quick Start for a new virtual private cloud (VPC) with default parameters builds the following Aurora MySQL environment in the AWS Cloud.

![Project Diagram](https://github.com/ahsan598/aws-lift-and-shift-webapp/blob/main/aws-lift-and-shift-webapp.svg)


### Implementation Summary:

1. Design Phase:
Planned the architecture, including VPC design, subnetting, and security group configurations.
Defined IAM roles and policies to ensure secure access to resources.

2. Deployment Phase:
Set up the VPC with necessary components (subnets, route tables, gateways).
Deployed the application using Elastic Beanstalk, linking it to ELB for load balancing.
Configured RDS for database management and ElastiCache for caching needs.
Integrated Amazon MQ for messaging and ensured security and monitoring configurations.

3. Optimization Phase:
Tuned configurations for performance optimization, including caching strategies with ElastiCache.
Implemented monitoring and logging with CloudWatch and Elastic Beanstalk health checks.

4. Maintenance Phase:
Regularly updated IAM policies to align with changing security requirements.
Monitored performance metrics and scaled resources as needed.
Managed backups, patching, and recovery processes for RDS and other services.
This approach ensured a seamless lift and shift of the web application to AWS, leveraging the cloud's scalability, reliability, and security features.