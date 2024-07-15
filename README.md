## Deploying Amazon Aurora MySQL Database in AWS

### Resources created in this project:
- Amazon Virtual Private Cloud (Amazon VPC)
- Internet Gateway (IGW)
- NAT Gateway (across all public subnets)
- NACL- Network Access Control List (across all AZs)
- Amazon VPC subnets (public, private) in all the Availability Zones (AZs)selected
- Mulitple VPC Security Groups
- Bastion instances in public subnet
- Amazon Aurora MySQL Database in private subnet 
- Aurora Db Replica in private subnet (another AZ)


### Project Architecture:
##### Deploying this Quick Start for a new virtual private cloud (VPC) with default parameters builds the following Aurora MySQL environment in the AWS Cloud.

![Project Diagram](https://github.com/ahsan598/aws-project-1/blob/main/amazon-aurora-db-in-aws.svg)


### Deployment:
As shown in Figure, sets up the following:

- A Virtual Private Cloud (VPC) configured with public and private subnets, to provide with our own virtual network on AWS.

- Your VPC must have two private subnets in different Availability Zones for the database instances.

#### In the public subnets & private subnets:

- Managed network address translation (NAT) gateways to allow outbound internet access for resources in the private subnets.

- A Linux bastion host in public subnet to allow inbound Secure Shell (SSH) access to resources in the private subnets.

- An Aurora MySQL database (DB) cluster in a security group, including DB Replica in another AZ.




### Test the Deployment:

#### To test the deployment, confirm that the MySQL database is accepting connections by following below steps:

1. Launch MySQL Workbench on your workstation.
2. On the Database menu, choose Connect to Database.
3. Enter the following in the Connect to Database dialog box.
    a. In the Hostname field, enter 127.0.0.1
    b. In Port field, enter the value for the AuroraClusterPort parameter.
    c. In Username, enter the value for the DBMasterUsername parameter.
    d. Choose OK.
4. In the Connect to MySQL Server dialog box, enter the administrator password (DBMasterUserPassword) that you entered during stack creation.
5. A MySQL Workbench dashboard appears.
6. Terminate the SSH tunnel by pressing Ctrl+C. You’ve completed the testing.