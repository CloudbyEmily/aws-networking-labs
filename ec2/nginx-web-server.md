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
