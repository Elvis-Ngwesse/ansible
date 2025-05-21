# Step 1: SSH into the server as 'demo_user'.
# This assumes you are using the correct private key for either GCP or AWS.
# Replace 'your_instance_ip' with the actual IP address of your server.
# For AWS, use your AWS private key file (.pem) and the corresponding user (ec2-user or demo_user).
# For GCP, use your GCP private key file (.pem or .pub depending on the setup).

# SSH into a GCP server as demo_user
ssh -i ~/.ssh/gcp_key demo_user@your_instance_ip

# SSH into an AWS EC2 server as ec2-user, then switch to demo_user
ssh -i ~/Desktop/Repositories/ansible/my-ssh-key.pem ec2-user@<your-aws-instance-ip>

# Once logged into the AWS EC2 instance, switch to demo_user using sudo
# This assumes 'demo_user' was successfully created using the Ansible playbook and has the necessary sudo privileges.
sudo su - demo_user

# Step 2: Check if 'demo_user' exists and verify user details.
# The 'id' command shows the UID, GID, and group membership of 'demo_user'.
# This confirms the user has been created and details like group membership are correctly assigned.
id demo_user

# Step 3: List files in the current directory (including hidden files).
# The '-la' option displays all files, including hidden ones, with detailed information such as permissions, ownership, and groups.
ls -la

# Step 4: Print the current working directory.
# The 'pwd' command shows the full path to the directory you're currently in.
# It's useful for verifying that you're in the correct directory (such as the home directory of 'demo_user').
pwd

# Step 5: Display the contents of the 'authorized_keys' file for 'demo_user'.
# This file stores the public SSH keys that are authorized to log into the server as 'demo_user'.
# You should see the public SSH key you added through the Ansible playbook in this file.
cat /home/demo_user/.ssh/authorized_keys

# Step 6: Test if 'demo_user' has sudo access by running a command as that user.
# The 'whoami' command returns the name of the current user. Running it with sudo for 'demo_user' confirms that the user has sudo access.
# If 'demo_user' has the correct sudo privileges, this command should print 'demo_user'.
sudo -u demo_user whoami

# Step 7: Check the contents of the sudoers file for 'demo_user'.
# This shows the sudoers configuration that grants 'demo_user' passwordless sudo access.
# If the user was successfully added to the sudoers file, you should see the line you added in the playbook:
# 'demo_user ALL=(ALL) NOPASSWD: ALL'.
sudo cat /etc/sudoers.d/demo_user
