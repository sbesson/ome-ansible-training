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
$ git clone https://github.com/ome/ansible-examples-omero.git 
# or
$ git clone git@github.com:ome/ansible-examples-omero.git
~~~
{: .bash} 

Change directory to the 'public-user' example in the clone of ansible-examples-omero
~~~
$ cd ansible-examples-omero/public-user
~~~
{: .bash} 

Install the prerequisite roles, defined in requirements.yml
~~~
$ ansible-galaxy install -r ../requirements.yml -p roles
~~~
{: .bash}

Add some port forwarding to the Vagrantfile, to allow us to connect to OMERO.
~~~
$ rm Vagrantfile && wget https://gist.github.com/kennethgillen/648105ba0f78440ca41e45963c471744/raw/Vagrantfile
~~~
{: .bash} 

> ## Want to manually edit the Vagrantfile instead?
>
>  We want to end up with the following:
>  ...
>  ~~~
>  config.vm.provider "virtualbox" do |vb|
>  config.vm.network "forwarded_port", guest: 80, host: 8080
>  config.vm.network "forwarded_port", guest: 443, host: 8443
>  config.vm.network "forwarded_port", guest: 4063, host: 4063
>  config.vm.network "forwarded_port", guest: 4064, host: 4064
>    vb.customize....
>  ~~~
>  {: .code}
{: .callout}

Tell Vagrant to kick off the creation of our VM (Takes 30s)
~~~
$ vagrant up
~~~
{: .bash} 


Create an inventory file for the vagrant-driven local VM:
~~~
$ echo "localhost ansible_port=$(vagrant ssh-config | grep Port | awk '{print $2}') ansible_user=vagrant " > vagrant-inventory
~~~
{: .bash}

It should look similar to:
~~~
$ cat vagrant-inventory
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
>$ echo $(vagrant ssh-config | grep Port | awk '{print $2}')
>~~~
>{: .bash} 
>
>
<br/>
>Where is the SSH key used to connect to the current Vagrant VM's `vagrant` user?
>~~~
>$ echo $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')
>~~~
>{: .bash} 
{: .solution}

Verify Ansible can connect to the VM - the ping module again.
~~~
ansible -i vagrant-inventory -m ping localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')
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
>echo $(ansible -i vagrant-inventory -m ping localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')  | grep -oP '(?<=hosts:)\d+')
># Use sed to delete that line. Replace "LINENUMBER" with the line number from the output above.
>$ sed -i.bak -e "LINENUMBERd" ~/.ssh/known_hosts
>~~~
>{: .bash} 
> Next time you run the `ansible -m ping` command, you'll be asked whether you trust the host key is correct. Type `yes` to continue.
{: .solution}

Putting it all together - running the 'public user' example playbook. Estimated 15m runtime.
~~~
$ ansible-playbook -i vagrant-inventory --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}') --become  playbook.yml
~~~
{: .bash}

{% include links.md %}
