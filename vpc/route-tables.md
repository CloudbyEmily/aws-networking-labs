# AWS Route Tables

## What is a Route Table?

A route table is a set of rules that determines where network traffic should go.

Every subnet in an AWS VPC is associated with a route table.

A route table answers the question:

"Where should this packet go next?"

This is similar to the Linux networking command:

```bash
ip route

which shows the routes configured on a Linux system.

Route Table Components

A route table contains:

Destination

The network address where traffic is trying to go.

Example:

0.0.0.0/0

This represents all IPv4 traffic.

Target

The destination where AWS sends the traffic.

Examples:

Local
Internet Gateway
NAT Gateway
Virtual Private Gateway
Local Route

Every VPC automatically receives a local route.

Example:

Destination:

10.0.0.0/16


Target:

local

This allows resources inside the VPC to communicate with each other.

Example:

EC2 Instance

10.0.1.10

        |

        |

Database

10.0.2.20

Traffic stays inside the VPC.

Public Subnet Route Table

A public subnet requires a route to an Internet Gateway.

Example:

Destination        Target

10.0.0.0/16        local

0.0.0.0/0          Internet Gateway

Traffic flow:

User

↓

Internet

↓

Internet Gateway

↓

Public Subnet

↓

EC2 Instance
Private Subnet Route Table

Private resources should not receive direct inbound internet traffic.

Instead, outbound traffic can use a NAT Gateway.

Example:

Destination        Target

10.0.0.0/16        local

0.0.0.0/0          NAT Gateway

Traffic flow:

Private EC2

↓

NAT Gateway

↓

Internet
Route Tables and Troubleshooting

When a resource cannot communicate, check:

Is the subnet associated with the correct route table?
Does the route table have the correct destination?
Is the target configured correctly?
Does the resource need an Internet Gateway or NAT Gateway?
Are security controls allowing traffic?
Connection to Linux Networking

Linux:

Packet

↓

Routing Table

↓

Gateway

↓

Destination

AWS:

Packet

↓

VPC Route Table

↓

Internet Gateway / NAT Gateway

↓

Destination

The concept is the same:

A router must know the next place to send traffic.

Key Concept

Subnets define where resources live.

Route tables define where traffic goes.

A properly configured route table allows resources to communicate with the correct destinations.
