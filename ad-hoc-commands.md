## Ansible ad-hoc Commands
#### Syntax
```
 > ansible [pattern] -m [module] -a "[module options]"
```
#### Rebooting Servers
```
 > ansible <group name> -a "/sbin/reboot"
 > ansible <group name> -a "/sbin/reboot" -f 10
 > ansible <group name> -a "/sbin/reboot" -f 10 -u <username>
 > ansible <group name> -a "/sbin/reboot" -f 10 -u <username> --become [--ask-become-pass]
 > ansible <group name> -a "/sbin/reboot" -f 10 -u <username> --become [-K]
```
#### Managing files
```
 > ansible <group name> -m copy -a "src=/etc/hosts dest=/tmp/hosts"  #Copy file from Source to Destination
 > ansible <group name> -m file -a "dest=/srv/foo/a.txt mode=600"    #Change file permissions
 > ansible <group name> -m file -a "dest=/srv/foo/b.txt mode=600 owner=praveen group=praveen"  #Change Owner Permissions
 > ansible <group name> -m file -a "dest=/path/to/c mode=755 owner=praveen group=praveen state=directory" #Create Directory
 > ansible <group name> -m file -a "dest=/path/to/c state=absent" #Delete Directory
```
#### Managing packages
```
 > ansible <group name> -m apt -a "name=git state=present"
 > ansible <group name> -m apt -a "name=git-3.0 state=present" #Specific Version 
 > ansible <group name> -m apt -a "name=git state=latest"      #Latest Version
 > ansible <group name> -m apt -a "name=git state=absent"      #To Uninstall
```
#### Managing users and groups
```
 > ansible <group name> -m user -a "name=praveen password=<crypted password here>"
 > ansible <group name> -m user -a "name=praveen state=absent"
```
#### Managing services
```
> ansible <group name> -m service -a "name=httpd state=started"
> ansible <group name> -m service -a "name=httpd state=restarted"
> ansible <group name> -m service -a "name=httpd state=stopped"
```
#### Gathering facts
```
 > ansible <group name> -m setup
```


