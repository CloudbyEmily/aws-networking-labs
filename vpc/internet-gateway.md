# AWS Internet Gateway

## What is an Internet Gateway?

An Internet Gateway (IGW) is an AWS component that allows communication between a VPC and the public internet.

It provides a connection point between:

Internet

↓

Internet Gateway

↓

VPC

---

## Why Do We Use Internet Gateways?

Internet Gateways allow public resources to communicate with the internet.

Examples:

- Public EC2 instances
- Load Balancers
- Bastion Hosts

A VPC without an Internet Gateway cannot communicate directly with the internet.

---

## How Traffic Uses an Internet Gateway

A public subnet requires:

1. An Internet Gateway attached to the VPC
2. A route table with an internet route
3. A resource with a public IP address

Example route:


Destination: 0.0.0.0/0

Target: Internet Gateway


---

## Traffic Flow Example


User

↓

Internet

↓

Internet Gateway

↓

Public Subnet

↓

EC2 Instance


---

## Troubleshooting

If a public EC2 instance cannot reach the internet:

1. Check Internet Gateway attachment
2. Check subnet route table
3. Check public IP address
4. Check Security Groups
5. Check Network ACLs

---

## Connection to Linux Networking

Linux:


Computer
↓
Routing Table
↓
Gateway
↓
Internet


AWS:


EC2
↓
Route Table
↓
Internet Gateway
↓
Internet


The concept is the same:

Traffic needs a path to reach its destination.
