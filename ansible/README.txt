Purpose
Automate the installation and setup of Docker on the VM.
Ensure Docker runs on system startup for continuous operation.
Simplify server configuration with Ansible playbooks.
Ansible Playbook (playbook.yml)
hosts: all
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Docker dependencies
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - apt-transport-https
        - ca-certificates
        - curl
        - software-properties-common

    - name: Add Docker GPG key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker repository
      apt_repository:
        repo: deb https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Install Docker
      apt:
        name: docker-ce
        state: present

    - name: Start Docker service
      systemd:
        name: docker
        state: started
        enabled: yes
How to Run the Playbook
Ensure Ansible is installed on your control machine.
Add the target VM’s IP to the inventory file (hosts):
[azure_vm]
52.166.65.78 ansible_user=onoadmin ansible_ssh_private_key_file=~/.ssh/id_rsa  # WSL path
Run the playbook using the following command:

ansible-playbook -i <VM_PUBLIC_IP>, -u onoadmin --private-key ~/.ssh/id_rsa playbook.yml

Verification
To check if Docker is installed and running, SSH into the VM and run:

docker --version
systemctl status docker
This ensures Docker is properly installed and starts automatically, enabling a smooth deployment process.







