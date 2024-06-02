## Ansible

# Ansible Installation

Ansible is an open source automation platform. It is very, very simple to setup and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Installation steps:

Add a EPEL (Extra Packages for Enterprise Linux)third party repository to get packages for Ansible 
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

Install Ansible
``` 
yum install ansible -y 
```

Check Ansible version 

``` 
ansible --version
```

Create a new user for ansible administration & grant admin access to user (Master and Slave)
```
useradd ansadmin
passwd ansadmin
# below command addes ansadmin to sudoers file. But strongly recommended to use "visudo" command if you are aware vi or nano editor. 
echo "ansadmin ALL=(ALL) ALL" >> /etc/sudoers
```

Using keybased authentication is advised. If you are still at learning stage use password based authentication (Master & Slave)
```
# sed command replaces "PasswordAuthentication no to yes" without editing file 
 sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
``` 
Login as a ansadmin user on master and generate ssh key (Master)
```
ssh-keygen
```
Copy keys onto all ansible client nodes (Master)
```
ssh-copy-id <target-server>
```
Update target servers information on /etc/ansible/hosts file (Master)
``` 
echo "<target server IP>" > /etc/ansible/hosts
```
Run ansible command as ansadmin user it should be successful (Master)
``` 
ansible all -m ping
```

### Ansible Help
which provides a summary of the most common Ansible command-line options and subcommands
```
ansible -h, --help
```
```
usage: ansible [-h] [--version] [-v] [-b] [--become-method BECOME_METHOD]
               [--become-user BECOME_USER] [-K] [-i INVENTORY]
               [--list-hosts] [-l SUBSET] [-P POLL_INTERVAL]
               [-B SECONDS] [-A MODULE_ARGS] [-m MODULE_NAME]
               [--playbook-dir BASEDIR] [-e EXTRA_VARS] [--vault-id VAULT_IDS]
               [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
               [-f FORKS] [-T TIMEOUT] [--private-key PRIVATE_KEY_FILE]
               [--scp-extra-args SCP_EXTRA_ARGS]
               [--sftp-extra-args SFTP_EXTRA_ARGS]
               [--ssh-common-args SSH_COMMON_ARGS]
               [--ssh-extra-args SSH_EXTRA_ARGS]
               [-o] [--connection CONNECTION] [--limit LIMIT]
               [--tags TAGS] [--skip-tags SKIP_TAGS] [-u REMOTE_USER]
               [--ask-pass | --password-file PASSWORD_FILE]
               [-k] [--check] [--diff] [pattern] [module] [args]

Executes an Ansible ad hoc command

positional arguments:
  pattern               host pattern
  module                module name to execute (default=command)
  args                  module arguments

optional arguments:
  --ask-pass            ask for connection password
  -k, --ask-pass        ask for connection password
  -K, --ask-become-pass
                        ask for privilege escalation password
  -b, --become          run operations with become (nopasswd implied if
                        missing)
  --become-method BECOME_METHOD
                        privilege escalation method to use (default=sudo),
                        use `ansible-doc -t become -l` to see available
                        methods
  --become-user BECOME_USER
                        run operations as this user (default=root)
  -C, --check           don't make any changes; instead, try to predict some
                        of the changes that may occur
  -D, --diff            when changing (small) files and templates, show the
                        differences in those files; works great with --check
  -e EXTRA_VARS, --extra-vars EXTRA_VARS
                        set additional variables as key=value or YAML/JSON
  -f FORKS, --forks FORKS
                        specify number of parallel processes to use
                        (default=5)
  -h, --help            show this help message and exit
  -i INVENTORY, --inventory INVENTORY, --inventory-file INVENTORY
                        specify inventory host path or comma-separated host
                        list. --inventory-file is deprecated
  --limit LIMIT         further limit selected hosts to an additional pattern
  --list-hosts          outputs a list of matching hosts; does not execute
                        anything else
  -l SUBSET, --subset SUBSET
                        further limit selected hosts to an additional pattern
  -m MODULE_NAME, --module-name MODULE_NAME
                        module name to execute (default=command)
  -o, --one-line        condense output
  -P POLL_INTERVAL, --poll POLL_INTERVAL
                        set the poll interval if using -B (default=15)
  -B SECONDS, --background SECONDS
                        run asynchronously, failing after X seconds (default=N/A)
  --playbook-dir BASEDIR
                        Since this tool does not use playbooks, use this as a
                        substitute playbook directory.This sets the relative
                        path for many features including roles/ group_vars/
                        etc.
  --private-key PRIVATE_KEY_FILE, --key-file PRIVATE_KEY_FILE
                        use this file to authenticate the connection
  -T TIMEOUT, --timeout TIMEOUT
                        override the connection timeout in seconds
                        (default=10)
  --vault-id VAULT_IDS  the vault identity to use
  --ask-vault-pass      ask for vault password
  --vault-password-file VAULT_PASSWORD_FILES
                        vault password file
  -v, --verbose         verbose mode (-vvv for more, -vvvv to enable
                        connection debugging)
  -V, --version         show program's version number and exit
  --scp-extra-args SCP_EXTRA_ARGS
                        specify extra arguments to pass to scp only (e.g. -l)
  --sftp-extra-args SFTP_EXTRA_ARGS
                        specify extra arguments to pass to sftp only (e.g. -f
                        , -l)
  --ssh-common-args SSH_COMMON_ARGS
                        specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)
  --ssh-extra-args SSH_EXTRA_ARGS
                        specify extra arguments to pass to ssh only (e.g. -R)
  -u REMOTE_USER, --user REMOTE_USER
                        connect as this user (default=None)
  --connection CONNECTION
                        connection type to use (default=smart)
  --check               don't make any changes; instead, try to predict some
                        of the changes that may occur (same as -C)
  --diff                when changing (small) files and templates, show the
                        differences in those files; works great with --check
```
### Ansible Verbose level
* Causes Ansible to print more deburg messgaes 
* Adding multiple `-v` will increase the verbosity, the built-in plugins currenty evaluate upto `-vvvvvv`
* A reasonable level to start is `-vvv`, connection deburging might be require `-vvvv`
```
ansible all -m ping -v
```
### Ansible Inventroy/hosts
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
* If the managed nodes run on a non-standard `SSH Port`, Then you can input the port number after the hostname with the colon.
```
## 10.1.1.100:9040
## 10.1.2.100:6713
```
* Custom inventroy file can be provided as an input using -i or --inventroy flag
```
ansible all -i <hostfile> -m ping
```
#### Grouping Hosts
* You can organize your servers into different groups and subgroups(Nested Groups)
* It is also possible to aggregate multiple groups as childern under a parent group

