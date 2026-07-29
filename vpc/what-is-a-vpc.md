# What is a VPC?

## Overview

A VPC (Virtual Private Cloud) is a private network environment inside AWS that allows engineers to create and control how cloud resources communicate.

A VPC is similar to owning your own private section of a city. You decide:

- Where buildings are placed
- Which areas are public or private
- How traffic moves
- Who can access different resources

In AWS, the VPC is the foundation where resources such as EC2 instances, databases, and load balancers are deployed.

---

# Why Do We Use VPCs?

Without a VPC, cloud resources would not have a controlled network environment.

A VPC allows engineers to define:

- IP address ranges
- Subnets
- Routing rules
- Internet access
- Security controls

VPCs provide isolation, organization, and security for cloud workloads.

---

# VPC and CIDR

When creating a VPC, engineers assign a CIDR block.

Example:


10.0.0.0/16


The CIDR block defines the available IP address space inside the VPC.

The `/16` means:

- The first 16 bits identify the network.
- The remaining 16 bits identify hosts.

This creates thousands of possible IP addresses that can be divided into smaller networks.

---

# VPC Structure

A VPC contains smaller network sections called subnets.

Example:


VPC

10.0.0.0/16

    |

    |

Public Subnet

10.0.1.0/24

Private Subnet

10.0.2.0/24


Subnets allow engineers to organize resources based on their purpose and security requirements.

---

# Availability Zones

AWS regions contain multiple Availability Zones.

A VPC can span multiple Availability Zones to improve reliability.

Example:


AWS Region

|

|--- Availability Zone A

| |
| Public Subnet
| Private Subnet

|--- Availability Zone B

    |
    Public Subnet
    Private Subnet

Using multiple Availability Zones helps applications remain available if one location experiences problems.

---

# Public and Private Networks

## Public Subnet

A public subnet contains resources that need direct communication with the internet.

Examples:

- Load Balancers
- Public EC2 instances
- Bastion Hosts

Traffic can flow through:


Internet

↓

Internet Gateway

↓

Public Subnet


---

## Private Subnet

A private subnet contains resources that should not be directly reachable from the internet.

Examples:

- Application servers
- Databases

Private resources can access the internet through a NAT Gateway, but outside users cannot directly initiate connections to them.

Example:


Private EC2

↓

NAT Gateway

↓

Internet


---

# VPC Connection to Networking Fundamentals

The concepts from Linux networking apply directly to AWS.

Linux:


IP Address

↓

Routing Table

↓

Gateway

↓

Destination


AWS:


User

↓

DNS

↓

Internet Gateway

↓

VPC

↓

Subnet

↓

Route Table

↓

EC2 Instance

↓

Application


The same networking principles exist, but AWS provides the infrastructure.

---

# Troubleshooting VPC Connectivity

When traffic cannot reach a destination, check the layers:

1. DNS
2. VPC configuration
3. Route Tables
4. Internet Gateway or NAT Gateway
5. Security Groups
6. Network ACLs
7. Application services

Cloud engineers troubleshoot by finding where traffic stopped.

---

# Key Concept

A VPC is the foundation of AWS networking.

It gives engineers control over:

- Network boundaries
- IP addressing
- Traffic flow
- Security
- Resource communication

Understanding VPCs is the first step toward designing and troubleshooting AWS environments.
