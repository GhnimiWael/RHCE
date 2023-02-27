In this lesson, you will lean about:
- [✔️]  Understanding Variables
- [✔️]   Using Variables
- [✔️]   Understanding Variable Precedence
- [✔️]   Managing Host Variables
- [✔️]   Using Multi-values Variables
- [✔️]   Using Ansible Vault
- [✔️]   Working with Facts
- [✔️]   Creating Custom Facts
- [✔️]  Lesson 5 Lab: Working With Variables and Facts

<p align="center" width="100%">
    <img width="100%"src="https://cdn.educba.com/academy/wp-content/uploads/2020/06/Ansible-Facts.jpg"> 
</p>

## Lesson 5.1 - Understanding Variables
- Ansible uses variables to manage *differences between systems*. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries.
- Example format of variables: *{{webserver}}*
- A variable is a label is assigned to specific value to make it easy to refer to that value throughout the playbook
- Variables can be defined by administrators at different levels
- A fact is a special type of variable, that refers to a current stat of Ansible manged system
- Variables are particularly useful when dealing with managed hosts where specifies are different
	- Set a variable web_service on Ubuntu and Red Hat
	- Refer to the variable web_service instead of the specific service name

## Lesson 5.2 - Using Variables
- Variables can be set at different levels
	- In playbook
	- In inventory (deprecated)
	- In inclusion files
- Variable names have some requirements
	- The name must start with a letter
	- Variable names can only contain letters, numbers, and underscores

