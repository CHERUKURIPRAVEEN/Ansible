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
#### Ansible Inventroy/hosts
* Ansible keeps track of all the servers that is knows about through a `hosts` file written in `ini` format
* It contains information of hots that Ansible can manage
* Default file: /etc/ansible/hosts
* Hosts can be grouped together
* Both hostnames and IP address are supported
* A host can be a member of multiple groups
* Inventroy can be static or dynamic

```
ansible all -m ping -i inventroy.ini
```
Example:
````
Ex:1 Un-Grouped hosts
## praveen.example.com
## aws.example.com
## 10.1.1.100
## 10.1.2.100

Ex 2: A collection of hosts belonging to the 'app-servers' group
## [app-servers]
## 10.1.1.100
## 10.1.2.100
## alpha.example.com
## beta.example.com
````
