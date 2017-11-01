---
layout: page
title: Setup
permalink: /setup/
---

This training session is in the form of a follow-along workshop.

To follow along, you'll need the following:

Software Requirements:
* Ansible (2.0+) 
* Virtualbox (5+)
* Vagrant (1.9+) 

Git clones of:
* [github.com/ome/ansible-examples-omero](https://github.com/ome/ansible-examples-omero)
* [github.com/openmicroscopy/managment_tools](https://github.com/openmicroscopy/management_tools)
* my training code?

## Installing the requirements

### Ansible
The installation instructions for all platforms are at [docs.ansible.com](http://docs.ansible.com/ansible/latest/intro_installation.html). OSX users: I've personally installed Ansible with homebrew, using `brew install ansible`, though the Ansible project are currently recommending users install it via `pip`. I'd recomend trying homebrew, then pip if there are any problems.

> ## pip, El Capitan (10.11) and beyond
> 
> If you're using El Capitan (10.11) or a more recent version of OS X, you may be better installing to the pip `--user` scheme. See [these instructions](http://binarynature.blogspot.co.uk/2016/01/install-ansible-on-os-x-el-capitan_30.html). Normal `pip` might well be fine - I'm definitely no expert here. 
{: .callout}

### Virtualbox 
Follow the graphical installer you can download from [virtualbox.org](https://www.virtualbox.org/wiki/Downloads)

### Vagrant
1 Follow the graphical installer you can download from [vagrantup.com](https://www.vagrantup.com/downloads.html)

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

output:
~~~
centos/7         (virtualbox, 1710.01)
~~~
{: .output}

### Source

Make git clones of the following, in your favourite forks/clones directory, e.g. ~/forks/.

* [github.com/ome/ansible-examples-omero](https://github.com/ome/ansible-examples-omero)
* [github.com/openmicroscopy/managment_tools](https://github.com/openmicroscopy/management_tools)

> ## Problems getting set up?
>
> Ask in slack, and someone will help.

{: .keypoints}

{% include links.md %}



