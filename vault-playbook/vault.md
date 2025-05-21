# -----------------------------
# Create a YAML File with Secrets
# -----------------------------

# Step 1: Create a file with sensitive variables
# Example: secrets.yml

mysql_root_password: your_root_password
mysql_db_user: your_user
mysql_db_password: your_password
mysql_db_name: your_db

# -----------------------------
# Encrypt the File Using Ansible Vault
# -----------------------------

# Run this command to encrypt the file:
ansible-vault encrypt secrets.yml

# You'll be prompted:
# enter password
# confirm password

# To confirm encryption, double-click the file or open it in a text editor.
# You should see something like:
# $ANSIBLE_VAULT;1.1;AES256 ...

# -----------------------------
# Create a Playbook That Uses the Encrypted File
# -----------------------------

# Store your vault password in a file:
# Example: vault_password.txt

# Run your playbook:
ansible-playbook vault-playbook/vault-secret.yaml --ask-vault-pass

# Or run it using the vault password file:
ansible-playbook vault-playbook/vault-secret.yaml \
--vault-password-file=~/Desktop/Repositories/ansible/vault-playbook/vault_password.txt

# -----------------------------
# To Decrypt the File (Optional)
# -----------------------------

ansible-vault decrypt secrets.yml

# -----------------------------
# Remote Machine Setup
# -----------------------------

# Create an encrypted secret file specifically for the remote machine:
# Example: vault_secrets.yml

ansible-vault encrypt vault_secrets.yml

# -----------------------------
# Create install_mysql_docker.yaml Playbook
# -----------------------------

# This is your main playbook to install Docker and MySQL, and run a container.

ansible-playbook vault-playbook/install_mysql_docker.yaml --ask-vault-pass

# Or with password file:
ansible-playbook vault-playbook/install_mysql_docker.yaml \
--vault-password-file=~/Desktop/Repositories/ansible/vault-playbook/vault_password.txt

# -----------------------------
# Manual Verification on Remote Machine
# -----------------------------

# SSH into your instance:
ssh -i /path/to/your-key.pem ubuntu@<REMOTE_IP>

# List running containers:
docker ps

# List all containers (including stopped ones):
docker ps -a

# Check logs for the MySQL container:
docker logs mysql_server

# Access the container:
docker exec -it mysql_server bash

# Log in to MySQL inside the container:
mysql -u root -p

# Example SQL commands:
SHOW DATABASES;
USE your_database_name;
SELECT * FROM your_table;

# Exit MySQL:
exit

# Exit container shell:
exit
