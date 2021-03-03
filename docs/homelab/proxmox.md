---
layout: default
title: Proxmox
parent: Homelab
nav_order: 1
has_children: true
---

# Proxmox
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## What the Proxmox?

Proxmox Virtual Environment is an open-source server virtualization management platform. What is a virtualization environment/virtual machine? A virtual machine is just like any other computer or server, except it doesn't have any physical hardware. Instead, it runs on the same hardware as the virtualization host. Hence the *virtual* environment.

### But Hyper-EXSi-Xen!

I use Proxmox because:
   - [x] Free and open-source
   - [x] First hypervisor I used
   - [x] PCI-E passthrough capability
   - [x] Noob friendly web UI
   - [x] Most importantly, I'm able to accomplish what I need to with the least amount of struggle

### But Docker!

Yeah Docker and whales are cool, but why not have the best of both worlds? Spin up a Docker host on a VM and be able to utilize system snapshots and rollbacks with all your containers. Check out my [Docker page]({% link docs/homelab/docker.md %}) for more info.

## Installation

For a barebones homelab, any relatively modern computer should work fine as long as the CPU supports virtualization. My first server was my old gaming pc perfectly fulfilled my needs at the time.

### Get Proxmox

Download the latest installer from the [Proxmox website](https://www.proxmox.com/en/downloads/category/iso-images-pve) and follow [their installation guide](https://pve.proxmox.com/wiki/Prepare_Installation_Media) on creating the USB installer.

### Install Proxmox

**WARNING:** Anything on your soon to be server machine is going to be deleted, so do what you need to prior.

Proceed with booting from your USB and follow the on screen instructions to install Proxmox. Once you hit the network settings, a few things to keep note of. 

- Hostname: You can use anything you want here, but choose a good domain name so that you can implement it later.
  - `hostname.domain_name.top_domain_name` is the structure, so `pve.danny.home` works.
  - You shouldn't use `.com, .local` or any other public top level domains due to resolving issues.
- IP Address: Make sure that this is outside of the available range so there are not any conflicts later. Keep this address to access Proxmox later.

Finish up the guided install wait until your system restarts and you’ll see a text only screen prompting you to visit `https://YourServerIP:8006` (from another computer). Don’t forget the **s** in https or it will fail to load the page.

### Initial Config Changes

Once you reach the login screen, use `root` for the user name and the password you used during the installation earlier. 

By default, Proxmox grabs software updates from the enterprise repository, which requires a paid subscription to use. Switching over to the free community reposity requires just a few commands. 

Select your Proxmox host and open a Shell window using the Shell button on the top right. We'll be performing the following steps with the commands below:

1. Update container template database
2. Disable the enterprise repository
3. Add the community repository
4. Update and install all available software updates

```bash
pveam update
sed -i '1s/^/#/' /etc/apt/sources.list.d/pve-enterprise.list
echo 'deb http://download.proxmox.com/debian buster pve-no-subscription' >> /etc/apt/sources.list
apt update && apt dist-upgrade
```

After installing the updates, reboot the server and feel free to poke around Proxmox. There's a lot to learn and explore within Proxmox, so play around and familiarize yourself with your new virtualization server. Create some VMs or containers, automate some backups, connect your network share, test various services, there's always plenty to do. 

## Virtual Machines vs Containers

Proxmox has the ability to run both VMs and containers, so which and when should you use each? Let's quickly define them both. 

- VMs are simulated entire computers. They have their own BIOS and simulated hardware, which will allow you to run completely different operating systems on top of your base operating system. It's a full fledged computer in your window. 
- Containers are much thinner, since there is no simulated hardware. A container VM shares the same kernel as the Proxmox system host, allowing it to install and start up extremely faster than a VM. It hides host processes from the container, and pretends that a subdirectory is really the root and starts from there. 

Each have their use cases and depending on the situation, planned services, required secuirty levels, etc., and you have to determine which one to use.

Let's checkout the services I have running!