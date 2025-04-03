# VM Setup in Azure
Deployed a VM using the Azure Cloudshell:
- Image: Ubuntu Server 22.04 LTS
- Size: B1s (low cost for lab use)
- Resource Group: ubuntu-lab-rg
- VM Name: lowcostvm
- Networking: Default VNet and NSG with SSH open (port 22)

# Update System 
sudo apt update && sudo apt upgrade -y

# Import MongoDB GPG Key & Add Repo
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor

echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] \
https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | \
sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update

# Install MongoDB
sudo apt install -y mongodb-org


# Start and Enable MongoDB
sudo systemctl start mongod
sudo systemctl enable mongod

# Modify mongod.conf
sudo nano /etc/mongod.conf

Changes made:
net:
  port: 27017
  bindIp: 127.0.0.1

security:
  authorization: enabled

# Saved and exicted. Then restarted MongoDB
sudo systemctl restart mongod

# Create Admin User 
Entered MongoDB shell:
mongosh

Then:
use admin
db.createUser({
    user: "admin",
    pwd: "StrongP@ssw0rd123!",
    roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
})

Exited and verified authentication:
mongosh -u admin -p --authenticationDatabase admin

# Enable and Configure UFW
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw allow from <my-ip>/32 to any port 27017

Checked status:
sudo ufw status verbose

# Shutdown (Deallocate) VM to for Cost Savings
az vm deallocate --resource-group ubuntu-lab-rg --name lowcostvm

# Restart VM
az vm start --resource-group ubuntu-lab-rg --name lowcostvm
