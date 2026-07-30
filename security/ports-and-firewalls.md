# Ports and Firewalls

## What Are Ports?

Ports are communication endpoints that allow a computer to direct network traffic to the correct service.

A server uses an IP address to identify the machine and a port number to identify the specific service receiving traffic.

Example:


Server IP: 203.0.113.10
Port: 443
Protocol: TCP
Service: HTTPS


Garden analogy:

- IP address = the garden's address
- Port = the specific gate
- Service = the person answering the gate

---

# TCP and UDP

## TCP

TCP is connection-oriented and focuses on reliability.

Examples:

- SSH
- HTTP
- HTTPS

TCP verifies that data arrives correctly.

## UDP

UDP is connectionless and focuses on speed and simplicity.

Examples:

- DNS
- DHCP

UDP does not establish a connection before sending data.

---

# Common Network Ports

| Service | Protocol | Port |
|---|---|---|
| SSH | TCP | 22 |
| HTTP | TCP | 80 |
| HTTPS | TCP | 443 |
| DNS | UDP/TCP | 53 |
| DHCP Client | UDP | 68 |

---

# AWS Security Groups vs Linux Firewalls

## AWS Security Groups

Security Groups operate at the AWS network level.

They control whether traffic is allowed to reach an AWS resource.

Example:

Allow:


TCP
Port 443
Source: Internet


The traffic can reach the server.

---

## Linux Firewall

A Linux firewall operates inside the server.

Traffic may reach the machine but still be blocked by the operating system.

Example:


Internet
|
Security Group ✅
|
Linux Firewall ❌
|
Service


---

# Troubleshooting Ports and Services

When troubleshooting connectivity, follow the packet path.

## Step 1: Check Listening Ports

Command:

```bash
ss -tuln

Flags:

-t = TCP
-u = UDP
-l = Listening
-n = Numeric ports

Example:

tcp LISTEN :22

means a service is listening for SSH connections.

Step 2: Test Web Connectivity

Command:

curl https://example.com

This tests:

DNS resolution
Network connectivity
HTTPS communication
Web server response

View headers:

curl -I https://example.com

Common responses:

200 = Success
403 = Forbidden
404 = Not Found
500 = Server Error
Step 3: Test DNS

Commands:

nslookup example.com

or:

dig example.com

DNS translates hostnames into IP addresses.

DNS commonly uses:

UDP Port 53
Step 4: Check Services

Command:

systemctl status service-name

Example:

systemctl status ssh

A service can exist but not be running.

Step 5: Review Logs

Command:

journalctl -u service-name

Logs help identify why a service failed.

Example:

journalctl -u ssh

If a service never starts, there may not be service-specific logs. Check:

service status
startup conditions
system logs
Troubleshooting Example: SSH Failure

Problem:

"User cannot SSH into a server."

Check:

Is port 22 listening?
ss -tuln
Is SSH running?
systemctl status ssh
Review logs:
journalctl -u ssh

Possible causes:

Security Group blocks port 22
Linux firewall blocks traffic
SSH service is stopped
No service is listening on port 22
