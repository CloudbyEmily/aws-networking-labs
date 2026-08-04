# SSH Packet Flow

## Connection

Source:
Local Linux terminal

Destination:
EC2 Public DNS

Protocol:
SSH

Port:
22/TCP

## Flow

1. User runs SSH command.
2. DNS resolves EC2 hostname to public IP.
3. Packet travels across the Internet.
4. AWS Internet Gateway receives traffic.
5. Route table directs traffic to the subnet.
6. Network ACL evaluates traffic.
7. Security Group allows TCP port 22.
8. EC2 network interface receives packet.
9. SSH service authenticates the user.
10. User gains access to Ubuntu server.
## My Observations

The SSH connection demonstrated how multiple AWS networking components work together:

- DNS translated the EC2 hostname into a public IP address.
- The Internet Gateway allowed communication between the VPC and the Internet.
- The route table directed external traffic toward the Internet Gateway.
- The Security Group allowed inbound SSH traffic on TCP port 22.
- The EC2 instance received the packet through its network interface and authenticated my SSH key.
