
#######################################################
# Create kube_user
#######################################################
- ansible-playbook create-user-playbook/create_user.yml

#######################################################
# Install K3 dependencies
#######################################################
- ansible-playbook kubernetes-playbook/k3_dependencies.yaml

#######################################################
# Initialize your Master node
#######################################################
- ansible-playbook kubernetes-playbook/init_master.yaml

#######################################################
# Connect your worker nodes to the master node
#######################################################
- ansible-playbook kubernetes-playbook/add_workers.yaml

#######################################################
# Login to the master node as kube_user
#######################################################
- ssh -i ~/.ssh/gcp_key kube_user@master_ip
- kubectl get nodes
- kubectl get all -A

#######################################################
# Deploy app to cluster
#######################################################
- kubectl apply -f kubernetes-playbook/deployment.yaml

#######################################################
# Kubernetes Dashboard
#######################################################
- Run file
    - ansible-playbook kubernetes-playbook/dashboard.yaml
- Copy kubeconfig file
    - ansible master-ip -m fetch -a "src=/home/kube_user/.kube/config dest=./kubeconfig flat=yes"
    - remover this two lines
         certificate-authority-data: LSO********
         server: https://10.0.1.3:6443
    - replace with
         insecure-skip-tls-verify: true
         server: https://master-ip:6443
- Download lens
    - https://github.com/lensapp/lens/releases/tag/v2024.1.300751-latest
    - add kubeconfig downloaded to lens config




#######################################################
#######################################################
# Install NFS server
#######################################################
- mkdir kubernetes
- cd kubernetes
- Make sure nfs server is running
    - showmount -e [master node ip]
    - Add user if it does not exist
        - ./users_playbook.yaml
    - if not install playbook 
        - ./nfs_server_playbook.yaml
    - Apply nfs provisioner on master node
        - ./nfs_provider_playbook.yaml
    - Create pvc
        - ./pvc_playbook.yml
    - Check if exist on file system
        - ls -l /k8mount
        - df -h /k8mount
        - cd /k8mount
    - Check pv and pvc created
        - kubectl get pvc -n dev
          kubectl get pv -n dev
        - kubectl get pods --all-namespaces
    - Deploy sample nginx app
        - ./deploy_nginx_playbook.yml


#######################################################
# Uninstall NFS-Server
#######################################################
- Remove nfs-server
    - sudo systemctl stop nfs-kernel-server
      sudo systemctl disable nfs-kernel-server
      sudo apt remove --purge -y nfs-kernel-server
      sudo rm -rf /etc/exports
      sudo rm -rf /var/lib/nfs
      sudo apt autoremove -y
      sudo apt autoclean
- Remove folder
    - sudo rm -rf /k8mount/*
    - sudo rm -rf /k8mount

#######################################################
# Copy files to master
#######################################################
- Either run the below file or copy individual files
    - ./copy_helm_charts_playbook.yaml
- Individual adhoc commands
    - ansible all -m copy -a "src=../../Helm dest=/home/kube_user mode=0755" -b
      ansible master -m copy -a "src=../../Helm/dynamodbchart dest=/home/kube_user/Helm mode=0755" -b
      ansible master -m copy -a "src=../../Helm/chartpostgress dest=/home/kube_user/Helm mode=0755" -b
      ansible master -m copy -a "src=../../Helm/bookchart dest=/home/kube_user/Helm mode=0755" -b
      ansible master -m copy -a "src=../../Helm/employeechart dest=/home/kube_user/Helm mode=0755" -b
      ansible master -m copy -a "src=../../Helm/chartnginx dest=/home/kube_user/Helm mode=0755" -b
      ansible master -m copy -a "src=../../Helm/helmfile.yaml dest=/home/kube_user/Helm mode=0755" -b

#######################################################
# Delete kubernetes and dependencies
#######################################################
sudo systemctl stop kubelet
sudo systemctl disable kubelet
sudo apt-get remove -y kubeadm kubelet kubectl
sudo apt-get autoremove -y
sudo apt-get purge -y kubeadm kubelet kubectl
sudo apt-get remove -y kubernetes-cni kubernetes-client kubernetes-server
sudo apt-get autoremove -y
sudo apt-get remove -y docker.io containerd.io
sudo apt-get purge -y docker.io containerd.io
sudo apt-get autoremove -y
sudo rm -rf /etc/kubernetes
sudo rm -f /etc/apt/sources.list.d/kubernetes.list
sudo rm -f /etc/apt/keyrings/kubernetes-apt-keyring.asc
sudo rm -f /etc/default/kubelet
sudo rm -rf /etc/systemd/system/kubelet.service.d
sudo userdel kubelet
sudo groupdel kubelet
sudo iptables -F
sudo apt-get autoclean
sudo apt-get autoremove -y
sudo reboot




sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 234654DA9A296436
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo tee /etc/apt/trusted.gpg.d/kubernetes.asc



sudo rm /etc/apt/sources.list.d/kubernetes.list
sudo apt-get clean
sudo apt-get update


curl -4 -s https://dl.k8s.io/apt/doc/apt-key.gpg | sudo apt-key add -
sudo touch /etc/apt/sources.list.d/kubernetes.list
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://dl.k8s.io/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list



echo “deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /” | sudo tee /etc/apt/sources.list.d/kubernetes.list
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg