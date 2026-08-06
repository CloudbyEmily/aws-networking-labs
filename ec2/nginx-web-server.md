# Nginx Web Server Deployment

## Overview

Deployed an Nginx web server on an AWS EC2 Ubuntu instance.

The EC2 instance now serves a webpage publicly through HTTP traffic.

## Installation

Installed Nginx:

    sudo apt update
    sudo apt install nginx

## Service Verification

Checked the Nginx service:

    systemctl status nginx

Result:

- nginx.service is active
- Web server is running

## Port Verification

Confirmed Nginx is listening on HTTP port 80:

    sudo ss -tulpn | grep nginx

Result:

- TCP port 80 is listening
- Nginx is accepting web requests

## Local Testing

Tested from inside the EC2 instance:

    curl localhost

Received the default Nginx HTML page.

## Public Access

Updated the Security Group:

Inbound Rule:

| Type | Protocol | Port | Source |
|---|---|---|---|
| HTTP | TCP | 80 | 0.0.0.0/0 |

The web server became publicly accessible through the EC2 public DNS name.

## Architecture

Internet users can now reach:

Internet → Internet Gateway → Route Table → Security Group → EC2 → Nginx → Web Page
## Verification

After installing Nginx, I verified the web server was running correctly.

### Verify service status

```bash
systemctl status nginx
```

Result:

```
Active: active (running)
```

### Verify Nginx is listening on port 80

```bash
sudo ss -tulpn | grep nginx
```

Result:

```
tcp LISTEN 0 511 0.0.0.0:80
```

This confirmed that the Nginx service was actively listening for incoming HTTP requests.

### Verify locally

```bash
curl localhost
```

The command returned the default **"Welcome to nginx!"** page, confirming the web server was responding on the EC2 instance itself.

### Verify from the Internet

After adding an inbound HTTP (TCP port 80) rule to the EC2 Security Group, I opened the EC2 Public DNS in a web browser.

The browser displayed the default **"Welcome to nginx!"** page.

This confirmed the complete network path was working:

```
Internet
    ↓
Internet Gateway
    ↓
Route Table
    ↓
Public Subnet
    ↓
Security Group (HTTP port 80)
    ↓
EC2 Instance
    ↓
Nginx Web Server
```

## My Observations

- Installing Nginx also created and enabled the `nginx` service.
- `systemctl status nginx` confirmed the service was active and running.
- `ss -tulpn` showed that Nginx was listening on TCP port 80.
- `curl localhost` verified that the web server was functioning from within the EC2 instance.
- Accessing the Public DNS from my browser confirmed that external users could reach the web server through the Internet Gateway, route table, subnet, and Security Group.
