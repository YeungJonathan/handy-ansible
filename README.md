# Jono's notes for ansible playbook and deploy

## Ansible Architecture
- Inventory maps host
- Configuration sets 
- Modules define actions
- Playbooks to coordinate multiple tasks
- Python to build the execution
- Ssh to deliver the task

## 2 Execution types
- Remote execution
- Local execution
    - When remote box is not executing plays

## Deploy steps
create file and cd into it

Vagrant
- vagrant init (initialise vagrant file)
- Configure the vagrant file:

      config.vm.define “web” do |web|
		web.vm.box = "centos7"
		web.vm.hostname = "web"
		web.vm.network "private_network", ip: "192.168.33.20" //please fill in your private network address
		web.vm.network "forwarded_port", guest:80, host:8080
		end

- vagrant up
- vboxmanage list runningvms (check if servers running)

## Install ansible in server

- Umbuntu box

      vagrant ssh name
      sudo apt-get install ansible

- centOS box

      vagrant ssh name
      sudo yum install peel-release
      sudo yum install ansible
      
      OR
      
      sudo yum install gcc
      sudo yum install python-setuptools
      sudo easy_install pip
      sudo yum install python-devel
      sudo pip install ansible

Create inventory then store IP address inside 

With DNS:

    [db]
    db1.company.com ansible_ssh_user=jono ansible_ssh_pass=123
    db2.company.com ansible_python_interpreter=/usr/bin/python

    [datacenter-west:children]
    db
    [datacenter-west:vars]
    ansible_ssh_user=ansible_user
    ansible_ssh_pass=#45e!@Gh
    ntp-server=5.6.7.8

Without DNS:

    ￼￼web1 ansible_ssh_host=192.168.33.20 ansible_ssh_user=vagrant ansible_ssh_pass=vagrat
    -a "name={{username}} password=12345"
￼
--sudo add super control

    ssh vagrant@address => save footprint => known host in ~/.ssh
    ansible address -i inventory -u vagrant -m ping -k
    
-i = inventoryFile, -u = username, -m = module, -k = prompts password

ssh password = vagrant

## Log-in vagrant server

vagrant ssh name

Ad-hoc
ansible all -I inventory -u vagrant -m command -a “command” 

command => run any type of command in the system
-a => allows put parameters in the command module

## Tree
sudo apt-get install tree

Configuration file
- ansible.cfg
 
      [default]
      host_key_checking=False

[defaults] = set default

    export ANSIBLE_HOST_KEY_CHECKING = True
    
export => set configuration without entering the file

Modules
- Core
- Extras
- Deprecated

ansible-doc -l => shows all modules
ansible-doc <name> => finds specific modules
ansible-doc -s <name> => finds examples of a specific module

Copy module
- Copies a file from local box to remote system
- ‘Backup’ capability
- Validate remotely

Fetch module
- Pulls a file from remote host to local system
- Use md5 checksums to validate

Apt module
- Install update or delete package
- Update entire system

Yum module
- Same as apt module but using redhat based system

Service module
- Stop, start or restart services



