# vagrant_adventure
Experiment - what is vagrant, can I it to create a multi-node docker swarm development environment?


# Step 1 - Install Vagrant

https://www.vagrantup.com/downloads looks simple enough.

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
```

# Step 2 - Install Virtualbox

Vagrant needs something to do the actual hosting of machines.

Going to add the oracle repository so I get a new version & can keep it up to date

_note_ - using ubuntu here.

```
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib"
sudo apt-get update && sudo apt-get install virtualbox-6.1
```

# Step 3 - Change where  disk images

I dont have a lot of space free on my main drive, adding a load of disk images will go poorly.

First Virtualbox

```
vboxmanage setproperty machinefolder /path/to/directory/
```

Then Vagrant

```
echo 'export VAGRANT_HOME=/media/de-admin/ubuntudata/.vagrant.d' >> ~/.bashrc 
```


# Step 4 - Install Ansible

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu

```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```


# Step 5 - Use Ansible to set up docker swarm on the vagrant machines

I looked at these docs:
https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html
https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html
https://www.vagrantup.com/docs/provisioning/ansible_intro
https://www.innuy.com/blog/installation-of-docker-swarm-using-ansible/

I also found this example of a playbook for setting up compose
https://gist.github.com/yonglai/d4617d6914d5f4eb22e4e5a15c0e9a03

I figured I should probably try running the commands over ssh first so I know what I'm doing
https://techviewleo.com/setup-docker-swarm-cluster-on-rocky-linux/

some ansible, swarm, vagrant specific examples:
https://codereview.stackexchange.com/questions/270183/ansible-playbook-to-install-docker-swarm
https://github.com/acuto/swarm-vagrant-ansible


```bash
sudo dnf install -y lvm2 device-mapper device-mapper-persistent-data device-mapper-event device-mapper-libs device-mapper-event-libs
sudo dnf install -y dnf-utils
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker
```

