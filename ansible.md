## Ansible
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
* Custom inver=ntroy file can be provided as an input using -i -r --inventroy flag
```
ansible all -i hostfile.inv -m ping
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
Ex:
[dev_web_servers]
web01 ansible_host=10.1.1.100 ansible_port=22 ansible_user=ansadmin ansible_ssh_private_key_file=web.pem ansible_python_interpreter=/bin/python

[dev_db_servers]
db01 ansible_host=10.1.1.100 ansible_port=22 ansible_user=ansadmin ansible_ssh_private_key_file=db.pem ansible_python_interpreter=/bin/python
db01 ansible_host=10.1.1.100 ansible_port=22 ansible_user=ansadmin ansible_ssh_private_key_file=db.pem ansible_python_interpreter=/bin/python
```












