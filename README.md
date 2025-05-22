________________________
# Create virtual env
________________________
install python
python3 -m venv venv
source venv/bin/activate

# ------------------------------------------------------------
# ✅ Python & Ansible prerequisites
# ------------------------------------------------------------
# Install GCP SDK and API dependencies
pip install requests google-auth google-api-python-client

# Install AWS SDK for Python
pip install boto3 botocore

# Install the GCP Ansible collection
ansible-galaxy collection install google.cloud


# ------------------------------------------------------------
# ✅ Enable GCP Compute Engine API
# ------------------------------------------------------------

# Must be run in your active GCP project
gcloud services enable compute.googleapis.com

# ------------------------------------------------------------
# ✅ Clone and apply Terraform configuration
# ------------------------------------------------------------

# Clone the Terraform repo
git clone git@github.com:Elvis-Ngwesse/terraform.git

# Navigate to Google Cloud VM directory
cd Create-VirtualMachine/google

# Edit Terraform code to provision 3 compute engine instances
# Then follow steps in terraform.md to deploy:
# - terraform init
# - terraform apply

# ------------------------------------------------------------
# ✅ Set Ansible configuration for AWS
# Depending on which cloud provider you're targeting, export the correct inventory and config file paths.
# Make sure to run command in same directory as playbook command to be run
# ------------------------------------------------------------
export ANSIBLE_INVENTORY=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/inventory_aws_ec2.yaml
export ANSIBLE_CONFIG=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/ansible.cfg

# ------------------------------------------------------------
# ✅ Set Ansible configuration for GCP
# Depending on which cloud provider you're targeting, export the correct inventory and config file paths.
# Make sure to run command in same directory as playbook command to be run
# ------------------------------------------------------------
export ANSIBLE_INVENTORY=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/inventory_gcp.yaml
export ANSIBLE_CONFIG=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/ansible.cfg

# ------------------------------------------------------------
# ✅ Remove cache before executing or switching between providers
# ------------------------------------------------------------
rm -rf ansible-config/facts_cache

# ------------------------------------------------------------
# ✅ Test dynamic inventory and connectivity
# ------------------------------------------------------------
# aws key
chmod 400 my-ssh-key.pem

# Show inventory as a host graph
ansible-inventory --graph

# Show full dynamic inventory as JSON
ansible-inventory --list

# Ping all hosts with verbose output
ansible all -m ping -vv

# Basic ping test to verify connectivity
ansible all -m ping

# ------------------------------------------------------------
# ✅ Run Ansible playbooks
# ------------------------------------------------------------
# List directories on remote hosts
ansible-playbook basic-playbook/list_directories.yaml

# List installed software
ansible-playbook basic-playbook/list_installed_software.yaml

# Update all packages on remote hosts
ansible-playbook basic-playbook/update_servers.yaml

# Create user
ansible-playbook create-user-playbook/create_user.yaml

# Display commands
ansible-playbook commands-playbook/unix_commands.yaml

# Nginx commands
ansible-playbook nginx-playbook/nginx.yaml

# Vault commands
ansible-playbook vault-playbook/vault-secret.yaml --ask-vault-pass
ansible-playbook vault-playbook/vault-secret.yaml \
--vault-password-file=~/Desktop/Repositories/ansible/vault-playbook/vault_password.txt

ansible-playbook vault-playbook/install_mysql_docker.yaml --ask-vault-pass
ansible-playbook vault-playbook/install_mysql_docker.yaml \
--vault-password-file=~/Desktop/Repositories/ansible/vault-playbook/vault_password.txt

# Nginx commands
ansible-playbook roles-playbook/roles.yaml


# ------------------------------------------------------------
# 📁 File: ansible-config/inventory_gcp.yaml
# ------------------------------------------------------------
# 🔹 GCP dynamic inventory plugin for Ansible
# - Automatically discovers GCP instances in `new-devops-project-1`
# - Filters instances with the label `ansible-managed=true`
# - Groups hosts by zone and name prefix
# - Requires a service account JSON key for authentication

# ------------------------------------------------------------
# 📄 File: ansible.cfg
# ------------------------------------------------------------
# 🔹 Ansible configuration file
# - Sets dynamic inventory to inventory_gcp.yaml
# - Disables SSH host key checking
# - Sets default SSH user and key
# - Enables GCP and other inventory plugins
# - Configures fact caching, callbacks, and privilege escalation

