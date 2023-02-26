# Lesson 1 - Installing Ansible
In this lesson, you will lean about:
- [✔️] Understanding Ansible 
- [✔️] Host Requirement 
- [✔️] Installing Ansible on the Control Node
- [✔️] Preparing Managed Nodes 
- [✔️] Verifying Ansible Installation 
- [ ] Lesson 1 Lab : Installing Ansible

## Lesson 1.1 - Understanding Ansible
<p align="center" width="100%">
    <img width="100%"src="https://www.tutorialspoint.com/ansible/images/ansible_works.jpg"> 
</p>
- Ansible is a *CMS* (Configuration Management System)
- The idea of Ansible is we are going to work with one central computer, this computer is called *Ansible Control Node*, on this computer, you need to install *Ansible Software*
- To connect the *Ansible Control Node* with other host (managed computers, the only thing here you need is to use *SSH*  (Secure SHELL)
- On the control node, we are going to crate a playbook:
	- A playbook, basically, is a kind of script, but a script on steroids.
- To use Ansible, we need to install Python

## Lesson 1.2 - Host Requirements
On the Ansible Control Node:
- Python
- Ansible Software
- Configurations:
	- SSH - Keygen (SSH keys going to be copied on the managed nodes)
	- Dedicated a User account ( with the name *ansible*) - You can choose the name you want (bob, alice, .. etc)
		- PS: You can't manage the managed hosts using *root* account directly.

On the other nodes (managed nodes):
- python
- Configurations:
	- *sudo* configuration need to be setup-ed on every managed nodes 

PS: Another important configuration need to be present: 
- *IP addresses* -We could recommend that you work with fixed IP addresses.
- *DNS* - Can be configured directly on the */etc/hosts* repository.

> You can install Ansible:
> 1. From the repositories, you need the EPEL repository (EPEL is the Extra Packages for Enterprise Linux)
> 2. Python pip package installer

## Lesson 1.3 - Installing Ansible on the Control Node
- Ansible can be installed from the repositories, or using Python pip installer
- In this procedure we'll discuss Python pip as well as the repositories method

### Using Pythpn pip to install Ansible
On control.example.com:
1. *yum install python3*
2. *yum -y install python3-pip*
3. *alternatives --set python /usr/bin/python3*
4. *su - ansible; pip3 install ansible --user*
5. *ansible --version*

### Installing Ansible from 
On control node:
1. use *subscription-manager repos --list* to verify the name of the latest available repository
2. Use *subscription-manager repos --enable=ansible-2-for-rhel-8-x86_64-rpms* to enable the repository
3. Use *ym install -y ansible* to install Ansible3 software
4. *ansible --version* to verofy
5. On managed nodes: *yum install python3*

### PS: If Using Centos8
- On Centos 8, the Ansible software is expected to be available from the EPEL repository
- Use *yum install -y epel-release* to make this repository available
- Use *ym install  python3 ansible* to install the ansible software

## Setting up the requirement environement
After installing Ansible, you'll need to set up a dedicated Ansible user account:
On all nodes: 
1. *useradd ansible*
2. *echo password  | passwd --stdin ansible*
3. *echo "ansible ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ansible*

On control node:
1. *su - ansible; ssh-keygen*
2. *ssh-copy-id ansible1.example.com; ssh-copy-id ansible2.example.com*

## Lesson 1.4 - Preparing Managed nodes
### Preparing Managed Nodes  - 1
- Ansible needs a dedicated non-root user account with sudo privileges that can SSH into the manged nodes without entering a password
- Im going to use here an account with the name *ansible* to do so
- To set up this account, the following steps heave already completed:
	1. *useradd ansible*
	2. *echo password  | passwd --stdin ansible*
	3. *echo "ansible ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ansible*

### Preparing Managed Nodes  - 2
On the control host, generate an SSH key and copy over the managed nodes
1. *su - ansible; ssh-keygen*
2. *ssh-copy-id ansible1.example.com; ssh-copy-id ansible2.example.com*
Install Python3
1. *yum install python3*
2. *alternatives --set python /usr/bin/python3*

## Lesson 1.5 - Verify Ansible Installation
- As user *ansible*, type *ansible --version*
- Further verification is not possible yet, as additional settings need to be done

## Lesson 1 Lab - Ansible Installation
- Set up the basic installation of Ansible.
> GOOD LUCK <3 

