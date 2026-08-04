# EC2 Troubleshooting Notes

## Issue 1: EC2 Instance Connect Failed

Problem:

Browser-based EC2 Instance Connect failed with an SSH connection error.

## Investigation

Checked:

- Instance state: Running
- Status checks: Passed
- Internet Gateway attached
- Route table configured
- Security Group allowed SSH
- Public IP existed
- DNS hostnames enabled

## Resolution

Used SSH from local Linux terminal with the EC2 key pair.

Command:

ssh -i ~/key-pair-1.pem ubuntu@public-dns-name

Result:

Successfully connected.
