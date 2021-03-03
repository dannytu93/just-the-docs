---
layout: default
title: Ansible
parent: Proxmox
grand_parent: Homelab
---

# Ansible

Once you start spinning up more and more VMs or containers, you may run into the issue of updating each individual system all at once. Configuration management systems are designed to streamline that process of controlling large numbers of servers, and [Ansible](https://www.ansible.com/) is a popular tool just for the task. Ansible uses SSH to execute the automated tasks and YAML files to define those details. 

## Configuration

<dl>
  <dt>Virtualization Type</dt><dd>Container</dd>
  <dt>OS</dt><dd>Ubuntu</dd>
  <dt>Cores</dt><dd>1 CPU</dd>
  <dt>Memory</dt><dd>512 MB</dd>
  <dt>Disk Size</dt><dd>4 GB</dd>
</dl>

## Installation

Ansible works by having **one** Ansible Control Node and **one or more** Ansible Hosts. The control node is the server used to control the hosts over SSH. We'll also cover setting up SSH keys between the nodes for better security. 

```bash
# Refresh and upgrade your system's packages
apt update

# Install the Ansible software
apt install ansible
```

The inventory file contains information about the hosts you'll manage with Ansible. You're able to add as few or many servers as you'd like, and you can also organize specific hosts or groups based on playbooks or templates. Let's open and edit the default Ansible inventory.

```bash
nano /etc/ansible/hosts
```

The default inventory file containes a number of examples that you can use as reference to set up your inventory. The example below defines a group named `[servers]` with 3 different servers in it, each defined by their custom alias and IP address.

```
[webservers]
server1 ansible_host=192.168.100.11
server2 ansible_host=192.168.100.12

[dbservers]
server3 ansible_host=192.168.100.13
server4 ansible_host=192.168.100.14

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

The `all:vars` subgroup defines the `ansible_python_interpreter` host parameter that will be valid for all hosts included in this inventory. This parameter ensures that the remote server uses the `/usr/bin/python3` Python 3 executable instead of `/usr/bin/python` Python 2.7, which is not present on recent Ubuntu versions. 

With our inventory updated, let's connect and test the connection between servers. We'll start on our Ansible control node. 

By defauly, `ssh-keygen` will create a 3072-bit RSA key pair, which is secure for most use cases. You can also add the `-b 4096` flag to bump it to a larger 4096-bit key!

```bash
ssh-keygen
```

Now that you have generated a key, we'll copy the public key to your Ansible host. The quickest method is to use the `ssh-copy-id` utility. Simply specify the remote host in the command and enter the remote server's password for verification. Once complete, test if you're able to ping from the Ansible control node to the host.

```bash
ssh-copy-id username@remote_host

# You can also replace the server name with 'all' to ping your entire inventory
ansible server1 -m ping -u root
```

Note:  
{: .label .label-yellow } 

A common error that I expereinced were that my hosts did not permit SSH root login. To resolve this, **on your host server** run the following command which allows root login through SSH.

```bash
sed -i -e 's|#PermitRootLogin prohibit-password|PermitRootLogin yes|g' /etc/ssh/sshd_config
```

Now that Ansible is up and running with our connected hosts, we can create our playbook to automate tasks! 

```bash
# Create a central location for our playbooks and write our first playbook
mkdir /root/ansible_playbooks
nano /root/ansible_playbooks/example_playbook.yml
```

Playbooks contain all of the commands that we wish to automate. A *Play* is a full Ansible run, it can have several playbooks and roles that start from a single playbook. An example of a playbook is below. 

```yml
---
- name: Install basic webserver packages
  hosts: webservers
  tasks:
     - name: NGINX install
       apt: pkg=nginx state=present
       notify:
       - restart nginx
     - name: Enable NGINX on boot
       service: name=nginx state=started enabled=yes
  handlers:
    - name: restart nginx
      service: name=nginx state=restarted

- name: Install mySQL
  hosts: dbservers
  tasks:
     - name: Install mySQL
       apt: pkg=mysql-server
     - name: Install Git
       apt: pkg=git
```

The `name` is the name of our play. We can call this whatever wa want, but it's used to describe the overall purpose of the tasks grouped below it. The `hosts` field tells Ansible which group from our inventory to run on. So in this example, the tasks for `webservers` are to install NGINX and enable it during boot, and for the `dbservers` to install mySQL and Git.

To run this playbook, use the following command:

```bash
ansible-playbook /root/ansible_playbooks/example_playbook.yml
```

With our playbook ready to run, you can now execute any command or playbook remotely from your Ansible control node. General updates, provisioning servers, deploying monitoring services, there's so much to automate with Ansible~

## Reference Links

[Ansible Homepage](https://www.ansible.com/)

[Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html)