```
ansible dev_servers -i hosts -m ping
```
```
mail.example.com

[dev_web_servers]
foo.exapmle.com
bar.example.com

[dev_db_servers]
db1.example.com
db2.exaple.com

[development:children]
dev_web_servers
dev_db_servers
```
#### Common Petterns for Targeting Hosts and Groups
https://docs.ansible.com/ansible/latest/inventory_guide/intro_patterns.html

#### Host Aliases
* You can use aliases to name servers in a way that facilitates refrencing those servers later, when runing commands and play books.
* To use alias, include a variable named ansible_host after the alias name, conatining the corresponding IP address or hostname of the server

```
[dev_web_servers]
web01 ansible_host=10.1.1.100

[dev_app_servers]
app01 ansible_host=10.1.2.100
app02 ansible_host=10.1.2.200
app03 ansible_host=10.1.2.300
```

#### Ansible Host Variables
* You can assign the variables to the hosts that will be used in palybooks, such as:
```
ansible_user
ansible_ssh_pass(linux)
ansible_password(windows)
ansible_ssh_private_key_file
ansible_connection(ssh for linux, winrm for windows and local host)
ansible_port
ansible_python_interpreter
```
```
Ex:1
[dev_web_servers]
web01 ansible_host=10.1.1.100 ansible_port=22 ansible_user=ansadmin ansible_ssh_private_key_file=web.pem ansible_python_interpreter=/bin/python

[dev_db_servers]
db01 ansible_host=10.1.1.100 ansible_port=22 ansible_user=ansadmin ansible_ssh_private_key_file=db.pem ansible_python_interpreter=/bin/python
db01 ansible_host=10.1.1.100 ansible_port=22 ansible_user=ansadmin ansible_ssh_private_key_file=db.pem ansible_python_interpreter=/bin/python
-------------------------------------
Ex:2
[dev_web_servers]
web01 ansible_host=10.1.1.100

[dev_db_servers]
db01 ansible_host=10.1.1.100
db01 ansible_host=10.1.1.100

[dev_web_servers:vars]
ansible_user=answeb
ansible_ssh_private_key_file=web.pem

[dev_db_servers:vars]
ansible_user=ansdb
ansible_ssh_private_key_file=db.pem


[all:vars]
ansible_port=22
ansible_python_interpreter=/bin/python
```

