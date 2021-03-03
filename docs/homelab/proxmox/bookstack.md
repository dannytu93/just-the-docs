---
layout: default
title: BookStack
parent: Proxmox
grand_parent: Homelab
---

# BookStack

BookStack is a platform for storing and organizing information. As your own personal wiki, you can store whatever you want with it, from technical documentation to recipies, whatever is important to you. I've decommissioned my BookStack in favor of GitHub in the form of this wiki.

## Configuration

<dl>
  <dt>Virtualization Type</dt><dd>Container</dd>
  <dt>OS</dt><dd>Ubuntu</dd>
  <dt>Cores</dt><dd>1 CPU</dd>
  <dt>Memory</dt><dd>1 GB</dd>
  <dt>Disk Size</dt><dd>8 GB</dd>
</dl>

## Installation

### Script Install

The BookStack developers have included an installation script with their installation documentation. To preface, this script **must** be used on a fresh instance of Ubuntu 20.04. Since this script installs Apache, MySQL 8.0, and PHP-7.4, it could overwrite any pre-existing setups on the system. Installing this in a container makes that super easy!

```bash
# Refresh your system's packages
apt update

# Download the script
wget https://raw.githubusercontent.com/BookStackApp/devops/master/scripts/installation-ubuntu-20.04.sh

# Make it executable
chmod a+x installation-ubuntu-20.04.sh

# Run the script with admin permissions
sudo ./installation-ubuntu-20.04.sh
```

Once the installation is complete, head over to http://ServerIPAddress and you're ready to start your wiki!

## Reference Links

[BookStack Homepage](https://www.bookstackapp.com/)

[BookStack Official Documentation for installing on Ubuntu via script](https://www.bookstackapp.com/docs/admin/installation/#ubuntu-2004)
