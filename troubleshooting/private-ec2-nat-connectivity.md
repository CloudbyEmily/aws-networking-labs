
# Private EC2 → NAT Gateway → Internet Troubleshooting Lab

## Overview

This lab demonstrates how a private EC2 instance can access the public
internet without having a public IPv4 address.

The primary objective is to understand and troubleshoot the complete traffic
path:

Private EC2
↓
Private Subnet
↓
Private Route Table
↓
NAT Gateway
↓
Public Subnet
↓
Internet Gateway
↓
Internet

This lab also demonstrates how AWS Systems Manager (SSM) can provide
administrative access to a private EC2 instance without requiring SSH from
the public internet.

---

# 1. Lab Objectives

By completing this lab, I demonstrated that I can:

- Build a VPC with public and private subnets
- Configure route tables
- Deploy a NAT Gateway
- Understand public vs. private subnet routing
- Launch an EC2 instance without a public IP
- Configure EC2 security groups
- Configure IAM permissions for Systems Manager
- Connect to a private EC2 instance using SSM
- Verify that the SSM Agent is running
- Troubleshoot DNS resolution
- Verify HTTPS connectivity
- Verify the EC2 routing table
- Understand how NAT enables private resources to reach the internet
- Trace network traffic through AWS infrastructure

---

# 2. AWS Architecture

VPC: my-vpc-1
CIDR: 10.0.0.0/16

                         INTERNET
                             │
                             │
                    Internet Gateway
                             │
                             │
                 ┌───────────┴───────────┐
                 │                       │
          Public Subnet            Private Subnet
          10.0.1.0/24              10.0.2.0/24
                 │                       │
          NAT Gateway              Private EC2
          10.0.1.240               10.0.2.247
                 │
                 │
          Public Route Table
          0.0.0.0/0 → IGW
                 │
                 │
          Internet Gateway
                 │
                 ▼
              Internet

Private EC2 route:

0.0.0.0/0 → NAT Gateway

Public subnet route:

0.0.0.0/0 → Internet Gateway

3. VPC Configuration

VPC:

Name: my-vpc-1
VPC ID: vpc-027c476b691d8e426
CIDR: 10.0.0.0/16
DNS resolution: Enabled
DNS hostnames: Enabled

DNS support is important because the private EC2 must be able to resolve
hostnames such as:

archive.ubuntu.com

4. Subnet Configuration
Public Subnet

Name:

public-subnet-1

CIDR:

10.0.1.0/24

Availability Zone:

us-east-1a

Route table:

public route table

The public subnet contains the NAT Gateway.

Private Subnet

Name:

private-subnet-1

CIDR:

10.0.2.0/24

Availability Zone:

us-east-1a

Route table:

private-route-table-1

Auto-assign public IPv4:

Disabled

The private EC2 instance is located here.

5. NAT Gateway

NAT Gateway:

nat-gateway-1

NAT Gateway ID:

nat-0c63e30c249cc89ec

Connectivity type:

Public

Subnet:

public-subnet-1

Private IP:

10.0.1.240

Public Elastic IP:

23.22.155.144

State:

Available

The NAT Gateway must be located in a public subnet because it needs a route
to the Internet Gateway.

Important distinction:

The PRIVATE EC2 is in the private subnet.

The NAT Gateway is in the PUBLIC subnet.

The private EC2 does not connect directly to the Internet Gateway.

6. Private Route Table

Route table:

private-route-table-1

Route table ID:

rtb-0351047c35c4f328f

Associated subnet:

private-subnet-1

Routes:

10.0.0.0/16 → local

0.0.0.0/0 → NAT Gateway

The important route is:

0.0.0.0/0 → nat-0c63e30c249cc89ec

This means:

"For destinations outside the VPC, send the traffic to the NAT Gateway."

7. Public Route Table

Route table:

rtb-04efa46a4f1b13127

Associated subnet:

public-subnet-1

Routes:

10.0.0.0/16 → local

0.0.0.0/0 → Internet Gateway

The important route is:

0.0.0.0/0 → igw-0711902da3cc4e895

This gives the NAT Gateway a path toward the internet.

8. Private EC2

Instance:

private-ec2-1

Instance ID:

i-06df839ccd2bedb58

Private IPv4:

10.0.2.247

Public IPv4:

None

Subnet:

private-subnet-1

