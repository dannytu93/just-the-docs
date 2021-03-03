---
layout: default
title: UniFi Controller
parent: Proxmox
grand_parent: Homelab
---

# UniFi Controller

To manage UniFi products, you require the UniFi Controller software. UniFi does offer the [UniFi Cloud Key](https://www.ui.com/unifi/unifi-cloud-key/) which runs a local instance of the software, but instead of spending about $100, we can self-host the software for free! 

## Configuration

<dl>
  <dt>Virtualization Type</dt><dd>Container</dd>
  <dt>OS</dt><dd>Ubuntu</dd>
  <dt>Cores</dt><dd>1 CPU</dd>
  <dt>Memory</dt><dd>1 GB</dd>
  <dt>Disk Size</dt><dd>4 GB</dd>
</dl>

## Installation

### Script Install

There's no need to reinvent the wheel and explain all the necessary commands when someone else has done the work. UniFi has verified that these scripts are in safe and working conditions and are an easy way to be up and running quickly. You can find installation scripts that have been so generously compiled by Glenn R. below. 

[UniFi Install Scripts](https://glennr.nl/s/unifi-network-controller){: .btn .btn-purple }

```bash
# For super easy, latest and greatest 1 line install, use the following:
rm unifi-latest.sh &> /dev/null; wget https://get.glennr.nl/unifi/install/install_latest/unifi-latest.sh && bash unifi-latest.sh`
```
{: .fs-3 }

To start, make sure you're connected and logged in as root on your Ubuntu/Debian machine. 

```bash
# We'll start by updating and installing the necessary package.
apt-get update; apt-get install ca-certificates wget -y

# Copy the link location of the script version you're after. Download the script by using the wget command. Make sure to replace the x's with your wanted version!
wget https://get.glennr.nl/unifi/install/unifi-x.x.x.sh 

#Run and follow the shell script with the command below.
bash unifi-x.x.x.sh
```

Once the installation is complete, head over to https://ServerIPAddress:8443 and you're all set!

## Reference Links

[UniFi Controller Homepage](https://www.ui.com/software/)

[UniFi Official Documentation for installing on Ubuntu](https://help.ui.com/hc/en-us/articles/220066768-UniFi-How-to-Install-and-Update-via-APT-on-Debian-or-Ubuntu)

[UniFi Install Scripts by Glenn R.](https://glennr.nl/s/unifi-network-controller)