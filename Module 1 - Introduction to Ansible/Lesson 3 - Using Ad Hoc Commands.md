In this lesson, you will lean about:
- [✔️] Using Ad Hoc Command
- [✔️] Understanding Ansible Modules
- [✔️] Using *ansible-doc* to get Module Documentation
- [✔️] Introducing Essential Ansible Modules
- [✔️] Lesson 3 Lab: Using Ad Hoc Commands

<p align="center" width="100%">
    <img width="100%"src="https://www.middlewareinventory.com/wp-content/uploads/2019/01/Screen-Shot-2019-01-16-at-4.48.32-AM.png"> 
</p>

## Lesson 3.1 - Using Ad Hoc Command
- To run tasks on Ansible, you'll typically want to use *Playbooks* - Playbooks are easy to reproduce
- A Playbook is YAML file in which tasks can be defined
- For some tasks, writing a playbook is just too much more
	- In such case, you can Ad hoc commands
- Ad hoc commands are also useful for testing for testing if your playbook was successful
### Ad hoc Command Ingredients
- The basic structure is `ansible hosts -m module [-a 'module arguement'] [-i inventory]`
- The *hosts* part specifies on which host the command should be  performed
- The *module* part indicates which Ansible module to use; following the module you'll specify module arguments
- If inventory is not specified, you'll need to specify the inventory as an argument
- An example of an ad hoc command is *ansible all -m user -a "name=lisa"*
	- And to verify the command works, use *ansible all -m command  -a "id lisa"*

## Lesson 3.2 - Understanding Ansible Modules
- Ansible comes with lots of modules that allow you to perform specific tasks on managed hosts
- When using Ansible, you'll always use modules to tell Ansible what you want it to do,  in ad-hoc commands as well as in playbooks.
- Many modules are provided with Ansible, if required you can develop you own modules
- Use *ansilble-doc -l* for list of modules currently available
- All modules work with arguments, *ansible-doc* will show which arguments are available and which are required
- Ansible modules are idempotent; which means that when running them again they'll give the same result and if a tasks has already been configured, they won't do it again.

## Lesson 3.3 - Using ansible-doc to get Module Documentation
- The *ansible-doc* command provides authoritative documentation about modules
- Use *ansible-co -l* for a list of all modules
- Use *ansible-doc modulename* to get the documentation for a specific module

### Understanding Module Status
- Modules are very actively developed by the community, and the status field in the module documentation indicates the current status
- *stableinterface* - The module is stable and safe to use
- *preview* - The module is in tech preview and its keywords may change
- *deprecated*  - The module should not be used anymore, and will be removed in a future release.
- *removed* - The module has been removed, and the documentation only still exists to help users migrating to its replacement

### Understanding Module Support Status
- The *suppored_by* field in ansible-doc indicates who is responsible for supporting a module.
	- *core* - The module is supported by the core ansible developers
	- *curated* - The primary support responsibility is with partners and companies in the community. Proposed changes are reviewed by the core developers.
	- *community* - Support is completely within the community

## Lesson 3.4 - Introducing Essential Ansible Modules
- *ping* - verifies the ability to log in and that Python has been installed:
	- *ansible all -m ping*
- *service* - checks if a service is currently running
	- *ansible all -m service -a "name=httpd state=started"*
- *command* - runs any commands, but not through a shell
	- *ansible all -m command -a "/sbin/reboot -t now"*
- *shell* - runs arbitrary commands through a shell
	- *ansible -m shell -a set*
- *raw* - runs a command on a remote host without need for python
- *copy* - copies a file to the managed host
	- *ansible all -m copy -a "content="hello world" dest=/etc/motd*

## Lesson 3 Lab: Using Ad Hoc Commands
- Use *ping* module in and hoc command to verify that all of your hosts have been set up successfuly
- Use the appropriate ad hoc command to install python on a host you want to manage
> GOOD LUCH <3 
