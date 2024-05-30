## Ansible
#### Ansible Help
which provides a summary of the most common Ansible command-line options and subcommands
```
ansible -h, --help
```
#### Ansible Verbose level
* Causes Ansible to print more deburg messgaes 
* Adding multiple `-v` will increase the verbosity, the built-in plugins currenty evaluate upto `-vvvvvv`
* A reasonable level to start is `-vvv`, connection deburging might be require `-vvvv`
```
ansible all -m ping -v
```
