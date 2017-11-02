---
title: "Trying it out"
teaching: 25
exercises: 0
questions:
objectives:
- "Try our some basic Ansible code"
keypoints:
- "Run simple shell commands"
- "Add more hosts inventory files"
- "Gather data from multiple hosts in one command"
---

This episode describes the tools we use to build and manage lessons.
These simplify many tasks, but make other things more complicated.

## Trying it out


#### 1. Module: Ping

~~~
$ ansible localhost -m ping
~~~
{: .bash}

~~~
 [WARNING]: Host file not found: /usr/local/etc/ansible/hosts

 [WARNING]: provided hosts list is empty, only localhost is available

localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
~~~
{: .output}

#### 2. Module: Setup

~~~
$ ansible localhost -m setup 
~~~
{: .bash}
~~~
..... woah!
~~~
{: .output}

Trying again: what info are we interest in? e.g. CPU cores:

~~~
$ ansible localhost -m setup | grep processor_cores
~~~
{: .bash}
~~~
 [WARNING]: Host file not found: /usr/local/etc/ansible/hosts
 [WARNING]: provided hosts list is empty, only localhost is available
        "ansible_processor_cores": "4",
~~~
{: .output}

> ## What's with all these [WARNING] statements?!
> 
> Ansible is looking for an inventory file - let's create one.
>
{: .callout}

#### 3. Inventory Files

* must be able to ssh to the host
* can specify ssh username and port number to use if they are different to your current O.S. username and standard :22

ssh-keyscan loop for eel and cowfish

create eel and cowfish inventory

ansible all "/bin/echo hello"


Further reading at [ansible.com - getting started] (http://docs.ansible.com/ansible/latest/intro_getting_started.html)

{% include links.md %}