VPC:

my-vpc-1

Instance type:

t3.micro

Operating system:

Ubuntu

The lack of a public IPv4 address is intentional.

This means the instance cannot be directly SSHed into from the public
internet.

9. Security Group

Security group:

private-ec2-secgroup

Inbound:

No public SSH access was configured.

Outbound:

All traffic → 0.0.0.0/0

The outbound security group rule allows the EC2 instance to initiate
connections.

Security Groups are stateful, so return traffic for an allowed connection
is automatically permitted by the Security Group.

10. Network ACL

Private subnet Network ACL:

acl-0e14473cf3eb7f8d6

Inbound:

100 → All traffic → 0.0.0.0/0 → ALLOW

Outbound:

100 → All traffic → 0.0.0.0/0 → ALLOW

The default "*" deny rule occurs after rule 100, so rule 100 permits the
traffic.

Because NACLs are stateless, inbound and outbound traffic must independently
be permitted.

11. Systems Manager Access

The private EC2 does not have a public IP.

Therefore, normal direct SSH access from the internet is not available.

Instead, AWS Systems Manager Session Manager was configured.

IAM role:

private-ec2-ssm-role

The role provides the EC2 instance with permissions required for Systems
Manager.

The instance was then successfully registered with Systems Manager.

This allowed us to connect to the private EC2 without opening SSH port 22
to the internet.

12. SSM Agent Verification

After connecting to the private EC2 through Session Manager, the SSM Agent
was verified with:

sudo snap services amazon-ssm-agent

Result:

Service Startup Current Notes
amazon-ssm-agent.amazon-ssm-agent enabled active -

This confirms:

Startup: enabled

Current: active

Therefore, the SSM Agent is installed and running.

13. DNS Troubleshooting

From the private EC2:

nslookup archive.ubuntu.com

The instance returned valid DNS records.

Example:

Server: 127.0.0.53
Address: 127.0.0.53#53

Non-authoritative answer:

archive.ubuntu.com
→ archive.ubuntu.com.cdn.cloudflare.net

IPv4 addresses were returned.

This demonstrated that DNS resolution was working.

The local address:

127.0.0.53

is Ubuntu's local DNS stub resolver.

It forwards DNS queries through the configured resolver infrastructure.

14. HTTPS Connectivity Test

The following command was used:

curl -I https://archive.ubuntu.com

The result included:

HTTP/2 200

This is significant.

The EC2 successfully:

Resolved archive.ubuntu.com using DNS
Created an HTTPS connection
Sent an HTTP HEAD request
Received a successful HTTP response

Therefore, the private EC2 has working outbound HTTPS connectivity.

15. NAT Traffic Flow

The private EC2 has:

Private IP:

10.0.2.247

When it accesses an internet destination such as:

archive.ubuntu.com:443

the traffic follows this general path:

10.0.2.247:ephemeral-port
│
▼
Private subnet
│
▼
Private route table
│
│ 0.0.0.0/0 → NAT Gateway
▼
NAT Gateway
10.0.1.240
│
▼
NAT translates the private source
│
▼
NAT public Elastic IP
23.22.155.144
│
▼
Internet Gateway
│
▼
Internet
│
▼
archive.ubuntu.com:443

The response returns through the NAT Gateway, which maintains the translation
and sends the response back to the private EC2.

16. DNS vs HTTPS

A critical troubleshooting distinction demonstrated in this lab:

DNS resolution and HTTPS connectivity are different problems.

DNS:

archive.ubuntu.com
↓
DNS
↓
IP address

HTTPS:

10.0.2.247:ephemeral-port
↓
destination-IP:443

If DNS fails, the EC2 may never obtain the destination IP.

If DNS works but HTTPS fails, the problem may instead involve:

Route tables
NAT Gateway
Internet Gateway
Security Groups
NACLs
Destination availability
Port 443
Application/service availability
17. EC2 Routing Table Verification

The EC2 routing table was checked with:

ip route

Result:

default via 10.0.2.1 dev ens5 proto dhcp src 10.0.2.247 metric 100

10.0.0.2 via 10.0.2.1 dev ens5 proto dhcp src 10.0.2.247 metric 100

10.0.2.0/24 dev ens5 proto kernel scope link src 10.0.2.247 metric 100

10.0.2.1 dev ens5 proto dhcp scope link src 10.0.2.247 metric 100

