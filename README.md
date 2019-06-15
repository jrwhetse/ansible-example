# Ansible Examples

## Overview

**Ansible**, a free-software platform for configuring and managing computers, combines multi-node software deployment, ad hoc task execution, and configuration management. It manages nodes (which must have Python 2.4 or later installed on them) over SSH or over PowerShell.

## References

Website: [https://www.ansible.com] (https://www.ansible.com)

Best Practices: [http://docs.ansible.com/ansible/playbooks_best_practices.html]
(http://docs.ansible.com/ansible/playbooks_best_practices.html)

Documentation: [http://docs.ansible.com/ansible] (http://docs.ansible.com/ansible)

Modules: [http://docs.ansible.com/ansible/modules_by_category.html] (http://docs.ansible.com/ansible/modules_by_category.html)

## Installation (RHEL 6)

yum install ansible -y

### ansible

rpm -ql ansible    

    # global config    
    /etc/ansible/ansible.cfg    

    # global hosts    
    /etc/ansible/hosts    
    
    # global roles    
    /etc/ansible/roles
    
## Configuration

### Setup SSH To Remember Credentials (Optional)  
    
    ssh-agent bash    
    ssh-add ~/.ssh/id_rsa
    
### Setup SSH Host Key Checking Auto Accept

There are 4 different ways to auto accept host key checking. Modifying global configuration is recommended.

#### System Configuration File

Uncomment the following in /etc/ansible/ansible.cfg    
    
    [defaults]    
    host_key_checking = False
    
#### User Configuration File

Add the following to ~/.ansible.cfg    
 
    [defaults]    
    host_key_checking = False
    
#### Environment Variable    
    
    export ANSIBLE_HOST_KEY_CHECKING=False
    
#### Command Line    

    ansible -e 'host_key_checking=False' ...
    
## Examples

### Command Line 

#### Ping
    
    # Using built-in module with explicit host    
    ansible -m ping -u <local_user> --ask-pass --become-user=root --ask-become-pass <hostname>    
    
#### Touch

    # Using built-in module with explicity host    
    ansible -m file -a 'path=/tmp/touch.file state=touch mode="u=rw,g=r,o=r"' -u <local_user> --ask-pass --become-user=root --ask-become-pass <host>

### Playbooks

#### Ping 

     # Using host file with host group    
     ansible -i hosts -m ping -u <local_user> --ask-pass --become-user=root --ask-become-pass all

     # Using playbook with host file. Host group is defined in playbook    
     ansible-playbook ping.yml -i hosts -u <local_user> --ask-pass --become-user=root --ask-become-pass

#### Touch
     
     # Using host file with host group    
     ansible -i hosts -m file -a 'path=/tmp/touch.file state=touch mode="u=rw,g=r,o=r"' -u <local_user> --ask-pass --become-user=root --ask-become-pass all

    # Using playbook with host file. Host group is defined in playbook    
    ansible-playbook touch.yml -i hosts -u <local_user> --ask-pass --become-user=root --ask-become-pass

## Tips

### To Force Password Prompt For Local and Remote User

If all excution will be performed by your local user and remote root user, do the following
    
Uncomment the following lines in /etc/ansible/ansible.cfg 
    
    ask_sudo_pass = True    
    ask_pass      = True

Add the following properties to playbooks

    become: true    
    become_user: root

In the touch example above, the --ask-pass, --become-user=root and --ask-become-pass can be removed

    ansible-playbook <playbook> -i <host_file> -u <local_user>
    
### Create .gitkeep in all dirs

    find . -type d -empty -not -path "./.git/*" -exec touch {}/.gitkeep \;

### Test Edit

### Pull Request Change
test
