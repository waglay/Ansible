# Ansible

## Inventory File
- First of all ansible has a control machine and other workers/target machine.
- Ansible should be installed in the control machine, in our case, local machine i.e. laptop.
- We need to write a inventory file which consists of pointers to the target machine including the host username, host ip and ssh key file to the host, by default port 22 is used for ssh, we can also define custom ssh     port as well.
- After adding all the details, the hosts informations can be added in groups as parent and child, the structure is followed in inventory.yml file in this repo.
- Variables can be added in the inventory file as well.

## Adhoc commands
- After adding the inventory file we can test it using adhoc commands to test the connection or to execute package installation or several other tasks.
- To execute the adhoc commands we can use ```ansible -m ansible.builtin.yum -a "git" -i inventory.yml``` similar structure.

## Ansible Playbook
- We can define and cluster all the tasks that can be performed in target machine in playbook.
- We can use the yml format for the file.
- Here we define the hosts or groups we are executing the modules or changes.

## ansible.cfg
- This is the configuration file of ansible.
- This resides in /etc/ansible/ansible.cfg but it has the lowest priority.
- It can be defined in env as ANSIBLE_CONFIG which is of first priority, after that a ansible.cfg file in the working dir as in this repo and after that home dir as in ~/.ansible.cfg
- We can change the setting to change the ```host_key_checking``` to false. for bypass the signature verification during ssh and we can also define the inventory file location so that we donot have to pass ```-i```       flag everytime we execute the ansible or ansible-playbook command.
- We can also set the ``` become ``` property as true in the configuration file itself rather than defining it in the playbook.

## Variables
- We can define variables in the playbook itself which is of the highest priority. It can be defined before tasks.
- We can define the variables on seperate dir with the dir structure as global_vars/(group name) as in global_vars/all or global_vars/servers. Here the children has more priority than the parent as in all will have lower priority than servers and server1 which is host defined will have more priority.
- We can also pass the variable as we do in docker like ```- e``` which will have higher priority than hosts.
- Ansible itself gathers a lot of information and variables in the ```gathering facts``` section. We can use these variables as we like in ```conditions``` we can use ```when``` keyword to define conditions.
- We can store the result of an execution or the logs in a variable as well.
- ```
  - hosts: web_servers
    tasks:
      - name: Run a shell command and register its output as a variable
        ansible.builtin.shell: /usr/bin/foo
        register: foo_result
        ignore_errors: true

      - name: Run a shell command using output of the previous task
        ansible.builtin.shell: /usr/bin/bar
        when: foo_result.rc == 5
  ```

## Handlers
  - These are like triggers that is set alongside tasks, it is a task but triggered task which is executed if a ```notify``` keyword is defined in a task. It will only notify if the task is changed, there is no             notification on unchanged tasks.
 
## Templates, Files and Copy
  - Templates are similar to copy but the files defined in template can use the variables from ansible.
  - By default templates have the ```file extension``` as ```.j2``` also known as ```Jinja2 format``` 
  - We can change file permissions, ownerships and file related work using files.
  - Copy is copy.😁
 
## Roles
  - Roles are define using the ```ansible-galaxy init``` command and used to manage all the things defined above in their seperate dirs, so handlers will have it's own dir and it has ```main.yml``` file where the           handlers go. Similarly, the tasks go in tasks dir, the inventory files go in inventory dir, the variables go in variables dir and the playbook will define the roles instead of tasks in a list format. 

## debug
  - It is used as echo and can have ```var``` and ```msg``` as a parameter where var can have direct access to the variable where as msg is used to       deliver a message and can have variables as ```{{variable name}}``` 