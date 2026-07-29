# AWS Request Flow

## How Traffic Moves Through AWS

Cloud engineers need to understand where traffic travels and where it can stop.

A request moves through multiple layers:


User
↓
DNS
↓
AWS Networking
↓
Application
↓
Database


---

## Example: User Accesses a Website

A user enters:


www.example.com


The request begins with DNS.

DNS translates the website name into an IP address.


User

↓

DNS

↓

IP Address


---

## Public AWS Traffic Flow

A common public application flow:


User

↓

Internet

↓

Internet Gateway

↓

Public Subnet

↓

Load Balancer

↓

Application Servers


The route table determines where traffic goes.

---

## Private Application Traffic

Many applications separate public and private resources.

Example:


Load Balancer

↓

Private Application Server

↓

Database


Private resources are protected from direct internet access.

---

## Where Traffic Can Stop

If a request fails, investigate each layer:

1. DNS
2. Route Tables
3. Internet Gateway
4. NAT Gateway
5. Security Groups
6. Network ACLs
7. Application Services
8. Database Connections

---

## Cloud Troubleshooting Mindset

The goal is not to memorize every service.

The goal is to find:

"Where did traffic stop?"

Then identify:

"Why did traffic stop?"

---

## Key Concept

AWS networking is a chain.

If one part of the chain fails, communication fails.

Understanding traffic flow helps engineers troubleshoot problems quickly.
