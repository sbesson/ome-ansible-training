---
title: "Creating an OMERO server"
teaching: 25
exercises: 0
objectives:
- "Experience the installation of an OMERO server via Ansible"
keypoints:
- "Vagrantfile defines your VM"
- "OME publish example playbooks"
---
OME have published (and maintain) a set of example playbooks.

Here we're going to execute one of those examples.

> ## Don't worry!
>
> Most of the following commands include some BASH substitutions to simplify connecting to a Vagrant VM from different machines. It's really not as complicated as this makes it look.
>
{: .callout}

1 Acquire a copy of the examples (you may have done this already):

~~~
Git clone https://github.com/ome/ansible-examples-omero
~~~
{: .bash} 

Change directory to the clone of ansible-examples-omero
~~~
cd ansible-examples-omero/public 
~~~
{: .bash} 

Update the vagrantfile (possibly do a manual edit?)
~~~
rm Vagrantfile && wget https://gist.githubusercontent.com/kennethgillen/648105ba0f78440ca41e45963c471744/raw/c6e05535bb20ce08e515d0a10615406838728291/Vagrantfile
~~~
{: .bash} 

To Explain
(Takes 30s..)
~~~
vagrant up
~~~
{: .bash} 


Create a hosts file:
~~~
echo "localhost ansible_port=$(vagrant ssh-config | grep Port | awk '{print $2}') ansible_user=vagrant " > hosts-file
~~~
{: .bash}

It should look similar to:
~~~
cat hosts-file
~~~
{: .bash}
~~~
localhost ansible_port=2222 ansible_user=vagrant
~~~
{: .output}


> ## Asking Vagrant how to connect to the virtual machine
> 
> Vagrant takes care of running >1 machine at once by 
> assigning the VMs different port numbers for SSH for example.
> We can ask vagrant the SSH port of the current machine, which we
> need to give to the ansible-playbook command, via the host file.
> See [docs.ansible.com](http://docs.ansible.com/ansible/latest/intro_inventory.html) 
> for more. These shell snippets acheive the function of asking vagrant for these details.
>
<br/>
>What port is the current Vagrant VM using for SSH?
>~~~
>echo $(vagrant ssh-config | grep Port | awk '{print $2}')
>~~~
>{: .bash} 
>
>
<br/>
>Where is the SSH key used to connect to the current Vagrant VM's `vagrant` user?
>~~~
>echo ${vagrant ssh-config | grep IdentityFile | awk '{print $2}'}
>~~~
>{: .bash} 
{: .solution}

Verify it's working:
~~~
ansible -i hosts-file -m ping localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')
~~~
{: .bash}

>## Seeing ssh errors?
>
>If you've an `~/.ssh/known_hosts` entry for `localhost` then this new 
>VM is likely to conflict. You will have to delete the existing entry 
>with the corresponding line number in the output from Ansible.
>This can be done manually, or with the following snippets:
>~~~
># Looking in the output for known_hosts:line-number
>echo $(ansible -i hosts-file -m ping localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')  | grep -oP '(?<=hosts:)\d+')
># Use sed to delete that line. Replace "LINENUMBER" with the line number from the output above.
>sed -i.bak -e "LINENUMBERd" ~/.ssh/known_hosts
>~~~
>{: .bash} 
> Next time you run the `ansible -m ping` command, you'll be asked whether you trust the host key is correct. Type `yes` to continue.
{: .solution}

~~~
ansible-playbook -i hosts-file --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}') --become  playbook.yml
~~~
{: .bash}

The README.md explains what we need to do:

{% include links.md %}
