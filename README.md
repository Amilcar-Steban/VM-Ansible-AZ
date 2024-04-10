

# <p align="center"><strong> :video_game: MARIO BROSS WITH ANSIBLE AND TERRAFORM ON AZURE :video_game: </strong></p>

<p align="center">
  <img src="images/8.png" alt="Image 8">
</p

## Autor :raising_hand:
*   Amilcar Rodriguez - A00369769

## Description

This project aims to create a files of configuration with tool ansible to install docker and its dependencies and building an image of mario bross. All configurations use the VM create in the repository "VM-TF-Modules" in azure using terraform. 

<p align="left">
  <img src="images/3.png" alt="Image 3">
</p

## Step 1
Create three folders, and inside create this files:
* inventario/
    * host.ini
* playbooks/
    * install_docker.yaml
    * run_container.yaml
* roles/
    * docker_container/
        * tasks/
            * main.yaml
    * docker_install/
        * tasks/
            * main.yaml
* ansible.cfg

### host.ini:

    [azure_vm]
    172.191.9.213 ansible_user=adminuser ansible_ssh_private_key_file=~/.ssh/vm-deploy-key-ssh

In this file, we put public ip of virtual machine of azure, in this case is 172.191.9.213. Also, put the username and the file of public key create in the repository "VM-TF-Modules".

### install_docker.yaml
    ---
    - hosts: azure_vm
      become: yes
      roles:
      - docker_install

this file describes a series of tasks to be executed on a set of remote machines identified by the group "azure_vm", also, indicate that need a user root or with privilegies and also the roles, here you specify the list of roles to be executed on the target machines.

### run_container.yaml
    ---
    - hosts: azure_vm
      become: yes
      roles:
      - docker_container

this file describes a series of tasks to be executed on a set of remote machines identified by the group "azure_vm", also, indicate that need a user root or with privilegies and also the roles, here you specify the list of roles to be executed on the target machines.

### roles/docker_container/tasks/main.yaml

    - name: Ejecutar Mario Bros
      docker_container:
        name: supermario-container
        image: "pengbai/docker-supermario:latest"
        state: started
        ports:
        - "8367:8080"

This file is the container of Mario Bros, when we install the steps of file "run_container.yaml", the main.yaml is run

### roles/docker_install/tasks/main.yaml

    the file in repository...

This file is the container the installation of docker and its dependencies , when we install the steps of file "install_docker.yaml", the main.yaml is run and install all in order.


### ansible.cfg
    [defaults]
    roles_path = ./roles
    inventario = ./inventario/hosts.ini

This file is important because inside is the paths defaults


## Step 2

Now, we will use this commands to up ansible configuration:

    ansible-playbook -i inventario/hosts.ini playbooks/install_docker.yaml
    ansible-playbook -i inventario/hosts.ini playbooks/run_container.yaml

with this commands, we install docker with its dependencies and run container of Mario Bros inside VM in azure, because we using ssh to connect with ansible and execute the previous files.

<p align="left">
  <img src="images/1.png" alt="Image 1">
</p

<p align="left">
  <img src="images/2.png" alt="Image 2">
</p

## Step 3

Now, we have that permit the inbound port in VM, so, in the network configuration, we can add new rule to permit the port that we define in the file "roles/docker_container/tasks/main.yaml"

<p align="left">
  <img src="images/4.png" alt="Image 4">
</p

## Step 4

Finally we can play Mario bros, put in the browser your public ip of VM and port, so: http://172.191.9.213:8367/

<p align="left">
  <img src="images/5.png" alt="Image 5">
</p

Unfortunately, I am very bad at playing mario bros :disappointed:

<p align="left">
  <img src="images/6.png" alt="Image 6">
</p

