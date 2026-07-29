# AWS Networking Troubleshooting

## Troubleshooting Mindset

Cloud engineers troubleshoot by finding where traffic stopped.

A request travels through multiple layers:


User
↓
DNS
↓
AWS Networking
↓
Security Controls
↓
Application
↓
Database


---

## Website Is Unavailable

Possible checks:

1. Check DNS resolution
2. Check Internet Gateway
3. Check Route Tables
4. Check Security Groups
5. Check Load Balancer health
6. Check application service status

---

## Private EC2 Cannot Reach Internet

Check:

1. Is a NAT Gateway configured?
2. Does the private subnet route to the NAT Gateway?
3. Does the NAT Gateway subnet have an Internet Gateway route?
4. Are outbound rules allowed?

---

## Users Can Access Website But Cannot Load Data

Possible causes:

- Database connection failure
- Security Group blocking database traffic
- Application configuration issue
- Database performance problems

---

## Service Is Running But Users Cannot Connect

Check:

1. Is the correct port open?
2. Is the Security Group allowing traffic?
3. Is the Network ACL allowing traffic?
4. Is the application listening on the correct interface?
5. Check logs

---

## Troubleshooting Layers

Think in layers:


Layer 1:
DNS

Layer 2:
AWS Networking

Layer 3:
Security

Layer 4:
Operating System

Layer 5:
Application

Layer 6:
Database


---

## Key Concept

Do not guess.

Follow the traffic path.

Find where communication stopped.

Then fix the cause.