#### Run Commnads on Localhost
* Ansible uses SSH Protocol to connect managed hosts
* If you do not have any managed nodes, you can test Ansible commands against the control node by using `localhost` or `127.0.0.1` for the host name
* Ansible sets up an implicit localhost entry if you do not have localhost in your inventroy
* In addiyion, instaed of using the SSH connection type, Ansible connects to localhost using the special local connection type by default.

```
ansible localhost -m ping

OUTPUT:

ansuser@ip-172-31-16-10:~$ ansible localhost -m ping
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

#### Ansible Inventroy 
```
ansible-inventory -i hosts --list

OUTPUT:
ansuser@ip-172-31-16-10:~$ ansible-inventory -i host --list
{
    "Dev_app_servers": {
        "hosts": [
            "172.31.27.200",
            "172.31.27.300"
        ]
    },
    "Dev_db_servers": {
        "hosts": [
            "172.31.27.201",
            "172.31.27.301"
        ]
    },
    "Dev_web_servers": {
        "hosts": [
            "web01",
            "web02",
            "web03"
        ]
    },
    "_meta": {
        "hostvars": {
            "web01": {
                "ansible_host": "172.31.27.199"
            },
            "web02": {
                "ansible_host": "172.31.20.168"
            },
            "web03": {
                "ansible_host": "ec2-54-211-218-105.compute-1.amazonaws.com"
            }
        }
    },
    "all": {
        "children": [
            "ungrouped",
            "Dev_web_servers",
            "Dev_app_servers",
            "Dev_db_servers"
        ]
    }
}
```
* Below command showing yaml format
```
ansible-inventory -i host --list -y
ansuser@ip-172-31-16-10:~$ ansible-inventory -i host --list -y
all:
  children:
    Dev_app_servers:
      hosts:
        172.31.27.200: {}
        172.31.27.300: {}
    Dev_db_servers:
      hosts:
        172.31.27.201: {}
        172.31.27.301: {}
    Dev_web_servers:
      hosts:
        web01:
          ansible_host: 172.31.27.199
        web02:
          ansible_host: 172.31.20.168
        web03:
          ansible_host: ec2-54-211-218-105.compute-1.amazonaws.com
```
```
ansible-inventory -i host --graph
ansuser@ip-172-31-16-10:~$ ansible-inventory -i host --graph
@all:
  |--@ungrouped:
  |--@Dev_web_servers:
  |  |--web01
  |  |--web02
  |  |--web03
  |--@Dev_app_servers:
  |  |--172.31.27.200
  |  |--172.31.27.300
  |--@Dev_db_servers:
  |  |--172.31.27.201
  |  |--172.31.27.301
```
#### Ansible Ad-hoc Commands
* In Ansible, Ad-hoc commands refer to one-time commands that are executed from the command line againest one or more managed hosts.
* These commands are meant for quick tasks, testing and troubleshooting rather than for complex automachine scenarios
* Ad-hoc commands are particulary useful for tasks that don't require writing a full playbook
* The basic structure of an ad-hoc commands is as follows

```
ansible <host-pattern> -m <module> -a "<module arguments>"

