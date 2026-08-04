# HTTP Request Flow to EC2 Nginx Server

## Request Path

A user's browser requests the EC2 public DNS address.

Traffic flow:

Browser

↓

Internet

↓

Internet Gateway

↓

Route Table

↓

Network ACL

↓

Security Group

TCP Port 80 Allowed

↓

EC2 Ubuntu Instance

↓

Nginx Web Server

↓

HTML Response

## Security Group Configuration

HTTP traffic is allowed:

- Protocol: TCP
- Port: 80
- Source: 0.0.0.0/0

This allows public web traffic while keeping other ports closed.

## SSH vs HTTP

SSH:

- Port 22
- Restricted to administrator IP
- Used for server management

HTTP:

- Port 80
- Public access
- Used to deliver website content
