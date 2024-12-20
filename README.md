# ovia-ansible-demo-repo
Just practicing...

Ansible Playbook to install and configure nginx
Here's an Ansible playbook to install and start the NGINX web server on a target machine. This playbook assumes you have proper SSH access to the target machine and that it is configured in your Ansible inventory file.
Playbook: install_nginx.yml
---
- name: Install and configure NGINX
  hosts: webservers
  become: yes  # Escalate privileges to perform admin-level tasks
  tasks:
    - name: Update the package cache
      apt:
        update_cache: yes

    - name: Install NGINX
      apt:
        name: nginx
        state: present

    - name: Ensure NGINX is started and enabled at boot
      service:
        name: nginx
        state: started
        enabled: yes
Steps to Execute:
1.	Prepare the Inventory File: Define your target servers in an inventory file (e.g., inventory.ini):
[webservers]
192.168.1.100 ansible_user=your_user
2.	Run the Playbook:
ansible-playbook -i inventory.ini install_nginx.yml
3.	Verify: After running the playbook, verify that NGINX is installed and running by visiting the server's IP address in a web browser (e.g., http://192.168.1.100).



How do I have proper SSH access to the target machine and that it is configured in my Ansible inventory file?


To have proper SSH access to the target machine and configure it in your Ansible inventory file, follow these steps:
________________________________________
1. Ensure SSH Access to the Target Machine
You need to configure SSH access to the target machine for Ansible to work.
Steps to Set Up SSH Access:
1.	Install SSH: Ensure that the target machine has an SSH server installed and running.
o	On Ubuntu/Debian:
sudo apt update && sudo apt install openssh-server
sudo systemctl start ssh
sudo systemctl enable ssh
2.	Create an SSH Key Pair:
o	On your control machine (where you run Ansible), generate an SSH key pair:
ssh-keygen -t rsa -b 4096
o	Save the key pair to the default location (~/.ssh/id_rsa).
3.	Add Your Public Key to the Target Machine:
o	Copy the public key to the target machine using ssh-copy-id:

ssh-copy-id your_user@target_machine_ip
o	Alternatively, manually copy the content of ~/.ssh/id_rsa.pub from your control machine and paste it into ~/.ssh/authorized_keys on the target machine.
4.	Test SSH Access:
o	Verify you can connect to the target machine without a password:
ssh your_user@target_machine_ip
________________________________________
2. Configure the Ansible Inventory File
Ansible needs to know the IP addresses or hostnames of the target machines and the credentials to use.
Create or Edit the Inventory File:
Create a file named inventory.ini and define the target machine(s). For example:
[webservers]
192.168.1.100 ansible_user=your_user ansible_ssh_private_key_file=~/.ssh/id_rsa
•	192.168.1.100: Replace with the IP address or hostname of your target machine.
•	ansible_user: The SSH username used to connect to the target machine.
•	ansible_ssh_private_key_file: Path to the private key file used for authentication (default is ~/.ssh/id_rsa).
________________________________________
3. Verify Connectivity
Before running a playbook, test the connection using the ping module:
ansible -i inventory.ini webservers -m ping
If the connection is successful, you’ll see a response like this:
192.168.1.100 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
________________________________________
Troubleshooting SSH Issues:
•	Permission Denied: Ensure the correct SSH user and key are being used.
•	Firewall Blocking SSH: Open port 22 on the target machine (e.g., with ufw or iptables).
•	SSH Service Down: Ensure the SSH service is running (sudo systemctl start ssh).
________________________________________
Once this is set up, you're ready to run Ansible playbooks targeting your machines! Let me know if you need further guidance.

YAML Playbook to Clone a Git Repository
---
- name: Clone a Git repository
  hosts: all
  become: yes
  tasks:
    - name: Install Git
      apt:
        name: git
        state: present
        update_cache: yes

    - name: Clone the Git repository
      git:
        repo: 'https://github.com/your-username/your-repo.git'
        dest: /path/to/destination
        version: main  # Change to the desired branch/tag/commit
        force: yes     # Forces a fresh checkout if the repository already
			  exists                        
				 
