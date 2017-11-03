---
title: "Overview of terms"
teaching: 10
exercises: 0
objectives:
- "Learn "
keypoints:
- "Ansible - configuration automation"
- "Ansible inventory - our "
- "Vagrant - drives local VM provider like VirtualBox"
---

We'll start with a rough overview of:

* Ansible
* Inventory file
* Vagrant

#### Ansible

* Like ssh, but on can run against >1 at once.
* Agent-less
* Human-readable (yaml) language to define state of a system

#### Inventory (files)

* List(s) of systems
* Groups

#### Vagrant

* Takes care of the config and overhead of creating Virtual Machines
* Vagrantfile
* Test playbooks in a full vm rather than e.g. docker
* For the purposes of this training: local vm

#### Virtualbox

* Lightweight Virtual Machine hypervisor

{% include links.md %}