The important entry is:

default via 10.0.2.1

This is the EC2 operating system's default gateway.

AWS then applies the subnet's VPC route table to determine where traffic
should go next.

The subnet route table contains:

0.0.0.0/0 → NAT Gateway

Therefore:

EC2
→ subnet gateway
→ VPC routing
→ NAT Gateway
→ Internet Gateway
→ Internet

18. EC2 Network Interface Verification

The EC2 network interface was checked with:

ip addr

The primary interface was:

ens5

The instance has:

10.0.2.247/24

This confirms that the EC2 is attached to the private subnet's
10.0.2.0/24 network.

19. apt update Verification

The final real-world test was:

sudo apt update

The command successfully downloaded approximately:

31.2 MB

and completed with:

112 packages can be upgraded.

This is strong evidence that the private EC2 successfully reached Ubuntu
package repositories over the internet.

The traffic required:

DNS resolution
+
HTTPS connectivity
+
NAT
+
Internet Gateway
+
working routing
+
working security controls

20. Troubleshooting Method

The most important lesson from this lab is to follow the traffic instead
of guessing.

When a private EC2 cannot reach the internet, investigate in this order:

Step 1 — Instance

Is the EC2 running?

Is the network interface up?

ip addr

Step 2 — Local Routing

Check:

ip route

Confirm a default route exists.

Step 3 — DNS

Test:

nslookup archive.ubuntu.com

If DNS fails, investigate DNS before investigating HTTPS.

Step 4 — Private Route Table

Verify:

0.0.0.0/0 → NAT Gateway

Step 5 — NAT Gateway

Verify:

State = Available
Located in public subnet
Public connectivity type
Elastic IP attached
Step 6 — Public Route Table

Verify:

0.0.0.0/0 → Internet Gateway

Step 7 — Security Group

Verify outbound traffic is permitted.

Step 8 — Network ACL

Verify both inbound and outbound traffic are permitted.

Remember:

NACLs are stateless.

Step 9 — HTTPS

Test:

curl -I https://archive.ubuntu.com

A successful:

HTTP/2 200

demonstrates successful HTTPS communication.

21. What This Lab Proved

This lab proved that a private EC2 instance can access the public internet
without having a public IPv4 address.

The private EC2:

Has no public IP
Lives in a private subnet
Uses a private route table
Routes internet-bound traffic to a NAT Gateway
Uses the NAT Gateway's public address for internet communication
Uses the Internet Gateway from the NAT Gateway's public subnet
Successfully resolves DNS
Successfully establishes HTTPS connections
Successfully runs apt update

The lab also demonstrated that administrative access does not require
opening SSH to the internet.

Systems Manager Session Manager provided access to the private instance.

22. Key Architecture Principle

A private subnet does NOT mean:

"No internet communication is possible."

It means:

"The resources do not have a direct public route through an Internet
Gateway."

A private EC2 can still reach the internet through:

Private EC2
↓
Private Route Table
↓
NAT Gateway
↓
Public Route Table
↓
Internet Gateway
↓
Internet

This is one of the fundamental patterns used in AWS architectures.

23. Senior Engineer Troubleshooting Mindset

The goal is not to memorize:

"NAT Gateway gives private instances internet access."

The goal is to be able to prove it.

For every connection, ask:

Source
↓
Destination
↓
Route
↓
Port
↓
Protocol
↓
Security controls
↓
Response path

For this lab:

Source:

10.0.2.247

Destination:

archive.ubuntu.com

Protocol:

HTTPS / TCP

Destination port:

443

Route:

Private EC2
→ Private Route Table
→ NAT Gateway
→ Internet Gateway
→ Internet

Security:

Security Group
+
Network ACL

Return traffic:

Internet
→ NAT Gateway
→ Private EC2

This traffic-flow approach is the foundation of AWS networking
troubleshooting.

24. Current Lab Status

COMPLETE:

VPC
Public subnet
Private subnet
Internet Gateway
Public route table
Private route table
NAT Gateway
Private EC2
Private EC2 security group
Network ACL verification
IAM role for SSM
SSM connectivity
SSM Agent verification
DNS verification
EC2 route verification
HTTPS verification
apt update verification

Current conclusion:

PRIVATE EC2 → NAT GATEWAY → INTERNET

is working successfully.
