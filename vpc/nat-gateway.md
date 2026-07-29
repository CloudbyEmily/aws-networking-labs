# AWS NAT Gateway

## What is a NAT Gateway?

A NAT Gateway allows resources in a private subnet to access the internet without allowing direct inbound internet connections.

NAT stands for:

Network Address Translation

It provides a controlled path for private resources to reach the outside world.

---

## Why Use a NAT Gateway?

Private resources may need internet access for:

- Software updates
- Installing packages
- Connecting to external services

Examples:

- Application servers
- Private EC2 instances
- Internal workloads

---

## Private Subnet Traffic

A private subnet uses a route table pointing internet traffic to a NAT Gateway.

Example:


Destination: 0.0.0.0/0
Target: NAT Gateway


Traffic flow:


Private EC2

↓

Private Route Table

↓

NAT Gateway

↓

Internet Gateway

↓

Internet


---

## NAT Gateway Security Benefit

Private resources can make outbound connections.

However, users on the internet cannot directly start connections to those private resources.

---

## Troubleshooting

If a private EC2 instance cannot reach the internet:

1. Check NAT Gateway exists
2. Check private route table
3. Check NAT Gateway subnet
4. Check Internet Gateway
5. Check Security Groups and NACLs

---

## Key Concept

Internet Gateway:


Public resources → Internet


NAT Gateway:


Private resources → Internet


Both control how AWS resources communicate outside the VPC.
