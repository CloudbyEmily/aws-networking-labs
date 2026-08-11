# AWS VPC, EC2 & RDS Cloud Architecture

## Project Overview

This project demonstrates the design and deployment of a foundational AWS cloud architecture using Amazon VPC, EC2, and Amazon RDS for MySQL.

The project focuses on AWS networking, compute, database infrastructure, security groups, subnet architecture, DNS configuration, and relational database design.

## Architecture

The environment consists of:

* Amazon VPC
* Public subnet for EC2
* Additional subnet for RDS infrastructure
* Internet Gateway
* Route table
* Amazon EC2
* Amazon RDS for MySQL
* Security Groups
* RDS DB subnet group

### Architecture Flow

User
↓
Internet
↓
Internet Gateway
↓
VPC
↓
EC2
↓
MySQL / TCP 3306
↓
Amazon RDS MySQL

## AWS Networking

The project VPC is:

`practice-midterm-vpc-1`

The VPC contains subnets in multiple Availability Zones to support the RDS database subnet group.

### Subnets

* Public subnet: `10.0.0.0/24` — `us-east-1a`
* RDS subnet: `10.0.1.0/24` — `us-east-1b`

An RDS DB subnet group was created using subnets in both Availability Zones.

## EC2

The EC2 portion of the architecture uses an Ubuntu instance deployed in the public subnet.

The EC2 instance is responsible for providing the compute environment used to communicate with the MySQL database.

## Amazon RDS

A MySQL RDS instance was deployed inside the project VPC.

Configuration includes:

* MySQL Community Edition
* Single-AZ deployment
* General Purpose SSD storage
* 20 GiB allocated storage
* Public accessibility enabled
* RDS DB subnet group spanning multiple Availability Zones

## Security

The architecture uses AWS Security Groups to control network access.

MySQL traffic uses TCP port `3306`.

The intended architecture restricts database access to the EC2 security group rather than allowing unrestricted MySQL access from the internet.

This demonstrates the principle of least privilege by allowing the communication required by the application while limiting unnecessary inbound access.

## DNS Configuration

During the RDS deployment, the VPC required DNS resolution and DNS hostnames to be enabled before a publicly accessible RDS instance could be created.

Both settings were enabled on the VPC.

## Troubleshooting & Lessons Learned

Several AWS configuration issues were encountered during deployment.

### VPC mismatch

The initial RDS configuration attempted to use a different VPC from the EC2 security group.

The issue was resolved by verifying the EC2 VPC and ensuring the RDS configuration used:

`vpc-09012eaad36849fbc`

### RDS subnet requirements

The VPC initially contained only one subnet.

An additional subnet was created in another Availability Zone, followed by the creation of an RDS DB subnet group using both subnets.

### VPC DNS requirements

Public RDS deployment initially failed because DNS resolution and DNS hostnames were not enabled on the VPC.

Both settings were enabled, after which the RDS instance was successfully created.

## Database Design

The RDS database will contain three tables:

* `students`
* `aws_basics`
* `linux_commands`

The table schemas will be documented here after they are created and verified.

## Project Status

### Completed

* [x] VPC created
* [x] Public subnet created
* [x] Internet Gateway configured
* [x] Route table configured
* [x] EC2 instance deployed
* [x] Additional RDS subnet created
* [x] RDS DB subnet group created
* [x] VPC DNS resolution enabled
* [x] VPC DNS hostnames enabled
* [x] MySQL RDS instance deployed

### In Progress

* [ ] Configure and verify RDS security group
* [ ] Establish EC2-to-RDS connection
* [ ] Create `1pu_database`
* [ ] Create database schemas
* [ ] Verify all three tables

## Skills Demonstrated

* AWS VPC networking
* Subnet and Availability Zone design
* Route tables
* Internet Gateway
* EC2 deployment
* Amazon RDS
* MySQL
* Security Groups
* DNS configuration
* Network troubleshooting
* Least-privilege security concepts
* Git and GitHub documentation
