# AWS Security Groups

## What is a Security Group?

A Security Group is a resource-level firewall that controls inbound and outbound traffic for AWS resources.

Security Groups are attached to resources such as:

- EC2 instances
- Load Balancers
- Databases

A Security Group decides:

"Is this traffic allowed to reach this resource?"

---

## How Security Groups Work

Security Groups use rules that define:

- Protocol
- Port
- Source or destination

Example:


Type: SSH

Protocol: TCP

Port: 22

Source: My IP


This allows SSH access from an approved location.

---

## Inbound Rules

Inbound rules control traffic coming into a resource.

Example:


Internet

↓

Security Group

↓

EC2 Instance


If the inbound rule does not allow the traffic, the connection is blocked.

---

## Outbound Rules

Outbound rules control traffic leaving a resource.

Example:


EC2 Instance

↓

Security Group

↓

Internet


---

## Security Group Characteristics

Security Groups are:

- Stateful
- Resource-level
- Allow rules only

Stateful means return traffic is automatically allowed.

Example:

A user connects to a web server:


User Request

↓

EC2

↓

Response Back


The response is automatically allowed.

---

## Troubleshooting

If a service is running but users cannot connect:

1. Check Security Group rules
2. Check the correct port is open
3. Check source IP permissions
4. Check subnet routing
5. Check application status

---

## Key Concept

Security Groups protect individual AWS resources.

They answer:

"Is this resource allowed to communicate?"
