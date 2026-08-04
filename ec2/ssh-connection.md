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
## My Observations

- The ChromeOS Downloads folder was separate from my Linux container filesystem.
- I had to copy the EC2 private key into my Linux environment before SSH could access it.
- EC2 Instance Connect failed, so I diagnosed the issue by checking networking components.
- Successful SSH changed my terminal from my local machine:

  itsemilyelisabethb@penguin

  to the remote EC2 server:

  ubuntu@ip-10-0-1-228

- This confirmed I was now administering a cloud-hosted Linux machine.
