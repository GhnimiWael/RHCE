In this lesson, you will lean about:
-  [✔️] Using YAML to Write Playbooks
- [✔️]  Verifying Playbook Syntax
- [✔️]  Writing Multiple-Play Playbooks
- [✔️]  Lesson 4 Lab: Getting Started with Playbooks

<p align="center" width="100%">
    <img width="100%"src="https://www.guru99.com/images/1/070919_1256_AnsibleTuto10.png"> 
</p>

## Lesson 4.1 - Using YAML to Write Playbooks
### Why Playbooks ? 
- Ad-hoc commands can be used to run one or few tasks
- Ad-hoc commands are convenient to test, or when a complete managed infrastructure hasn't been set up yet
- Ansible Playbooks are used to run multiple tasks against managed hosts in a scripted way
- In Playbooks, one or multiple plays are started
	- Each play runs one or more tasks
	- In these tasks, different modules are used to perform the actual work
- Playbooks are written in YAML, and have the *.yml* or *.yaml* extension

### Understanding YAML
- YAML is *Another Markup Language* according to some
- According to others is stands for YAML Ain't Markup Language
- Anyway, it's an easy-to-read format to structure tasks/items that need to be created.
- In YAML files, items are using indentation with spaces to indicate the structure of the data
- Data elements at the same level should have the same indentation
- Child items are indented more than the parent items
- There is no strict requirement about the amount of spaces that should be used, but 2 is common.
### Writing Your 1st Playbook
```yaml
---
-name : deploy vsftpd
 hosts: ansible2.example.com
 tasks:
  - name: install vsftpd
    yum: name=vsftdp
  - name: enable vsftpd
    service: name=vsftpd enabale=true
  - name: create readme file
    copy:
      content: "free downloads for everyone"
      dest: /var/ftp/pub/README
      force: no
      mode: 04444
...
```
- Use *ansible-playbook vsftpd.yml* to run the playbook
- Notice that a successful run requires the *inventory* and *become* parameters to be set correctly, and also requires access to an inventory file.
- The output of the *ansible-playbook* command will show what exactly has happened.
- Playbooks in general are idempotent, which means that running the same playbook again should lead to the same result
- Notice there is no easy way to undo changes made by a playbook

## Lesson 4.2 - Verifying Playbook Syntax
### Optimizing vim for YAML
- In ~/.vimrc, include the following setting:
	- *autocmd FileType yaml setlocal ai ts=2 sw=2*
### Verifying Playbook Syntax
- *ansible-playbook --syntax-check vsftpd.yml* will perform a syntax check
- Use *-v[vvv]* to increase output verbosity
	- *-v* - will show task results
	- *-vv* will show task results and task configuration
	- *-vvv* also shows information about connections to managed hosts
- Use the *-C* option to perform a dry run

## Lesson 4.3 - Writing Multiple-Play Playbooks
### Understanding Plays
- A play is a series of tasks that are executed against selected hosts from the inventory, using specific credentials
- Using multiple plays allows running tasks on different hosts, using different credentials from the same playbook
- Within a play definition, escalation parameters can be defined:
	- *remote_user* - the name of the remote user
	- *become* - to enable/disable privilege escalation
	- *become_method* - to allow using an alternative escalation solution
	- *become_user* - the target user used for privilege esclation

## Lesson 4 Lab - Getting Started with Playbooks
- Write a playbook that installs the httpd package on ansible2.example.com
- Ensure that it started and that the firewall is opened to allow access to it
- Also create a file */var/www/html/index.html*with some welcome text
- Lastly, write a playbook that will undo all modifications

> GOOD LUCK <3

