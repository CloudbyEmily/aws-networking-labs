# AWS VPC Architecture

## Basic AWS Network Design


AWS Region

|
|
VPC
10.0.0.0/16

|
|----------------------|
| |

Public Subnet Private Subnet

10.0.1.0/24 10.0.2.0/24

| |

Load Balancer Application Server

                   |

                   Database

---

## Traffic Path

Public traffic:


User

↓

Internet

↓

Internet Gateway

↓

Public Subnet

↓

Load Balancer


Private traffic:


Load Balancer

↓

Private Subnet

↓

Application Server

↓

Database


---

## Key Concepts

- VPC creates the network boundary
- Subnets organize resources
- Route tables control traffic direction
- Internet Gateway provides public access
- Private resources use controlled outbound access
- Security Groups and NACLs protect traffic