### Defining Variables
- Variables can be defined in vars block in the beginning of playbook
```yml
- hosts: all
  vars:
    web_package: httpd
```
- Alternatively, variables can be defined in a variable file, which will be included from the playbook:
```yml
- hosts: all
  vars_files:
    - vars/users.yml
```
- After defining the variables, it can be used later in the playbook
- Notice that order does matter !
- Refer to a variable as *{{web_package}}*
- In conditional statements (discussed later), no curly braces are needed to refer to variable values
	- `when : "'not found' in command_result.err"``
	- `{% if ansible_facts['devices]['sdb'] is defined %'}`
	  `Secondary disk size: {{ansible_facts['devices']['sdb']['size']}}`

### Lesson 5.3 - Understanding Variables Precedence
### Understanding Variable Scope
- Variables can be set with different types of scope
	- *Global scope* - This is when a variable is set from inventory or the command line
	- *Play scope* - This is applied when it is set from a play
	- *Host Scope* - This is applied when set in inventory or using a host variable inclusion file
- When the same variable is set at different levels, the most specific level gets precendence
- When a variable is set from the command line, it will overwrite anything else
	- `ansible-playbook site.yml -e "web_package=apache"`
### Understanding Built-in Variables
- Some variables are built in and cannot be used for anything else
	- *hostvars*
	- *inventory_hostname*
	- *inventory_hostname_short*
	- *groups*
	- *group_names*
	- *ansible_check_mode*
	- *ansible_play_batch*
	- *ansible_play_hosts*
	- *ansible_version*

## Lesson 5.4 - Managing Host Variables
### Using Host Variables
- Variables can be assigned to hosts and to groups of hosts
- The old way of doing so is through inventory, but this is now deprecated because it mixes two types of information in one file.
- Instead, you should use directories to populate hosts and group variables
### Defining Host Variables Through Inventory
- A variable can be assigned directory to a host
```inventory
[servers]
web1.example.com web_package=httpd
```

- Alternatively, variables can be set for host groups
```
[servers:vars]
web_package=httpd
```
### Using Include Files
- To define host and host group variables, directories should be created in the current project directory
- Use `~/myproject/host_vars/web1.example.com` to include host specific variables
- Use `~/myproject/group_vars/webservers` to include host group specific variables
- Notice that you don't have to define variable in these directories in the playbook, they are picked automatically

## Lesson 5.5 - Using Multi-valued Variables
### Understanding Array and Dictionary
- Multi-valued variables can be used in playbooks
- When using a multi-valued variable, it can be written as an array(list), or as a dictionary (hash)
- Each of these has their own specific use cases.
#### Understanding Dictionary (Hash)
- Dictionaries can be written in 2 ways:
```yml
users:
  linda:
    username:linda
    shell: /bin/bash
  lisa:
    username: lisa
    shell: /bin/bash
```
 - Or:
```
users:
  linda: {username:'linda', shell:'/bin/bash'}
  lisa: {username: 'lisa', shell:'/bin/bash'}
```
- To address items in dictionary, you can use 2 notations:
	- `variable_name['key'], as in users['linda]['shell']'`
	- `variable_name.key, as in users.linda.shell`
- You cannot `loop` or `with_items` on dictionaries

#### Using array (list)
- arrays provide a list of items, where each item can be addressed separately:
```users:
- username: linda
  shell: /bin/bash
- username: lisa
  shell: /bin/bash
```

- Individual items in the array can be addressed, using the `{{ var_name[0]}}` notatiob
- To access all variables, you can use `with_item` or `loop`

## Lesson 5.6 - Using Ansible Vault
### Dealing with Sensitive Data
- some modules require sensitive data to be processed
- This may include webkeys, passwords, and more
- To process sensitive data in secure way, Ansible Vault can be used
- Ansible Vault is used to encrypt and decrypt files
- To mange this process, the *ansible-vault* command is used
### Creating an Encrypted File
- To create an encrypted file, use `ansible-vault create playbook.yml`
- This command will prompt for a new vault password, and opens the file in `vi` for further editing.
- As an alternative for entering password on the prompt, a vault password file may be used, but you'll have to make sure this file is protected in another way: `ansible-vault create --vault-passowrd-file=vault-pass playbook.yml`
- To view a vault encrypted file, use `ansbile-vault view playbook.yml`
- To edit, use `ansible-vault edit playbook`
- Use `ansible-vault encrypt playbook.yml` to encrypt an existing file, and use `ansible-vault decrypt playbook.yml` to decrypt it.
### Using Playbook with Vault
- To run a playbook that access Vault encrypted files, you need to use `--vault-id @prompot` option to be prompted for a password
- Alternatively, you can store the password as a single-line string in a password file, and access that vault using the `vault-password-file=vault-file` option
### Managing Vault Files
- When setting up projects with Vault encrypted files, it makes sense to use separate files to store encrypted and non-encrypted variables
- To store host or host-group related variable files, you can use the following structure:
- This replaces the solution that was discussed earlier, where all variables are stored in a file with the name of the host or host group

## Lesson 5.7 - Working with Facts
- Ansible facts are variables that are automatically set and discovered by ansible on managed hosts
- Facts contain information about hosts that can be used in conditionals
- For instance, before installing specific software you can check that a managed host run a specific kernel version
### Managing Fact Gathering
- By default, all playbooks perform fact gathering before running the actual plays
- You can run fact gathering in an ad-hoc command using using the `setup` module
- To show facts, use the `debug` module to print the value of the `ansible_facts` variables
```yml
- name: show all facts
  hosts: all
  - name: print facts
      debug:
         var: ansible_facts
```
- Notice that in facts, a hierarchical relationship is shown where you can use the dotted format to refer to specific fact.
### Displaying Fact Names
- In ansible 2.4 and before, Ansible facts were stored as individual variables, such as `ansible_hostname` and `ansible_interfaces`
- In Ansible 2.5 and later, all facts are stored in one variable with the name `ansible_facts`, and referring to specific facts happen in a different ways :
	- `ansible_facts['hostname']`
	- `ansible_facts[interfaces]`
- The old approach is referred to as `injecting facts as variables` an this behavior can managed though the `inject_facts_as_vars` parameter
### Turning off Fact Gathering
- Disabling Fact Gathering may seriously speed up playbooks
- Use `gather_facts: no` in the play header to disable
- Even if fact gathering is disabled, it can be enabled again bu running the `setup` module in a task

## Lesson 5.8 - Creating Custom Facts
- Custom facts allow the administrators to dynamically generate variables which are stored as facts
- Custom facts are stored in an `ini` or `json` file in the `/etc/ansible/facts.d` directory on the managed hosts
	- The name of these file muse be end in `.fact`
- Custom facts are stored in the `ansible_facts.ansible_locat` variable
- Use `ansible hostname - m  steup -a "fileter=ansible_local"` to display local facts
- Notice how fact filename and label are used in the fact
- Custom Fact Example File:
```fact
[localfacts]
package = vsftpd
service = vsftpd
state = started
```

## Lesson 5 Lab: Working with Variables and Facts
- Create a playbook that will first define local facts on the file server. These facts should define the name of the smb service, and samba package
- Ensure the first play copies these facts to the file servers
- Next, in the same playbook, use a second play that will install and enable the smdb service
