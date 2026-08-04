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
