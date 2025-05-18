


Install required Python packages:
pip install requests google-auth google-api-python-client
ansible-galaxy collection install google.cloud

Enable the Compute Engine API in your GCP project
# Enable the Compute Engine API
gcloud services enable compute.googleapis.com

Clone terraform project
git clone git@github.com:Elvis-Ngwesse/terraform.git
cd Create-VirtualMachine/google
edit number of compute engines to 3
run terraform commands found on terraform.md file


export ANSIBLE_INVENTORY=/Users/elvisngwesse/Desktop/Repositories/ansible/inventory_gcp/inventory_gcp.yaml
export ANSIBLE_CONFIG=/Users/elvisngwesse/Desktop/Repositories/ansible/inventory_gcp/ansible.cfg

ansible-inventory --graph
ansible-inventory --list
ansible all -m ping -vv
ansible all -m ping


📁 inventory_gcp/inventory_gcp.yaml
This is a dynamic inventory plugin configuration for Ansible that:
Automatically discovers GCP instances in the new-devops-project-1 project
Filters instances with the label ansible-managed=true
Groups hosts by zone and name prefix
Requires a service account JSON key to authenticate with GCP

📄 ansible.cfg
This is the Ansible configuration file that:
Sets the dynamic inventory source to inventory_gcp/inventory_gcp.yaml
Disables SSH host key checking (for smoother automation)
Sets the default SSH user and private key for remote connections
Enables necessary inventory plugins like gcp_compute


cd inventory-01
ansible-playbook google/list_directories.yaml
ansible-playbook google/list_installed_software.yaml
