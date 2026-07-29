# AWS Subnets

## What is a Subnet?

A subnet is a smaller network section inside a VPC.

A VPC provides the overall network environment, while subnets divide that environment into smaller sections where resources can be organized and managed.

A good analogy is:

VPC = A large property

Subnet = A specific section inside that property

Each subnet has its own IP address range and exists inside one Availability Zone.

---

# Why Do We Use Subnets?

Subnets allow cloud engineers to:

- Organize resources
- Separate workloads
- Control network traffic
- Improve security
- Design highly available applications

Instead of placing every resource into one large network, AWS allows engineers to create smaller controlled environments.

---

# Subnet and CIDR

Subnets receive CIDR blocks from the VPC CIDR range.

Example:

VPC:
10.0.0.0/16


Possible subnets:


Public Subnet

10.0.1.0/24

Private Subnet

10.0.2.0/24


The subnet CIDR must fit inside the VPC CIDR range.

---

# Public Subnets

A public subnet contains resources that need communication with the internet.

Examples:

- Load Balancers
- Public EC2 instances
- Bastion Hosts

Traffic path:


User

↓

Internet

↓

Internet Gateway

↓

Public Subnet

↓

Resource


A public subnet requires:

- A route table
- A route to an Internet Gateway
- Resources configured for public access

---

# Private Subnets

A private subnet contains resources that should not be directly accessible from the internet.

Examples:

- Application servers
- Databases
- Internal services

Traffic path:


Application Server

↓

NAT Gateway

↓

Internet


Private resources can make outbound connections while remaining protected from direct inbound internet traffic.

---

# Availability Zones

Each subnet exists inside one Availability Zone.

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

Using multiple Availability Zones improves reliability and availability.

---

# Subnet Troubleshooting

When a resource cannot communicate, check:

1. Is the subnet CIDR correct?
2. Is the resource in the correct subnet?
3. Does the subnet have the correct route table?
4. Is internet access required?
5. Is the security configuration allowing traffic?

---

# Key Concept

A VPC creates the network.

Subnets organize the network.

Public and private subnets allow engineers to control where resources live and how traffic flows.