ansible web_servers -m ping -i hostfile
```
* Ad-hoc commands are useful for quick operation, but if you find yourself performing the same tasks repeatedly or needing more complex automation, its often better to create `ansible playbook`
* Rebooting Servers
```
ansible <group name> -a "/sbin/reboot"
ansible <group name> -a "/sbin/reboot" -f 10
ansible <group name> -a "/sbin/reboot" -f 10 -u <username>
ansible <group name> -a "/sbin/reboot" -f 10 -u <username> --become [--ask-become-pass]
ansible <group name> -a "/sbin/reboot" -f 10 -u <username> --become [-K]
```
* Managing files
```
ansible <group name> -m copy -a "src=/etc/hosts dest=/tmp/hosts"  #Copy file from Source to Destination
ansible <group name> -m file -a "dest=/srv/foo/a.txt mode=600"    #Change file permissions
ansible <group name> -m file -a "dest=/srv/foo/b.txt mode=600 owner=praveen group=praveen"  #Change Owner Permissions
ansible <group name> -m file -a "dest=/path/to/c mode=755 owner=praveen group=praveen state=directory" #Create Directory
ansible <group name> -m file -a "dest=/path/to/c state=absent" #Delete Directory
```
* Managing packages
```
ansible <group name> -m apt -a "name=git state=present"
ansible <group name> -m apt -a "name=git-3.0 state=present" #Specific Version 
ansible <group name> -m apt -a "name=git state=latest"      #Latest Version
ansible <group name> -m apt -a "name=git state=absent"      #To Uninstall
```
* Managing users and groups
```
ansible <group name> -m user -a "name=praveen password=<crypted password here>"
ansible <group name> -m user -a "name=praveen state=absent"
```
* Managing services
```
ansible <group name> -m service -a "name=httpd state=started"
ansible <group name> -m service -a "name=httpd state=restarted"
ansible <group name> -m service -a "name=httpd state=stopped"
```
* Gathering facts
```
ansible <group name> -m setup
```
#### Ansible modules
* Ansibe `modulues` are the building blocks of ansible automation
* Theare small units of code that perform specific tasks on tagest machines
* They are responsible for carrying out actions such as `installing packages`, `manageing files`, `configuring services` and more
* There are hundreds of Ansible modules avaliable and categorized into various groups based on their functionality
#### Ansible Command Vs Shell Module
* The `command` and `shell` modules execute commands on remote nodes. They are almost identical.
* The shell module is used when we need to execute a command in remote servers, in the shell of your choice. By default the commands are run on the /bin/bash shell. You can make use of the various operations such as |, <, > etc. and environmental variables such as $HOME.
* The command module cannot use special shell operators such as redirections (<, >), pipes (|), & or access user specific environment variables like $HOSTNAME.
* The Command Module doesnot process the commands through a shell. So, it does not support the above operations.

```
ansible all -m command -a "df -h | grep xvda1" -i hosts

ansible all -m shell -a "df -h | grep xvda1" -i hosts
```
#### Understanding Privilege Escalation
* For security reasons, Ansible connects to remote hosts as nonprivilege user.
* For escalating privileges to get administrative access as `root`, we need to configure few parameters under `[privilege_escalation]` section of the Ansible configuration file.
* To enable privilege escaltion, the directive `become = true` should be set in the configuration file but we can override it when running ad hoc commands or Ansible Playbooks in case if a task or play does not require privilege escalation
* Other configurations such as `become_method`, `become_user`, `become_ask_pass` also need to be set in the configuration file. 

#### Privilege Esacaltion using connection Variables
* We can also define different become options in hosts file for each managed node or group.

```
ansible_become
ansible_become_method
ansible_become_user
ansible_become_password

ansible_user
ansible_connection
ansible_host
ansible_python_interoreter
ansible_port
ansible_ssh_pass
ansible_ssh_private_key_file
```

1. **ansible_become**: set to `true` to activate privilege escation.
2. **ansible_become_method**: Define how to escalate privileges on managed hosts, Default is `sudo`
3. **ansible_become_user**: set to user desired privileges - the user you become, NOT the user you login as. Default value is `root`
4. **ansible_become_password**: Passwdord use when escalating privileges on managed hosts. Usually, the root password or normal user password if added to sudoers file.

* Privileges elascations using command line flages
https://docs.ansible.com/ansible/2.7/user_guide/become.html

#### Ansible Binaries/Utilies
* Most of us are familier with `ansible` and `ansible-playbook`, but those are not the only utilities ansible provides.
* Below is a complete list of ansible utilites/binaries

➡️ **ansible**: used to run Ansible ad-hoc commands

➡️ **ansible-playbook**: execute Ansible Playbooks

➡️ **ansible-config**: Provides information about the configuration settings used by Ansible

➡️ **ansible-console**: opens an interactive Ansible console that allows you to run ad-hoc commands and perform tasks in an interactive mode

➡️ **ansible-doc**: used to access documentation for ansible module, plugins and playbook keywords.

➡️ **ansible-galaxy**: used to intract with ansible galaxy, which is a public repository of ansible roles

➡️ **ansible-inventroy**: shows inventory. Can generate dynamic inventory

➡️ **ansible-vault**: used to encrypt/decrypt sensitive data such as passwords, API keys, and other secrets in ansible playbook and files

** Listing Avalible Modules
* Run `ansible-doc -l` command to see a list of all the available modules
* There are as many as 8000+ module available
```
ansible-doc -l | wc -l
```
* The `ansible-doc` commands in ansible is used to access documentation and information about ansible modules, plugins, and playbook keywords
* It provides detailed explanations of how to specific ansible components including their parameters, options and examples.
* This command is particularly useful when you need to understand how to use a module or lookup a modules documentation quickly.

```
ansible-doc <module_name>
ansible-doc setup
```
#### Ansible Gather Facts









