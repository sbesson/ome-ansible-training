---
layout: page
title: Setup
permalink: /setup/
---

#### This training session is in the form of a follow-along workshop.

To follow along, you'll need the following:

#### Hardware Requirements:
* 1.5GB ram free
* 2GB local disk free

#### Software Requirements (links/instructions below):
* Ansible (2.0+) 
* Virtualbox (5+)
* Vagrant (1.9+) 

#### Git clones of:
* [github.com/ome/ansible-examples-omero](https://github.com/ome/ansible-examples-omero)
* [github.com/openmicroscopy/managment_tools](https://github.com/openmicroscopy/management_tools)

### Installing the requirements

### Ansible
The installation instructions for all platforms are at [docs.ansible.com](http://docs.ansible.com/ansible/latest/intro_installation.html). 

OS X users: The Ansible project are currently recommending users install it via `pip`. I've read that using `--user` can make installation easier on 10.11+, so perhaps try `pip install --user ansible`.

You can test your install by running:
~~~
ansible localhost -m ping
~~~
{: .bash}
~~~
localhost | SUCCESS => {
    "changed": false,
        "ping": "pong"
        }
~~~
{: .output}

### VirtualBox 
A lightweight method of running virtual machines on your desktop/laptop. Follow the graphical installer you can download from [virtualbox.org](https://www.virtualbox.org/wiki/Downloads). Once that completes, VirtualBox should be ready to use.

### Vagrant

This is a tool which can automate the configuration of local virtual machines, including VirtualBox. With this we can create a VM from a text definition in one command.

1 Follow the graphical installer you can download from [vagrantup.com](https://www.vagrantup.com/downloads.html).

2 Once vagrant is installed, download the `centos/7` `box` as follows:

Run (in any directory):
~~~
vagrant box add centos/7
~~~
{: .bash}

When prompted, pick `virtualbox`

Confirm the `box` was installed with:

~~~
vagrant box list | grep centos/7
~~~
{: .bash}
~~~
centos/7         (virtualbox, 1710.01)
~~~
{: .output}

### Ansible Source and related files

Make git clones of the following repositories, in your favourite clones directory, e.g. ~/git-clones/.

* [github.com/ome/ansible-examples-omero](https://github.com/ome/ansible-examples-omero)
* [github.com/openmicroscopy/managment_tools](https://github.com/openmicroscopy/management_tools)


> ## Problems getting set up?
>
> Ask in slack, and someone will help.
{: .keypoints}

{% include links.md %}



