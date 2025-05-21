

Create a YAML file with sensitive data (e.g., secrets.yml)

Encrypt the file using Ansible Vault:
ansible-vault encrypt secrets.yml

enter password
confirm password

double click on file verify file is encrypted

Create a Playbook that Uses the Encrypted File

store password in vault_password.txt

ansible-playbook vault-playbook/vault-secret.yaml --ask-vault-pass
ansible-playbook vault-playbook/vault-secret.yaml --vault-password-file=~/Desktop/Repositories/ansible/vault-playbook/vault_password.txt

Decrypting the File
ansible-vault decrypt secrets.yml



remote machine
Create a vault_secrets.yml file:

Encrypt the file using Ansible Vault:
ansible-vault encrypt vault_secrets.yml

enter password
confirm password


Create install_mysql_docker.yaml


ansible-playbook vault-playbook/install_mysql_docker.yaml --ask-vault-pass