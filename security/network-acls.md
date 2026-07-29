# AWS Network ACLs

## What is a Network ACL?

A Network Access Control List (NACL) is a subnet-level firewall that controls traffic entering and leaving a subnet.

Network ACLs protect groups of resources inside a subnet.

They answer:

"Is this subnet allowed to communicate?"

---

## Security Groups vs Network ACLs

Security Groups:

- Resource-level firewall
- Attached to resources
- Stateful
- Allow rules only

Network ACLs:

- Subnet-level firewall
- Protects all resources in a subnet
- Stateless
- Allows and denies rules

---

## How Network ACLs Work

Inbound traffic:


Internet

↓

Network ACL

↓

Subnet

↓

Resource


Outbound traffic:


Resource

↓

Subnet

↓

Network ACL

↓

Internet


---

## Example Rule

Allow HTTP traffic:


Rule:

Protocol: TCP

Port: 80

Action: Allow


A matching deny rule would block the traffic.

---

## Stateless Behavior

Network ACLs are stateless.

This means inbound and outbound traffic are evaluated separately.

Example:

A user connects to a web server:


Inbound Request

↓

NACL Check

↓

Server Response

↓

Outbound NACL Check


Both directions must be allowed.

---

## Troubleshooting

If traffic cannot reach a subnet:

1. Check Network ACL rules
2. Check Security Groups
3. Check route tables
4. Check subnet placement
5. Check application status

---

## Key Concept

Security Groups protect resources.

Network ACLs protect subnets.

Together they provide multiple layers of network security.
