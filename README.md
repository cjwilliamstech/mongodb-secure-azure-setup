# MongoDB Secure Setup on Azure Ubuntu VM

This is a personal lab project where I manually installed and secured MongoDB 6.0 on an Ubuntu 22.04 LTS virtual machine in Microsoft Azure. The goal was to practice Linux system administration, database configuration, and basic security hardening in a cloud environment.

## Stack Used

- Azure Virtual Machine (Ubuntu 22.04 LTS)
- MongoDB 6.0 (Community Edition)
- UFW (Uncomplicated Firewall)
- SSH (via Azure Cloud Shell and VS Code)

## Project Overview

### Key Tasks Completed

- Installed MongoDB 6.0 from the official repository
- Configured `mongod.conf` for secure IP binding and authentication
- Created an admin user with appropriate roles
- Verified authentication using `mongosh`
- Enabled and configured UFW to limit open ports
- Allowed optional access to MongoDB from a trusted private IP
- Deallocated VM in Azure to manage lab costs

## Commands & Configuration

### Add MongoDB GPG Key and Repo

```bash
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor

echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] \
https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | \
sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update
sudo apt install -y mongodb-org