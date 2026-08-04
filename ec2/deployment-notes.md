# EC2 Deployment Notes

## Objective

Deploy an Ubuntu EC2 instance inside a custom AWS VPC and make it accessible through SSH.

## Region

AWS Region:
us-east-1 (N. Virginia)

## Instance Details

Name:
ubuntu-web-server-1

Instance Type:
t3.micro

Operating System:
Ubuntu Server 26.04 LTS

## Networking

VPC:
my-vpc-1

VPC CIDR:
10.0.0.0/16

Subnet:
public-subnet-1

Subnet CIDR:
10.0.1.0/24

Availability Zone:
us-east-1a

Private IP:
10.0.1.228

## Internet Connectivity

Internet Gateway:
internet-gateway-1

Route:

0.0.0.0/0 → Internet Gateway

## Security Group

Security Group:
ubuntu-ssh-sg-1

Inbound Rule:

SSH
TCP
Port 22
Source:
My public IP address

## Result

Successfully deployed Ubuntu EC2 instance and verified SSH access.
