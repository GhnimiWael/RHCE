# Lesson 2 - Setting up a Managed Environment

In this lesson, you will lean about:
- [✔️] Setting up Static Inventory
- [✔️]  Understanding Dynamic Inventory
- [✔️]  Understanding Ansible Configuration Files
- [✔️] Managing Ansible Configuration Files
- [✔️] Lesson 2 Lab: Setting up an Ansible Managed Environment
<p align="center" width="100%">
    <img width="100%"src="https://miro.medium.com/max/708/1*8H4XYCNV-xamG0joz22apQ.png"> 
</p>

## Lesson 2.1 - Setting up static Inventory
- We can use DNS or hosts in */etc/hosts* , to help the control to discover who is available and how to get in touch
- But in Ansible environment you need more -> You need to know which machine are manageable from an ansible perspective, and that is what *inventory* is all about.

- An Inventory can be used to point to: 
	- An individual  server --> *Static Inventory*
	- A couple of servers on a rack -
	- Many server or many managed machine in a cloud environment --> *Dynamic Inventory*

- In Inventory you can do a couple of things:
	- Define *groups* 
	- Variables

### Managing Static Inventory
- In a minimal form, a static inventory is a list of host names and IP addresses that can be managed by ansible.
- Host  can be grouped in inventory to make it easy to address multiple hosts at once.
- A host can be member of multiple groups
- Nested groups are also available
- It is common to work with project-based inventory files
- Variables can be set from the inventory file - but this is deprecated practice.
- Ranges can be used:
	- server[1:20] matches server1 up to server20.
	- 192.168.[4:5].[0:255] matches two full class C subnets.

### Inventory File Locations
- /etc/ansible/hosts is the default inventory
- Alternative inventory location can be specified through the ansible.cfg configuration file.
- Or use *-i inventory* option to specify the location of the inventory file
- It is common practice to put the inventory file in the current project directory
### Example of Static Inventory File
```inventory
[webservers]
web1.example.com
web2.example.com

[fileservers]
file1.example.com
file2.example.com

[servers:children]
webservers
filerservers
```
### Host Groups Usage
- Functional host groups
	- Web 
	- lamp
- Regional host groups
	- europe
	- africa
- Staging host groups
	- test
	- development
	- production

## Lesson 2.2 - Understanding Dynamic Inventory
- Dynamic inventory can be used to discover inventory in dynamic environments such as cloud
- Many community-provided dynamic inventory scripts are available
- These scripts are referred to in the same way as static inventory, but must have the execution permission set
- Alternatively, it's relatively easy to write your own dynamic inventory
- You can go to the following GitHub link:https://github.com/ansible-community/contrib-scripts/tree/main/inventory and find any type of dynamic environment

## Lesson 2.3 - Understanding Ansible Configuration Files
- Before dive in, let's see an example of and *ansible configuration file*  - *ansible.cfg*.
```ansible
[defaults]
inventory = inventory
remote_user = ansible
host_key_checking = false

[privielge_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```
- Settings in *ansible.cfg* are organized in 2 sections:
	- [default] sets default settings
		- *inventory* specifies the path to the inventory file
		- *remote_user* is the name of the user that logs in on the remote hosts
		- *host_key_checking* is checking if any errors in ssh keys
	- [privilege_escalation] specifies how Ansible runs commands on *managed hosts (nodes)*
		- *ask_pass*  specifies whether or not to prompt for a password
		- *become* indicates if you want to automatically switch to the *become_user*
		- *become_method* sets how to become the other user
		- *become_user* specifies the target remote user
		- *become_ask_pass* sets if a password should be asked for when escalating

### Connecting to the Remote Host
- The default protocol to connect to the remote host is *SSH*
	- Key based authentication is the common approach, but password based authentication is possible as well
	- Use *ssh-keygen* to generate a public/private SSH key pair, and next use *ssh-cop-id* to copy the public key over the manged hosts
- Other connection methods are available, to mange Windows for instance, use *ansible_connection: winrm* and set *ansible_port:5986*
### Escalation Privileges
- *sudo* is the default mechanism, *su* can also be used but is uncommon
- A password can be asked for, but it's common to to do password-less escalation
- To setup password-less sudo, create a snapin file */etc/sudoers.d/* with the following content: 
	- *ansible ALL=(ALL) NOPASSWD:ALL*

## Understanding Localhost Connections
- Ansible has an implicit localhost entry to run Ansible command on the localhost
- When connecting to localhost, the default *become* settings are not use, but the account that ran the Ansible command is used.
- Ensure this account has been configured with the appropriate *sudo* privileges

## Lesson 2.4 - Managing Ansible Configuration Files
- The ansible.cfg file is considered in order of precedence:
	- */etc/ansible/ansbile.cfg* is the default.
	- *~/.ansible.cfg* if exists will be overwrite the default
	- *./ansible.cfg* is the configuration file in the current Ansible project directory and it will always have precedence if it exists.
- Alternatively, if a variable *ANSIBLE_CONFIG* exists to refer to a specific config file, this will always have precedence.
- Use *ansible --version* to find which configuration file currently is used

## Lesson 2 Lab - Setting up an Ansible Managed Environment
- Finalize setting up the Ansible managed environment and ensure your setup meets the following requirement
	- User *ansible* is used for all management tasks on managed hosts
	- The *inventory* is set up correctly with few hosts 
	- The *ansible.cfg* is configured for sudo privilege escalation
	- SSH key-based login has been configured
	
> GOOD LUCK <3 

