# ------------------------------------------------------------
# ✅ Python & Ansible prerequisites
# ------------------------------------------------------------

# Install required Python packages for GCP dynamic inventory
pip install requests google-auth google-api-python-client

# Install Ansible collection for Google Cloud
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
# ✅ Set Ansible configuration for AWS (optional)
# ------------------------------------------------------------

export ANSIBLE_INVENTORY=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/inventory_aws.yaml
export ANSIBLE_CONFIG=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/ansible.cfg

# ------------------------------------------------------------
# ✅ Set Ansible configuration for GCP
# ------------------------------------------------------------

export ANSIBLE_INVENTORY=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/inventory_gcp.yaml
export ANSIBLE_CONFIG=/Users/elvisngwesse/Desktop/Repositories/ansible/ansible-config/ansible.cfg

# ------------------------------------------------------------
# ✅ Test dynamic inventory and connectivity
# ------------------------------------------------------------

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
