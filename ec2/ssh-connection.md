# SSH Connection to EC2

## Purpose

Connect from a local Linux environment to an AWS EC2 Ubuntu instance using SSH key authentication.

## Key Setup

The EC2 private key was downloaded through the AWS Console.

The key was copied from ChromeOS Downloads into the Linux environment:

cp /mnt/chromeos/MyFiles/Downloads/key-pair-1.pem ~/

Permissions were restricted:

chmod 400 ~/key-pair-1.pem

## SSH Command

Connection command:

ssh -i ~/key-pair-1.pem ubuntu@public-dns-name

## Successful Connection

After connecting, the terminal changed from:

itsemilyelisabethb@penguin

to:

ubuntu@ip-10-0-1-228

This confirmed remote access to the AWS EC2 instance.
